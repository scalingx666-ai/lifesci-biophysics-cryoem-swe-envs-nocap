# Issue

**Title:** Follow the redirects HttpURLConnection declines (307/308, cross-protocol)

## Description

.

`HttpURLConnection` follows 301, 302 and 303 within a protocol. It never follows 307 or 308, and never follows a redirect that changes http to https. Both gaps have broken this build:

| When | Service | Redirect |
|---|---|---|
| 2025-12 | CATH | 301 http→https — cached the redirect body as classification data (#1138) |
| 2026-08-29 | ECOD | 308 to a rewritten path — nightly red on all `EcodInstallationTest` |

A browser follows either without comment, so neither service had reason to expect the change would break us.

## What this adds

`openConnectionFollowingRedirects(URL, timeout)` handles what the JDK leaves:

- **307 and 308**, and **cross-protocol 301/302/303** — the only ones that reach us, since the JDK already consumed the rest.
- **Relative `Location`** resolved against the request. ECOD's is relative.
- **Five-hop cap**, and a loop reported by name rather than chased.
- **https→http is refused.** A redirect must not downgrade the transport silently. Anything refused is returned as-is, so `checkHttpStatus` still decides what a non-2xx means.

Used by `downloadFile`, `downloadFileWithValidation` and the `URL` overload of `createValidationFiles`.

The decision is split into `redirectTargetFor(code, location, url)` so the rules are testable without a server.

## Note on ECOD specifically

This deliberately does **not** change `DOMAINS_PATH`. ECOD's own site and API still publish `/ecod/distributions/…`; the 308 to `/ecod-legacy/` is an internal rewrite they introduced when their new app took over `/ecod/`. BioJava's URL is already the advertised one — the only defect was not following the redirect. Hardcoding `/ecod-legacy/` would pin us to something unadvertised.

## Testing

`mvn test -pl biojava-core -Dtest=FileDownloadRedirectTest` — 13 tests, no network:

- 7 rule tests, pure, covering each status, relative and absolute locations, the https→http refusal, and unusable locations.
- 6 end-to-end through a local `HttpServer`: a 308 and a 307 followed, the validated download storing the real body with a matching `.size`, a loop, an over-long chain, and a redirect with no `Location`.

Full `biojava-core` suite: 535 tests, no failures.

## Scope

Other call sites have the same exposure — `AllChemCompProvider`, `SiftsChainToUniprotMapping`, `Astral`, `PDBStatus`, `URLConnectionTools`, and `UniprotProxySequenceReader`, whose hand-rolled version carries the comment *"This method should be moved to a utility class in BioJava 5.0"*. Migrating them is a follow-up rather than part of this, since each needs its own judgement; inventory filed separately.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a57e199c763`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.