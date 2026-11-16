# Issue

**Title:** Fix error that occurs when DSL selection for ligand is "(mass > 1.5)"

## Description

If `restrained_ligand_atoms` is set to `(mass > 1.5)`, the following error occurs for me (Python 3.6, yank 0.22.1):
```
NoneType: None
Traceback (most recent call last):
  File "/home/chodera/miniconda/bin/yank", line 11, in <module>
    load_entry_point('yank==0.22.1', 'console_scripts', 'yank')()
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/cli.py", line 73, in main
    dispatched = getattr(commands, command).dispatch(command_args)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/commands/script.py", line 136, in dispatch
    yaml_builder.run_experiments()
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/experiment.py", line 793, in run_experiments
    send_results_to='all')
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/mpi.py", line 520, in distribute
    *other_args, **kwargs)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/mpi.py", line 373, in exec_tasks
    raise error
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/mpi.py", line 358, in exec_tasks
    results.append(task(distributed_arg, *other_args, **kwargs))
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/experiment.py", line 3093, in _run_experiment
    built_experiment.run(n_iterations=switch_experiment_interval)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/experiment.py", line 450, in run
    alchemical_phase = phase.initialize_alchemical_phase()
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/experiment.py", line 316, in initialize_alchemical_phase
    alchemical_phase = self.create_alchemical_phase()
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/experiment.py", line 302, in create_alchemical_phase
    **create_kwargs)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/yank.py", line 976, in create
    sampler_states[0], topography)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/restraints.py", line 2749, in determine_missing_parameters
    self._pick_restrained_atoms(topography)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/restraints.py", line 2808, in _pick_restrained_atoms
    defined_atoms = atom_selector.compute_atom_intersect(atoms, top_key)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/restraints.py", line 459, in compute_atom_intersect
    return compute_atom_set(input_atoms)
  File "/home/chodera/miniconda/lib/python3.6/functools.py", line 803, in wrapper
    return dispatch(args[0].__class__)(*args, **kw)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/restraints.py", line 450, in compute_atom_str
    output = topography.select(input_string, as_set=False)  # Preserve order
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/yank.py", line 509, in select
    selection_output, errors = selector.select(selection)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/yank.py", line 423, in select
    region_output_unmapped = list(self._get_region_set(region_string))
  File "/home/chodera/miniconda/lib/python3.6/site-packages/yank/yank.py", line 605, in _get_region_set
    parsed_output = mmtools.utils.math_eval(region_set_string, variables=variables)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/openmmtools/utils.py", line 324, in math_eval
    return _math_eval(ast.parse(expression, mode='eval').body)
  File "/home/chodera/miniconda/lib/python3.6/site-packages/openmmtools/utils.py", line 316, in _math_eval
    raise TypeError('Cannot parse expression: {}'.format(expression))
TypeError: Group 16/16 Node 1/1: Cannot parse expression: (mass > 1.5)
```
I've added `TypeError` to the list of exceptions that `SelectRegion` catches in order to allow this to proceed onto the `SelectDsl` selector.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `80ee4f82d308`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.