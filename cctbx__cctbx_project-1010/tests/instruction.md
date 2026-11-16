# Issue

**Title:** Make `mpi_helper`'s method non-rooted if `root=None`

## Description

A common pattern in multiprocessing is to collect data on the root (typically rank 0), process it there, and broadcast it back to other ranks. However, the better practice is typically to use a non-rooted operation: pass the inputs between all ranks and perform calculations independently on each. This removes the need for a second communication (broadcast) and improves code readability.

Non-rooted approach is inferior only in these rare cases where the cost of the switching to non-rooted operation is much larger than the cost of the secondary broadcast. Non-rooted approach might potentially also cause issues with memory consumption if the input or temporary objects (created when analyzing input) are very volumous.

In xfel, communicating between ranks can be done using the `mpi_helper` class, whose methods alias more complex operations to improve readability and remove repetition. Currently all of the `mpi_helper` methods are rooted, forcing one to copy or rewrite them whenever an non-rooted collective is more desired.

This patch slightly modifies all `mpi_helper` methods, allowing them to use non-rooted mpi4py collectives. Whenever their keyword argument `root` (default `0`) is set to `None` instead of an integer, the methods will use a non-rooted collective instead of the collective variant. This will produce identical output on all ranks, removing need for some follow-up broadcasts and allowing for cleaner and potentially faster code.

Following the update, I have also modified three (cctbx_project) instances where `mpi_helper` methods were used and could be changed to utilize non-rooted collective variants to potentially speed up the execution. Each was done in a separate commit to revert if change is not desired. The change will also positively affect code in other repositories utilizing the `mpi_helper`.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `e9bce95d2fe0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.