Betancourt has a [very intriguing paper](https://arxiv.org/abs/1405.3489) on using contact geometry to do annealing in a principled way.

Geometrically, the idea is to start with the $2n$ dimensional symplectic manifold (for $n$ the dimension of the target) typical to Hamiltonian Monte Carlo (i.e. phase space), and then add one more dimension, correpsonding to the inverse temperature $\beta$, in such a way that this new $2n+1$ dimensional manifold has a contact geometry.

This is a kind of odd-dimensional counterpart to symplectic geometry, and the key insight is that one can define a dynamics on this manifold under which a natural stationary distribution (that restricts to the desired target at a given temperature) is preserved.

## Details

Betancourt likes to work at the intersection of differential geometry and measure theory, where differential forms specify measures. The analytic details of this are discussed [here](https://arxiv.org/abs/1410.5110).

Let $\pi$ denote the target distribution, living on a space $Q$ and the targets at different temperatures as

$$
\pi_\beta = \frac{1}{\int_Q (\frac{d\pi}{d\pi_B})^\beta d\pi_B}(\frac{d\pi}{d\pi_B})^\beta\pi_B
$$

where $\pi_B$ is a reference measure (we're interested in interpolating between $\pi$ and $\pi_B$).

Define $\Delta V := -\log \frac{d\pi}{d\pi_B}$.

A canonical distribution on phase space has the form $\pi_H := e^{-H}\Omega$, where $\Omega$ is the symplectic form, living on $T^*Q$, i.e. the cotangent bundle over $Q$.

Betancourt first observes that we can lift from a given target $\pi$, by defining:

$$
\pi_H := \omega^*\pi \wedge \xi = 
$$

where $\omega$ is the projection defining the cotangent bundle, and 


TODO FIX:
    $\xi$ are horizontal forms (forms that annihilate vertical vector fields, i.e. vector fields which map to $0$ under the projection).

(Betancourt, in his [paper on geometry](https://arxiv.org/abs/1410.5110), outlines how $\xi$ amounts to a choice of disintegration, which is what is needed to lift the measure onto the symplectic manifold).

## The contact manifold

Call the (2n+1) dimensional contact manifold $\mathcal{R}$.

The geometry is given by a contact form $\alpha$, and $\Omega_c = \alpha \wedge(d\alpha)^n$, with the properties:

$$\Omega_c \neq 0$$

We get a dynamics via a function $H_c : \mathcal{R} \to \mathbb{R}$. The dynamics is given by the unique $X(H_c)$ satisfying:

$$
\alpha(X(H_c)) = H_c
$$

$$
d\alpha(H_c, \cdot)|_{\{v \in T\mathcal{R}: \alpha(v) = 0\}} = -dH_c|_{\{v \in T\mathcal{R}: \alpha(v) = 0\}}
$$

$H_c$ is *not* preserved, nor is the volume form, but they both are on the set $H_c^{-1}(0)$:

$$
\mathcal{L}_{X(H_c)}H_c|_{H_c^{-1}(0)} = 0
$$

$$
\mathcal{L}_{X(H_c)}\alpha|_{H_c^{-1}(0)} = 0
$$

(proof: todo)

### The contact Hamiltonian

One chooses

$$
H_c = T + V_B + \beta(\gamma)\Delta V + \log Z(\beta(\gamma)) + H_0
$$

This choice has the following key properties:

First, $\pi_{H_c} := e^{-H_c}\Omega_c$ is invariant under the dynamics (which we can tractably simulate with contact integrators!).


Second, the measure takes the form:

$$
\pi_{H_c} := \frac{1}{\int_{T^*Q} e^{-(T+V_B+\Delta V\beta(\gamma)+H_0)}|_{\beta(\gamma)}}e^{-\Delta V+H_0}\alpha\wedge \pi_H
$$

which
TODO understand why VVVV

projects to the desired distribution $\pi_{H(\beta)}$ for each choice of $\beta$.
    todo: define $\pi_{H(\beta)}$

This means that we can sample from $\pi_{H_c}$, and each sample at a value $\beta'$ is a sample from $\pi_{H(\beta)}$.

## Implementation




