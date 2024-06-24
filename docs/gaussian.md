It is possible to derive some useful exact results in the Gaussian case, which yield sime intuitions for more complex models. (This is adapted from Jakob Robnik's notes).

We consider the non-stochastic case, without rescaling, so Hamiltonian dynamics with:

$$
H(x,\Pi) = \frac{d}{2}\log\frac{|\Pi|^2}{d} + \mathcal{L}(x)
$$

where as usual $\mathcal{L}(x) \propto -\log p(x)$, and $x, \Pi \in \mathbb{R}^n$. We then find the Lagrangian by a Legendre transform:

$$
L(x,\dot x) = \mathrm{sup}_{{\xi}} \{\dot{x} \cdot {\xi} - H(x, {\xi}) \} = \frac{d}{2} \log \vert \dot{x} \vert^2 - \mathcal{L}(x) + d
$$

??? Derivation

    $$
    \frac{d}{d{\xi_i}} (\dot{x} \cdot {\xi} - H(x, {\xi}) ) := \frac{d}{d{\xi_i}} (\dot{x} \cdot {\xi} - \frac{d}{2}\log\frac{|{\xi}|^2}{d} - \mathcal{L}(x))
    $$

    $$
    = \frac{d}{d{\xi_i}} (\dot{x} \cdot {\xi} - \frac{d}{2}\log|{\xi}^2|) = \dot x - d \frac{\xi_i}{|\xi|^2}
    $$

    $$
    \Rightarrow \xi = \frac{\dot x|\xi|^2}{d}
    $$

    Then note that $|\dot x| = d\frac{|\xi|}{|\xi|^2} = \frac{d}{|\xi|} \Rightarrow |\xi| = \frac{d}{|\dot x|}$, so that $\xi = \frac{\dot x|\frac{d}{|\dot x|}|^2}{d} = d\frac{\dot x}{|\dot x|^2}$.

    Using this value of $\xi$, we find:

    $$
    \mathrm{sup}_{{\xi}} \{\dot{x} \cdot {\xi} - H(x, {\xi}) \} = \dot{x} \cdot d\frac{\dot x}{|\dot x|^2} - \frac{d}{2}\log\frac{|d\frac{\dot x}{|\dot x|^2}|^2}{d} - \mathcal{L}(x)$$

    $$
    = d - \frac{d}{2}\log \frac{d}{|\dot x|^2} - \mathcal{L}(x) = d + \frac{d}{2}\log\frac{|\dot x|^2}{d} - \mathcal{L}(x)
    $$

TODO resolve ^

Since a standard Gaussian has $p(x) \propto e^{-\frac{1}{2}|x|^2}$, $\mathcal{L}(x) = \frac{|x|^2}{2}$. TODO: we ignore the constant right?

"so
the dynamics is confined to the x1-x2 plane. " - general argument?

We see (standard argument from classical mechanics?) that the motion is confined to a plane, and without loss of generality, we let $x_1, x_2$ span that plane.

Then inspecting the Lagrangian, and noting the spherical symmetry of the potential, we see that in polar coordinates $r,\phi$, $\phi$ will be cyclic, so that $\Pi_\phi := \frac{d}{dt}\frac{\partial \mathcal{L}}{\partial \dot\phi} = \frac{\partial \mathcal{L}}{\partial \phi} = 0$. This gives us a constant of motion, in addition to the energy.