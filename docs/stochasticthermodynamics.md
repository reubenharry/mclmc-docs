
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

In a deterministic system, fixing a time interval $\tau$, the phase point at the start of a trajectory uniquely determines that trajectory. If $z$ is the phasepoint denoting a given trajectory, then $M(U_\tau(z))$ is the phase point denoting the reverse trajectory, where $U$ is the time evolution operator and $M$ flips the momentum.

Because of the one-to-one correspondence, we can work with phase points as "coordinates" for trajectories. For $z$ is the phase point corresponding to a trajectory, let $dz$ be a volume in the space of trajectories. In a Hamiltonian system, phase volume is invariant under the dynamics, and also over flipping momentum, so $M(U_\tau(dz))=U_\tau(dz) = e^{\int \nabla \cdot F}dz$ (abusing notation) is the volume of reverse trajectories.
TODO: ??


### Reversibility

For a given $F$, let $\gamma$ be a trajectory (i.e. a function of $[0,\tau] \to \mathcal{M}$), we now define a measure of reversibility as a **function of distributions over trajectories**, namely the *dissipation function* $\Omega$:

$$
\Omega_\tau(F)(x) := \log\frac{P_{F,\tau}(d\gamma)}{P_{F,\tau}(d(r(\gamma)))}
$$

where $P_{F,\tau}$ is a distribution over trajectories according to the field $F$ of length $\tau$ and $r(\gamma)$ is the reverse trajectory to $\gamma$.
<!-- , $x$ is a *trajectory*, **not a phase point**, and $r(x)$ is the reverse trajectory.  -->

When $\Omega_\tau(F)(\gamma) = 0$, $\gamma$ is reversible.

We now work in the "coordinates" given by labeling each trajectory by a phase point $z_0$ at its start, so that

$$
\Omega_\tau(F)(z_0) := \log\frac{P(dz_0)}{P(M(U_{F,\tau}(dz_0)))} =  \log\frac{P(dz_0)}{P(U_{F,\tau}(dz_0))}
$$

$$
= \log\frac{f(z_0)}{f(U_{F,\tau}(z_0))} - \int \nabla \cdot F
$$

where $P$ is the distribution on phase space, and $f$ is the probability density function of the distribution $P$, so that $P(dz) = f(z)dz$. We used a standard result about the determinant of the Jacobian of a map.


## Relationship to MCMC

We begin with the case of the Metropolis-Hastings algorithm with a deterministic Hamiltonian proposal kernel $q_\tau$, where $\tau$ is the time the dynamics runs. In this case, the acceptance probability takes the form:


$$
\alpha(z_\tau , z_0) = \min\left(1, \frac{\pi(z_\tau)q_\tau(z_0|z_\tau)}{\pi(z_0)q_\tau(z_\tau|z_0)}\right)
$$

(We'll work in phase space going forward).

The probability $P_2(z_0,z_\tau) := \pi(z_0)q_\tau(z_\tau|z_0)$ is the probability of starting at a point $z_0$ (drawn from the canonical distribution) and evolving to a point $z_\tau$ after Hamiltonian dynamics for time $\tau$. Since the dynamics is deterministic, we see that this joint probability is the marginal of the probability over trajectories. 

That is, if $\gamma$ is a trajectory with boundaries $z_0, z_\tau$, so that $\delta(\gamma) = (z_0, z_\tau)$ then $P_2(z_0,z_\tau) = \delta^*P(z_0,z_\tau)$ (where $^*$ denotes the pushforward and $\delta$ is the boundary operator).

We see immediately therefore that $\frac{\pi(z_\tau)q_\tau(z_0|z_\tau)}{\pi(z_0)q_\tau(z_\tau|z_0)}$ is nothing other than $e^{-\Omega_\tau}$, a fact we derive by more indirect means in the MAMS paper (the argument with the delta functions).

<!-- One can either describe such a path by a phase point and a time interval, $(z, \tau)$, or by the two boundary points of the path, $(z_0, z_\tau)$. -->

## Using the Crooks theorem to derive the MH acceptance probability

Crooks' theorem (https://shilingliang.com/files/Fluctuation_Theorems.pdf) applies in a setting where we have a time dependent Hamiltonian $H(z, \lambda)$, with $\lambda$ going from $A$ to $B$ over the interval $[0, \tau]$.

There is then a natural notion of the probability of a trajectory, starting from the equilibrium distribution of $H(\cdot, \lambda = A)$ performing work W, where $W(z_0) = H(z_\tau, B) - H(z_0, A) - \int_0^\tau |\frac{dz_\tau}{dz_0}|dt$

$$

$$

<!-- The observation that $\frac{\pi(z_\tau)q_\tau(z_0|z_\tau)}{\pi(z_0)q_\tau(z_\tau|z_0)}=e^{-\Omega_\tau}$ is useful because  -->


Observe that $\frac{\pi(z')q_\tau(z|z')}{\pi(z)q_\tau(z'|z)}$ is equal to $e^{-\Omega_\tau}$, the dissipation function:

$$
\Omega_\tau := \ln (\frac{P(dz, 0)}{P(dz',0)})
$$

where $P$ here is the measure over *trajectories* of the Hamiltonian system. To see why this is, observe first that:

$$
\ln (\frac{P(dz, 0)}{P(dz',0)}) = \ln (\frac{f(dz, 0)}{f(dz',0)}) - \int_0^\tau |\frac{dz'}{dz}|
$$

where $f$ is the density of the measure, and the Jacobian determinant is understood by viewing $z'$ as a function of $z$

### Work and Heat: attempt 1

Let $D(F)_\tau$ be the operator that moves $z_0$ forward to it's new value under the *discretized* dynamics (e.g. leapfrog) obtained from $F$ , and $U^{-1}_\tau$ be the operator that moves $z_\tau$ back to $z_0$ under the reverse *exact* dynamics.

Then given a quantity $H$, such that $p(z)\propto e^{-\beta H}$, consider the time-dependent quantity $H(z,t) := H(D(F)_t(U_t^{-1}z(t)))$.

Observe that 

$$
H(z(t_1),t_1) - H(z(t_0),t_0) = \int \pd{H(z(t),t)}{z} F dt + \int \pd{H(z(t),t)}{t} dt
$$

We view the second term as work $W$, in the sense that it is the change in energy due to the change in $H$, which must be an effect of the world external to the system. This means that the first term is heat $Q$.

In a Hamiltonian system, $H$ as above is the Hamiltonian, and $\int \pd{H}{z} F dt = \int (\dot p \dot q - \dot p \dot q) dt = 0 = T\int \nabla \cdot F = T|\pd{z(z_0)}{z_0}|$.



In a canonical isokinetic system, $H = |u| + V(q)$, so that $\int\pd{H}{z}Fdt = \int\nabla V(x)\dot x + \int \frac{u^T}{|u|}(uu^T - I)\nabla V(x)$

$= \int u\cdot \nabla V(x) + \int|u|u\cdot \nabla V(x) - \int\frac{1}{|u|}u\cdot \nabla V(x) \propto \int u \cdot \nabla V(x) = T\int \nabla \cdot F = T|\pd{z(z_0)}{z_0}|$.

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

### Work and Heat: attempt 2

Suppose we have a Hamiltonian $H_s$ with an auxilliary parameter $\lambda(t)$. In fact, for present purposes, we let $\lambda(t) = t$.

Let $D(F)_\tau$ be the operator that moves $z_0$ forward to it's new value under the *discretized* dynamics (e.g. leapfrog) obtained from $F$ , and $U^{-1}_\tau$ be the operator that moves $z(\tau)$ back to $z_0$ under the reverse *exact* dynamics.

Note that we use $z(t)$ to denote the position $z$ at time $t$ under the exact dynamics.

Then define the time-dependent quantity $H(z,t) := H_s(D(F)_t(U_t^{-1}z(t)))$.

Observe that $\int H(z(t),t)dt = \langle H_s \rangle$, where the expectation is with respect to the stationary distribution of the discretized dynamics.

Further 

$$
d\langle H_s \rangle = \langle dH_s \rangle + \int H_s(z)dp(z)
$$

$$
= \int dH_s(z)p(z)dz + \ldots
$$

$$
= \int dH_s(D_tU_t^{-1}U_tD^{-1}z)p(z)dz + \ldots
$$

$$
= \int dH_s(D_tU_t^{-1}y)p(z)\frac{dy}{dz}dz + \ldots
$$

where $y=U_tD^{-1}z$

Under assumptions that I'm not sure about yet, we have $\langle H \rangle = \int H(z(t),t)dt$ (let's imagine that the time variation in the Hamiltonian amount to some change in $H$ within some bounded range, as indeed is the case for this time dependence, which is simply the difference between the discretized and exact Hamiltonian), so that

$$
d\langle H \rangle = (\int \pd{H(z(t),t)}{z}F dt + \int \pd{H(z(t),t)}{t} dt)dt
$$

But we also have, by definition, that $\langle H \rangle = \int H(z, \lambda)p(z, \lambda)dzd\lambda = \int H(z, t)p(z, t)dzdt$, so that $d\langle H \rangle = \langle dH\rangle + \int H(z,t)dp$, where these two terms are the standard definitions of work and heat respectively, as one forms that are not in the image of the exterior derivative.

Then



## 3



Suppose we are in a Hamiltonian setting, and our dynamics are given by $\dot z = F(z)$, where $F$ is determined in terms of the Hamiltonian $H$.

Let $D(F)_\tau$ be the operator that moves $z_0$ forward to its new value $z(\tau)$ after time $\tau$ under the \emph{discretized} dynamics obtained from $F$ (which we take here without loss of generality to be Velocity Verlet), and let $U^{-1}(F)_\tau$ be the operator that moves $z(\tau)$ back to $z_0$ under the reverse \emph{exact} dynamics.


Then define a time-dependent Hamiltonian $H(z,t) = H(D(F)_t(U_t^{-1}(F)z))$.

Observe that 

$$
H(z(t_1),t_1) - H(z(t_0),t_0) = \int \pd{H(z(t),t)}{z}\cdot F dt + \int \pd{H(z(t),t)}{t} dt 
$$

or 

$$
dH = (\pd{H(z(t),t)}{z}\cdot F + \pd{H(z(t),t)}{t}) dt
$$

Understanding physically that any direct variation of $H$ with time represents an effect of the world external to the system, we view the second term as the physical work $W_P$. We refer to the first term as heat $Q$.


In a Hamiltonian system, $Q = \int \pd{H}{z} F dt = \int (\dot p \cdot\dot x - \dot p \cdot \dot x) dt = 0$. This means that if our target distribution is $p(z)\propto e^{-H(z)}$, and our trajectories are given by Velocity Verlet discretization of the flow of $F$  we have

$$
W = \log \frac{p(z)}{p(z_0)} - \int \nabla \cdot F
= H(D(F)_t z_0) - H(z(0))
= H(z(t), t)- H(z(0),0)
= \int \pd{H(z(t),t)}{t} dt = W_P .
$$

$\nabla \cdot F$ vanishes because the integrator is symplectic. Physically, the failure of reversibility of the discretized dynamics, measured by $W$, is the work on the system induced by the discretization (at unit temperature).

We also have an ergodic average, i.e. that $\langle H \rangle := \int H(z)p_s(z)dz = \int H(z(t), t)dt$, where $p_s$ is the stationary distribution of the discretized dynamics. Then we see that 




We also see that $Q = \int \nabla \cdot B = |\pd{z_B(z_0(s))}{z_0(s)}| = |\pd{z_F(Z_0(t))}{Z_0(t)}|$ by the Liouville formula, so that $\mathbb{E}_{z_0 \sim p}[Q(z_0)] = \int Q(z_0)dz_0$, where $p$ is the microcanonical distribution on Hamiltonian phase space, is the change in volume in phase space induced by the dynamics over a given time interval, which is the change in entropy of the microcanonical distribution. Noting that in statistical mechanics, $Q = \Delta S$ (at unit temperature), we see that for our definition of $Q$, $\langle Q \rangle$ is the thermodynamic heat.

% $d\langle E\rangle = \sum_i E_idp_i + \sum_i dE_ipi := Q + W$, we see that $\langle Q \rangle$ is 

## 4

Suppose we start in a microcanonical distribution, and evolve under Hamiltonian dynamics for a time dt, changing also our distribution.

Then the change in energy is $\log p(z')/p(z)$, where $p$ in the numerator is microcanonical at a different energy shell to $p$ in the denominator, if the integrator changes the energy. Meanwhile, the heat 

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

