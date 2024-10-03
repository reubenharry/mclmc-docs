## Choosing the integrator

One choice is the standard 2nd order leapfrog integrator. See [here](/integrators) for background information on deriving integrators.

The fancier integrator discussed in [Testing and tuning symplectic integrators for Hybrid Monte Carlo algorithm in lattice QCD](/references/#numerical-integrators) turns out to work well in practice.

$$
\mathcal{O} = \mathcal{O}_{\epsilon \lambda}^{1} \circ \mathcal{O}_{\epsilon/2}^{2}\circ \mathcal{O}_{\epsilon (1-2\lambda)}^{1} \circ \mathcal{O}_{\epsilon/2}^{2} \circ \mathcal{O}_{\epsilon \lambda}^{1}
$$

with $\lambda \approx 0.19318$. This is referred to as the Mclachlan integrator in the Blackjax implementation. It is also symplectic.

A crucial caveat is that, while for Hamiltonian ODE (i.e. $\frac{d}{dt}x = \{x,H\}$), we have $\mathcal{O}^{1} = e^{\{\cdot, V\}}$ and $\mathcal{O}^{2} = e^{\{\cdot, T\}}$, which are simple to calculate for the standard Hamiltonian, our equation of interest is now

$$\log \mathcal{O}^{1} \equiv \bold{u} \cdot \partial_{x} 
$$ 

$$
\log \mathcal{O}^{2} \equiv - \frac{1}{d-1} \nabla V(\bold{x})^T (\mathbb I - \bold{u} \bold{u}^T)\partial_{\bold{u}} 
$$


<!-- - 1/d~ u \cdot g(x) \partial_r   -->

so the form of the updates needs to be rederived appropriately[^1]. This yields the surprisingly involved:

$$
    \mathcal{O}^1_{\epsilon}(\bold{x}, \bold{u}) = (\bold{x} + \epsilon \bold{u}, \bold{u})
$$

$$
    \mathcal{O}_{\epsilon}^2(\bold{x}, \bold{u}) = \bigg( \bold{x}, \,
    \frac{\bold{u} + (\sinh{\delta}+ \bold{e} \cdot \bold{u} (\cosh \delta -1))  }{\cosh{\delta} + \bold{e} \cdot \bold{u} \sinh{\delta}}\bold{e})
$$
    
<!-- , \,
    w \,(\cosh \delta + {e} \cdot u \sinh \delta) \bigg  -->

where $\delta = \epsilon \vert \nabla E(x) \vert / (d-1)$ and $\bold{e} = - \nabla E(x) / \vert \nabla E(x) \vert$.[^2]

Derivation

We wish to solve $\dot u = (\log \mathcal{O}_2)p = -(I - uu^T)(\nabla S(x)/(d − 1)) = \frac{1}{d-1}(-\nabla S(x) + u\cdot \nabla S(x)) u := g - \dot h(t)u(t)$. Note that $x$ is fixed here.

We can solve as 

$$
u(t) = \frac{u(0) + s(t)g}{\dot s(t)}
$$

with $s(t) = \int_0^t e^{h(t')} dt'$, so that $\ddot s(t) = \dot h(t) \dot s(t)$, and $\dot u(t) = \frac{\dot s(t)g}{\dot s(t)}+ (u(0) + s(t)g)(-\dot s(t)^{-2})\ddot s(t) = g - u(t)\dot s(t)^{-1}\ddot s(t) = g - \dot h(t)u(t)$.

We now do one more step: *under construction*

<!-- TODO -->

and solve with 

$$
s(t) = \frac{a}{b}(\cosh(t\sqrt b) - 1) + \frac{1}{\sqrt{b}}\sinh(t\sqrt b)
$$

for $a=u(0)\cdot g$ and $b=|g|^2 = |\nabla V(x)|^2/(d-1)$.

Putting this all together, we have

$$
u(t) = \frac{u(0) + (\frac{a}{b}(\cosh(t\sqrt b) - 1) + \frac{1}{\sqrt{b}}\sinh(t\sqrt b))g}{\cosh(t\sqrt{b}) + \frac{a}{\sqrt{b}}\sinh(t\sqrt{b})}
$$

which reduces to the desired result.



TODO

There is also the variable $r$ which is the logarithm of the energy of the original Hamiltonian system (see [here](/tutorial.md)). We can calculate the integrator for $r$ by using $r = \log \vert v' \vert$, so that $\dot r = -u \nabla V(x)/(d-1)$ (here we use the rescaled $V$ so we have $\frac{1}{d-1}$ instead of $\frac{1}{d}$). This gives the update of $r(t) = r(0) + \log \dot s(t) = \Delta E$. That is, we are able to analytically calculate, up to our discrete approximation, the change in the kinetic energy of the original Hamiltonian system.

[^1]: See the appendix of [Hamiltonian Dynamics with Non-Newtonian Momentum for Rapid Sampling](/references/#microcanonical-hamiltonian-monte-carlo) for a derivation.

[^2]: Note that in the original paper, you'll see $d$, not $d-1$. The latter removes the need for the consideration of auxilliary weights; see [here](tutorial.md).

<!-- As a result, the action of the Lagrangian is the expected energy. But since the defining feature of the true trajectory is that it minimizes the action, small variations like the numerical trajectory should be very close in action, and thus have almost the same expected energy. Empirically, this line of argument is made plausible by the fact that non-symplectic integrators (e.g. RK4) work fine. -->

## Integrator stability

Symplectic integrators are argued to be long-term stable, becuase they are the exact Hamiltonian flows of the so-called shadow Hamiltonian, which is for a small stepsize usually similar to the original Hamiltonian. They exactly preserve the shadow Hamiltonian, which forces stability. 

The ESH integrator in the rescaled time is not symplectic, but we will here show that the MCHMC Hamiltonian poseses an extrodinary propery, which also suggests stability. 

Hamiltonian dynamics can equivalently be described with Lagrangian dynamics. The Lagrangian is a Legendre transform of the Hamiltonian:

$$
L = {\Pi} \cdot \dot{{x}} - H({x}, {\Pi}) 
$$


where ${\Pi}$ is to be understood as a function of $\dot{{x}} = \frac{\partial H}{\partial {\Pi}}$.

In the present case ${\dot{x}} = \frac{d \Pi}{\vert {\Pi} \vert}$, so we get

$$ L = d - H $$

or since the Lagrangian formalism is invariant to rescaling and shifting:

$$
L' = H
$$

This is a very special property: **the Lagrangian equals the Hamiltonian**. The Lagrangian dynamics state that the solution flows are the functional extrema of the action, which is the time integral of the Lagrangian, namely

$$S = \int dt L({x}(t), \dot{{x}}(t))$$

In our case, the action equals the expected energy, meaning that the expected energy does not change if we slightly perturb the exact solution. This means that numerical solutions must preserve the expected energy well.

