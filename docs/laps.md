## The late adjusted parallel sampler (LAPS)

LAPS is a parallel sampler based on a simple idea: once an MCMC chain has reached the typical set, it is trivial to parallelize, but not before.

Building on other methods like Pathfinder, the idea is then to equilibrate (aka burn in, aka thermalize) efficiently. LAPS does so by running chains without MH in an initial phase, while also tuning the step size and other parameters. Then it switches to an MH adjusted sampler.
