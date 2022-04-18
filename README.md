![IonSim logo](images/logo3_SM.png)

# IonSim

IonSim.jl is a lightweight Julia package for simulating the dynamics of a configuration of trapped ions interacting with laser light.

IonSim leverages [QuantumOptics.jl](https://qojulia.org/) to deliver a performant, quantitatively faithful tool for simulating fundamental interactions in trapped ion experiments. Several ion species and trap configurations are implemented. Everything is written in the language of experimentalists (ions and lasers, not qubits and gates).

* *Fast*: Runtimes comparable to QuTiP (Cython).
* *Intuitive*: You set up your simulation the same way that you set up your experiments.
* *Flexible*: Full control over RWA cutoff frequencies, Lamb-Dicke order approximations, Hilbert space truncation and methods.
* *Open Source*: All source code is freely available and built with extensibility in mind.

<iframe src="https://ghbtns.com/github-btn.html?user=HaeffnerLab&repo=IonSim.jl&type=star&count=true&size=medium" frameborder="0" scrolling="0" width="170" height="30" title="GitHub" style="padding-bottom: 10px;"></iframe>

## Documentation

[The IonSim docs are at this link](https://docs.ionsim.org/).

## Examples

[Check out all of our examples here](https://examples.ionsim.org/).

## Planned features

In the immediate future, we plan to implement:
* configurable ion species
* using ion traps with different noise models

See [our GitHub issues](https://github.com/HaeffnerLab/IonSim.jl/issues) for the full details.

## Contact the developers

IonSim is maintained by [Hartmut Haeffner's trapped ion group](https://ions.berkeley.edu/) at UC Berkeley.

If you'd like to contribute to IonSim.jl, head over to our [GitHub page](https://github.com/HaeffnerLab/IonSim.jl).
* If you have a good idea of what you'd like to do and how to do it, the preferred method is to submit a pull request on GitHub.
* If you're less sure about your ideas, would like some feedback, or want to open up a discussion, then feel free to open an issue on GitHub.
