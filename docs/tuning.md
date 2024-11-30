## Tuning

MCLMC has two parameters, *step size* $\epsilon$ and momentum decoherence time $L$.

One of these, $L$, determines the SDE itself, in particular the amount of stochasticity. The other, $\epsilon$, controls not the SDE, but the way it is turned into a random walk (discretized), namely the amount by which time is moved forward in each discrete jump.

It is critical to tune both well.

[Microcanonical Hamiltonian Monte Carlo](https://arxiv.org/pdf/2212.08549.pdf) introduces an algorithm for this purpose. 

### Tuning step size

The idea is that we want to minimize bias and variance, in the sense of $b_1 := \frac{1}{d}\sum_i^d z_i$ and $\sigma^2 := \frac{1}{d}\sum_i^d (z_i - b_1)^2$, where:

$$
z_i = \frac{E_{emp}[x_i^2] - E[x_i^2]}{E[x_i^2]}
$$

and $x_i$ is the component of $x$ along a single dimension. $E_{emp}$ is the expectation under the empirical distribution induced by the sampler, i.e. calculating the expectation using the samples. $E$ is the true expectation, which of course we don't in general have access to.


The idea of the tuning algorithm is that $Var[E]/d$ (where $E$ is the energy) is a proxy for bias, because the underlying SDE is energy preserving. We therefore try to minimize $Var[E]/d$.

Roughly, we do this by running a chain, keeping a running average of the variance, and updating the step size at each step.

### Tuning L

The intuition for tuning $L$ is related to the typical set, defined as the set $\{x : -log(p(x)) \approx S \}$, where $\approx$ is left to be determined.

Assuming the typical set is topologically simple (i.e. basically an n-dimensional deformed ball), we want momentum to decohere by roughly the time it takes to cross the typical set.

So our goal is to estimate the radius of the typical set. For an isotropic Gaussian with covariance $\sigma^2 I$, this has radius $\sigma d^\frac{1}{2}$. Therefore we can make a guess by estimating the variance of each dimension of the target distribution and averaging them:

$$
\sigma^2_{eff} := \frac{1}{d}\sum_{i}^d Var[x_i^2]
$$

This is our first guess, and we use the chain from the calculation of the step size for this purpose.

We refine it by doing a subsequent run (using this initial $L$), to obtain a chain, calculating the effective sample size $n_{eff} := \frac{1}{d}\sum_i^d n^{(i)}_{eff}$, using an autocorrelation-based effective sample size estimate, and reasoning:

$$
l \cdot n_{eff} = \epsilon \cdot n
$$

where $l$ is the distance between effective samples. We assert that $L$ should be on the order of $l$ (in fact, we use $L=0.4 l$), because we want roughly an effective sample for each decoherence of $L$.
