# Issue

**Title:** pimd_scr

## Description

# **Summary** (Briefly summarize the purpose of this pull request)

Add NPT-SCR method to PIMD.

# **Modification** (Describe the main changes made in this pull request)

New usage:
```
ensemble pimd_scr ...
```
Similar to old one 
```
ensemble pimd ...
```
but using SCR barostat instead of Berendsen barostat.

# **Licensing** (Please read and confirm before submitting this pull request)

By submitting this pull request, I agree that my contribution will be included in GPUMD and redistributed under the existing GPUMD license. Newly added source files follow the existing GPUMD license-header style when applicable. No external source code with incompatible licensing has been copied into this pull request.

# **Validation** (Describe how this pull request was tested)

```
potential   Si_2022_NEP4_3body.txt
velocity    300 seed 12345
time_step   2

dump_thermo 10
ensemble    pimd_scr 16 300 300 50 0 0 0 0 0 0 100 100 100 100 100 100 500
run         2000
```

<img width="1775" height="1137" alt="pimd_berendsen_vs_scr_4ps" src="https://github.com/user-attachments/assets/631576fa-a950-40c0-9012-e839a18ac921" />


# **Others**

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `36c6d5e05adb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.