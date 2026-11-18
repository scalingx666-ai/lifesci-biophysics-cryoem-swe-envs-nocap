# Issue

**Title:** Add how-to-guide on stimulating a cell with spike trains

## Description

This adds a new how-to guide which explains how one can connect synapses to pre-defined spike trains:

https://jaxley.readthedocs.io/en/beta/how_to_guide/predefined_spike_trains.html

The tutorial will go live with version 1.0.0

```python
t_max = 100.0  # ms
dt = 0.025  # ms
time_steps = int(t_max // dt)

firing_rate = 50  # Hz
spike_prob = firing_rate * dt / 1000

spike_train = np.random.binomial(1, spike_prob, size=time_steps)

dummy = jx.Cell()
cell = jx.read_swc(fname, ncomp=1)
net = jx.Network([dummy, cell])
net.cell(1).insert(Leak())

# Connect pre-synaptic dummy to the morphologically detailed cell.
connect(net.cell(0), net.cell(1).branch(3).comp(0), ExpDecaySynapse())
net.set("ExpDecaySynapse_gS", 0.1)  # Synaptic strength.
net.set("ExpDecaySynapse_decay_tau", 5.0)  # decay time in ms

# Clamp the voltage of the pre-synaptic cell to the spike train.
net.cell(0).set("v", 0.0)  # Initial state.
net.cell(0).clamp("v", spike_train)

net.cell(1).branch(3).comp(0).record()
v = jx.integrate(net, delta_t=dt)
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `3bd298bd7d7d`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.