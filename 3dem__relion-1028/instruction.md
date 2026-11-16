# Issue

**Title:** ManualPicker: automatically save when closing the window

## Description

ManualPicker says:

> NOTE: in order to write the new list of coordinate STAR files, you need to re-count the particles or quite this program through the File menu. Do NOT kill the program using the operating system's window manager!

but this is not user-friendly. Most people don't notice this message, and even if they do see this, they just forget to use the menu.

With this patch, ManualPicker automatically saves the list even when closed by the window manager.

This also fixes a problem in the `[File]-[Quit]` menu. It used to save the list *without* recounting. Now it recounts and then saves the list.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `93049a20eb3f`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.