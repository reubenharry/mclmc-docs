
$\newcommand{\pd}[2]{\frac{\partial #1}{\partial #2}}$

$\newcommand{\R}{\mathbb{R}}$
$\newcommand{\RR}{\mathbb{R}}$
$\newcommand{\C}{\mathbb{C}}$
$\newcommand{\N}{\mathbb{N}}$


Recall that

$$
\det(\pd{z(z_0)}{z_0}(t)) = \int_0^t Tr(DF(z(z_0)(s)))ds
$$

where $F$ is the vector field of the dynamics, $z$ is the path of the dynamics, viewed as a function of the initial condition $z_0 : \mathcal{M}$ for the configuration manifold $\mathcal{M}$, and $DF$ is the Jacobian of $F$.

In a deterministic system, fixing a time interval $\tau$, a phase point is isomorphic to a trajectory. If $z$ is the phasepoint denoting a given trajectory, then $M(U(z))$ is the phase point denoting the reverse trajectory, where $U$ is the time evolution operator and $M$ flips the momentum.

For $z$ is the phase point corresponding to a trajectory, let $dz$ be a volume in the space of trajectories. In a Hamiltonian system, phase volume is invariant under the dynamics, and also over flipping momentum, so $M(U_\tau(dz))=U_\tau(dz) = e^{\int \nabla \cdot F}dz$ (abusing notation) is the volume of reverse trajectories.


### Reversibility

For a given $F$, let $x$ be a trajectory (i.e. a function of $[0,\tau] \to \mathcal{M}$), we now define a measure of reversibility as a **function of distributions over trajectories**, namely the *dissipation function* $\Omega$:

$$
\Omega_\tau(F)(x) := \log\frac{P_F(dx)}{P_F(d(r(x)))}
$$

where $P_F$ is a distribution over trajectories, $x$ is a *trajectory*, **not a phase point**, and $r(x)$ is the reverse trajectory. 

When $\Omega_\tau(F)(x) = 0$, $x$ is reversible.

We now work in the "coordinates" given by labeling each trajectory by a phase point $z_0$, so that

$$
\Omega_\tau(F)(z_0) := \log\frac{P(dz_0)}{P(M(U_{F,\tau}(dz_0)))} =  \log\frac{P(dz_0)}{P(U_{F,\tau}(dz_0))}
$$

$$
= \log\frac{f(z_0)}{f(U_{F,\tau}(z_0))} - \int \nabla \cdot F
$$

where $P$ is the distribution on phase space, and $f$ is the probability density function of the distribution $P$, so that $P(dz) = f(z)dz$. We used a standard result about the determinant of the Jacobian of a map.

<!-- ??? FT

    Noting that $\Omega_\tau(F)(z) = -\Omega_\tau(F)(M(U_{F,\tau}(z)))$, we see that:

    $$
    \frac{P(\Omega= A)}{P(\Omega = -A)} = \frac{\int dz_0 \delta(\Omega_\tau(F)(z_0) - A)f(z_0)dz_0}{\int dz'_0 \delta(\Omega_\tau(F)(z'_0) + A)f(z'_0)dz'_0}
    $$

    $$
    = \frac{\int dz_0 \delta(\Omega_\tau(F)(z_0) - A)f(z_0)dz_0}{\int dz'_0 \delta(-\Omega_\tau(F)((M\circ U_{F,\tau})z_0) + A)f(z_0)dz_0}
    $$

    = TODO... delta function trick, cancellations...

    $$
    = e^A
    $$

    Further, $\langle e^{-\Omega_\tau(F) }\rangle = 1$, so that by Jensen's equality $0 = \log 1 = \log \langle e^{-\Omega_\tau(F) }\rangle \geq \langle \log e^{-\Omega_\tau(F) }\rangle = \langle {-\Omega_\tau(F) }\rangle$.

    This is a dynamical expression related to the second law of thermodynamics: in expectation over starting points, a forward path is more probable than a backwards one. -->

## Hamilton's equations




## Relationship to MCMC

Reversibility, as defined above, is a relationship between the distribution over paths in phase space, and the reverse paths. 

If one only cares about the boundaries (the start and end points) then one can marginalize, to obtain a density over the two boundary points:

$$
p(a,b) = p(a)q(b | a)
$$

Pushed forward to this space, reversibility becomes the condition:

$$
p(z_0)q(z_\tau|z_0) = p(z_\tau)q(z_0 | z_\tau)
$$

recognizable from MCMC.

If the dynamics are deterministic, no information is lost by marginalization, but in general the dynamics need not be.

We see therefore that $e^{-\Omega}$ is the MH correction term.

<!-- One can either describe such a path by a phase point and a time interval, $(z, \tau)$, or by the two boundary points of the path, $(z_0, z_\tau)$. -->

### Work and Heat

Let $D(F)_\tau$ be the operator that moves $z_0$ forward to it's new value under the *discretized* dynamics (e.g. leapfrog) obtained from $F$ , and $U^{-1}_\tau$ be the operator that moves $z_\tau$ back to $z_0$ under the reverse *exact* dynamics.

Then given a quantity $H$, such that $p(z)\propto e^{-\beta H}$, consider the time-dependent quantity $H(z,t) := H(D(F)_t(U_t^{-1}z(t)))$.

Observe that 

$$
H(z(t_1),t_1) - H(z(t_0),t_0) = \int \pd{H(z(t),t)}{z} F dt + \int \pd{H(z(t),t)}{t} dt
$$

We view the second term as work $W$, in the sense that it is the change in energy due to the change in $H$, which must be an effect of the world external to the system. This means that the first term is heat $Q$.

In a Hamiltonian system, $H$ as above is the Hamiltonian, and $\int \pd{H}{z} F dt = \int (\dot p \dot q - \dot p \dot q) dt = 0 = T\int \nabla \cdot F = T|\pd{z(z_0)}{z_0}|$.



In a canonical isokinetic system, $H = |u| + V(q)$, so that $\int\pd{H}{z}Fdt = \int\nabla V(x)\dot x + \int \frac{u^T}{|u|}(uu^T - I)\nabla V(x) = \int u\cdot \nabla V(x) + \int|u|u\cdot \nabla V(x) - \int\frac{1}{|u|}u\cdot \nabla V(x) \propto \int u \cdot \nabla V(x) = T\int \nabla \cdot F = T|\pd{z(z_0)}{z_0}|$.

(As usual, I'm just ignoring all the factors of $d$ which need to be added for correctness).

But inspecting the definition of the dissipation function $\Omega$ when $p$ is a canonical density, we see that it is nothing but $W/T$:

$$
\log \frac{p(z)}{p(z_0)} - \int \nabla \cdot F
$$

$$
= \beta (H(z(t), t)- H(z(t),0)) - \int \nabla \cdot F
$$

$$
= \beta (H(D_\tau z_0) - H(z(0))) - \int \nabla \cdot F = \beta W
$$

Physically, we can understand this as meaning the following: the failure of reversibility of the discretized dynamics is the work induced by the discretization (at unit temperature).

$Q = T|\pd{z(z_0)}{z_0}|$ is heat exchange in the sense that if you were to integrate it over a volume of phase space, you would get the volume change, and therefore the entropy change. Put another way, $\langle Q/T \rangle = \Delta S$.

Presumably we can do clever things by appealing to the Crooks fluctuation theorem, which should apply here.


!!! Remarks

    Recall that

    $$
    d\langle E \rangle = \sum_i dE_ip(E_i) + E_idp(E_i) := w + q
    $$

    where I have written $w$ and $q$ instead of the conventional, and confusing $\bar dw$ and $\bar dq$

    For the canonical distribution, the entropy is:

    $$
    S[p] = -\sum_x p(x)\log\frac{1}{Z}e^{-\beta E(x)} = \log Z + \beta\langle E \rangle
    $$

    Further, we can calculate the exterior derivative of $S$:

    $$
    dS = d(\log Z) + d(\beta \langle E \rangle) = -\frac{1}{Z}\sum_a e^{-\beta E_a}(E_ad\beta + \beta dE_a) + \langle E\rangle d\beta + \beta d\langle E\rangle
    $$

    $$ = \beta(d\langle E\rangle - \langle dE \rangle) = \beta q = \frac{q}{T}
    $$




<!-- ## The Crooks Fluctuation Theorem

We now consider a theorem more specialized to Hamiltonian dynamics with a time dependence:

$$
H(z,t) = T(p(t)) + V(x(t),\lambda(t))
$$

Then

$$
H(z, \tau) - H(z, 0) = \int_0^\tau \pd{H}{t}dt = \int_0^\tau \pd{H}{\lambda}d\lambda + \int_0^\tau \pd{H}{z}\dot z dt
$$ -->