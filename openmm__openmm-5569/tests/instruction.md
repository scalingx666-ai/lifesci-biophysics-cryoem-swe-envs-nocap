# Issue

**Title:** Added more Context methods to Simulation

## Description

This is a minor quality of life improvement I've had in mind to make for a while.  A Simulation is a wrapper around a Context.  It provides its own methods for some functions, but for others you have to access the Context directly.  Every time I find myself typing `simulation.context`, it feels like a little bit of unnecessary boilerplate.  It also increases cognitive burden.  For even the simplest uses, you need to understand the distinction between the Simulation and the Context and remember which methods are called on which one.

So this PR adds four more methods to Simulation: `setPositions()`, `setVelocities()`, `setVelocitiesToTemperature()`, and `getState()`.  Those are the most common Context methods that people use all the time.  All of them are identical to the corresponding Context methods.  There are lots of other methods you still need to access the Context for, but trying to add all of them would make Simulation excessively complex.  I just wanted to include the basic ones.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `146a320f97bd`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.