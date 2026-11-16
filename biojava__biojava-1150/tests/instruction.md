# Issue

**Title:** Keep the ECOD read lock balanced when loading fails

## Description

`getDomainsForPdb` and `getAllDomains` release the read lock **inside** their `try` block — so the loader they call can take the write lock — and re-acquire it immediately afterwards:

```java
domainsFileLock.readLock().lock();
try {
    while (domainMap == null) {
        domainsFileLock.readLock().unlock();
        indexDomains();                       // if this throws...
        domainsFileLock.readLock().lock();    // ...this never runs
    }
    ...
} finally {
    domainsFileLock.readLock().unlock();      // ...and this unlocks a lock we do not hold
}
```

When the loader throws, the re-acquisition is skipped, and the outer `finally` unlocks a lock the thread no longer holds. `IllegalMonitorStateException` is then thrown *from a finally block*, so it **supersedes the `IOException` that actually caused the failure**.

## Why it matters, concretely

Last night's nightly (`33303708377`) is a clean demonstration. One upstream change — ECOD now answers `http://prodata.swmed.edu/ecod/distributions/…` with a 308 that `HttpURLConnection` will not follow,  — produced eight failures wearing three different faces:

```
EcodInstallationTest.testDownloads:93   » HttpStatus HTTP 308 Permanent Redirect for http://…
EcodInstallationTest.testByPDB:148      » IllegalMonitorState attempt to unlock read lock, not locked by current thread
EcodInstallationTest.testAllDomains:104 NullPointer Cannot invoke "…getAllDomains()" because "ecod" is null
```

Only the first names the cause. The second is this bug. Diagnosing it meant reading past the noise to find the three tests that had told the truth.

## The fix

Re-acquire in a `finally` of its own, so the lock is balanced on both paths and the original exception propagates intact:

```java
domainsFileLock.readLock().unlock();
try {
    indexDomains();
} finally {
    domainsFileLock.readLock().lock();
}
```

Six lines in each of the two methods, no behaviour change on the success path. The other four read-lock sites in the class are already balanced — they lock, return, and unlock in a `finally` without releasing in between — so they are untouched.

## Scope

This does not fix the ECOD download itself; that is #1149, and it needs `FileDownloadUtils` to follow the redirects the JDK declines. This only stops a failing load from disguising its own cause.

The third face above — `EcodFactory` handing back `null` after swallowing the error — is the same family of masking and is also not addressed here.

## Testing

`mvn test -pl biojava-structure -Dtest=EcodParserTest` — green, and the class compiles unchanged otherwise. The failure path is not covered by an automated test: reproducing it needs a download that throws, which today means either the network being down or ECOD still being broken. If you would like it pinned, the natural way is a small unit test that injects a failing loader, which would mean making the loader overridable — happy to add that if you think it is worth the seam.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `2a57e199c763`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.