# Issue

**Title:** Prevent XXE (Xml eXternal Entity) injection when parsing XML data

## Description

Biojava is vulnerable to XXE injection due to accepting XXEs from an untrusted source. This change configures the DocumentBuilder to use secure processing to prevent this. See https://docs.oracle.com/en/java/javase/13/security/java-api-xml-processing-jaxp-security-guide.html

Another user created a PR for this last year, but it's targeting their own fork, so I'm sure it was never seen: 

[JFrog's Xray scan](https://jfrog.com/xray/) flags this as a medium severity vulnerability (but there's no CVE registered for it), and cites srikanthprathi's PR (above) as the only reference.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `53f307ee230c`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.