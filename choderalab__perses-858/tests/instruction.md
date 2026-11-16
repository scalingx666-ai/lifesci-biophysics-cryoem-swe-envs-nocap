# Issue

**Title:** Add GPU testing to our CI

## Description

## Description

This workflow spins up an ec2 instance with a GPU (`p2.xlarge`), runs our tests, then terminates the EC2 instance.  Right now I have it just use one python version and openmm from conda-forge, but we can expand this if we want either in this PR or at a later date.

## Motivation and context

I just wanted to see how long these tests take and see how it behaves. To save CI time, we might want to set this to only trigger manually so after a PR is ready to go, we run the tests on AWS. We could also only run a subset of the tests as well, such as skipping any tests that are CPU only anyway. 

I used this github action https://github.com/machulav/ec2-github-runner and had to add the following secrets to the repository to get this to work. 

```
AWS_ACCESS_KEY_ID
AWS_REGION 
AWS_SECRET_ACCESS_KEY
GH_PERSONAL_ACCESS_TOKEN  
```

 


## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Added GPU testing to our CI
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `c790a780a203`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.