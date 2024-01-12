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

# Microcanonical Monte Carlo Tutorial

!!! Note 
    See the lefthand sidebar for further tutorials, and the righthand sidebar for the contents of this page.


??? Prerequisites

    - some familiarity with probability and Bayesian inference
    - general knowledge of Markov chain Monte Carlo methods
    - basic understanding of linear ordinary differential equations (in particular, the Hamiltonian ODE of classical physics)

## Overview

Here is a perspective on Markov Chain Monte Carlo algorithms in general which will be useful with regard to the present algorithm. 

The goal is to sample from a distribution $p(x) \propto e^{-E(x)}$, where $E$ is known. To this end, we do the following:

1. Specify some discrete, preferably Markovian, stochastic process that has as a fixed point under its flow a distribution $q$, such that $p$ has some simple relationship to $q$.

2. Run the process forward to generate samples from $q$, under the assumption of ergodicity, and transform into samples from $p$.

When $E$ is (computably) differentiable, we can use Hamiltonian Monte Carlo (HMC), which follows the above steps with some detail added:

1. Choose a Hamiltonian $H$, such that for $q(x,z) \propto e^{-bH(x,z)}$ (known in statistical mechanics as the *canonical distribution*), we have $p(x) = \int dz q(x,z)$. Any classical $H$ of the form $H= T(z) + E(x)$ (for $z$ momentum and $x$ position) will do. 
2. The Hamiltonian ODE implies a volume preserving deterministic process (flow), call it $f$, which when discretized by a symplectic (volume preserving) integrator $S$, gives a discrete process $S(f)$.
3. Add occasional resampling of momentum for reasons of ergodicity, to yield the desired discrete stochastic process.
4. Obtain samples from $q$, which are (x,z) pairs, and throw away the $z$ part, to get samples from $p$.

This in turn can be generalized by moving from the Hamiltonian ODE to any SDE that has $q$ as its fixed point distribution. [This paper](https://proceedings.neurips.cc/paper_files/paper/2015/file/9a4400501febb2a95e79248486a5f6d3-Paper.pdf) gives a recipe for constructing such all such SDEs. Specific choices of SDE yield familiar algorithms, in particular Hamiltonian Monte Carlo (HMC), Langevin Monte Carlo (LMC) and the various Riemannian and/or stochastic gradient varieties of those.

One has the option of further embellishing the discrete process with Metropolis-Hastings (MH) steps. Calculating the MH ratio only requires the transition probability ratio, since the probability of $p$ does not change (since the ODE is volume preserving by Liouville's theorem, and so is the discrete process (thanks to the sympletic integrator). One has to be slightly careful about the transition probabilities; see section 5.2 of [this introduction](https://arxiv.org/pdf/1701.02434.pdf). 

If one doesn't adjust, the samples from $q$ will be at least slightly biased, a bias which can be reduced by limiting step size of the integrator. The same goes for LMC, where the adjusted verision is known, sensibly, as the Metropolis Adjusted Langevin Algorithm (MALA).

This would seem to be the final word in this problem. Not so.

What is proposed in [this paper](https://arxiv.org/pdf/2212.08549.pdf) is to instead consider a distribution $q(z,x) \propto \delta(H(x,z)-E)$ (known in statistical mechanics as the micocanonical ensemble), with $H$ chosen carefully, such that the marginal over x, i.e. $q(x)=\int dz q(x,z)$, is equal to $p(x)$ as before. One can then consider both the analogs of HMC and LMC in this *microcanonical* setting.

[This paper](https://arxiv.org/pdf/2212.08549.pdf) and [this paper](https://arxiv.org/pdf/2303.18221.pdf) establish the requisite properties of these inference algorithms, consider appropriate choices of $H$ and kinetic energy to ensure the right marginal, sketch a proof that the stationary distribution is $q$ and show that it works very well both on toy problems and simple real ones.


## Choosing H

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
p(\vx) \propto \int_{\mathbb{R}^d} \delta(H(\vx, v) - E) dv = 
$$



$$
\begin{align*}
    &= (1/Z) \int { d\vv} ~{ \delta\left(E(\vx) + d/2 \log v^2/d - c\right)} \\
    &= (1/Z) \int { J( \phi)~d \phi~ \rho^{d-1}~d\rho} ~ { \delta\left(E(\vx) + d \log \rho - c -d/2 \log d \right)} \\
    &= (1/Z) \int { J( \phi)~d \phi~ \rho^{d-1}~d\rho} ~{ \rho/d ~ \delta(\rho - e^{-E(\vx)/d + c/d + \half \log d})} = {e^{-E(\vx)} / Z} ~~~~
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

More esoteric choices like a relavitistic Hamiltonian (which is non-separable) are considered in the paper too.

<!-- To see this, note that the integral can be converted into (hyper)spherical coordinates. The angular part of the integral can be evaluated separately (we assume a rotationally symmetric $H$), giving the numerator. -->

<!-- The denominator is the integral $\int \delta(H(\mu))d\mu$, which is a standard result. -->



The Hamiltonian yields a deterministic process, but doesn't give an ergodicity guarantee. [Robnik et al.](https://arxiv.org/pdf/2212.08549.pdf) proposes completely changing the momentum direction after every $L$ steps, which preserve the norm, and hence the energy, but make the process ergodic.

They subsequently offer a closely related algorithm where the momentum is changed partially at every step.

## Time rescaling

With some thought, one can convert the MCHMC ODE with the logarithmic kinetic energy to the following form:

$$
\frac{d}{ds}\begin{bmatrix}
x \\
v
\end{bmatrix}
=
\begin{bmatrix}
v \\
-P(v)\nabla S(x)/(d-1) \\
\end{bmatrix}
$$

Here is how we get this.

We begin by choosing $T(v) = \frac{d}{2}\log(|v|^2/d)$. Hamilton's equations give:

$$
\frac{d}{dt}\begin{bmatrix}
x \\
v
\end{bmatrix}
=
\begin{bmatrix}
\frac{v}{|v|}\frac{1}{w} \\
-\nabla V(x)
\end{bmatrix}
$$

where $w(t) = |v(t)|/d$.

Numerical integration of the SDE requires a small step size because when $|v|$ is small, i.e. when the trajectory does a u-turn, $\frac{d}{dt}x$ becomes large.

To ameliorate the problem, one can consider a new flow $x \circ s$, where $s : \mathbb{R} \to \mathbb{R}$ is defined so that $\frac{d}{dt}s(t) = w(t)$.

Then[^1] with $u(t) = v(t)/|v(t)|$:

[^1]: Note that the original paper defines $s$ and $t$ switched.

$$
\frac{d}{dt}\begin{bmatrix}
x \\
u \\
w
\end{bmatrix}
=
\begin{bmatrix}
u \\
-P(u)(\nabla V(x)/d) \\
-w u \cdot \nabla V(x) /d
\end{bmatrix}
$$

with $\rho_\infty(x)w_\infty(x) \propto e^{-V(x)}$.

We then note:

$$
H(x,u) =  \frac{d}{2}\log(|u|^2/d) + V(x) \\
\Rightarrow w(x) = e^{(E-V(x))/d}
$$

so that

$$
\rho_\infty(x) \propto e^{(V(x))/d}e^{-V(x)} = e^{(1/d -1)V(x)} = e^{-((d-1)/d)V(x)}
$$

We then simply rescale $S$ by $d/(d-1)$ to obtain:

$$
S'(x) = \frac{d}{d-1} V(x)
$$

so that rederiving the equations from $S'$, we obtain:

$$
\frac{d}{ds}\begin{bmatrix}
x \\
u
\end{bmatrix}
=
\begin{bmatrix}
u \\
-P(u)\nabla S(x)/(d-1) \\
\end{bmatrix}
$$

Since now $\rho_\infty(x) = e^{-S(x)}$, we no longer need to keep track of the weights.

Moreover, we can study this ODE in its own right, and this is the approach taken in the [Microcanonical Langevin Monte Carlo](https://arxiv.org/pdf/2303.18221.pdf) paper.

It is no longer symplectic, and no longer has any (direct) relationship to a Hamiltonian.

## Discretization

We must convert our differential equation into a discrete process

$$
step_\epsilon(x,u) \mapsto (f_\epsilon(x,u), g_\epsilon(x,u))
$$

where $\epsilon$, which has dimensions of time, is the amount forward in time that the step moves.

The price of discretization is that our dynamics is only approximately equal to the ODE, so $step_\epsilon(x,u) \approx \phi_\epsilon(x,u)$. As $\epsilon \to 0$, they become equal, but the cost of running the algorithm goes up.

See the [section on tuning](/theorydocs/docs/tuning.md) and the [section on the integrator](/theorydocs/docs/choice_of_integrator.md) for more information.

## Stochasticity

The paper [Hamiltonian Dynamics with Non-Newtonian Momentum for Rapid Sampling](https://arxiv.org/pdf/2111.02434.pdf) proposes roughly the above equation, but this does not result in ergodicity. That is, while the target distribution is stationary, the flow may not converge to it.

As a remedy, the paper [Microcanonical Hamiltonian Monte Carlo](https://arxiv.org/pdf/2212.08549.pdf) proposes to add either full stochastic momentum resampling every $n$ steps, or partial momentum changes every step.

Focusing on the latter, we do an update 

$$
z \sim \mathcal{N}(0,1) \\
\Phi^O_{\epsilon, L}(x,u) = (x, n(u+z\sqrt{d^{-1}(e^{2\frac{\epsilon}{L}}-1)}))
$$

where $n(u) = \frac{u}{|u|}$, and $L$ is a parameter of our choosing with dimension of time (see the [section on tuning](/theorydocs/docs/tuning.md)). The reason for this curious looking expression is that it can be shown that 

$$
\langle step_\epsilon^n(u) \cdot u \rangle = e^{-\frac{\epsilon n}{L}}
$$

which means that the correlation between $u$ and the momentum at a time $n\cdot\epsilon$ later decays at a rate controlled by $L$. As $L$ increases, the time to decorrelate increases, so think of $L$ as the *decoherence time* of the momentum.

These stochastic jumps are applied to the discrete random walk obtained from the ODE. However, it is natural to ask if one can formulate a *stochastic differential equation* (SDE) for which the discretization results in this same random walk. The benefit is that one can then analyze the properties of the SDE using more abstract tools. That is the topic of [Microcanonical Langevin Monte Carlo](https://arxiv.org/pdf/2303.18221.pdf). 




<!-- This takes us out of the symplectic (Hamiltonian) setting, and indeed the eventual resulting SDE will have a stationary distribution that does not resemble -->

<!-- The consequence of this is that the integrator must be rederived, using the standard  -->


