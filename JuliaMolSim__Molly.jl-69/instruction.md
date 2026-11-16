# Issue

**Title:** Have loggers in NamedTuples / make most loggers instances of a GeneralObservableLogger

## Description

# Suggested changes:
Replace every instance of a `System`'s loggers as a `Dict` to a `NamedTuple`.
This is important for type-inference reasons, especially when the `System` has several different loggers. In the case of `TimeCorrelationLogger`, the speedup is significant.

# Cons
This introduces ugly syntax. For example, if
```julia
sys.loggers=(coords=CoordinateLogger(args...))
```
the syntax to access the recorded coordinate history is 
```julia
sys.loggers.coords.coords
```
This could be alleviated by having a common (possibly non-exported) type for all loggers which push some observation onto a history vector, like

```julia
struct GeneralObservableLogger {T,F}
    n_steps::Int64
    history::Vector{T}
    observable::F
end

function log_property!(logger::GeneralObservableLogger,s::System,neighbors=nothing,step_n::Integer=0;parallel::Bool=true)
    if (step_n % logger.n_steps) == 0
        obs=logger.observable(sys,neighbors;parallel=parallel)
        push!(logger.history,obs)
    end
end
```
and have `TemperatureLogger`, `CoordinateLogger`, etc... wrap a constructor for this type.
So logged observations would be accessed through
`sys.loggers.coords.history`
and so on.
This would also shorten `loggers.jl` by removing a lot of boilerplate code / make it easier for users to define custom loggers of this kind. I can implement this change if that sound good.

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `d5e26c4c5d37`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.