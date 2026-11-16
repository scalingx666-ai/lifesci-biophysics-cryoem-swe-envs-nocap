# Issue

**Title:** Meta: the prompt names the knowledge base that is attached

## Description

## Summary

When you attach a knowledge base to a SciLink session (`--knowledge-dir`, or by agreeing when asked), the meta agent was never told that one was attached. It passed the knowledge base on to its planning specialist, but its own instructions listed only knowledge bases that were *not* attached. Asked for a plan that needed facts recorded in the knowledge base, it searched your uploads, found nothing and stopped without asking planning. Now its instructions name the attached knowledge base and its documents, and say that planning is grounded in it.

This is the meta half of the problem #686 fixes on the planning side. Each PR helps on its own; with both, the request goes to planning once and comes back with the facts.

## Technical description

**The defect.** `MetaOrchestratorAgent._kb_note()` is appended to the system prompt by the constructor and by `set_meta_mode`, which `attach_knowledge_dir` calls to refresh the prompt. It built a "KNOWLEDGE BASES (detached)" list, and its first line was `if self.knowledge_dir: return ""`. With a KB attached, the prompt said nothing about knowledge bases at all.

**The change.**
- `_kb_note()` returns `_attached_kb_note()` when a KB is attached.
- The note is a "KNOWLEDGE BASE (attached)" line giving:
  - the manifest name and description, or the path for a plain directory;
  - its path;
  - up to 15 source names, from the manifest or from `default_kb_docs.sources.json`.
- It adds one sentence: every planning delegation is grounded in it, so a task that needs what it records goes to `delegate_to_planning`.
- The detached listing is unchanged.

**Tests: `tests/test_meta_attached_kb_note.py`.**
- An attached store KB is named with its description and sources.
- An attached plain directory is named by its path.
- Detached KBs are still listed when none is attached.
- Attaching moves the prompt from the detached list to the attached note, through `set_meta_mode` as `attach_knowledge_dir` does.

The three attached-KB tests fail on `main`.

**Live verification** (Bedrock `us.anthropic.claude-opus-4-8`, KB built with `amazon.titan-embed-text-v2:0`).
- **Setup.** An autonomous meta session with a named KB attached received a plain request, with no routing hint: design one XRD re-check "using the lab's own instrument and storage procedure as recorded in the attached knowledge base", naming the asset tag and vial code that exist only in the KB. The KB's cached copies were excluded from the search.
- **Results:**

| code | delegations | outcome |
|---|---|---|
| `main` | none | searched uploads and session state, stopped after 23 s; neither fact |
| this branch | 3 × planning | the meta routed to planning. The planning child still lacked its workspace paths (the #686 bug) and twice reported the KB unreachable; on the third try the meta read `lab.md` itself and put the values in the task. Both facts named. |
| this branch + #686 | 1 × planning | one delegation, with the values **not** in the task; planning found both facts in the KB. |

Full offline suite: no failures that `main` does not have.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `1ebc70d3c7e9`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.