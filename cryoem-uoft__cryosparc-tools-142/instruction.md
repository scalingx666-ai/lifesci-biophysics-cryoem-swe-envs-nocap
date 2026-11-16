# Issue

**Title:** fix: correct typevar generated definitions

## Description

Previously typevars were not correctly generated in the api client type stub, in their original generic form defined in CryoSPARC:
```py
class ResourcesNamespace(APINamespace):
    ...
    def find_targets(self, *, lane: Optional[str] = ...) -> List[SchedulerTarget]: ...
    def find_nodes(self, *, lane: Optional[str] = ...) -> List[SchedulerTarget[Node]]: ...
    def find_clusters(self, *, lane: Optional[str] = ...) -> List[SchedulerTarget[Cluster]]: ...
```

Now generate correctly:

```py
class ResourcesNamespace(APINamespace):
    ...
    def find_targets(self, *, lane: Optional[str] = ...) -> List[SchedulerTarget]: ...
    def find_nodes(self, *, lane: Optional[str] = ...) -> List[SchedulerTarget_Node_]: ...
    def find_clusters(self, *, lane: Optional[str] = ...) -> List[SchedulerTarget_Cluster_]: ...
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6282fb7c6dab`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.