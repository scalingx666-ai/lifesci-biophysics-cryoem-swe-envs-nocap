# Issue

**Title:** Document comm and multi_sim_comm (#1443, part A)

## Description

##### Description

Part **A** of #1443: document the replica communicators so their contract is visible to both action authors and MD-code authors. Documentation only; no code changes.

- `Action::comm`: the ranks of this replica; valid on every rank.
- `Action::multi_sim_comm`: one rank per replica, valid only on rank 0 of `comm`. Elsewhere it is `MPI_COMM_SELF`, which reports one replica and reaches no other replica. Includes the read-on-rank-0-and-broadcast pattern.
- `PlumedMain::comm` / `multi_sim_comm`: a short note plus a pointer to the `Action` comments.
- `GREX::intracomm` / `intercomm`: what each holds, and that `setMPIIntercomm` also sets `PlumedMain::multi_sim_comm` and is passed only on rank 0.
- `developer-doc/mdTemplate.md`: new section *Dealing with multiple replicas* next to *Dealing with parallelism*. It shows which calls an MD code makes on which processes (taken from `driver`) and what each process then sees inside PLUMED. It is linked from `Action::multi_sim_comm` with `\ref mpireplicas`.

When this is merged into `master`, the comment can also point to `getNumberOfReplicas()` / `getReplicaIndex()` (#1481).

##### Target release

I would like my code to appear in release __v2.9__

##### Type of contribution

- [X] changes to code or doc authored by PLUMED developers, or additions of code in the core or within the default modules
- [ ] changes to a module not authored by you
- [ ] new module contribution or edit of a module authored by you

##### Copyright

- [X] I agree to transfer the copyright of the code I have written to the PLUMED developers or to the author of the code I am modifying.

##### Tests

- [ ] I added a new regtest or modified an existing regtest to validate my changes.
- [ ] I verified that all regtests are passed successfully on [GitHub Actions](https://github.com/plumed/plumed2/actions).

<!-- Comments only, so no regtest. -->

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3a685c2c11d9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.