![IonSim logo](images/logo3_SM.png)

# IonSim

IonSim leverages [QuantumOptics.jl](https://qojulia.org/) to deliver a performant, quantitatively faithful tool for simulating fundamental interactions in trapped ion experiments. Several ion species and trap configurations are implemented. Everything is written in the language of experimentalists (ions and lasers, not qubits and gates).

* *Fast*: Runtimes comparable to QuTiP (Cython).
* *Intuitive*: You set up your simulation the same way that you set up your experiments.
* *Flexible*: Full control over RWA cutoff frequencies, Lamb-Dicke order approximations, Hilbert space truncation and methods.
* *Open Source*: All source code is freely available and built with extensibility in mind.

<iframe src="https://ghbtns.com/github-btn.html?user=HaeffnerLab&repo=IonSim.jl&type=star&count=true&size=medium" frameborder="0" scrolling="0" width="170" height="30" title="GitHub" style="padding-bottom: 10px;"></iframe>

# Downloading

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

# Documentation

IonSim.jl is a tool to simulate the dynamics of a collection of trapped ions interacting with laser light.

IonSim primarily performs two jobs:
1. Keep track of the physical parameters necessary for describing the system, with a structure and nomenclature designed to be intuitive for experimentalists.
2. Using these parameters, construct a function that quickly evaluates the system's Hamiltonian at a particular point in time.

This functional form of the Hamiltonian can then be used either as input to any of the solvers implemented in QuantumOptics.timeevolution, or in the native solver. The native solver is a thin wrapper around QuantumOptics functions that implement additional checks.

Alternatively, one may construct a DynamicalProblem, which itself can be given as input to the native solver.


[All documentation is here](https://docs.ionsim.org/dev/).

The code is MIT-licensed; find the code [on GitHub](https://github.com/HaeffnerLab/IonSim.jl).

# Planned features

In the immediate future, we plan to implement:
* configurable ion species
* using ion traps with different noise models

See [our GitHub issues](https://github.com/HaeffnerLab/IonSim.jl/issues) for the full details.

# Contact the developers

IonSim is maintained by [Hartmut Haeffner's trapped ion group](https://ions.berkeley.edu/) at UC Berkeley.

If you'd like to contribute to IonSim.jl, head over to our [GitHub page](https://github.com/HaeffnerLab/IonSim.jl).
* If you have a good idea of what you'd like to do and how to do it, the preferred method is to submit a pull request on GitHub.
* If you're less sure about your ideas, would like some feedback, or want to open up a discussion, then feel free to open an issue on GitHub.

# Examples

[Check out all of our examples here](https://examples.ionsim.org/).

## Molmer-Sorenson

```
using IonSim

# Construct the system
C = Ca40(["S-1/2", "D-1/2"])
L1 = Laser(ϕ=π); L2 = Laser()  # note the π-phase between L1/L2
chain = LinearChain(
        ions=[C, C], com_frequencies=(x=3e6, y=3e6, z=2.5e5),
        vibrational_modes=(;z=[1])
    )
T = Trap(
        configuration=chain, B=6e-4, Bhat=(x̂ + ẑ)/√2,
        lasers=[L1, L2]
    )
mode = T.configuration.vibrational_modes.z[1]

# Set the laser parameters
ϵ = 10e3
d = 350  # correct for single-photon coupling to sidebands
Δf = transition_frequency(T, 1, ("S-1/2", "D-1/2"))
L1.Δ = Δf + mode.ν + ϵ - d
L2.Δ = Δf - mode.ν - ϵ + d
L1.k = L2.k = ẑ
L1.ϵ = L2.ϵ = x̂
# set 'resonance' condition: ηΩ = 1/2ϵ
η = abs(get_η(mode, L1, C))
E = Efield_from_pi_time!(η/ϵ, T, 1, 1, ("S-1/2", "D-1/2"))
Ω = t -> t < 20 ? E * sin(2π * t / 80)^2 : E  # ampl. ramp
L1.E = L2.E = t -> Ω(t)

# Build Hamiltonian
h = hamiltonian(T, lamb_dicke_order=1, rwa_cutoff=Inf)

# Solve
t, sol= solve(0:.1:220, C["S-1/2"] ⊗ C["S-1/2"] ⊗ mode[0], h)
```

```
import PyPlot
const plt = PyPlot

SS = expect(ionprojector(T, "S-1/2", "S-1/2"), sol)
DD = expect(ionprojector(T, "D-1/2", "D-1/2"), sol)
SD = expect(ionprojector(T, "S-1/2", "D-1/2"), sol)
DS = expect(ionprojector(T, "D-1/2", "S-1/2"), sol)
plt.plot(t, SS, label="SS")
plt.plot(t, DD, label="DD")
plt.plot(t, SD, label="SD")
plt.plot(t, DS, label="DS")
plt.plot(t, @.(Ω(t) / 2E), ls="--", label="scaled ramp")
plt.legend(loc=1)
plt.xlim(tout[1], tout[end])
plt.ylim(0, 1)
plt.xlabel("Time (μs)")
```

![Molmer-Sorenson example output](images/msplot_tight.png)

## Rabi flop

## RAP