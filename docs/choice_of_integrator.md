## Choosing the integrator

One choice is the standard 2nd order leapfrog integrator. See [here](/integrators) for background information on deriving integrators.

The fancier minimal norm integrator discussed in [Testing and tuning symplectic integrators for Hybrid Monte Carlo algorithm in lattice QCD](/references/#numerical-integrators) turns out to work well in practice.

$$
\mathcal{O} = \mathcal{O}_{\epsilon \lambda}^{1} \circ \mathcal{O}_{\epsilon/2}^{2}\circ \mathcal{O}_{\epsilon (1-2\lambda)}^{1} \circ \mathcal{O}_{\epsilon/2}^{2} \circ \mathcal{O}_{\epsilon \lambda}^{1}
$$

with $\lambda = 0.19318...$.

A crucial caveat is that, while for Hamiltonian ODE (i.e. $\frac{d}{dt}x = \{x,H\}$), we have $\mathcal{O}^{1} = e^{\{\cdot, V\}}$ and $\mathcal{O}^{2} = e^{\{\cdot, T\}}$, which are simple to calculate for the standard Hamiltonian, our equation of interest is now

$$\log \mathcal{O}^{1} \equiv u \cdot \partial_{x} 
$$ 

$$
\log \mathcal{O}^{2} \equiv - 1/d~ g(x)^T (\mathbb I - u u^T)\partial_{u} - 1/d~ u \cdot g(x) \partial_r  
$$

so the form of the updates needs to be rederived appropriately[^1]. This yields the surprisingly involved:

$$
    \mathcal{O}^1_{\epsilon}(x, u, w) = (x + \epsilon u, u, w)
$$

$$
    \mathcal{O}_{\epsilon}^2(x, u, w) = \bigg( x, \,
    \frac{u + (\sinh{\delta}+ {e} \cdot u (\cosh \delta -1)) {e} }{\cosh{\delta} + {e} \cdot u \sinh{\delta}}, \,
    w \,(\cosh \delta + {e} \cdot u \sinh \delta) \bigg) 
$$

where $\delta = \epsilon \vert \nabla E(x) \vert / (d-1)$ and ${e} = - \nabla E(x) / \vert \nabla E(x) \vert$.[^2]

[^1]: See the appendix of [Hamiltonian Dynamics with Non-Newtonian Momentum for Rapid Sampling](/references/#microcanonical-hamiltonian-monte-carlo) for a derivation.

[^2]: Note that in the original paper, you'll see $d$, not $d-1$. The latter removes the need for the consideration of auxilliary weights; derivation is 🚧 under construction 🚧.

<!-- Robnik et al. conjecture that this doesn't matter. They observe that the Lagrangian of the Hamiltonian in question is the same as the Hamiltonian itself (
        via todo Legendre
        up to a constant
). -->
<!-- As a result, the action of the Lagrangian is the expected energy. But since the defining feature of the true trajectory is that it minimizes the action, small variations like the numerical trajectory should be very close in action, and thus have almost the same expected energy. Empirically, this line of argument is made plausible by the fact that non-symplectic integrators (e.g. RK4) work fine. -->

