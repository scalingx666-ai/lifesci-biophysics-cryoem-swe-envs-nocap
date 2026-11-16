# Issue

**Title:** Fix RSPEC-1940 boolean checks

## Description

Objet

Corriger les violations de la règle Sonar RSPEC‑1940 (“Boolean checks should not be inverted”) afin de rendre la lecture du code plus naturelle et consensuelle.

Modifications clés

1. SecStrucTools, BioAssemblyTools, Element, Author, Location, Equals, InsdcParser : remplacement des négations imbriquées (!(...)) par des tests affirmatifs explicites, ce qui clarifie l’intention de chaque condition.
2. CeCalculatorEnhanced et CECalculator : réécriture des comparaisons sur nAtom avec une inégalité directe (>=) pour éliminer les doubles négations et faciliter la compréhension du seuil appliqué.
3. Aucune logique métier modifiée ; il s’agit exclusivement d’améliorations de style pour se conformer à RSPEC‑1940 et harmoniser la lisibilité du code.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `94c6ebcab5c2`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.