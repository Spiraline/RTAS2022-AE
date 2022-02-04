# Artifact Evaluation - RTAS 2022

## Submitted paper (About us)

### Guaranteeing Safety Despite Physical Errors in Cyber-Physical Systems
- Authors: Jongwoo Han, Seonghyeon Park, Haejoo Jeon, Chang-Gun Lee
- E-mails: {jwhan, seonghyeonpark, haejoojeon, cglee}@rubis.snu.ac.kr
- Real-Time Ubiquitous Systems Laboratory (RUBIS)
- Seoul National University (SNU)

## AE Chair
Thanks

## A brief overview
hi

## About the setup
bye


## Connecting the remote (SNU)
hi

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

By default parameter setting, `acc.bat` takes about 15 minutes and `density.bat` takes 50 minutes.
For shorter experiments, the `dag_num` and `instance_num` parameters can be reduced. If `dag_num` is 100, `acc.bat` takes only 10 seconds and `density.bat` takes about 3 minutes.
For details, refer to **Configurable parameter** section.

**TODO : 10000 DAG time for density**

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

<div style="text-align:center;">
    <img src="https://user-images.githubusercontent.com/44594966/152480132-e22b78e3-b523-454f-a40c-76198dc19da0.PNG" alt="syn_cfg" width="450" height="300"/>
</div>

| parameter | type | description            | effect                            |
|:-----------:|:------:|:------------------------:|:------------------------:|
| `exp`     | str (`acc`, `density`, `std`) | select experiment type | hihihihihiihihihihihihihihihihihidishahfioeqnfiqnfdq<br/>wqndpiwqnddqwd |
|`exp_range`|[start, end, step]|                        |                                   |
|           |      |                        |                                   |

* `exp` (str): select experiment type (`acc`, `density`, `std`)
* `exp_range` ([start, end, step]): same as python `range()`
    - `density_range`: The values are on a scale of 100 times (Required in `density` experiment)
    - `std_range`: The values are on a tenfold scale (Required in `std` experiment)
* `dag_num` (int): set the number of DAGs
* `instance_num` (int): set the number of instances
* `core_num` (int): set the number of cores
* `node_num` ([mean, dev]): set the number of nodes between `[mean-dev, mean+dev]`
* `depth` ([mean, dev]): set the depth of DAG between `[mean-dev, mean+dev]`
* `exec_t` ([mean, dev]): set the execution time of task between `[mean-dev, mean+dev]`

* `backup_ratio` (float): execution time ratio of backup node
* `sl` : Self-looping node's accuracy function is $A(L) = 1 - e^{-L/sl\_exp + ln0.3} - \left| N(0, sl\_std) \right|$
    * `sl_unit` (float) : $e_{S, 1}$
    * `sl_exp` (float)
    * `sl_std` (float): (Not required in `std` experiment)

* `acceptance_threshold` (int): Acceptance threshold for score function
* `baseline` ([small, large]): loop count for `BaseLine Small` and `BaseLine Large`
* `density` (float): (Not required in `density` experiment)
* `dangling_ratio` (float): dangling DAG node # / total node #


<div style="page-break-after: always;"></div>

# Experiment (b) (Fig. 1(b))

![connect.bat](https://user-images.githubusercontent.com/44594966/152341296-bd1cf8da-4619-4704-983c-f7aa8c092071.PNG)

<div style="page-break-after: always;"></div>

# Experiment (c) (Fig. 13)


<div style="page-break-after: always;"></div>

# Experiment (d) (Fig. 15)




## Notes
Maybe low performance due to virtual machine
NDT is probability based.