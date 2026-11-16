# Issue

**Title:** Multi-user hardening: token auth, per-user session roots, remote-safe UI

## Description

## Summary

You can now run `scilink-web` on a lab server and share it with a group. Each person signs in with their own access token, gets their own private session space, and cannot see or touch anyone else's sessions.

- **Nothing changes for the usual single-user case.** With no flags, `scilink-web` behaves exactly as before: no sign-in, sessions in the directory you launched from, folder paths on your machine still offered. The existing 42 backend tests pass unchanged, and a no-flag run was checked in the browser: straight to the welcome screen, no sign-in, the upload menu still has all three items.
- **Sharing.** Start with `--users users.json` (a `{"name": "token"}` map) for per-user tokens, or `--token` / `SCILINK_WEB_TOKEN` for one shared token. Binding to a non-loopback address without either is refused with instructions. Hand each person a sign-in link of the form `https://host/?token=…`, or they paste the token on the sign-in screen. The sidebar shows who is signed in and offers sign-out.
- **Remote-safe.** On a shared server the "Use a folder on this machine" option disappears from the upload menu, and the endpoints behind it refuse. "Quit App" is disabled on a multi-user server so one person cannot shut it down for everyone.
- **TLS** is a reverse proxy's job; the docs give a Caddy one-liner and an nginx block.

Verified live with a two-user server bound to all interfaces: sign-in gate, isolation in both directions, cookie flow for the event stream, per-user directories on disk, and the browser sign-in link, hidden folder item, and sign-out.

## Technical description

### Backend

- `scilink/server/auth.py` (new). `AuthConfig.single(token)` and `AuthConfig.from_users_file(path)` (validated names, tokens ≥ 16 chars, unique). Tokens are compared with `hmac.compare_digest` across all users without early exit. Two credential carriers: `Authorization: Bearer <token>` (scripts, curl) and the `scilink_web` cookie minted by `POST /api/v1/auth/login` (HttpOnly, SameSite=Lax, Secure when the request came over HTTPS per `X-Forwarded-Proto`). Cookie sessions are in memory, so a server restart logs everyone out. `AuthMiddleware` is pure ASGI: guards `/api/v1/*` except `/api/v1/auth/*`, sets `scope.state.user` (`default` when auth is off), answers 401 JSON with `WWW-Authenticate: Bearer`. `user_root(session_root, auth, user)` returns the shared root unless per-user roots are on, then `<root>/users/<name>/`, created on first use and fenced against escaping names.
- `scilink/server/app.py`: `create_app(session_root, serve_frontend, auth=None, local_files=True)`. One `SessionManager` per user root, created lazily via `_mgr(request)`; every endpoint takes the request and resolves its manager and session through the requesting user, so another user's session id is simply a 404. For auth-off and single-token deployments the default manager is created eagerly and exposed as `app.state.manager`, the historical attribute tests rely on; multi-user servers expose `app.state.manager_for_user(name)`. New endpoints `GET /auth/me`, `POST /auth/login`, `POST /auth/logout`. `/folders` and `/plan_dirs` return 403 when `local_files` is false. `/quit` returns 403 on a multi-user server. `/config` gains `auth {required, user, multi_user}` and `local_files`. CORS now allows credentials for the Vite dev origin.
- `scilink/server/cli.py`: `--token`, `--users FILE`, `--insecure-no-auth`; a non-loopback `--host` without auth exits 2 with the reason and a token-generation one-liner; `local_files` is set from whether the bind is loopback; startup prints the auth mode and the sign-in link form.
- `scilink/server/schemas.py`: `LoginRequest`.

### Frontend

- `api.ts`: `UnauthorizedError` on 401, `authMe` / `login` / `logout`, `AuthInfo`, `auth` and `local_files` on the config type.
- `App.tsx`: asks `/auth/me` first (public). If sign-in is required and the URL carries `?token=`, exchanges it for the cookie and scrubs the parameter from the URL before anything else loads; otherwise shows `LoginScreen`. Config and sessions load only once signed in. A 401 on config drops back to sign-in. Sign-out clears state and returns to the screen.
- `LoginScreen.tsx` (new): token field and button; the token is only ever sent to `/auth/login`, never stored by the page.
- `Sidebar.tsx`: "Signed in as … · Sign out" when the server requires auth.
- `PreChatHero.tsx`: `localFiles` prop (default true); when false the plan and meta heroes pass no `onLocalPath`, so the menu has no local-folder item and the path inputs never appear.

### Tests

`tests/test_web_auth.py` (new, 5): config validation; no-auth posture unchanged (open API, `default` user, root is the session root); single token via bearer and via cookie including logout and no `users/` subtree; multi-user isolation (per-user roots created on first touch, live sessions and resumable discovery invisible across users, 404 on another user's session for GET/tree/DELETE, quit 403, config names the user); `local_files=False` gating of `/folders` and `/plan_dirs` with uploads still allowed. Existing `tests/test_web_server.py`: 42 pass unchanged. `tsc -b` clean.

### Docs

`docs/react_web_ui.md`: new "Sharing on a lab server" section (flags, users file, sign-in link, reverse proxy examples for Caddy and nginx, what is disabled remotely, what is not yet covered). Roadmap: multi-user hardening marked done.

### Not covered, on purpose

- Per-user LLM credentials are entered per session in the sidebar as before; the server does not store them.
- No rate limiting on `/auth/login`; tokens are long random strings. Put the proxy's rate limit in front if the server is internet-facing.
- Cookie sessions do not survive a server restart.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7e32352be3b1`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.