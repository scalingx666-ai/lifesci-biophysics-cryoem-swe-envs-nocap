# Issue

**Title:** Vectorized Recordings

## Description

Same as PR #805 but from my side is ready. 

[tmp.py](https://github.com/user-attachments/files/31867032/tmp.py)

Running the above script on an H100 on the this branch gave me

```
Created Network: 12.188806571997702s
Inserted channels: 0.0035030310973525047s
Inserted synapses: 4.91081273695454s
Added stimulus: 4.646241574082524s
Added recording: 4.641703790053725s
Lowering: 28.356287381146103s
Compiling: 0.7313280128873885s
Running: 0.006916529033333063s
```

while on the main branch it returned

```
Created Network: 12.139616114087403s
Inserted channels: 0.003935906104743481s
Inserted synapses: 11.178901250008494s
Added stimulus: 5.253586169797927s
Added recording: 5.075950373895466s
Lowering: 32.77653562184423s
Compiling: 22.02194092888385s
Running: 0.008850491140037775s
```

The compiling time is greatly reduced.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `61675ec0bae8`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.