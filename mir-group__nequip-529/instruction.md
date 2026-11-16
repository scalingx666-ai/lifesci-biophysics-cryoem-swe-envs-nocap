# Issue

**Title:** Schedule-Free Optimizer Lightning Module

## Description

## Description
This PR introduces a new ScheduleFreeLightningModule that integrates Facebook's Schedule-Free optimizer (https://github.com/facebookresearch/schedule_free) into the NequIP training framework. It allows users to use the AdamWScheduleFree optimizer through NequIP’s config system and Lightning integration.

## Motivation and Context
This change adds support for a promising optimization strategy that removes the need for manually scheduled learning rates and enables smoother convergence. It helps researchers explore schedule-free training techniques within the NequIP framework.

## How Has This Been Tested?

Manually trained a NequIP model using ScheduleFreeLightningModule with various optimizer settings.

Confirmed compatibility with NequIP’s config system and CLI via nequip-train.

## Types of changes
<!-- What types of changes does your code introduce? Put an `x` in all the boxes that apply: -->
- [ ] Bug fix (non-breaking change which fixes an issue)
- [x] New feature (non-breaking change which adds or improves functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to change)
- [ ] Documentation improvement (updates to user guides, docstrings, or developer docs)

## Checklist:
<!-- Put an `x` in all the boxes that apply. If you're unsure about any of
     these, don't hesitate to ask. We're here to help! -->
- [x] I have read ["Contributing to NequIP"](../docs/dev/contributing.md).
- [x] My code follows the code style of this project and has been formatted using `black`.
- [ ] All new and existing tests passed, including on GPU (if relevant).
- [ ] I have added tests that cover my changes (if relevant).
- [x] I have updated `CHANGELOG.md`.
- [x] I have updated the documentation (if relevant).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `72c59f9c52b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.