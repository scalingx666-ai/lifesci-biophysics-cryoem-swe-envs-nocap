# Issue

**Title:** fix: avoid crash in VMD on MacOS

## Description

due to Tk bug triggering OSX crash when using file extensions with more than one dot, or not beginning with a dot.

https://core.tcl-lang.org/tk/tktview/080a28104e25e054f8f5869d9333aa57c221b896

Bug is fixed upstream, but I am adding a platform-specific hack to work around it.

Might be fixed in Tk 9.0?

Marked draft until tested on the relevant platform. [Update: tested successfully]

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `7de54568cdc0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.