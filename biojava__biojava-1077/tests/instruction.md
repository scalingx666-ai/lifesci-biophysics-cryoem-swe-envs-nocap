# Issue

**Title:** Changed the SCOP Installation URLs

## Description

I was going through the tutorial and was displaying a jmol panel.  When I selected Show SCOP Domains for the color on the selection, I received a `FileNotFoundException`.   A review of the Berkeley site shows that they changed from http to https for the scheme.   They also appear to have changed the path portion of the URL in version 2 from 'scop' to 'scope.'

All of the unit tests worked before and after this change.  I tried to create a unit test to show the failure, fix it, and run the unit test to show that it was fixed, but I couldn't quite follow the path to get to the ScopInstallation code from the Swing application.

To recreate my original error, the following code can be used (derived from the tutorial).

```
         Structure structure = StructureIO.getStructure("4FRW");
         System.out.println(StructureTools.getNrAtoms(structure));
         StructureAlignmentJmol jmolPanel = new StructureAlignmentJmol();

         jmolPanel.setStructure(structure);

         // send some commands to Jmol
         jmolPanel.evalString("select * ; color chain;");
         jmolPanel.evalString("select *; spacefill off; wireframe off; cartoon on;  ");
         jmolPanel.evalString("select ligands; cartoon off; wireframe 0.3; spacefill 0.5; color cpk;");
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a3d943783b1d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.