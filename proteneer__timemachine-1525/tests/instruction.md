# Issue

**Title:** Speed-up setup_intermediate_state while maintaining trace-ability.

## Description

This PR speeds-up `setup_intermediate_state` by about 30-50x while still maintaining our calling to call `jax.jit`, `jax.vmap`, and `jax.grad` on functions that invoke `setup_intermediate_state()`

[2x speed-up] - pre-align idxs and parameters in the constructor.
[2x speed-up] - pre-compute min/max bounds used by interpolation code in the constructor.
[2x speed-up] - vectorize `interpolate_*_params` with `jax.vmap` and `jax.jit`
[1.5x speed-up] - caching `get_dummy_atoms_a, get_dummy_atoms_b, get_core_atoms`.
[1.5x speed-up] - caching `src_chiral_idxs` and `dst_chiral_idxs`.

timings for 128 calls to `setup_intermediate_state()` on the hif2a ligand pair:

master:

jit time: 0.63s
run time: 20.91s

this pr:

jit time: 0.41s
run time: 0.56s


``` python
import time

def test_setup_intermediate_state():
    ff = Forcefield.load_default()
    mol_a, mol_b, core = get_hif2a_ligand_pair_single_topology()
    st = single_topology.SingleTopology(mol_a, mol_b, core, ff)

    import cProfile
    import pstats

    # call once to warm-up jit
    start_time = time.time()
    st.setup_intermediate_state(0.5)
    print("jit time", time.time() - start_time)
    profiler = cProfile.Profile()
    profiler.enable()
    start_time = time.time()
    for lam in np.linspace(0, 1, 128):
        st.setup_intermediate_state(lam)
    print("run time", time.time() - start_time)
    profiler.disable()
    pstats.Stats(profiler).sort_stats(pstats.SortKey.CUMULATIVE).print_stats(100)


if __name__ == "__main__":
    test_setup_intermediate_state()
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `62b0990252e0`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.