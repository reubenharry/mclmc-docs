## More abstract formulation

In a [further paper](https://arxiv.org/pdf/2303.18221.pdf), Robnik and Seljak offer a simpler form of the SDE derived by time rescaling of the microcanonical SDE, and proof that it is stationary. This section makes use of tools from differential geometry.

They begin with the simpler case of the following ODE:

$$
\frac{d}{dt}\begin{bmatrix}
x \\
u
\end{bmatrix}
=
\begin{bmatrix}
u \\
(I - uu^T)(−\nabla S(x)/(d − 1))
\end{bmatrix}
$$

where $S(x) = -\log p(x)$, and $p$ is our target distribution.

They first observe that if $|u|=1$:

$$
\frac{d}{dt}(\langle u, u \rangle) = 2(u \cdot \dot u) = = (1 − \langle u, u \rangle)C = 0
$$

This constrains the flow of the ODE to a $2d-1$ dimensional manifold.

The marginal probability under the flow at time $t$ can be represented on the manifold by a top form $\rho(z)dz^1\wedge ...dz^{2d-1}$.

The next steps are to express the time evolution as an ODE (the continuity equation, in fact) in the general setting of the manifold, put it into coordinates, and sketch a proof of the invariant of the target distribution under the flow of the ODE.

🚧 Under construction 🚧

One then extends the ODE to an SDE and extends the proof accordingly:

🚧 Under construction 🚧

## Geometric intuition of geodesics

🚧 Under construction 🚧

