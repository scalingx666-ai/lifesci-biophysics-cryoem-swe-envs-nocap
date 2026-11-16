# Issue

**Title:** Add option to control whether water is alchemically changed into counterion for charge changes

## Description

## Description

<!-- Describe your changes in detail. -->
This PR allows the user to disable the introduction of counterion(s) for small molecule and protein mutation transformations.

## Motivation and context

<!--- Why is this change required? What problem does it solve? -->

<!-- Replace ??? with the issue number that this pull request resolves. -->
 

## How has this been tested?

<!--- Please describe in detail how you tested your changes. -->
@ijpulidos : Could you make sure that specifying `transform_waters_into_ions_for_charge_changes : False` in the yaml actually controls the introduction of the counterion? If the changes I made are right, you should see a logger message that says `Skipping counterion`.

Note that I added a test (for protein mutations, but not small molecules) that checks that this option is working properly. You could adapt this test for a small molecule example, or it might be sufficient to just double check that you see the logger statement when you include the `transform_waters_into_ions_for_charge_changes ` option in the yaml.

## Change log

<!-- Propose a change log entry. -->
<!-- Examples here https://github.com/choderalab/perses/blob/master/docs/changelog.rst -->
```
Add option for disabling the introduction of a counterion for charge changing transformations
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `a2cdad2b81fe`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.