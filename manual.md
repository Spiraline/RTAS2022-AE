# Artifact Evaluation - RTAS 2022

## Submitted paper (About us)

### Guaranteeing Safety Despite Physical Errors in Cyber-Physical Systems
- Authors: Jongwoo Han, Seonghyeon Park, Haejoo Jeon, Chang-Gun Lee
- E-mails: {jwhan, seonghyeonpark, haejoojeon, cglee}@rubis.snu.ac.kr
- Real-Time Ubiquitous Systems Laboratory (RUBIS)
- Seoul National University (SNU)

## AE Chair
Dear AE chair, Thank you for taking the time to evaluate this artifact. We hope that this document will make it easy to reproduce the experiments in the paper and meet the provided criteria. If there is any ambiguity, feel free to contact us through the email address above.

## A brief overview
The submitted artifacts consist of experiments for (a) Fig. 11 and (b) Fig. 1b, 13, and 15.

Experiment (a) is a scheduling simulation using synthetic tasks, covered in Section VII.A in the paper. The experiment compares the accuracy of the self-looping node and critical failure ratio of the DAG task of the proposed two budget analyses compared to the baseline in which the loop count is fixed.

Experiment (b) is an implementation of the autonomous driving (AD) program, covered in Section VII.B in the paper. The implementation is based on [Autoware.AI](https://www.autoware.org/), an open-source AD stack.
This experiment shows that the proposed safety guarantee mechanism is applicable to the actual AD system and can prevent physical errors.

## About the setup
Experiment (b) uses two machines: (1) a Linux computer where the AD stack is run and (2) a Windows computer that provides a simulated driving environment to the computer (1). 
Both machines require high performance. Additionally, computer (2) requires high-performance GPU e.g., NVIDIA GTX 1080 or higher.

So setting up all of these at your local machine requires a lot of time and effort, and we do not want to ask you to do that. So instead, we can set up remotely accessible servers at SNU(Seoul, South Korea) that you can immediately start assessment without any hassle.

## Connecting the remote (SNU)

You can access our server using a remote desktop client. On Windows, use remote desktop client app (Only available on Windows Pro edition); on OSX, use [Microsoft Remote Desktop](https://apps.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12).

Connect to remote using the following credential:

- Host: 147.46.114.95
- ID: rtas
- PW: rtas

### Windows Client

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153327218-03a5a27d-c275-4266-9215-2ead362abf2a.PNG" alt="windows_remote" height="300"/>
</div>

### OSX Client

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153325896-f41035be-c6dd-453e-a4d2-65605bfb4c1c.png" alt="osx_remote" height="300"/>
</div>

<div style="page-break-after: always;"></div>

# Experiment (a) (Fig. 11)

Experiment (a) is a scheduling simulation using synthetic tasks, covered in Section VII.A in the paper.
The experiment compares the accuracy of the self-looping node and critical failure ratio of the DAG task of the proposed two budget analyses compared to the baseline in which the loop count is fixed.

## Running experiment (a)

Open directory `syn` on the desktop.

Double-click `acc.bat` (Fig. 11(a)), and `density.bat` (Fig. 11(b) and (c)) to run the experiments.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/152480130-0e88e3e5-153f-40e9-a6fb-8218fddf41d5.PNG" alt="syn" width="450" height="300"/>
</div>

By default parameter setting, `acc.bat` takes about 15 minutes and `density.bat` takes 5 hours.
For shorter experiments, the `dag_num` and `instance_num` parameters can be reduced. If `dag_num` is 100, `acc.bat` takes only 10 seconds and `density.bat` takes about 3 minutes.
For details, refer to **Configurable parameter** section.

### Running experiment on local machine

In the case of Experiment (a), since it is simply a python script, it can be executed on your local machine. After you clone our public repository and install only a few python package, you can run execution file in the same way.

```
git clone https://github.com/Spiraline/RTAS2022-DAGGen -b AEC syn
python -m pip install -U PyYAML
python -m pip install matplotlib pandas
```

## What to expect
Fig. 11(a) shows the statistics (i.e., mean, quartiles, minimum and maximum value) of the achieved accuracy by the baseline and our methods. 

Fig. 11(b) compares the critical failure ratios by the above four methods as increasing the DAG's density $\rho$.

The critical failure can happen (1) when the achieved accuracy is lower than the acceptable threshold or (2) when the DAG execution misses the deadline.

Fig. 11(c) reports the breakdown of the reasons for the critical failures of Base Small and Base Large.


## Design

Both scripts generate a random DAG task according to the configuration. A backup DAG is generated in consideration of the randomly determined dangling DAG. Then, they find a budget for **Ours Classic** and **Ours CPC** with proposed budget analyses in Sections V and VI. In the case of baseline, the budget is statically determined according to the loop count.

In the case of the accuracy experiment (`acc.bat`), the accuracy reached when the self-looping node repeats the loop for a given budget is obtained. When acceptable accuracy is reached, the loop stops immediately.

In the case of a density experiment (`density.bat`), it additionally schedules the DAG task by applying the obtained execution time of the self-looping node. Then it determines whether unacceptable accuracy or deadline miss has occurred.

## Interpreting the results

Each figure in Fig. 11 will be saved in the `syn/res` directory.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/152480134-c88b60de-4df9-42db-bd76-9c5ba4f99250.PNG" alt="syn_res" width="450" height="300"/>
</div>

In Fig. 11(a), you will see that the accuracy does not fall compared to the baseline, even if the loop count is limited through budget analysis. In Fig. 11(b), you will see proposed mechanism can prevent critical failure.

## Configurable parameter

Each parameter has already been set to the value used in the paper.
However, you can modify as you wish by changing the yaml file inside `syn/cfg`.
Modify `acc.yaml` for the accuracy experiment (`acc.bat`), and modify `density.yaml` for the density experiment.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/152480132-e22b78e3-b523-454f-a40c-76198dc19da0.PNG" alt="syn_cfg" width="450" height="300"/>
</div>

The meaning and a brief explanation of each parameter are as follows.

| parameter | type | description | effect |
|:--:|:--:|:--:|:--:|
| `exp` | str<br/>(`acc`, `density`, `std`) | Experiment type | `acc`: measure accuracy of approaches<br/>`density`: comparison critical failure ratio according to density<br/>`std`: comparison critical failure ratio according to `sl_std`|
|`density_range`|[start, end, step]|Tenfold scale range of density to experiment with|Only valid when `exp` is `density`|
|`std_range`|[start, end, step]|100 times scale range of std to experiment with|Only valid when `exp` is `std`|
|`dag_num`| int | The number of DAG tasks<br/>to experiment with |A larger value can lead to more accurate results, but the experiment takes longer|
|`instance_num`| int |The number of instances to experiment with a single DAG task|same as `dag_num`|
|`core_num`| int |The number of cores $M$|When it is large, generating a feasible DAG with a large density is more difficult|
|`node_num`| [mean, dev] |The number of nodes $N$ |randomly selected from<br/>`[mean-dev, mean+dev]`|
|`depth`| [mean, dev] |The depth of DAG|If the depth is small and the $N$ is large, the DAG will have large width|
|`exec_t`| [mean, dev] |The execution time of node|mean represents $e_{avg}$|
|`backup_ratio`| float |The execution time ratio of backup node|Larger value is more likely to cause deadline miss |
|`sl_unit`| float |$e_{S,1}$|-|
|`sl_exp`| float |$A(L) = 1 - e^{-L/sl\_exp + ln0.3} - abs(\delta)$ |With small `sl_exp`, the acceptable accuracy is reached with fewer loop counts|
|`sl_std`| float |$\sigma$|A large $\sigma$ models a harsh physical situation with a high probability of physical errors<br/>(Not required in `std` experiment)|
|`acceptance_threshold`| int |Acceptable accuracy<br/>in 100 times scale|If the value is small, the acceptable accuracy is reached with fewer loop counts|
|`baseline`| [small, large] |The max loop count for `BaseLine Small` and `BaseLine Large`|Larger value is more likely to cause deadline miss, and smaller value is more likely to cause unaccpetable accuracy|
|`density`| float | The density of DAG task $\rho$ |deadline $D = \frac{e_{avg} \times N}{\rho \times M}$<br/>(Not required in `density` experiment)|
|`dangling_ratio`| float | Dangling DAG node # / total node # |Larger value makes the backup DAG simpler|

<div style="page-break-after: always;"></div>

# Experiment (b) (Figs. 1(b), 13, and 15)

Experiment (b) is an implementation of the autonomous driving (AD) program, covered in Section VII.B in the paper. The implementation is based on [Autoware.AI](https://www.autoware.org/), an open-source AD stack.
This experiment shows that the proposed safety guarantee mechanism is applicable to the actual AD system and can prevent physical errors.

## Overview of Remote setup

Experiment (b) uses two machines:
1. A Linux computer where the AD stack is run and
2. A Windows computer that provides a simulated driving environment to computer 1.

### Computer 1

Computer 1 is running our AD stack implementation based on [Autoware.AI](https://www.autoware.org/). The stack is running on Linux virtual machine in Virtualbox. The host machine is Ubuntu.

#### Sepcifications:

* CPU: Intel Core i7-8700 @3.20GHz (6 physical cores)
* Memory: 16GB
* Ubuntu 18.04 LTS
* VirtualBox 6.1

### Computer 2

#### Sepcifications:

* CPU: Intel Core i7-8700 @3.20GHz (6 physical cores)
* GPU: NVIDIA GeForce RTX 2080 SUPER
* Memory: 16GB
* Windows 10 Build 19042.631
* SVL Simulator Build 2021.3

## Running experiment (b)

### Getting the simulator ready

1. Run the simulator by double-clicking `svl` (on the desktop).

2. Click **Open Browser**, which will automatically show wise.svlsimulator.com website. Sign in with our account if necessary. (jwhan@rubis.snu.ac.kr / rtas2022)

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/152669703-290e0d81-3327-45de-ad52-a971e02d9794.PNG" alt="svl_main" width="300"/>
    <img src="https://user-images.githubusercontent.com/44594966/152669728-96430a56-2bb9-4f03-b863-5a0c4e5c676a.PNG" alt="svl_sign_in" width="300"/>
</div>

3. Navigate to the **Simulations** tab. Find the simulation instance named **API Only**.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/152669790-95295c03-f62c-4448-8861-ed816c6b130d.PNG" alt="svl_simul" width="300"/>
</div>

### Running the simulation

1. Double-click the `connect.bat` script on the `impl` directory. This will open up an ssh connection to the AD server. Execute ./core.sh to start the core AD modules.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/152480132-e22b78e3-b523-454f-a40c-76198dc19da0.PNG" alt="syn_cfg" width="450" height="300"/>
</div>


- Observe the vehicle moving in the SVL simulator window.
- You can right-click and drag to change the view angle or
- press `w`/`s` keys to zoom in/out.
- Press `Ctrl-C` at the `expb` terminal to stop the experiment.

## What to expect

## Design


## Interpreting the results

## Configurable Parameter

#### Common parameter

| parameter | description | effect |
|:--:|:--:|:--:|
|`res_t_log`|hi|hi|
|`ndt_lkas_flag`|hi|hi|
|`pnorm_threshold`|hi|hi|

#### voxel_grid_filter

| parameter | description | effect |
|:--:|:--:|:--:|
|`leaf_size`|hi|hi|
|`measurement_range`|hi|hi|

#### ndt_matching

| parameter | description | effect |
|:--:|:--:|:--:|
|`get_height`|hi|hi|
|`time_wall`|hi|hi|
|`accuracy_log`|hi|hi|

#### ndt_config

| parameter | description | effect |
|:--:|:--:|:--:|
|`init_*`|hi|hi|
|`resolution`|hi|hi|
|`step_size`|hi|hi|
|`trans_epsilon`|hi|hi|
|`max_iterations`|hi|hi|

#### lidar_euclidean_cluster_detect

| parameter | description | effect |
|:--:|:--:|:--:|
|`clip_min_height`|hi|hi|
|`clip_max_height`|hi|hi|
|`cluster_size_min`|hi|hi|
|`cluster_size_max`|hi|hi|

#### op_common_params

| parameter | description | effect |
|:--:|:--:|:--:|
|`pathDensity`|hi|hi|
|`rollOutDensity`|hi|hi|
|`rollOutsNumber`|hi|hi|
|`maxVelocity`|hi|hi|
|`maxAcceleration`|hi|hi|
|`maxDeceleration`|hi|hi|
|`width`|hi|hi|
|`length`|hi|hi|
|`wheelBaseLength`|hi|hi|
|`turningRadius`|hi|hi|
|`maxSteerAngle`|hi|hi|
|`steeringDelay`|hi|hi|

#### op_global_planner

| parameter | description | effect |
|:--:|:--:|:--:|
|`use_static_goal`|hi|hi|
|`multilap_flag`|hi|hi|
|`goal_pos_*`|hi|hi|
|`goal_ori_*`|hi|hi|

#### lkas

| parameter | description | effect |
|:--:|:--:|:--:|
|`debug_window`|hi|hi|

<div style="page-break-after: always;"></div>

## Notes
Maybe low performance due to virtual machine
NDT is probability based.