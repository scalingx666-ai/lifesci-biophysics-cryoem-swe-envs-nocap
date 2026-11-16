# Issue

**Title:** feat: dataset to HTML method for jupyter

## Description

Allows showing dataset without first casting it to a dataframe
Demo:
<img width="1218" alt="Screenshot 2025-03-10 at 19 25 51" src="https://github.com/user-attachments/assets/7811d4ad-7c92-45c0-bb32-76c0ac378b6a" />

[Relevant IPython docs](https://ipython.readthedocs.io/en/stable/api/generated/IPython.display.html#:~:text=_repr_html_,-:%20return%20raw%20HTML).

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `f5d7bf4bf657`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.