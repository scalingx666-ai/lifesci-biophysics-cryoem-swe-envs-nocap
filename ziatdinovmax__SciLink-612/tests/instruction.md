# Issue

**Title:** Native TLS for scilink serve (SSE) and scilink-web

## Description

.

## Summary

`scilink serve --transport sse` and `scilink-web` only spoke plain HTTP, so an off-box bind was unencrypted unless a reverse proxy terminated TLS. Both options from the issue are now in place: the reverse-proxy pattern is the documented, recommended deployment, and when no proxy is available both commands accept a certificate and key and serve HTTPS themselves. Client configs printed by `--print-mcp-json` show the right scheme.

## Technical description

- `scilink/server/tls.py`: `add_tls_arguments` (the same `--ssl-certfile` / `--ssl-keyfile` / `--ssl-keyfile-password` group on both parsers) and `tls_kwargs` (both files or neither, files must exist; returns uvicorn keyword arguments).
- `mcp_server.run_sse` takes `ssl_certfile` / `ssl_keyfile` / `ssl_keyfile_password` and passes them to `uvicorn.run`; `scilink serve` parses the flags for the SSE transport (a note is printed if given with stdio), reflects the scheme in the ready banner and in `--print-mcp-json` (URL and the start command hint).
- `scilink-web`: same flags, same threading; the printed URL uses the scheme. The sign-in cookie already reads `request.url.scheme` (or `X-Forwarded-Proto`), so it is `Secure` on native TLS with no change.
- Docs: `cli_reference.md` (flags and the proxy note), `connecting_agent_clients.md` (off-box SSE), `react_web_ui.md` (an HTTPS section with the Caddy/nginx pattern and the native flags).
- Tests (`tests/test_tls_flags.py`, 4): flag validation, `run_sse` threading and its pair check, `--print-mcp-json` scheme, and the web CLI threading and URL.

### Live check (self-signed certificate)

- `scilink serve --transport sse --ssl-certfile … --ssl-keyfile …`: an MCP client trusting the CA initialized over `https://127.0.0.1:8600/sse` and listed 40 tools; a client not trusting it failed with certificate verification; plain HTTP on the port is refused.
- `scilink-web --ssl-certfile … --ssl-keyfile … --token …`: the SPA and API served over HTTPS, the login response set the session cookie with `Secure`, an untrusting client failed verification (curl exit 60), plain HTTP refused.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `64c4815d1373`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.