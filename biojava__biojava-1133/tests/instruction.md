# Issue

**Title:** Fix FileDownloadUtils validation and implement checksum verification

## Description

The download-validation helpers added in 7.0.0 (#979, #980) have several gaps that only surface once a caller passes a real hash URL, or downloads from a server that can return 404. This fixes all of them and implements hash verification rather than leaving it stubbed.

### Correctness fixes

- `createValidationFiles(URL, ...)` passed a literal `Hash.UNKNOWN` to its `URLConnection` overload instead of the caller's argument, so it could never write a hash file and threw `IllegalArgumentException` for any caller that supplied a
 `hashURL`.
- Neither `downloadFile` nor `createValidationFiles` checked the HTTP status, so a 404 error page was written into the cache as though it were the requested file. Because the `.size` sidecar was then taken from that same error response, `validateFile` subsequently declared it **valid**. A new `HttpStatusException` lets callers tell "the resource is not there" apart from a transport failure, which matters for anything that tries several mirrors in turn.
- `downloadFile` used `FileChannel.transferFrom(rbc, 0, Long.MAX_VALUE)`, which is not guaranteed to drain a socket-backed channel and could silently truncate a download. Replaced with `Files.copy`, which loops to end of stream.
- `downloadFile` leaked its temporary file on every failure path.
- `validateFile` threw `NullPointerException` for a file with no parent directory, and again if `listFiles()` returned null; an empty `.size` file raised an unchecked `NoSuchElementException` that escaped the surrounding catch.
- `validateFile` checked only the first hash sidecar it found, ignoring the rest.

### New functionality

- `validateFile` now really verifies MD5, SHA-1 and SHA-256. Sidecars are written as bare lowercase hex and parsed tolerantly, so a file downloaded verbatim from a server in coreutils or BSD layout is also understood. A sidecar that cannot be parsed is skipped with a warning rather than failing an otherwise good download.
- `ETagPolicy` lets an `ETag` that is a bare hex digest be recorded as a checksum without a second request. **`files.wwpdb.org` and `files.rcsb.org` return the content MD5 as the `ETag`** — verified byte-exact — so every download from the wwPDB archive now gets a real integrity check for free. The `<mtime>-<size>` ETags used by the EBI servers contain a dash and can never be misread as a digest; a test pins that.
- `downloadFileWithValidation` downloads and validates over a single connection. The previous pattern opened one connection to read `Content-Length` and another to fetch the bytes, so the recorded size described a different response than the one written; if the resource changed in between, the cache entry was left permanently failing validation.
`LocalPDBDirectory` uses the new single-connection download, and its two-character directory hash is promoted to a reusable `getMiddleHash(String)`.

### Behaviour change

The existing four-argument `createValidationFiles` overloads now default to `ETagPolicy.USE_IF_HEX_DIGEST`, so callers start recording checksums where the server offers one.
Actually the work is targeted at 7.3.0 rather than a patch release (check next PR). Therefore This small behavioural change should be acceptable.

### Consumers fixed here

 — `CathInstallation` downloaded over `http`, which now 301s to `https`; `HttpURLConnection` will not follow a redirect that changes protocol, so the body of the 301 was cached as classification data. Commit `ed36d3a` switches the URL and routes the download through the status-checking path added here. Commits `5ec48ac` and `5d07241` close the same hole in `DownloadChemCompProvider`, which is not broken today only because the RCSB ligand endpoint still serves plain http.

### Testing

37 tests in `FileDownloadUtilsTest`, including the live wwPDB ETag-to-MD5 round trip and a 404 that must leave neither a cached file nor a `.size` sidecar behind. Full `biojava-core` suite: 522 tests, no failures.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `5b29945954ea`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.