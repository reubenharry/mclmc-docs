Notes:

" In the first phase the Markov chain converges towards the typical set from its initial
position in parameter space while the Markov chain Monte Carlo estimators suffer from
strong biases (Figure 7a). The second phase begins once the Markov chain finds the typical
set and persists through the first sojourn across the typical set. This initial exploration
is extremely effective and the accuracy of Markov chain Monte Carlo estimators rapidly
improves as the bias from the initial samples is eliminated (Figure 7b). The third phase
consists of all subsequent exploration where the Markov chain refines its exploration of the
typical set and the precision of the Markov chain Monte Carlo estimators improves, albeit
at a slower rate (Figure 7c)." - https://arxiv.org/pdf/1701.02434



" The importance of detailed balance is its
role in assuring self-adjointness of the operator that propagates probabilities." - https://arxiv.org/pdf/1402.7107

!!! Prereqs

    This section assumes that the reader is familiar with MCMC, the MH algorithm, and Hamiltonian Monte Carlo.

## Metropolis Hastings

For an introduction to the Metropolis-Hastings (MH) algorithm, see https://ermongroup.github.io/cs228-notes/inference/sampling/. In brief, we construct a step in our MCMC chain by sampling from some $q(z'|z)$ and accepting this sampling with probability $\mathrm{min} \big( 1,  e^{-W(z', z)}\big)$, where

$$
\begin{equation}
    W(z', z) = \frac{q(z \vert z') p(z')}{q(z' \vert z) p(z)}
\end{equation}
$$

    todo: 
    and reversible, i.e. $q(z'|z) = \delta(\phi(z,T)-z')$ for $\phi(\phi(z, T),) = z$, 
For present purposes, we are concerned with $q$ deterministic, and expressed as an ODE:


$$
\frac{d}{dt}\phi(z,t) = F(\phi(z,t))
$$

with $\phi(z,0)=z, \phi(z,T)=z'$. The key insight is that we can express $W$ as:

$$
\begin{equation} 
    W(z(T), z(0)) = -\log \frac{p(z(T))}{p(z(0))} - \int_{0}^T \nabla \cdot F(z(s)) ds
\end{equation}
$$

??? Derivation

First recall that $\delta(x-a)f(x) = \delta(x-a)f(a)$, and $\delta(f(x)) = \sum_i \delta(x-a_i)|\frac{df}{dx}a_i|^{-1}$, where $a_i$ are the roots of $f$. 

In our case, $f(z) = \phi(z)-z'$, so that $\delta(\phi(z)-z) = \delta(z-\phi(z'))|\frac{\partial \phi}{\partial z}(\phi(z'))|^{-1} = \delta(z-\phi(z'))|\frac{\partial \phi}{\partial z}(z)|^{-1}$.

Then

$$
\begin{equation}
    \frac{q(z \vert z')}{q(z' \vert z)} = \frac{\delta(\varphi(z') - z)}{\delta(\varphi(z) - z')} = \frac{\delta(\varphi(z') - z)}{\delta(z - \varphi(z'))} \, | \frac{\partial \varphi}{\partial z}(z) | = | \frac{\partial \varphi}{\partial z}(z) |
\end{equation}
$$

[Jacobi's formula](https://en.wikipedia.org/wiki/Jacobi%27s_formula) for an operator $M$ is:

$$
\frac{d}{dt}\det M = \det(M)tr(M^{-1}\frac{dM}{dt})
$$

which specializes to the Jacobian $D_z\phi(z,T)$ to give:

$$
\frac{d}{dt}\det D_z\phi(z,T) = \det(D_z\phi(z,T))tr(D_z\phi(z,T)^{-1}\frac{dD_z\phi(z,T)}{dt})
$$

$$
= \det(D_z\phi(z,T))tr(D_z\phi(z,T)^{-1}DF)
$$
TODO fix above 

by the basis independence of the trace. Exponentiating, we obtain:

$$
\det D_z\phi(z,T) = \det D_z\phi(z,0)\exp(\int_0^T tr(DF(\phi(z,s))) ds)
$$

Noting that the trace of the Jacobian is the divergence $\nabla \cdot F$, we see that the exponent of the second term of $W$ is $\delta(z-\phi(z'))|\frac{\partial \phi}{\partial z}(z)|^{-1} = \frac{q(z \vert z')}{q(z' \vert z)}$, as desired.



The usefulness of this formula is that it lets us derive both the MH update for HMC, but also other dynamics, such as MCLMC.

## MH for HMC



## MH for MCLMC

For Hamiltonian Monte Carlo, the sympectic nature of the integrator means that TODO

We can also incorporate MH into the MCHMC setting. Here, we need to calculate

The key is that we need to be able to calculate the acceptance ratio

TODO

As it turns out, we can do so