# Artifact Evaluation - RTAS 2022

## Submitted paper (About us)

### Guaranteeing Safety Despite Physical Errors in Cyber-Physical Systems
- Authors: Jongwoo Han, Seonghyeon Park, Haejoo Jeon, Chang-Gun Lee
- E-mails: {jwhan, seonghyeonpark, haejoojeon, cglee}@rubis.snu.ac.kr
- Real-Time Ubiquitous Systems Laboratory (RUBIS)
- Seoul National University (SNU)

## A brief overview
The submitted artifacts consist of experiments for (a) Fig. 11 and (b) Fig. 1, 13, and 15.

Experiment (a) is a scheduling simulation using synthetic tasks, covered in Section VII.A in the paper. The experiment compares the accuracy of the self-looping node and critical failure ratio of the DAG task of the proposed two budget analyses compared to the baseline in which the loop count is fixed.

Experiment (b) is an implementation of the autonomous driving (AD) program, covered in Section VII.B in the paper. The implementation is based on [Autoware.AI](https://www.autoware.org/), an open-source AD stack.
This experiment shows that the proposed safety guarantee mechanism is applicable to the actual AD system and can prevent physical errors.

## Contents

- `impl`: Window batch scripts for implementation experiment
- `impl_linux`: Linux shell scripts for implementation experiment
- `syn`: Synthetic experiment code
- `cover_sheet.md`: Cover sheet
- `README.md`: This document