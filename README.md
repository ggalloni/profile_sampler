# Profile Sampler for Cobaya

[![Testing](https://github.com/ggalloni/profile_sampler/actions/workflows/testing.yml/badge.svg)](https://github.com/ggalloni/profile_sampler/actions/workflows/testing.yml)

This package provides a sampler for the 
[Cobaya]([https://cobaya.readthedocs.io](https://github.com/CobayaSampler/cobaya))
framework that allows obtaining the profile likelihood of a given parameter of interest.

## Overview

The `Profiler` sampler inherits from the standard `Minimize` to loop various minimization 
processes on a selected parameter.
In the end, the output will be a `SampleCollection` of minimized points, storing the selected 
points and their corresponding $\chi^2_{min}$ (or maximum likelihood through exponentiation). 
It is also possible to obtain the posterior equivalent of that by setting `ignore_prior=False`.

This sampler works best if an MCMC run is already present on the same parameter space. 
Indeed, this allows the optimization of the initial point for minimizations and the covariance 
matrix of parameters.

Also, through the `Profiler.products` method, it is possible to recover the full set of minima 
among the parallel runs (`bets_of` parameter). This allows us to ensure that the minimum recovered 
is the absolute minimum. Furthermore, the dispersion of these points gives a rough idea of the 
accuracy of the minimization process.

### Parameters

- `profiled_param`: string identifying the profiled parameter. It must be part of the sampled parameters!
- `profiled_values`: list of profiled values at which to perform the minimization.
- `start`: as an alternative to explicitly providing a list of values, one can set a range by
  passing a starting and ending point with a number of steps in between those. Note that `profiled_values`
  takes precedence w.r.t. this method!
- `stop`: ending point of the range.
- `steps`: number of steps between `start` and `stop`. Note that the range is obtained using
  `numpy.linspace` and setting `endpoint=True`!

The rest of the parameters are the same as the `Minimize` sampler, thus refer to its 
[documentation](https://cobaya.readthedocs.io/en/latest/sampler_minimize.html).

## Usage

[Here](examples/run_profile_sampler.yaml), you can find a straightforward example to run the `Profiler` 
on a Gaussian likelihood.

A similar implementation of this code was used in [2405.04455](https://arxiv.org/abs/2405.04455) 
to profile the tensor-to-scalar ratio and the tensor spectral tilt (code under development at the 
time of paper writing). There you can also find more details on the theory of profile likelihoods.

## Bibliography

Please cite the following associated paper if you use this code in your work:
```
@article{Galloni:2024lre,
    author = "Galloni, Giacomo and Henrot-Versill\'e, Sophie and Tristram, Matthieu",
    title = "{Robust constraints on tensor perturbations from cosmological data: A comparative analysis from Bayesian and frequentist perspectives}",
    eprint = "2405.04455",
    archivePrefix = "arXiv",
    primaryClass = "astro-ph.CO",
    doi = "10.1103/PhysRevD.110.063511",
    journal = "Phys. Rev. D",
    volume = "110",
    number = "6",
    pages = "063511",
    year = "2024"
}
```
