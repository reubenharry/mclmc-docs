# Applications

## Bayesian Statistics

MCLMC is intended as a successor to NUTS, which is the version of Hamiltonian Monte Carlo used most widely in the social sciences for Bayesian analyses.

## Lattice Field Theories

See the later section of [this paper](https://arxiv.org/abs/2303.18221) for a comparison of MCLMC and HMC on a simple lattice field theory.

## Astrophysics

[Field-Level Inference with Microcanonical Langevin Monte Carlo
](https://arxiv.org/abs/2307.09504)

## Molecular Dynamics

Molecular Dynamics (MD) is a computational method for investigating the (classical) behavior of physical systems, which uses a numerical integrator and an (approximate) potential function to simulate the Hamiltonian dynamics of a molecular system, usually over a sufficient time period in order that equilibrium is reached.

However, if equilibrium thermodynamic information is the goal, the dynamics need not follow the true Hamiltonian, and MCLMC may be much faster. 

!!! Note 
    
    Here, we assume we are only interested in expectations over the configuration space marginal distribution $e^{-\beta V(x)}$ . In theory, we could apply the technique to the full phase space, $e^{-\beta H(\mu)}$.

We apply MD to two standard systems:

1. Alanine Dipeptide (AD): [notebook](ad_mclmc_clean.html)
2. Silicon Crystal: [notebook](si.html)

Both of these examples use [Jax MD](https://github.com/jax-md/jax-md), which interplays nicely with the Jax implementation of MCLMC. Periodic boundary conditions need to be applied (which requires no modification of the sampling code, but a change to the potential), and for notebook 2, a neighbour list needs to be calculated every so often (which currently is done via a modification of the sampling code). Other than this, MCLMC works here as normal.