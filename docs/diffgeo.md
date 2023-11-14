## Proving stationarity

The MCHMC sampler rests on the idea that for a particular differential equation we define, a given distribution $\rho_\infty$ is stationary, and that $\rho_\infty$ has our target distribution $p$ as a marginal.

This page outlines how we prove this using the tools of differential geometry. The reason for this mathematically heavy-weight approach is that it will make further proofs easier, in particular:

- that for a stochastic differential equation (SDE) which extends the ODE, $\rho_\infty$ remains stationary. This is described [here](sde.md)
- that the SDE is ergodic

!!! Note 
    
    This section makes use of tools from differential geometry, particularly Riemannian metrics and differential forms. Broad familiarity with these tools is assumed. See these notes for an introduction to [differential forms](http://www.damtp.cam.ac.uk/user/tong/gr/two.pdf) and [curvature](http://www.damtp.cam.ac.uk/user/tong/gr/three.pdf) on a manifold.

    See this [supplementary page](/geometry) for a quick reference sheet of definitions that are used below without proof.

## The details

We begin with the case of the following ODE:

$$
\frac{d}{dt}\begin{bmatrix}
x \\
u
\end{bmatrix}
=
\begin{bmatrix}
u \\
-(I - uu^T)(\nabla S(x)/(d − 1))
\end{bmatrix}
$$

where $(x,u)$ describes a point on a manifold $\mathcal{M}$ and $S(x) = -\log p(x)$, for $p$ our target distribution, i.e. the distribution of our choosing that we are eventually interested in sampling from.

!!! Note 
    
    Going forward, we'll write $(I - uu^T)(\nabla S(x)/(d − 1)) = P(u)f(x)$, to emphasize that $\dot u$ is described by a projection of a function of $x$.
    <!-- onto a hypersphere, in the sense that the $u$ component of $f(x)$ is set to $0$. -->

As with any ODE, we obtain a flow $\phi : \mathbb{R} \to (\mathcal{M} \to \mathcal{M})$, i.e. the group action that takes $t \in \mathbb{R}$ to time evolutions of $\mathcal{M}$. That is, $\phi(s)(z(t)) = z(t+s)$.

Distributions on the manifold are top forms $\rho$, and $\phi$ also acts on these, by a pushforward $\phi_*$.

We have the following PDE that corresponds to this flow on distributions (known as a continuity equation), where $B$ is the vector field, i.e. the map of type $(z : \mathcal{M}) \to T_z(\mathcal{M})$, from points on the manifold to points in the corresponding tangent space. 

$$
\begin{align} 
    \frac{d}{dt} \widehat{\rho}_t &= \frac{d}{ds} \big( \varphi_{s *} \widehat{\rho}_t \big) \vert_{s = 0} = \frac{d}{ds} \big( \varphi_{-s}^* \widehat{\rho}_t \big) \vert_{s = 0}  \\ 
    &\equiv - \mathcal{L}_{{B}} \widehat{\rho}_t = - \big( \mathrm{div}_{\widehat{\rho}_t} {B} \big) \widehat{\rho}_t 
\end{align}
$$


??? Derivation

    The first equation is true by definition. The second is true by the definition of the pushforward and pullback for a diffeomorphism (which are invertible, as indeed we know $\varphi$ to be).

    The third equation is true since this is the precise definition of the Lie derivative (changing variables to obtain the negation), and likewise for the fourth, by the definition of the divergence (of a top form $\eta$) as $(\mathit{div}_\eta X) := d\circ i_X$ and since  $d(i_X\eta) = (d \circ i_X + i_X \circ d)\eta = L_X\eta$ (where the first equation follows since a top form $\eta$ is closed, and the second by a standard property of the Lie derivative).





Our goal is to supply a distribution $\rho_\infty$ and to sketch a proof that $\frac{d}{dt} \widehat{\rho}_\infty=0$, i.e. that it is invariant under the action of $\phi$. At this point, we need to choose good coordinates and do some calculations.

The first key insight is that if $|u|=1$, then:

$$
\frac{d}{dt}(\langle u, u \rangle) = 2(u \cdot \dot u) = (1 − \langle u, u \rangle)C = 0
$$

In other words, the norm $|u|$ is constant, so the manifold $\mathcal{M}$ is $2d-1$ dimensional, where $d$ in the dimension of $x$ in the naive parametrization.

This means that it is natural to work with (hyper)spherical coordinates $\theta(u)$, in place of $u$. We label a single point $z$, with $z^1\cdots z^d$ corresponding to $x$ and the rest to $\theta$.

The next step is to propose the form of our stationary distribution:

$$
\rho_{\infty}(z) \propto e^{-S(x)} \sqrt{g(\theta)}dz^1\wedge ...dz^{2d-1}
$$

Evidently, marginalizing out $\theta$ gives $p\propto e^{-S(x)}$, as desired.


## The proof


The only properties of the spherical coordinations we need here are that:

$$
u_1 = \cos \vartheta^1
$$

and that

$$
    \det g_{\mu \nu}^{1/2} = \det (\frac{\partial u_k}{\partial \vartheta^{\mu}} \frac{\partial u_k}{\partial \vartheta^{{\nu}}})^{1/2} = \Pi_{k=1}^{d-2}(\sin \vartheta^k)^{d-1-k}
    % = \mathrm{Diag}[1,\, \sin^2(\vartheta^1),\, \sin^2(\vartheta^1) \sin^2(\vartheta^2),\, ...]_{i j},
$$


We now put the continuity equation in our coordinates using [this formula](/geometry/#divergence), as:

$$
\frac{d}{dt}\rho_\infty = 
- \big( \mathrm{div}_{{\rho}_\infty} {B} \big) {\rho}_\infty = \frac{1}{e^{-S(x)} \sqrt{g(\theta)}}((e^{-S(x)} \sqrt{g(\theta)})B^i)_{,i}
$$

Proceeding further requires $B$ in coordinates, which is:

$$
{B}(z) = 
u_i(\theta) \partial_i + g^{\mu\nu}(\theta)\frac{\partial u_i}{\partial \theta_\nu}P_{ij}(\theta) f_j(x) \partial_\mu.
$$


This allows us to split up the expression, into the Euclidean and spherical parts, to obtain:

$$
    \dot{\rho}_{\infty} = - \frac{1}{\sqrt{g(\theta)}e^{-S(x)}} \frac{\partial}{\partial x_i} \big( \sqrt{g(\theta)}e^{-S(x)} B^{i} \big)\rho_\infty
$$

$$
    - \frac{1}{\sqrt{g(\theta)}e^{-S(x)}} \frac{\partial}{\partial\theta_\mu} \big( \sqrt{g(\theta)}e^{-S(x)} B^{\mu} \big) \rho_{\infty} 
$$

$$ 
 = - \frac{1}{\rho_\infty} \frac{\partial}{\partial x_i} \big( \sqrt{g(\theta)}e^{-S(x)} u^i(\theta) \big)\rho_\infty   - \frac{1}{\sqrt{g(\theta)}} \frac{\partial}{\partial\theta_\mu} \big( \sqrt{g(\theta)} B^{\mu} \big) \rho_{\infty} 
$$



$$
    = - u \cdot \partial_{x} \rho_{\infty} - \frac{1}{\sqrt{g}} \partial_{\mu} \big( \sqrt{g} B^{\mu} \big) \rho_{\infty} 
$$



The first term is $u \cdot \partial_{x} \rho_{\infty} = (d -1) u \cdot {f} \rho_{\infty}$ simply by the definition of $f$, so it remains to show that the second term is the same.

<!-- We take advantage of the coordinates to have a different set of $\theta$ coordinates for each

SHOW:

$$
B_\mu = \partial_\mu(u\cdot f(x)) = \partial_\mu (u \cdot \delta_{1i}|f|) = |f|\partial_\mu(u_1) = -|f|\sin(\theta^1)\delta_{1\mu}
$$

todo

This concludes the proof. -->


🚧 Under construction 🚧

