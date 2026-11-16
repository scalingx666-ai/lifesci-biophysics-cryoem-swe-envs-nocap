# Issue

**Title:** scilink serve: call each model with its own provider's key

## Description

## Summary

`scilink serve` (the MCP server that Claude Desktop, VS Code and agent platforms connect to) now calls each model with the key of that model's own provider.

Before, it took the first key it found in the environment, in a fixed order (proxy, Gemini, OpenAI, Anthropic), whatever model you asked for. If you had more than one key set, the server could send one provider's key to another and every model call failed. It also had no notion of Amazon Bedrock, and it read `~/.scilink/credentials.env` only after it had already picked a key.

What you will notice:

- With several providers' keys set, `scilink serve --model <any of them>` just works.
- Bedrock models work with `AWS_BEARER_TOKEN_BEDROCK` or your normal AWS credentials, even when other keys are set.
- Keys kept in `~/.scilink/credentials.env` are found.
- If the key for the chosen model's provider is missing, the server says so at startup instead of failing on the first call.
- **One behaviour change to know about:** with `--base-url`, the server no longer hands over whichever provider key happens to be set. It uses `SCILINK_API_KEY` or `--api-key`. A local OpenAI-compatible server (Ollama, vLLM) that accepted any key now needs one passed explicitly, e.g. `--api-key unused`. The terminal shell already behaved this way.

## Technical description

### The defect

`scilink/cli/serve.py` resolved the key with

```python
for env_var in ["SCILINK_API_KEY", "GEMINI_API_KEY", "OPENAI_API_KEY", "ANTHROPIC_API_KEY"]:
```

independent of `--model` and `--base-url`. Consequences: a proxy key sent to a vendor endpoint (contradicting the "API key handling" rule in `CLAUDE.md`), one vendor's key sent to another, a non-`None` key passed for Bedrock, and the scan running before `_load_shared_credentials()` (which was called inside `create_server`).

### The change

- **`scilink/auth.py`: `resolve_llm_key(model_name, base_url=None, api_key=None) -> (key, source)`.** The one rule, read from the environment only: explicit key wins; with a base URL the proxy key (`SCILINK_API_KEY`), never a vendor's; no key for a Bedrock model (`source` says whether AWS credentials were seen); otherwise the env var of the model's own vendor via the existing `find_env_var_for_model`. `source` is `None` when nothing was found.
- **`scilink/cli/shell/bootstrap.py`: `resolve_llm_credentials`** now calls it and only asks when it returns no source. Branch-for-branch the same behaviour as before (flag, proxy, Bedrock, vendor); the one difference is that the Bedrock prefix match is case-insensitive.
- **`scilink/cli/serve.py`:** `main()` redirects stdout, imports the server module, calls `_load_shared_credentials()`, then `resolve_api_key(model, base_url, api_key)`:
  - a source was found → that key;
  - `--base-url` with no proxy key and no `--api-key` → error on stderr, exit 1 (the orchestrators would raise `ValueError` on this anyway);
  - a known vendor (anthropic / openai / google) with no env var → a warning on stderr, `None`;
  - Bedrock, or a model `infer_provider` does not recognise (`ollama/...`, `azure/...`, a local name) → `None`, silently: the AWS chain has sources the server cannot see, and another route's key is the client library's to find.
- `--api-key` / `--base-url` help text and `docs/cli_reference.md`, `docs/vscode_copilot_integration.md` updated (the latter offered `SCILINK_API_KEY` as a vendor key).

`create_server` is unchanged for direct callers; its own `_load_shared_credentials()` call stays and is idempotent.

### Verification

- `tests/test_serve_key_resolution.py` (new): the rule per vendor with every key set; proxy key only beside a base URL; a vendor key never to a base URL; Bedrock; the shell asks only when the rule finds nothing; `serve.main()` end to end with the server faked (the model's key, Bedrock, the credentials file read before resolution, exit on a keyless proxy, a local endpoint needing `--api-key`); no warning where the key is not knowable. The rule and shell tests run without the `mcp` package.
- **Live, Bedrock (`bedrock/us.anthropic.claude-opus-4-8`), through a real MCP stdio client**, with decoy `SCILINK_API_KEY` / `GEMINI_API_KEY` / `OPENAI_API_KEY` set beside the real Bedrock credential, one `scilink_orchestrate_analysis` call:
  - `origin/main`: `status: error`, `BedrockException - Invalid API Key format` (the decoy proxy key was sent to Bedrock);
  - this branch: `status: success`, the expected reply.
- Full offline suite compared against `origin/main` by failing-test ids: 27 failed / 28 errors on both (the same ids; none branch-only), 5027 passed on `main`, 5050 here.
- An independent reviewer agent read the diff before this PR; its findings (false warnings for Bedrock's default credential chain and unrecognised providers, the `--base-url` behaviour change needing to be stated, a stale doc line, tests skipping wholesale without `mcp`) are addressed in the second commit.

### Known and not changed here

- `infer_provider` classifies by substring, so `openrouter/google/gemini-x` or `vertex_ai/gemini-...` resolve to the Google key. The shell already behaved this way; `serve` now matches it. Tightening `infer_provider` affects the shell and the web UI and is a separate change.
- The planning orchestrator reuses the chat key for embeddings when no embedding key is given (pre-existing).
- `SCILINK_MODEL` in `credentials.env` is still read before the file is loaded (the `--model` default is evaluated at parse time).

### After the review (round 1)

- A test no longer leaves a fake `ANTHROPIC_API_KEY` in the pytest session, and `sys.stdout` is restored correctly under `pytest -s`.
- `auth.infer_provider` returns `None` for a name with another route's prefix (`vertex_ai/`, `openrouter/`, `azure/`, `ollama/`), so those models are no longer handed a vendor key or warned about one. This supersedes the "known and not changed" note on substring classification above.
- The docs name the three entry points that share the rule; `prepare-ff` and the web backend are #828.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a4160c400f2b`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.