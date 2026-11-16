# Issue

**Title:** Add Side Length checker

## Description

I added checks for side length to be greater than zero for `CubicBoundary` and `RectangularBoundary`.

I did two unit tests locally. One of them gave an error shown below. Looks like zygote might create a `CubicBoundary` with negative side length. However, I was not able to reproduce this again. 
```
Gradients: Error During Test at /Users/yushengzhao/projects/Molly.jl/test/zygote.jl:1
  Got exception outside of a @test
  AssertionError: Side lengths need to be larger than 0
  Stacktrace:
    [1] CubicBoundary
      @ ~/projects/Molly.jl/src/spatial.jl:38 [inlined]
    [2] accum(x::NamedTuple{(:side_lengths,), Tuple{SizedVector{3, Float64, Vector{Float64}}}}, y::SVector{3, Float64})
      @ Molly ~/projects/Molly.jl/src/zygote.jl:137
    [3] (::Zygote.var"#back#291"{:contents, Zygote.Context{false}, Core.Box, CubicBoundary{Float64}})(Δ::SVector{3, Float64})
      @ Zygote ~/.julia/packages/Zygote/xGkZ5/src/lib/lib.jl:237
    [4] (::Zygote.var"#2149#back#292"{Zygote.var"#back#291"{:contents, Zygote.Context{false}, Core.Box, CubicBoundary{Float64}}})(Δ::SVector{3, Float64})
      @ Zygote ~/.julia/packages/ZygoteRules/OgCVT/src/adjoint.jl:71
    [5] Pullback
      @ ~/projects/Molly.jl/test/zygote.jl:136 [inlined]
    [6] (::typeof(∂(λ)))(Δ::Float64)
      @ Zygote ~/.julia/packages/Zygote/xGkZ5/src/compiler/interface2.jl:0
    [7] (::Zygote.var"#68#69"{typeof(∂(λ))})(Δ::Float64)
      @ Zygote ~/.julia/packages/Zygote/xGkZ5/src/compiler/interface.jl:45
    [8] gradient(::Function, ::Float64, ::Vararg{Float64})
      @ Zygote ~/.julia/packages/Zygote/xGkZ5/src/compiler/interface.jl:97
    [9] macro expansion
      @ ~/projects/Molly.jl/test/zygote.jl:196 [inlined]
   [10] macro expansion
      @ /Applications/Julia-1.8.app/Contents/Resources/julia/share/julia/stdlib/v1.8/Test/src/Test.jl:1363 [inlined]
   [11] top-level scope
      @ ~/projects/Molly.jl/test/zygote.jl:2
   [12] include(fname::String)
      @ Base.MainInclude ./client.jl:476
   [13] top-level scope
      @ ~/projects/Molly.jl/test/runtests.jl:82
   [14] include(fname::String)
      @ Base.MainInclude ./client.jl:476
   [15] top-level scope
      @ none:6
   [16] eval
      @ ./boot.jl:368 [inlined]
   [17] exec_options(opts::Base.JLOptions)
      @ Base ./client.jl:276
   [18] _start()
      @ Base ./client.jl:522
Test Summary: | Pass  Error  Total     Time
Gradients     |    9      1     10  1m41.3s
ERROR: LoadError: Some tests did not pass: 9 passed, 0 failed, 1 errored, 0 broken.
in expression starting at /Users/yushengzhao/projects/Molly.jl/test/zygote.jl:1
in expression starting at /Users/yushengzhao/projects/Molly.jl/test/runtests.jl:81
ERROR: Package Molly errored during testing
```

## Task

Modify the repository so that the issue described above is resolved. The repository is checked out at base commit `6de8b96318b5`. Edit files in place; the verifier captures your changes via `git diff` and scores them against an oracle patch using SWE-RL-style diff-similarity reward.