# Issue

**Title:** Speed up u kln construction for Pairbar

## Description

* Speeds up Sequential and HREX (by about 2%), not bisection. Bisection requires more energy comparisons, due to the dynamic nature of the schedule.
* Only load each set of frames into memory once
* Evaluate all parameter sets of interest once, reducing the number of neighborlist constructions

# Benchmarks
Run on Nvidia A10g
Cuda Arch 8.6

## Complex
![image](https://github.com/user-attachments/assets/bd9e6985-1bfd-4604-97bf-e1b6582c5e19)
![image](https://github.com/user-attachments/assets/6619e731-ed2c-4411-a606-c6b4a9890449)

## Solvent
![image](https://github.com/user-attachments/assets/7f2e792c-221c-4057-a885-647bc5462855)
![image](https://github.com/user-attachments/assets/40b079eb-daec-4838-91d4-4cd095c19ee8)


## Master
<details><summary>Benchmark Plots</summary>
<p>

![vacuum_1000_benchmarks_master](https://github.com/user-attachments/assets/2544e06b-9535-4fb7-a004-01f79609f8b0)
![solvent_1000_benchmarks_master](https://github.com/user-attachments/assets/2802ecc0-e47a-4ac2-9b8a-2ee0d7799a36)
![complex_1000_benchmarks_master](https://github.com/user-attachments/assets/bf9d2d2c-9c0b-4138-b7d6-2e548ac64e05)
</p>
</details> 


## PR
<details><summary>Benchmark Plots</summary>
<p>

![vacuum_1000_benchmarks](https://github.com/user-attachments/assets/2aac3a72-02d0-4564-accc-f75af2e06f9f)
![solvent_1000_benchmarks](https://github.com/user-attachments/assets/f3ca78bd-e4ec-48e1-b95f-4f811d13c14d)
![complex_1000_benchmarks](https://github.com/user-attachments/assets/4a70bc89-67aa-41d8-a687-71bf3aa7b85c)
</p>
</details>

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `870e0a74dbfd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.