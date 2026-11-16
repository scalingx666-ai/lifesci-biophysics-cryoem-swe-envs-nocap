# Issue

**Title:** Fixing the flakiness caused by testGetProteinSequenceForStructure

## Description

As part of our course project CS527 at UIUC, we chose to fix flaky tests in Biojava project. Flaky tests are those tests that pass and fail without any code changes [https://hackernoon.com/flaky-tests-a-war-that-never-ends-9aa32fdef359]. Flaky tests are can be quite costly since they often require engineers to re-trigger entire builds on CI and often waste a lot of time waiting for new builds to complete successfully.

We used the NonDex tool to find flaky tests [ https://github.com/TestingResearchIllinois/NonDex ] . This is a tool developed in our research lab and has been widely accepted.

Iterating through the hashmap for entries is non-deterministic in nature, so we changed the HashMap to a LinkedHashMap and re-ran the test. The Junits were executed again and they passed. We ran the NonDex tool again and we observed that  flakiness caused due to the test disappears.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `65ad15fe58dc`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.