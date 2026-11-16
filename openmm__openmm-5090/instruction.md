# Issue

**Title:** Update OpenMM logo, stylesheet, and font in documentation

## Description

Switches out the OpenMM logo in the documentation for one with the "OpenMM" subtitle, updates some CSS for compatibility with changes to the Alabaster theme (see openmm/openmm-cookbook#47), and switches the font to match the main OpenMM website.

Should look the same as the cookbook, but since there's no automatic preview here, this is an example of before:

<img width="932" height="340" alt="Screenshot 2025-09-24 at 09 52 19" src="https://github.com/user-attachments/assets/d4267388-1114-4a59-9fd7-2750610a9411" />

and after:

<img width="932" height="340" alt="Screenshot 2025-09-24 at 09 52 29" src="https://github.com/user-attachments/assets/590a6c30-78bb-4fcc-aaf5-401c701da7c0" />

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `05472c9a8129`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.