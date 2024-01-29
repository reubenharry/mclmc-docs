## The isokinetic method

As it turns out, one can derive the MCLMC ODE in a number of ways. One is to start by assuming the constraint that the norm of $p$ is constant with time.

This was first done in the Molecular Dynamics community, under the name of the *isokinetic* method. (The authors of MCLMC became aware of this connection sometime after their original papers).

The idea is simply that we take Hamiltonian dynamics (with the potential energy $S$ being the log likelihood of the target distribution as usual, and $M=1$) and add a constraint that $\frac{d}{dt}(p^Tp)=0$ (by a Lagrange multiplier in the normal fashion):

$$
\frac{d}{dt}\begin{bmatrix}
q \\
p
\end{bmatrix}
= \begin{bmatrix}
p \\
-\nabla S(x) - \xi p
\end{bmatrix}
$$

Then the constraint implies that $0 = p^T\dot p=p^T(-\nabla S(x) - \xi p)$ so that $\xi = -p^T(\nabla S(x))(p^Tp)^{-1}$. Putting this back into the equation:

$$
\frac{d}{dt}\begin{bmatrix}
q \\
p
\end{bmatrix}
= \begin{bmatrix}
p \\
-\nabla S(x) +p^T(\nabla S(q))(p^Tp)^{-1}p
\end{bmatrix}$$

$$
= \begin{bmatrix}
p \\
(I - p(p^T(p^Tp)^{-1})) (-\nabla S(q))
\end{bmatrix}$$

$$
= \begin{bmatrix}
p \\
(I - \mathcal{P}(p)) (-\nabla S(q))
\end{bmatrix} \\
$$

where $\mathcal{P}$ is the projection operator onto the span of $p$ (note its idempotency).

We can derive the stationary distribution (see the [references](references/)) from the ansatz $\rho(p,q) = e^{-w(p,q)}f(p^Tp)$, which after use of the continuity equation and some algebra gives:

$$
\rho(p,q) \propto e^{-\frac{(d-1)S(q)}{||p||^2}}\delta(||p|| - ||p_0||)
$$

From here, we recover the MCLMC dynamics as given in the [MCLMC paper](https://arxiv.org/pdf/2303.18221.pdf) by setting the initial $||p_0||$ to $1$. Further, we rescale $S \mapsto S/(d-1)$.