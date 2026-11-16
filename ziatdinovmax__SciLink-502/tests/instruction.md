# Issue

**Title:** Add CP2K as a periodic_dft engine skill

## Description

Adds **CP2K** (Quickstep) as a third DFT engine for `PeriodicDFTAgent`, alongside VASP and Quantum ESPRESSO. Knowledge-only bundle (`cp2k.md` + `__init__.py`), like the QE skill; `PeriodicDFTAgent.supported_software()` auto-returns `['cp2k', 'qe', 'vasp']` with no agent change.

### What it covers
CP2K's nested `&SECTION`/`&END` GPW input: `&GLOBAL` run type; `&FORCE_EVAL/&DFT` (`&QS`, `&MGRID` cutoff, `&SCF`, `&XC` with GGA / PBE0 / HSE06 hybrids via `&HF` + dispersion, `&KPOINTS`, DFT+U, Fermi `&SMEAR`); and `&SUBSYS` (`&CELL`, `&COORD`, per-element `&KIND` with a GTH basis + pseudopotential), with worked examples for a semiconductor single point, a ferromagnetic metal, an antiferromagnet with Hubbard U, a geometry relaxation, and an HSE06 hybrid. `detect:` on the `cp2k[.psmp/.popt/...]` binaries.

### Validated by execution, not just by syntax
CP2K is notoriously unforgiving, so this skill was hardened by **actually running the decks it generates**, not only checking that they read correctly. Generating inputs across the DFT space — a semiconductor single point, a metal relaxation, a ferromagnetic metal, an antiferromagnetic correlated oxide with DFT+U, and an HSE06 hybrid — and running each in CP2K surfaced a class of decks that are physically correct on paper yet **abort at runtime** on CP2K's coupled setup rules. Those categorical rules are now encoded in the skill, and the generated decks run to a converged SCF across semiconductor / metal / magnetic-metal / DFT+U. The hybrid deck is set up correctly and runs (OT + Gamma, no abort); whether its screened-exchange range fits a given cell is a numerical adequacy question left to the loop (see *scope* below):

- **`&OT` and a k-point mesh are mutually exclusive** — a k-sampled run uses `&DIAGONALIZATION`, a Gamma-point run uses `&OT`.
- **DFT+U and hybrid functionals do not support k-points** — they must run **Gamma-point** (`Method not implemented for k-points` otherwise).
- **Hybrids require `&OT`** (a `&DIAGONALIZATION` hybrid, or ADMM MO-purification without OT, aborts).
- **The GTH-PBE `q<N>` valence must exist for the element** (a guessed `q` aborts) — with a standard-valence reference table.
- **Physics note (loop, not skill):** a periodic hybrid's exact-exchange range must fit within half the cell; for screened HSE the remedy is a large-enough supercell, whose sizing is numerical and left to the refinement step.

The categorical rules are front-loaded in the `planning` section (the loader only injects canonical sections) so the model reliably applies them.

### Scope: skill vs. the refinement loop
These are **categorical engine "no-nos"** — always-true rules a competent CP2K user knows *before* running, whose violation is a hard abort. They belong in the skill knowledge. The complementary problems are **numerical and system-dependent** — SCF convergence for a magnetic+U system, whether a hybrid's cutoff/cell is physically adequate — and need runtime feedback; those are the **refinement loop's** job (and the reference-property critic's), not the skill's. CP2K is a sharp illustration of that boundary because its categorical couplings are unusually dense: the skill encodes what's categorical, and the loop handles what's numerical.

### Examples decoupled from any evaluation systems
To avoid any appearance of tuning the skill to what it's checked against, every worked example uses a system **distinct from common test cases** (Ge, Co, MnO; the hybrid/relaxation examples are element-agnostic). Standard elements appear only in the general q-value reference table, which the physics scoring does not depend on.

### Tests
`tests/test_cp2k_skill.py`: CP2K is discovered, is a supported engine, its frontmatter parses and names the binaries, and it loads into canonical sections.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `eb866419f1f0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.