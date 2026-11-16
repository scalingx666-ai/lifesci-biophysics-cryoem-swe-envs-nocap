# Issue

**Title:** Web UI: clear provider fields when switching to a Custom model with no name

## Description

## Summary

Switching the main model from Bedrock to Custom left the Bedrock provider's fields visible on the Custom selection — the `Bedrock API key` label and the `AWS region` select — which shouldn't be there for a Custom model.

## Cause

The provider-config effect early-returned when the effective model was empty (Custom before a name is typed), so `providerInfo` / `providerFields` kept their previous (Bedrock) values.

## Fix

`Sidebar.tsx`: on an empty effective model the effect now clears `providerInfo`, `credInfo`, and `providerFields` instead of returning. Custom-with-no-name shows a generic `API key` label and no provider-specific fields; typing a name refetches, and switching back to a vendor preset repopulates its fields.

### Verified in Chrome

Bedrock → `Bedrock API key` + AWS region. Switch to Custom → `API key`, no region, Base URL shown. Type a custom name → still generic. Switch back to Bedrock → the Bedrock label and region return.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `51f5af253b27`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.