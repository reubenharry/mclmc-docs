As it turns out, one can derive the MCLMC differential equation from quite general principles.

This was first done in the Molecular Dynamics community, under the name of the *Isokinetic* method. (The authors of MCLMC became aware of this connection sometime after their original papers).

The idea is simply that we take Hamiltonian dynamics (with the potential energy $S$ being the log likelihood of the target distribution as usual) and add a constraint that $\frac{d}{dt}(p^TM^{-1}p)=0$ (by a Lagrange multiplier in the normal fashion):

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

Then the constraint implies that $0 = p^TM^{-1}\dot p=p^TM^{-1}(-\nabla S(x) - \xi p)$ so that $\xi = -p^TM^{-1}(\nabla S(x))(p^TM^{-1}p)^{-1}$. Putting this back into the equation:

$$
\frac{d}{dt}\begin{bmatrix}
q \\
p
\end{bmatrix}
= \begin{bmatrix}
p \\
-\nabla S(x) +p^TM^{-1}(\nabla S(x))(p^TM^{-1}p)^{-1}p
\end{bmatrix}$$

$$
= \begin{bmatrix}
p \\
(I - p(p^TM^{-1}(p^TM^{-1}p)^{-1})) (-\nabla S(x))
\end{bmatrix}$$

$$
= \begin{bmatrix}
p \\
(I - \mathcal{P}(p)) (-\nabla S(x))
\end{bmatrix} \\
$$

where $\mathcal{P}$ is the projection operator onto the span of $p$ (note its idempotency).

From here, we recover the MCLMC dynamics by setting $M=I$, and setting the initial $p^TMp=p^Tp$ to $1$. Further, we rescale $S \mapsto S/(d-1)$.