# Microcanonical Hamiltonian Monte Carlo

A couple of recent papers (notably https://arxiv.org/pdf/2212.08549.pdf) have proposed a really cool gradient based sampling technique, which appears to be very fast, especially at finding modes of distributions. The following notes explain how it works.

Broad prerequisites:

- some familiarity with probability and Bayesian inference
- general knowledge of Markov chain Monte Carlo methods
- linear ordinary differential equations (in particular, the Hamiltonian ODE of classical physics)


One way to view Markov Chain Monte Carlo algorithms, which is particularly enlightening for the gradient based versions like HMC, is as follows:

What we want:

- to sample from a distribution p(x)

What we do:

1. Specify some discrete, preferably Markovian, stochastic process that has as a fixed point under its flow a distribution $q$, such that $p$ can be obtained easily from $q$.

2. Run the process forward to generate samples from $q$, under the assumption of ergodicity, and transform into samples from $p$.

The more detailed version of this procedure for Hamiltonian Monte Carlo (HMC) looks like this:

1. Choose a Hamiltonian $H$, such that for $q(x,z) \propto e^{-bH(x,z)}$ (known in statistical mechanics as the *canonical distribution*), we have $p(x) = \int dz q(x,z)$. Any classical $H$ of the form $H= T(z) + V(x)$ (for $z$ momentum and $x$ position) will do. 
2. The Hamiltonian ODE implies a volume preserving deterministic process (flow), call it $f$, which when discretized by a symplectic (volume preserving) integrator S, gives a discrete process S(f).
3. Add occasional resampling of momentum for reasons of ergodicity, to yield the desired stochastic process.
4. Obtain samples from $q$, which are (x,z) pairs, and throw away the z part, to get samples from $p$.

This can be generalized by moving from the Hamiltonian ODE to any SDE that has $q$ as its fixed point distribution. [This paper](https://proceedings.neurips.cc/paper_files/paper/2015/file/9a4400501febb2a95e79248486a5f6d3-Paper.pdf) gives a recipe for constructing such all such SDEs. Specific choices of SDE yield familiar algorithms, in particular Hamiltonian Monte Carlo (HMC), Langevin Monte Carlo (LMC) and the various Riemannian and/or stochastic gradient varieties of those.

One has the option of further embellishing the discrete process with Metropolis-Hastings (MH) steps. Calculating the MH ratio only requires the transition probability ratio, since the probability of $p$ does not change (since the ODE is volume preserving by Liouville's theorem, and so is the discrete process (thanks to the sympletic integrator). One has to be slightly careful about the transition probabilities; see section 5.2 of [this introduction](https://arxiv.org/pdf/1701.02434.pdf). 

If one doesn't adjust, the samples from $q$ will be at least slightly biased. The same goes for LMC, where the adjusted verision is known, sensibly, as the Metropolis Adjusted Langevin Algorithm (MALA).

This would seem to be the final word in this problem. Not so.

What is proposed in [this paper](https://arxiv.org/pdf/2212.08549.pdf) is to instead consider a distribution $q(z,x) := \delta(H(x,z)-E)$ (known in statistical mechanics as the micocanonical ensemble), with $H$ chosen carefully, such that the marginal over x, i.e. $q(x)=\int dz q(x,z)$, is equal to p(x). One can then consider both the analogs of HMC and LMC in the microcanonical setting.

[This paper](https://arxiv.org/pdf/2212.08549.pdf) and [this paper](https://arxiv.org/pdf/2303.18221.pdf) establish the requisite properties of these inference algorithms, consider appropriate choices of H and kinetic energy to ensure the right marginal, sketch a proof that the stationary distribution is $q$ and show that it works very well both on toy problems and simple real ones.


## Choosing $H$

The simplest case is a separable $H$. [This paper](https://proceedings.neurips.cc/paper/2021/file/5b970a1d9be0fd100063fd6cd688b73e-Paper.pdf) proposes the following choice: 

$$
T(z) = \frac{d}{2}\log(\frac{z^2}{d}) 
$$

where $d$ is the dimensionality of the configuration space, and

$$
V(x) = -\log p'(x)
$$

where $p'(x)/Z := p(x)$ is the unnormalized probability distribution.


The short proof comes from the beginning of section 2.1 [here](https://arxiv.org/pdf/2111.02434.pdf).

$$
p(x) \propto \int_{\mathbb{R}^d} \delta(H(x, p) - E) d p = 
$$

$$
\newcommand{\ve}{\mathbf e}
\newcommand{\vv}{\mathbf v}
\newcommand{\vu}{\mathbf u}
\newcommand{\vx}{\mathbf x}
\newcommand{\vz}{\mathbf z}
\newcommand{\vf}{\mathbf f}
\newcommand{\vg}{\mathbf g}
\newcommand{\gd}{|\mathbf g|/d}
\newcommand{\half}{\frac{1}{2}}
\newcommand{\eps}{\epsilon}
\newcommand{\epshalf}{\nicefrac{\eps}{2}}
$$

$$
\begin{align*}
    {p(\vx)} &= \int d\vv ~p(\vx, \vv) = 1/Z' \int { d\vv} ~{ \delta\left(E(\vx) + d/2 \log v^2/d - c\right)} \\
    &= 1/Z' \int { J(\bm \phi)~d\bm \phi~ \rho^{d-1}~d\rho} ~ { \delta\left(E(\vx) + d \log \rho - c -d/2 \log d \right)} \\
    &= 1/Z' \int { J(\bm \phi)~d\bm \phi~ \rho^{d-1}~d\rho} ~{ \rho/d ~ \delta(\rho - e^{-E(\vx)/d + c/d + \half \log d})} = {e^{-E(\vx)} / Z} ~~~~
\end{align*}
$$

This uses a change of variables to hyperspherical coordinates, and the identity $\delta(h(z))=\delta(z-z^*)/|h'(z^*)|$, where $z^*$ is the unique root of $h$.

The Hamiltonian ODE then gives:

$$
\dot x = v/(v^2/d)
$$

$$
\dot v = -\partial_xE(x)
$$

[Robnik, De Luca, Silverstein and Seljak](https://arxiv.org/pdf/2212.08549.pdf) generalize this to a whole family of kinetic energies, but focus most of the attention on this one, or rather, a slight variation:

$$
T = \log
$$

This differs from $T(z) = \frac{d}{2}\log(\frac{z^2}{d}) = d\log z - \frac{d}{2}\log(d)$ by a scaling and constant term.

More esoteric choices like the relavitistic Hamiltonian (which is non-separable) are considered in the paper too.

<!-- To see this, note that the integral can be converted into (hyper)spherical coordinates. The angular part of the integral can be evaluated separately (we assume a rotationally symmetric $H$), giving the numerator. -->

<!-- The denominator is the integral $\int \delta(H(\mu))d\mu$, which is a standard result. -->



The Hamiltonian yields a deterministic process, but doesn't give an ergodicity guarantee. [Robnik et al.](https://arxiv.org/pdf/2212.08549.pdf) proposes adding momentum changes after every L steps, which preserve the norm, and hence the energy, but make the process ergodic.

They then generalize the previous approach by specifying an SDE (instead of the previous ODE) which continuously varies the momentum stochastically, according to:

$$
\ddot x = -\nabla V(x) - \gamma \dot X + (2\gamma)^{\frac{1}{2}}B
$$

where $B$ is Brownian. This can be reduced to a first order Langevin SDE, and solved as:

todo

It is a standard result (see 
    TODO)
    that the stationary distribution is the canonical distribution. 

## Time rescaling

## 🚧 Under construction 🚧

Numerical integration of the SDE requires a small step size because the movement changes direction quickly at high speed.

Instead, one can rescale time to a natural parameter, such that each step moves by the same length on the manifold.

<!-- This takes us out of the symplectic (Hamiltonian) setting, and indeed the eventual resulting SDE will have a stationary distribution that does not resemble -->

<!-- The consequence of this is that the integrator must be rederived, using the standard  -->

## Showing that the stationary distribution obtains

## 🚧 Under construction 🚧

## Choosing the integrator

## 🚧 Under construction 🚧


One choice is the standard 2nd order leapfrog integrator, which is sympletic (see [these notes](/maths/numericalintegration)).

The fancier minimal norm integrator discussed [here](https://arxiv.org/pdf/hep-lat/0505020.pdf) turns out to work well in practice.

$$
\Phi_{\epsilon} = \Phi_{\epsilon \lambda}^{V} \circ \Phi_{\epsilon/2}^{T}\circ \Phi_{\epsilon (1-2\lambda)}^{V} \circ \Phi_{\epsilon/2}^{T} \circ \Phi_{\epsilon \lambda}^{V}
$$

with $\lambda = 0.19318...$.

A crucial caveat is that, while for Hamiltonian ODE (i.e. $\frac{d}{dt}x = \{x,H\}$), we have $\Phi^{V} = e^{\{\cdot, V\}}$ and $\Phi^{T} = e^{\{\cdot, T\}}$, which are simple to calculate for the standard Hamiltonian, our equation of interest is now

TODO 

so the form of the updates needs to be rederived appropriately. This yields a fairly scary looking update:

$$
    \Phi^T_{\epsilon}(x, u, w) = (x + \epsilon u, u, w)
$$
$$
    \Phi_{\epsilon}^V(x, u, w) = \bigg( x, \,
    \frac{u + (\sinh{\delta}+ \boldsymbol{e} \cdot u (\cosh \delta -1)) \boldsymbol{e} }{\cosh{\delta} + \boldsymbol{e} \cdot u \sinh{\delta}}, \,
    w \,(\cosh \delta + \boldsymbol{e} \cdot u \sinh \delta) \bigg) 
$$

where $\delta = \epsilon \vert \nabla E(x) \vert / d$ and $ \boldsymbol{e} = - \nabla E(x) / \vert \nabla E(x) \vert$.

This integrator is not sympletic
CHECK
Robnik et al. conjecture that this doesn't matter. They observe that the Lagrangian of the Hamiltonian in question is the same as the Hamiltonian itself (
        via todo Legendre
        up to a constant
).
As a result, the action of the Lagrangian is the expected energy. But since the defining feature of the true trajectory is that it minimizes the action, small variations like the numerical trajectory should be very close in action, and thus have almost the same expected energy. Empirically, this line of argument is made plausible by the fact that non-symplectic integrators (e.g. RK4) work fine.

## More abstract formulation

In a [further paper](https://arxiv.org/pdf/2303.18221.pdf), Robnik and Seljak offer a simpler form of the SDE derived by time rescaling of the microcanonical SDE, and proof that it is stationary. This section makes use of tools from differential geometry.

They begin with the simpler case of the following ODE:

$$
\frac{d}{dt}\begin{bmatrix}
x \\
u
\end{bmatrix}
=
\begin{bmatrix}
u \\
(I - uu^T)(−\nabla S(x)/(d − 1))
\end{bmatrix}
$$

where $S(x) = -\log p(x)$, and $p$ is our target distribution.

They first observe that if $|u|=1$:

$$
\frac{d}{dt}(\langle u, u \rangle) = 2(u \cdot \dot u) = = (1 − \langle u, u \rangle)C = 0
$$

This constrains the flow of the ODE to a $2d-1$ dimensional manifold.

The marginal probability under the flow at time $t$ can be represented on the manifold by a top form $\rho(z)dz^1\wedge ...dz^{2d-1}$.

One can phrase the continuity equation as:

TODO
$$

$$

## Implementation

There is a Jax implementation of the code [here](todo). In particular, this is the momentum update:

```python
def update_momentum(d, eps):
    
  def update(u, g):
      g_norm = jnp.sqrt(jnp.sum(jnp.square(g)))
      e = - g / g_norm
      ue = jnp.dot(u, e)
      delta = eps * g_norm / (d-1)
      zeta = jnp.exp(-delta)
      uu = e *(1-zeta)*(1+zeta + ue * (1-zeta)) + 2*zeta* u
      delta_r = delta - jnp.log(2) + jnp.log(1 + ue + (1-ue)*zeta**2)
      return uu/jnp.sqrt(jnp.sum(jnp.square(uu))), delta_r
  
  return update
```

TODO: explain code

## Use with SMC

The speed of the mode finding behavior suggests that MCLMC would work well in concert with annealing, and in particular with Sequential Monte Carlo. The idea is that an initial high temperature (which here means an inverse multiplicative factor on $\log p(x)$) allows for mode exploration, which is subsequently lowered to the target temperature.