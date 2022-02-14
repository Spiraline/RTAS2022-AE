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
The submitted artifacts consist of experiments for (a) Fig. 11 and (b) Fig. 1, 13, and 15.

Experiment (a) is a scheduling simulation using synthetic tasks, covered in Section VII.A in the paper. The experiment compares the accuracy of the self-looping node and critical failure ratio of the DAG task of the proposed two budget analyses compared to the baseline in which the loop count is fixed.

Experiment (b) is an implementation of the autonomous driving (AD) program, covered in Section VII.B in the paper. The implementation is based on [Autoware.AI](https://www.autoware.org/), an open-source AD stack.
This experiment shows that the proposed safety guarantee mechanism is applicable to the actual AD system and can prevent physical errors.

## About the setup
Experiment (b) uses two machines: (1) a Linux computer where the AD stack is run and (2) a Windows computer that provides a simulated driving environment to the computer (1). 
Both machines require high performance. Additionally, computer (2) requires high-performance GPU, e.g., NVIDIA GTX 1080 or higher.

So setting up all of these at your local machine requires a lot of time and effort, and we do not want to ask you to do that. So instead, we can set up remotely accessible servers at SNU(Seoul, South Korea) that you can immediately start assessment without any hassle.

We also provide instructions for local setup. Refer to [Local Setup](##local-setup) section if needed.

<div style="page-break-after: always;"></div>

## Connecting the remote (SNU)

You can access our server using a remote desktop client. We will keep the server open at all times during the AE period. 
On Windows, use remote desktop client app (Only available on Windows Pro edition); on OSX, use [Microsoft Remote Desktop](https://apps.apple.com/us/app/microsoft-remote-desktop/id1295203466?mt=12).

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
    <img src="https://user-images.githubusercontent.com/44594966/153794278-f8839b19-5375-49c6-b93a-3f2fe901bc1b.png" alt="syn" width="450" height="300"/>
</div>

By default parameter setting, `acc.bat` takes about 15 minutes and `density.bat` takes 5 hours.
For shorter experiments, the `dag_num` and `instance_num` parameters can be reduced. If `dag_num` is 100, `acc.bat` takes only 10 seconds and `density.bat` takes about 3 minutes.
For details, refer to **Configurable parameter** section.

## What to expect
Fig. 11(a) shows the statistics (i.e., mean, quartiles, minimum and maximum value) of the achieved accuracy by the baseline and our methods. 

Fig. 11(b) compares the critical failure ratios by the above four methods as increasing the DAG's density *ρ*.

The critical failure can happen (1) when the achieved accuracy is lower than the acceptable threshold or (2) when the DAG execution misses the deadline.

Fig. 11(c) reports the breakdown of the reasons for the critical failures of Base Small and Base Large.

<div style="page-break-after: always;"></div>

## Design

Both scripts generate a random DAG task according to the configuration. A backup DAG is generated in consideration of the randomly determined dangling DAG. Then, they find a budget for **Ours Classic** and **Ours CPC** with proposed budget analyses in Sections V and VI. In the case of baseline, the budget is statically determined according to the loop count.

In the case of the accuracy experiment (`acc.bat`), the accuracy reached when the self-looping node repeats the loop for a given budget is obtained. When acceptable accuracy is reached, the loop stops immediately.

In the case of a density experiment (`density.bat`), it additionally schedules the DAG task by applying the obtained execution time of the self-looping node. Then it determines whether unacceptable accuracy or deadline miss has occurred.

## Interpreting the results

The results of each experiment are saved in the `syn/res` directory in the form of csv and image.
Each row in `acc.csv` means the final accuracy reached for each approach.
`density_X.csv` indicates the frequency of occurrence for each type of critical failure at a density of *0.X*.
In our cases, unacceptable accuracy may exist, but critical failure due to unacceptable accuracy is not considered because safety backup exists.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/152480134-c88b60de-4df9-42db-bd76-9c5ba4f99250.PNG" alt="syn_res" width="450" height="300"/>
</div>

In Fig. 11(a), you will see that the accuracy does not fall compared to the baseline, even if the loop count is limited through budget analysis. In Fig. 11(b), you will see proposed mechanism can prevent critical failure.

<div style="page-break-after: always;"></div>

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
|`core_num`| int |The number of cores *M*|When it is large, generating a feasible DAG with a large density is more difficult|
|`node_num`| [mean, dev] |The number of nodes *N* |randomly selected from<br/>`[mean-dev, mean+dev]`|
|`depth`| [mean, dev] |The depth of DAG|If the depth is small and the *N* is large, the DAG will have large width|
|`exec_t`| [mean, dev] |The execution time of node|mean represents&nbsp; <img src="https://user-images.githubusercontent.com/44594966/153800688-0335d69e-72bf-44e0-a052-22b3b933bafd.png" height="12">|
|`backup_ratio`| float |The execution time ratio of backup node|Larger value is more likely to cause deadline miss |
|`sl_unit`| float |<img src="https://user-images.githubusercontent.com/44594966/153800354-76fa56fb-99b0-446b-86a5-d58acca85a07.png" height="12">|-|
|`sl_exp`| float |<img src="https://user-images.githubusercontent.com/44594966/153800585-24904545-c2a7-4197-8abc-3986b5115818.png" height="20">|With small `sl_exp`, the acceptable accuracy is reached with fewer loop counts|
|`sl_std`| float |*σ*|A large *σ* models a harsh physical situation with a high probability of physical errors<br/>(Not required in `std` experiment)|
|`acceptance_threshold`| int |Acceptable accuracy<br/>in 100 times scale|If the value is small, the acceptable accuracy is reached with fewer loop counts|
|`baseline`| [small, large] |The max loop count for `BaseLine Small` and `BaseLine Large`|Larger value is more likely to cause deadline miss, and smaller value is more likely to cause unaccpetable accuracy|
|`density`| float | *ρ* |deadline&nbsp; <img src="https://user-images.githubusercontent.com/44594966/153800859-fde2b68e-76ba-4a8f-a3de-a1efffed1d1b.png" height="30"><br/>(Not required in `density` experiment)|
|`dangling_ratio`| float | Dangling DAG node # / total node # |Larger value makes the backup DAG simpler|

<div style="page-break-after: always;"></div>

# Experiment (b) (Figs. 1(b), 13, and 15)

Experiment (b) is an implementation of the autonomous driving (AD) program, covered in Section VII.B in the paper. The implementation is based on [Autoware.AI](https://www.autoware.org/), an open-source AD stack.
This experiment shows that the proposed safety guarantee mechanism is applicable to the actual AD system and can prevent physical errors.

## Overview of Remote setup

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153333701-528a4ea1-738d-48b0-9b78-a35cffedb0c0.PNG" alt="remote_stack" height="160"/>
</div>

Experiment (b) uses two machines:
1. A Linux computer where the AD stack is run and
2. A Windows computer that provides a simulated driving environment to computer 1.

### Computer 1

Computer 1 executes our AD stack implementation based on [Autoware.AI](https://www.autoware.org/). The stack is run on Linux virtual machine in Virtualbox. The host machine is Ubuntu.

#### Sepcifications:

* CPU: Intel Core i7-8700 @3.20GHz (6 physical cores)
* Memory: 16GB
* Ubuntu 18.04 LTS
* VirtualBox 6.1

### Computer 2

Computer 2 runs the [SVL simulator](https://www.svlsimulator.com/). The simulator provides a virtual driving environment. Computer 2 transmits the data obtained by the virtual sensor to Computer 1 and receives vehicle commands from Computer 1.

#### Sepcifications:

* CPU: Intel Core i7-8700 @3.20GHz (6 physical cores)
* GPU: NVIDIA GeForce RTX 2080 SUPER
* Memory: 16GB
* Windows 10 Build 19042.631
* SVL Simulator Build 2021.3

<div style="page-break-after: always;"></div>

## Running experiment (b)

### Getting the simulator ready

1. Run the simulator by double-clicking `svl` on the desktop (`C:\Users\rtas\Desktop\svl`).

2. Click **Open Browser**, which will automatically show **wise.svlsimulator.com** website. Sign in with our account if necessary. (jwhan@rubis.snu.ac.kr / rtas2022)

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/152669703-290e0d81-3327-45de-ad52-a971e02d9794.PNG" alt="svl_main" height="220"/>
    <img src="https://user-images.githubusercontent.com/44594966/153361526-bcaa5fe1-07ab-4291-8397-745666cd5932.png" alt="svl_sign_in" height="220"/>
</div>

3. Navigate to the **Simulations** tab. Click the simulation instance named **API Only**. The simulator is now ready.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153797660-90157f29-02da-4d98-b216-7533ac599bb0.png" alt="svl_simul" height="220"/>
    <img src="https://user-images.githubusercontent.com/44594966/153362103-575dee89-43a8-4833-a0e9-c068362900a6.png" alt="svl_simul" height="220"/>
</div>

<div style="page-break-after: always;"></div>

### Experiment b-1) Running the accuracy experiment (Fig. 1(b))

1. Double-click the `connect.bat` script on the `impl` directory. This will open up an ssh connection to the AD server. Enter the password for virtual machine in Computer 1. (PW : rtas)

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153362505-b0f9a6d4-f72f-4393-9274-b90fe2e53c0a.png" alt="impl" height="210"/>
    <img src="https://user-images.githubusercontent.com/44594966/153364039-5da107f0-cdd7-4378-bb1a-3da6dfa0c015.png" alt="connect_bat" height="210"/>
</div>

2. Execute `./acc.sh` on `connect` terminal to start the AD modules and ROS bridge between Autoware and SVL simulator.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153530809-e49fb93d-538f-465a-9eba-a8b0e3d2b54a.png" alt="acc.sh" height="220"/>
</div>

3. Click **Run Simulation** on SVL website. Simulator app will now display **API ready**.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153797743-b7611147-1089-44da-9932-f582e269a1d7.png" alt="run_sim" height="300"/>
</div>

<div style="page-break-after: always;"></div>

4. Double-click `svl_base_map.bat` on the `impl` directory to run simulation script. You will see the vehicle in the SVL simulator window. 
(Note: The simulation script is executed correctly only when rosbridge is turned on. So you must follow the steps in order.)

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153531678-45471dc1-3273-4b38-b33c-a33bdb8241e0.png" alt="svl_sim" height="220"/>
</div>

5. Vehicle will start driving after about 20s. Observe the vehicle moving in the SVL simulator window. The experiment will automatically end after 100s.

    - You can right-click and drag to change the view angle
    - Press `w`/`s` keys to zoom in/out.
    - Press `Ctrl-C` at the `connect` terminal to stop the experiment manually.

6. Close the `svl_base_map` terminal.

### Experiment b-2) Running the WCET experiment (Fig. 13)

1. Open the `connect` terminal. (Same as experiment b-1) Skip if the terminal is already on.

2. Execute `./wcet.sh` on `connect` terminal.

3. Click **Run Simulation** on SVL website.

4. Double-click `svl_base_map.bat` on the `impl` directory.

5. The experiment will automatically end after 100s. Close the `svl_base_map` terminal after it ends.

### Experiment b-3) Running the Driving progress experiment (Fig. 15)

1. Open the `connect` terminal. (Same as experiment b-1) Skip if the terminal is already on.

2. Execute `./progress_ndt.sh` on `connect` terminal to start AD modules without safety guarantee mechanism.

3. Click **Run Simulation** on SVL website.

4. Double-click `svl_obstacle_map.bat` on the `impl` directory.

5. Driving may fail because the safety guarantee mechanism is not applied in this simulation script. Whether or not localization has failed is displayed on the `connect` terminal. The experiment will automatically end after 40s. Close the `svl_obstacle_map` terminal after it ends.

6. Without closing the `connect` terminal, execute `./progress_ours.sh` to start AD modules with safety guarantee mechanism.

7. Click **Run Simulation** on SVL website.

8. Double-click `svl_obstacle_map.bat` on the `impl` directory.

9. The experiment will automatically end after 40s. Close the `svl_obstacle_map` terminal after it ends.

## What to expect

Fig. 1(b) shows the accuracy trends according to the loop count of NDT matching in several cases.
Fig. 13 shows the WCET trends according to the loop count of NDT matching.
Fig. 15 compares driving progress and execution time of NDT matching for original Autoware and ours, i.e., safety guarantee mechanism applied Autoware.

## Design

The purpose of this experiment is to provide practical implementation by applying the safety guarantee mechanism to an actual AD software, Autoware.

The implementation is based on the March 2021 snapshot of Autoware.AI. We use 12 nodes of the original autoware. Additionally, `LKAS` is implemented for safety backup, and `gnss_calibrator` is implemented for localization through GNSS sensor. `ndt_matching`, `op_behavior_selector` nodes are modified to activate the safety guarantee mechanism through a parameter.

## Interpreting the results
Simulations automatically save the necessary log to Computer 1. When you execute `plot.sh` on `connect` terminal, each log will be parsed and saved as an image file.
Double-click `impl/get_plot.bat` and enter the password (rtas) to copy image files from Computer 1 to Computer 2. Figures will be copied to `impl/res` directory.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153536932-b4a9bd20-4884-4014-bd4a-4bf1775c8a03.png" alt="plot_sh" height="220"/>
    <img src="https://user-images.githubusercontent.com/44594966/153537096-b210b798-925e-4d56-8cb1-b9fde9d06795.png" alt="impl_res" height="220"/>
</div>

Fig. 1(b) shows the accuracy according to the loop count in several cases. In some cases, you will see the `ndt_matching`, i.e., the self-looping module never reaches the acceptable accuracy even though the loop is repeated to the maximum.

In Fig. 13, you will see the trend line of `ndt_matching`, which means&nbsp; <img src="https://user-images.githubusercontent.com/44594966/153800354-76fa56fb-99b0-446b-86a5-d58acca85a07.png" height="12">.

In Fig. 15(a), you will see that car always keeps close to the lane center.
In Fig. 15(b), you will see that the execution of `ndt_matching` does not exceed the budget, i.e., horizontal line.
The gray area means the backup mode for Our modified Autoware.

## Configurable Parameter
Since we implement from original Autoware, legacy parameters also exist. Therefore, in these experiments, We explain the configurable parameters and the effects of changing them.

There are yaml files for each experiment in `impl/cfg`.

- `acc_config.yaml`: Expeiment b-1
- `wcet_config.yaml`: Expeiment b-2
- `progress_*_config.yaml`: Expeiment b-3

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153537264-e5a861f0-fa2c-4ca9-9636-26f1a5d9d1b1.png" alt="impl_cfg" height="250"/>
    <img src="https://user-images.githubusercontent.com/44594966/153537455-e388f488-d2b8-455f-ac63-ce6ffa849da3.png" alt="acc_cfg" height="250"/>
</div>

When you executes `configure.bat`, these files are copied to Computer 1 through secure copy.
If the experiment does not run normally due to the incorrect configuration, you can run `reset_config.sh` on `connect` terminal to make it the initial setting.

The meaning and a brief explanation of each parameter are as follows.

### Common parameter

| parameter | description | effect |
|:--:|:--:|:--:|
|`res_t_log`|Decide whether to log response time|-|
|`ndt_lkas_flag`|Decide whether to activate safety guarantee mechanism|-|
|`pnorm_threshold`|*1-pnorm\_threshold* will be accuracy threshold<br/>to activate safety backup|Smaller value activates<br/>safety backup more frequently|

### voxel_grid_filter

| parameter | description | effect |
|:--:|:--:|:--:|
|`leaf_size`|Filtering strength for input pointcloud|Smaller value makes the execution longer|
|`measurement_range`|Filtering range|Larger value makes the execution longer|

### ndt_matching

| parameter | description | effect |
|:--:|:--:|:--:|
|`get_height`|Decide whether to consider z-axis during localization|-|
|`time_wall`|Budget for self-looping node|Only valid when `ndt_lkas_flag` is `true`|
|`accuracy_log`|Decide whether to log accuracy<br/>according to the loop count|-|

### ndt_config

| parameter | description | effect |
|:--:|:--:|:--:|
|`init_*`|Where to start localization|The closer it is to the actual vehicle location,<br/>the better the first localization is|
|`resolution`|Grid size using in NDT algorithm|Larger value may improve localization quality<br/>but makes the execution longer|
|`step_size`|Maximum gradient update in one loop|Too large value leads to worse localization quality|
|`trans_epsilon`|*1-trans\_epsilon* will be<br/>accuracy threshold for localization|Small values makes localization harder|
|`max_iterations`|Maximum self-looping count|Only valid when `ndt_lkas_flag` is `false`|

### lidar_euclidean_cluster_detect

| parameter | description | effect |
|:--:|:--:|:--:|
|`clip_min_height`|Ignore point with a height lower than this value|Inappropriate values can cause the ground to be perceived as an object.|
|`clip_max_height`|Ignore point with a height higher than this value|The object may not be perceived when it is small|
|`cluster_size_min`|Minimum number of points in one cluster|-|
|`cluster_size_max`|Maximum number of points in one cluster|-|

### op_common_params

| parameter | description | effect |
|:--:|:--:|:--:|
|`maxVelocity`|Maximum velocity|Too large value makes driving unstable|
|`maxAcceleration`|Maximum acceleration|same as `maxVelocity`|
|`maxDeceleration`|Maximum deceleration|same as `maxVelocity`|
|`width`|Width of vehicle|Determine safety border of vehicle|
|`length`|Length of vehicle|same as `width`|

### op_global_planner

| parameter | description | effect |
|:--:|:--:|:--:|
|`use_static_goal`|Decide whether to set the goal point with script|You should set goal point in `rviz`<br/>when `false`|
|`multilap_flag`|Decide whether to continue driving<br/>the circular track more than once|-|
|`goal_*`|Position of the goal point|Only valid when `use_static_goal` is `true`|

## Notes
The experiment in the paper executes the AD stack directly on the host machine, not on a virtual machine. Therefore, quantitative values such as WCET may differ according to differences in machine performance.

Also, NDT is a probability-based algorithm. Therefore, even if the experiment is carried out under the same conditions, the driving may succeed or fail.

<div style="page-break-after: always;"></div>

## Local Setup

All the installation files and the compressed repository required for the experiment are in our server's `Documents` directory. Copy them by `Ctrl+C,V` via remote desktop if needed.

### Running experiment (a) on local machine

Since it is simply a python script, it can be executed on a low-performance machine. After you clone our public repository and install only a few python package, you can run execution file in the same way.

1. Install [Python 3](https://www.python.org/). We use 3.10.2 version.

2. Clone or download as ZIP [Spiraline/RTAS2022-AE](https://github.com/Spiraline/RTAS2022-AE) repository.

    ```
    git clone https://github.com/Spiraline/RTAS2022-AE
    ```

3. Install required packages.

    ```
    python -m pip install -U PyYAML
    python -m pip install matplotlib pandas
    ```

4. Follow the steps in [Running experiment (a)](#running-experiment-(a)) with `syn` directory.

### Running experiment (b) on local machine

#### Computer 1 (machine with AD stack)

1. Install [VituralBox](https://www.virtualbox.org/wiki/Downloads). We use 6.1 version.

2. Create virtual machine with Ubuntu (64-bit)

- We recommend a virtual hard disk size of 30 GB or more. With a smaller size, Autoware build may fail.
- Download [Ubuntu 18.04.6 iso file](https://releases.ubuntu.com/18.04/) and install Ubuntu from the downloaded file.
- You can use our VM image (`Documents/ubuntu_rtas.vdi`) that has already been installed Autoware.

3. Install ROS, Autoware and required packages. We provide a single shell script for all installation procedures in our [Autoware-LKAS](https://github.com/Spiraline/Autoware-LKAS) repository.

    ```
    sudo apt-get install git -y
    git clone https://github.com/Spiraline/Autoware-LKAS.git -b AEC rtas
    cd rtas
    bash setup/install.sh
    bash setup/source.sh
    ```

4. Open `Network Settings` in VirtualBox and add 22 and 9090 ports in `Port Forwarding Rules`. Check guest IP with `ifconfig` command in your virtual machine.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153808441-17623483-e4b4-47b9-8d78-6c2978bf5f1c.png" alt="network_setting_1" height="250"/>
    <img src="https://user-images.githubusercontent.com/44594966/153808700-fb0037af-9dcf-4158-b40a-e0d73c8dc81d.png" alt="network_setting_2" height="250"/>
</div>

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153809466-3c92bbf9-0b19-452b-bdb4-b17b88fe5a12.png" alt="network_setting_3" height="250"/>
    <img src="https://user-images.githubusercontent.com/44594966/153809426-6df19899-d3da-4974-beb1-f9ecc27f823f.png" alt="network_setting_4" height="250"/>
</div>

5. Allow firewall of two ports on the host machine if needed.

    ```
    sudo ufw allow 22
    sudo ufw allow 9090
    ```

6. Clone shell script for experiments from [RTAS2022-AE](https://github.com/Spiraline/RTAS2022-AE.git) repository. Use shell scripts in `impl_linux` directory.

    ```
    git clone https://github.com/Spiraline/RTAS2022-AE.git
    ```

#### Computer 2 (machine with simulator)

1. Download the [SVL simulator](https://www.svlsimulator.com/) zip file and extract. Create a shortcut to `simulator.exe` if needed.

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/153810179-3164826d-57ee-4ca3-82b3-ab6d4301a9ec.png" alt="network_setting_4" height="350"/>
</div>

2. Install [Python 3](https://www.python.org/).

3. Download [lgsvl/PythonAPI](https://github.com/lgsvl/PythonAPI.git) repository as ZIP file and extract. You can clone repository if you installed git on your machine.

4. Run below commands in repository to install required Python packages.

    ```
    python -m pip install --upgrade pip
    python -m pip install -r requirements.txt --user .
    python -m pip install -U PyYAML
    ```

5. Download [RTAS2022-AE](https://github.com/Spiraline/RTAS2022-AE.git) repository as ZIP file and extract. You can clone repository if you installed git on your machine.

6. Change address and port of **lgsvl_bridge** in `impl/svl_script/CubeTownBase.yaml` and `impl/svl_script/CubeTown_Obstacle.yaml` files. Address should be Computer 1's host machine IP.

7. Change port number in `impl/apply_config.bat`, `impl/connect.bat`, `impl/get_plot.bat` from 20022 to 22. The port number is written after the `-p` option.

8. Follow the steps [Running experiment (b)](#running-experiment-(b)) with `impl` directory.