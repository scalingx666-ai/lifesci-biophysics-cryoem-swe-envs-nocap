# Issue

**Title:** Update UI model options: gpt-5.6-sol and gemini-3.8-flash

## Description

## What this changes

Refreshes the model choices shown in the SciLink UI's model dropdown:

- The OpenAI option is now **gpt-5.6-sol** (was gpt-5.4).
- The Google option is now **gemini-3.8-flash** (was gemini-3.1-pro-preview).

Both the web (React) app and the Streamlit app pick up the change from a single list — no separate edit needed.

## On the Bedrock request

The ask included adding **gpt-5.6-sol on Amazon Bedrock if it exists there**. I could not confirm it does, so I did not add it:
- This machine has no AWS CLI, so I couldn't query the live Bedrock catalog.
- Bedrock's OpenAI offering today is the open-weight `gpt-oss` family, not the proprietary `gpt-5.x` chat models, so there's no known `bedrock/…gpt-5.6-sol` id to point at.

Adding a guessed model id would surface a broken option that fails at call time, so it's left out. If you have the exact Bedrock model id, it's a one-line add to the same list.

<details>
<summary>Technical details</summary>

- Single source of truth: `MODEL_OPTIONS` in `scilink/ui/config.py`. The Streamlit sidebar reads it directly; the React backend serves it via `/config` (`scilink/server/app.py:82`), and the frontend renders `config.models` (`webui/src/components/Sidebar.tsx:65`). One edit updates both surfaces.
- Provider inference verified: `auth.infer_provider("gpt-5.6-sol") == "openai"` and `auth.infer_provider("gemini-3.8-flash") == "google"`, so credential prefill and routing resolve correctly. `gpt-5.6-sol` also matches the wrappers' reasoning-class regex (`^gpt-5`), so it's handled like other gpt-5 reasoning models.
- No change to the existing `bedrock/us.anthropic.claude-opus-4-8` entry.

</details>

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `0d5dffa5b534`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.