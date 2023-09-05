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

## Integrator stability

Symplecting integrators are argued to be long-term stable, becuase they are the exact Hamiltonian flows of the so-called shadow Hamiltonian, which is for a small stepsize usually similar to the original Hamiltonian. They exactly preserve the shadow Hamiltonian, which forces stability. 

The ESH integrator in the rescaled time is not symplectic, but we will here show that the MCHMC Hamiltonian poseses an extrodinary propery, which also suggests stability. 

Hamiltonian dynamics can equivalently be described with Lagrangian dynamics. The Lagrangian is a Legendre transform of the Hamiltonian:

$$
L = {\Pi} \cdot \dot{{x}} - H({x}, {\Pi}) 
$$

which for the MCHMC Hamiltonian yields 

$$ L = d - H $$

or since the Lagrangian formalism is invariant to rescaling and shifting:

$$
L' = H
$$

This is a very special property: **the Lagrangian equals the Hamiltonian**. The Lagrangian dynamics state that the solution flows are the functional extrema of the action, which is the time integral of the Lagrangian, namely

$$S = \int dt L({x}(t), \dot{{x}}(t))$$

In our case, the action equals the expected energy, meaning that the expected energy does not change if we slightly perturb the exact solution. This means that numerical solutions must preserve the expected energy well.

