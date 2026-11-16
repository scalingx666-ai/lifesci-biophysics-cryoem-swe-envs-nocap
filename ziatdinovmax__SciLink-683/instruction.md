# Issue

**Title:** Knowledge bases and the sessions index survive concurrent writers

## Description

> Stacked on #678: it uses the `scilink/utils/file_lock.py` that PR adds. Review after #678; this PR's own change is the last commit.

## Summary

Two SciLink sessions adding documents to the same named knowledge base at the same time used to lose one of the additions silently. Two builds of one knowledge base could wipe each other's work in progress. Now the second one waits for the first and builds on what it published. The list of sessions you can resume (`~/.scilink/sessions.jsonl`) could likewise lose entries when several SciLink processes started at once. It now keeps every one.

Stage 0 of the agent-swarms proposal (#676).

## Technical description

**`scilink/knowledge/kb_store.py`**
- `create_kb`, `import_kb`, `add_to_kb`, `rebuild_kb` and `delete_kb` are wrapped by `_serialized_per_kb`.
- It validates the name, then holds `path_lock(<store>/.<name>)` for the whole call, across threads and processes (on Windows too, through #678's `msvcrt` lock).
- Before this:
  - every build staged into the fixed `.staging_<name>` and started by `rmtree`-ing it, so concurrent builds deleted each other's staging;
  - `add_to_kb` copied the live KB, added its documents and swapped in, so the last swap won and earlier additions were lost.
- Under the lock a second caller waits and then works on what the first one published. The fixed staging and `.bak` names become safe, because only the lock holder can be building that KB; a staging directory it finds belongs to a dead build.
- Readers: publication is the rename-based `_swap_into_place`, but readers copy a store KB file by file into a session cache, so a copy could straddle a publish. Fixed after review: `snapshot_kb` accepts a copy only if the KB's generation (directory inode, manifest) is unchanged across it, and all three copy sites use it (see "Review follow-ups").

**`scilink/sessions.py`**
- `_write_index` wrote the fixed `sessions.jsonl.tmp` and then `os.replace`d it. Its read-modify-write was guarded by a `threading.Lock`, so two processes shared the temp file and lost updates.
- Now `_index_lock()` holds `path_lock(index_path())` around each read-modify-write, and the file is published with `atomic_write_text`.
- If the lock file cannot be created (a read-only home), it runs unlocked, keeping the module's rule that the index never breaks a session.

**Tests: `tests/test_kb_store_concurrency.py`.**
- Concurrent additions from 3 threads all land.
- Two creates of one name give exactly one `created` and one `FileExistsError`, with the KB intact.
- Concurrent additions from 3 processes all land.
- `delete_kb` waits for a running addition.
- 5 processes × 8 registrations keep all 40 index records.
- A read-only home still registers without raising.

4 of these fail on `main`: both addition races, delete-during-add, and the sessions index. The existing `test_kb_store.py`, `test_sessions_index.py`, `test_shell_sessions.py` and KB provenance tests pass unchanged.

**Live verification** (Bedrock: `amazon.titan-embed-text-v2:0` embeddings, `us.anthropic.claude-opus-4-8`):
- A KB was built from one document with real Titan embeddings. Then three processes each added a document at the same moment. They finished at about 4, 6 and 8 s, which shows them waiting their turn.
- The published KB lists all four documents with 4 vectors.
- A retrieval query for a fact held only by each added document returns that document (3 of 3).
- Five processes registering 10 sessions each at once left all 50 records in the index.
- A planning agent given this KB loaded it (`kb_cache/`) but answered that no knowledge base was accessible. Not this PR: the cause is in plan mode (see below).

**Found, not changed (unrelated to this PR, noted for follow-up):**
- **A planning agent loses its workspace paths when its autonomy level changes.** `PlanningOrchestratorAgent.set_autonomy_level` rebuilds the system prompt from `get_system_prompt` alone. It drops the workspace block (data, knowledge and code directories) that the constructor and `_rebuild_system_prompt` append. `run_task` switches the level on every call, and the meta builds its planning child in CO_PILOT, so a delegated planning run never sees its knowledge directory in the prompt. Checked directly: built in AUTOPILOT with a knowledge dir, the prompt names it; after `set_autonomy_level(AUTONOMOUS)` it does not. To be fixed separately.
- **OpenMP on macOS (environment, not SciLink).** The pip wheels of torch, faiss-cpu and scikit-learn each bundle `libomp`. A process that loads torch before faiss and then searches a faiss index aborts with `OMP: Error #15`. The live script above did that; the real plan-mode path (a KB built earlier, then opened by a planning session) retrieved fine without any workaround. SciLink rightly does not set `KMP_DUPLICATE_LIB_OK`: the OpenMP runtime itself calls it unsafe and able to silently produce incorrect results. The live run set it only for its own process.
- Full offline suite: no failures that `main` does not have.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a7c7ab674758`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.