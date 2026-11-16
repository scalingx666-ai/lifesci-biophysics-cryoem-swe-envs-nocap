# Issue

**Title:** A swarm analysis item carries its own depth, by the rule a delegation and a fan-out branch use

## Description

When you ask SciLink for one analysis, or several analyses to be fused, you can say how thorough it needs to be:
- a full analysis to interpret and report;
- a quick look;
- just the numbers for another step, together with which quantities you need and a time budget.

A swarm (several delegations run at once) had no way to say this for its analysis items, so every one ran a full, thorough analysis even when a quick screening number was all that was needed.

Swarm analysis items now take the same three settings. All three paths read them by one shared rule, and a bad value (an unknown profile name, say) is refused with the reason before anything starts. An item's setting is also kept when the swarm re-runs it, after a memory-pressure cancel or after a finding it relied on was withdrawn. A quick item's measured cost is kept apart from a thorough one's, so neither sizes the other's share of the swarm's token budget.

.

---

### Technical details

- **One rule:** `fanout.analysis_depth(source)` reads `profile`, `targets` and `time_budget_s` from a delegation's arguments, a fan-out branch or a swarm item.
  - Unset keys are left out, so an item without them makes exactly today's call.
  - It raises `ValueError` with the reason for: a profile `resolve_profile` does not know, targets that are not strings, and a budget that is not a positive number.
  - A single target string becomes a one-item list. Before, `run_task` would have split it into characters.
- **Each path:**
  - **Single delegation:** `_delegate` refuses before opening a ledger entry. Before, the child raised mid-delegation.
  - **Fan-out:** normalisation refuses the call and names the branch.
  - **Swarm:** `normalize_items` refuses the item with its reason (`not_started`), and other modes ignore the fields as before. A fired reaction goes through the same normalisation, and `reactions.normalize_subscriptions` refuses a subscription whose `enqueue` carries a bad value.
- **Swarm item:**
  - Its depth is on its ledger entry (`depth`).
  - It reaches `run_task` on the thread placement, and on the process placement through `item_spec` / `run_item`.
  - A memory-guard requeue keeps it.
  - `retract_and_report`'s `rerun_items` carry the entry's `depth` and `pattern`.
  - `pattern` is now in the `run_swarm` schema and recorded on the entry. It sizes the item's memory estimate and time budget. As before, it does not scope the child: the task names its files.
- **Measured classes:** `peaks.item_class` appends the profile for a non-default preset: `…:quick`, `…:extract`, and a dict profile by its `base`.
  - Before this PR, every swarm analysis ran thorough, so one key fit.
  - Now a quick run's tokens and peak would otherwise size a thorough item's reservation, and the reverse.
  - A thorough item keeps the plain key, so existing `measured_items.json` rows still apply.
- **Budget values:** `time_budget_s` refuses NaN, infinity and a boolean. `0` stays "no budget", as every depth path has always treated a falsy value.
- **Schema:** `run_swarm` work items and subscription `enqueue` gain `profile` (the cold-start presets), `targets`, `time_budget_s` and `pattern`.
- **Docs:** CLAUDE.md, "A swarm item is a fresh agent".
- **Not included: #773 part 1.** The issue proposed one guidance sentence telling the meta that a subscription acts on one finding, so a step over every result goes after `run_swarm` returns. A live A/B against `main` (the real meta model, with `run_swarm` and the delegations intercepted) found no difference:
  - **Neutral prompts** ("identify the phases, then plan a follow-up"): neither arm declared a planning subscription, in 6 runs each.
  - **"As soon as a phase is confirmed, have a plan written":** both arms declared one, in 4 of 4 runs each.

  So the sentence is left out and #773 stays open.
- **Tests** (`tests/test_swarm_item_depth.py`, 18):
  - An item's depth reaches its worker and its entry.
  - Without depth the call is unchanged, and a planning item takes none.
  - A memory-guard rerun keeps it.
  - The process worker runs under it.
  - The same input reaches the child identically on all three paths (parametrised, including normalised inputs).
  - A bad value is refused on all three paths before anything runs.
  - A subscription is refused on declaration.
  - A re-run after a retraction starts from the entry's depth and pattern.
  - The schema offers the fields.
  - A quick or extract item is its own measured class, and thorough keeps today's key.
  - Through the real `run_swarm`, with a token budget:
    - only a quick run on record: thorough items fall back to the default reservation and are refused at 200k;
    - a thorough run on record: quick items are sized by the quick run and all run.

  17 of the 18 fail on `main`. The one that passes is the no-depth case.
- **Verification:**
  - The swarm, reaction, scheduler, board, fan-out, depth-channel and meta modules pass (200).
  - Full suite: the same 41 failing tests as `main` (2295d58f), by test id.
  - **Live (Bedrock):** a real swarm of two analysis items ran as separate process workers. The item given `profile: quick` and targets ran at `quick`, as its results record, and the other ran at the default. Both identified their phase correctly.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2295d58f851b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.