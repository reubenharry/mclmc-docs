## Onsager sampling: a proposed new method

!!! Summary

    importance sampling from chains by using the path-integral formula for their distribution.

One can think of MCMC sampling in the following way.

Suppose we wish to sample from a distribution $\pi$ on $S$ and know only $f(x) \propto \pi(x)$.

Define a chain as a function $\mathbb{Z}/n \to S$, where $n$ is the chain length and $S$ is the state space.

A given MCMC algorithm specifies a distribution $\Pi$ over chains with two properties:

1. It can be sampled ancestrally.
2. The marginal on its right hand boundary is the target distribution (in the limit of large $n$)

(1) means that we don't in general need to know the distribution over chains, which is easy to calculate in the discrete case, but harder in the continuous case. (2) means that we can generate samples from $\pi$ by sampling $x\sim\Pi$ and taking $x(n)$.

As a notation, let us write $\Pi(f)$ for the distribution over chains induced by $f$.

### Continuous time

MCMC also works in continuous time. That is, for chains $\mathbb{R}_{\leq n} \to S$, there are ways to specify distributions over chains that satisfy the two properties above, by an SDE. The caveat is that for (1) to hold, the SDE must be discretized, which incurs bias (which can in turn be corrected by MH).

A concrete example is Langevin dynamics:

$$
d X = \nabla \log f(X) dt + \sqrt{2} dW
$$

which can be discretized as:

$$
X_{t+1} = X_t + \epsilon \nabla \log f(X_t) + \sqrt{2\epsilon} Z
$$

where $Z\sim N(0,1)$.

The distribution this induces over continuous chains is not obvious, but is known. Given an SDE $dX = f(X) dt + g(X) dW$, the distribution over chains is given by:

$$
P(x | x_0) \propto \exp\left(- \int \frac{1}{2g(x,t)^2} (\dot x(t) - \nabla \log f(x(s),s) - x_0\delta(s))^2 ds\right)
$$

Taking Langevin dynamics in particular, and assuming $x_0=0$, we have:

$$
\Pi(x | f) \propto \exp\left(- \int \frac{1}{4} (\dot x - \nabla \log f(x(s)))^2 ds\right)
$$

### Importance sampling

Because we know the distribution over chains, we can perform importance sampling. That is, we can draw a sample  $x \sim \Pi(\cdot | g)$, and weigh it by $\Pi(x | f)/\Pi(x | g)$, at least approximately.

To calculate $\Pi(x | h)$, we approximate by the Riemann sum. That is, for each step of the chain (now discretized), we multiply by $\exp(-\frac{1}{4}(\dot x - \nabla \log h(x))^2)$. $\dot x$ can be approximated too as $\frac{x_{t+1} - x_t}{\epsilon}$. The ratio is then easy to calculate. I don't yet know what error this incurs.

More usefully, we can *sequentially* importance sample. That is, we can importance sample at each stage of the ancestral sampling of the chain. And more than that, we can run $m$ chains in parallel, and resample from them at each stage according to their weights. (This is a particle filter).

### What this means in practice

It's worth summarizing what this amounts to. The idea is that the dynamics of the "particle" are determined not by the log density of the target, but by some other log density, and then reweighting at each step (or every $q$ steps) adjusts the distribution over chains such that it has the desired distribution, which means that its boundary marginal is the target.

## Applications

It's not totally clear that this is useful. One application is maybe annealing. That is we can produce dynamics according to a high temperature version of the target, and reweight. One advantage over regular annealing here is that we can choose how often to resample, so it might be easier to determine a good annealing schedule.











### HMC and variants

Goal: sample from p(x), given only f(x) = p(x) up to a normalizing constant.


