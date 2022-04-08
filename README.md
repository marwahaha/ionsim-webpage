# IonSim

IonSim leverages [QuantumOptics.jl](https://qojulia.org/) to deliver a performant, quantitatively faithful tool for simulating fundamental interactions in trapped ion experiments. Several ion species and trap configurations are implemented. Everything is written in the language of experimentalists (ions and lasers, not qubits and gates).

* *Fast*: Runtimes comparable to QuTiP (Cython).
* *Intuitive*: You set up your simulation the same way that you set up your experiments.
* *Flexible*: Full control over RWA cutoff frequencies, Lamb-Dicke order approximations, Hilbert space truncation and methods.
* *Open Source*: All source code is freely available and built with extensibility in mind.

## Downloading

If you haven't done so already, download [Julia](https://julialang.org/) (platform specific instructions can be found [here](https://julialang.org/downloads/)). Next, open the Julia app, which should launch a [REPL](https://docs.julialang.org/en/v1/stdlib/REPL/#The-Julia-REPL-1) session, and install IonSim using the following commands:

```
using Pkg
Pkg.add("QuantumOptics")
Pkg.add(PackageSpec(url="https://github.com/HaeffnerLab/IonSim.jl.git"))
```

The main way you'll want to interact with IonSim is inside of a [Jupyter notebook](https://jupyter.org/). This requires [IJulia.jl](https://github.com/JuliaLang/IJulia.jl):

```
Pkg.add("IJulia")
```

And finally, if you'd like to follow along with the example notebooks, you'll need at least [PyPlot.jl](https://github.com/JuliaPy/PyPlot.jl), an interface to Python's [Matplotlib](https://matplotlib.org/) library:

```
Pkg.add("PyPlot")
```

Updating to the latest version of IonSim is easy:

```
Pkg.update("IonSim")
```

## What is IonSim?

IonSim.jl is a tool to simulate the dynamics of a collection of trapped ions interacting with laser light.

IonSim primarily performs two jobs:
1. Keep track of the physical parameters necessary for describing the system, with a structure and nomenclature designed to be intuitive for experimentalists.
2. Using these parameters, construct a function that quickly evaluates the system's Hamiltonian at a particular point in time.

This functional form of the Hamiltonian can then be used either as input to any of the solvers implemented in QuantumOptics.timeevolution, or in the native solver. The native solver is a thin wrapper around QuantumOptics functions that implement additional checks.

Alternatively, one may construct a DynamicalProblem, which itself can be given as input to the native solver.

## Documentation

[All documentation is here](https://docs.ionsim.org/dev/).

[Check out a number of examples here](https://examples.ionsim.org/).

The code is MIT-licensed; find the code [on GitHub](https://github.com/HaeffnerLab/IonSim.jl).

## Planned features for IonSim

In the immediate future, we plan to implement:
* configurable ion species
* using ion traps with different noise models

See [our GitHub issues](https://github.com/HaeffnerLab/IonSim.jl/issues) for the full details.

## Contact the developers

IonSim is maintained by [Hartmut Haeffner's trapped ion group](https://ions.berkeley.edu/) at UC Berkeley.

If you'd like to contribute to IonSim.jl, head over to our [GitHub page](https://github.com/HaeffnerLab/IonSim.jl).
* If you have a good idea of what you'd like to do and how to do it, the preferred method is to submit a pull request on GitHub.
* If you're less sure about your ideas, would like some feedback, or want to open up a discussion, then feel free to open an issue on GitHub.

