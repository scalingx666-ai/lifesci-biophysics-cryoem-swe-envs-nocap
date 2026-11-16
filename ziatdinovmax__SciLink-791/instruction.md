# Issue

**Title:** sim: agent-chosen, library-grounded pseudopotentials for VASP

## Description

(phase 1: VASP). Quantum ESPRESSO and CP2K/NWChem follow as separate PRs, per the issue.

## Problem

Nobody chose the pseudopotential. The executors never assembled a POTCAR, the UI script `cat`ed the plain `<El>/POTCAR`, and `VASPConfig.potcar_map` was unused. So results used non-recommended variants (plain Na instead of `Na_pv`, and so on), ENCUT was chosen without knowing ENMAX, nothing recorded which POTCARs were used, and a missing variant surfaced as a confusing launch failure (see the no-plain-`Ca` example in the issue comments).

## Design: the LLM makes the choice, code does the rest

```
skill defaults (MP set) + local inventory (headers only)
        │
agent picks a variant per element (default unless it states a reason)
        │
resolve:  user override > agent choice (if installed) > MP default > plain-element fallback
validate: ENCUT >= 1.3 x max ENMAX (when ENMAX is known)
        │
input_files += POTCAR.spec + prepare_pseudopotentials.sh
run_command  = "bash prepare_pseudopotentials.sh && <run_command>"   (campaign + convergence ladders)
        │   on the execution host: read POTCAR.spec -> cat from $VASP_PP_PATH -> POTCAR
        │   missing variant -> exit 3, "MISSING PSEUDOPOTENTIAL <El>:<variant> in <lib>"
result["pseudopotentials"] = {choice + sources, warnings, enmax, used: {variant, ENMAX, sha256, TITEL}}
```

- **Engine-neutral contract.** An engine opts in with a `pseudopotentials:` frontmatter block and skill hooks (`default_pseudopotentials`, `list_pseudopotentials`, `render_pseudopotential_prep`, `poscar_species`, `pre_run_command`, `describe_pseudopotentials`, `check_encut_floor`). The resolver lives in `skills/_shared/pseudopotentials.py`, ready for QE (UPF/SSSP) next.
- **Grounded, not recalled.** The prompt lists the recommended variant and the variants actually installed (ZVAL, ENMAX). The defaults come from pymatgen's `MPRelaxSet.yaml`, with a vendored fallback of variant names only, which matches pymatgen exactly.
- **No licensed content in prompts.** The agent and critic see names and header metadata only.
- **Executor-agnostic.** Assembly runs where the library lives, so local, cluster and UI runs share one path. A critic fix that edits `POTCAR.spec` takes effect on the next run.
- **User override.** `run_complete_workflow(..., pseudopotential_overrides=...)` (e.g. `VASPConfig.potcar_map`) always wins.
- **UI.** The submit script uses the prep script when the spec is present. Legacy output is byte-identical otherwise.

### Also in this PR: POSCAR grouped by species

`_inject_native_structure` now writes the VASP POSCAR with `sort=True`. An interleaved structure, such as a rock-salt supercell or a packmol box, otherwise produced one species block per atom run. A 64-atom MgO:Ca cell gave a 63-entry species line and a 63-block POTCAR, which VASP treats as 63 types and which breaks per-species tags such as `LDAUL`/`MAGMOM`. Selective-dynamics flags follow their atoms; there is a test for that. This is a cherry-pick of a fix that never reached `main`, plus a test.

## Deliberate changes from the issue plan

- **No separate "setup failure" class.** Because `POTCAR.spec` is an editable input, the run critic can fix a missing variant by editing it. When the library is readable at generation time, the resolver never picks a missing variant in the first place.
- **ENCUT floor in the convergence sweep: deferred.** The ENCUT check runs at generation (`check_encut_floor`). Feeding ENMAX into the convergence ladder's floor is a small follow-up.

## Testing

- `tests/test_vasp_pseudopotentials.py`: 23 tests, all on synthetic POTCAR headers (no licensed files in the repo). They cover resolve precedence and fallbacks, both library layouts (`<lib>/<variant>` and `<lib>/potpaw_PBE/<variant>`), repeated species, the prep script executed for real (POSCAR order, the missing-variant exit 3, a missing `$VASP_PP_PATH`, an edited spec being honoured), agent injection and prompt block, `_with_pre_run`, a `LocalExecutor` run that assembles POTCAR without VASP, and the UI script.
- `tests/test_dft_native_structure.py`: species grouping plus constraint survival.
- Related suites (pipeline, refinement, convergence, VASP hooks, native structure) pass. The only failures in the wider sweep are pre-existing (script-style tests expecting a `model_name` fixture), identical on `main`.

**Live, with real VASP:**
- **NaCl:** `POTCAR.spec` = `Na_pv`, `Cl` (both `source: default`). The assembled POTCAR's TITEL lines match, provenance (variant, ENMAX, sha256, TITEL) is recorded under `result["pseudopotentials"]["used"]`, the command run was `bash prepare_pseudopotentials.sh && mpirun vasp_std`, and VASP finished.
- **MgO with a Ca substituent** (a library with `Ca_pv`/`Ca_sv` but no plain `Ca`): `Ca_sv` was selected and assembled, and VASP started and iterated. No missing-POTCAR failure. That run then hit the test allocation's wall-clock caps; it used the pre-sort POSCAR (63 blocks) on a small allocation, which is unrelated to this change.

🤖 Generated with [Claude Code](https://claude.com/claude-code)

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d18c9a663edb`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.