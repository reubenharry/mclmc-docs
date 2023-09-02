## Symplectic Numerical Integration

The solution of an [ODE] or SDE is a function $f : \mathbb{R} \to \mathbb{R}^n$.

In the absence of an analytic solution to a given ODE, computing $f(t)$ is not possible.

However, we can transform the original ODE into a difference equation (the discrete equivalent of a differential equation) which we can solve constructively, (i.e. via step by step computation) for a given number of steps.

(This section assumes familiarity with the Hamiltonian formulation of classical physics). The Hamiltonian ODE is 

$$\frac{d}{dt}f = \{f, H\} := L(H)f$$

$L$ is clearly linear, so that for $H=T+V$, $L(H) = L(T)+L(V)$. Furthermore, $L(H)$ is itself a linear operator on functions (again by basic properties of the Lie bracket).

We then have, by the series definition of the exponential:

$$
f(t+\epsilon) = e^{\epsilon L(H)}f(t)
$$

This is true for *any* $\epsilon$, not just small.

The trick to constructing symplectic integrators is to approximate $e^{\epsilon L(H)}f(t)$ to nth order in $\epsilon$, e.g. for $n=2$. 

We can do this by expedient use of the [Baker–Campbell–Hausdorff formula](https://en.wikipedia.org/wiki/Baker%E2%80%93Campbell%E2%80%93Hausdorff_formula), which describes multiples of (potentially non-commutative) exponentials via a commutator series:

$$
e^{\epsilon X}e^{\epsilon Y} = e^{\epsilon X+\epsilon Y+\frac{\epsilon^2}{2}[X,Y] + \mathcal{O}(\epsilon^3)}
$$

In particular, we can show that 

$$ e^{\epsilon O_1/2}e^{\epsilon O_2}e^{\epsilon O_1/2}$$

$$ 
= e^{\epsilon O_1/2 + \epsilon O_2 + \frac{\epsilon^2}{4}(O_1O_2 - O_2O_1)}e^{\epsilon O_1/2} + \mathcal{O}(\epsilon^3)
$$

$$ 
\approx e^{\epsilon O_1/2 + \epsilon O_2 + \frac{\epsilon^2}{4}(O_1O_2 - O_2O_1)}e^{\epsilon O_1/2} 
$$

$$= e^{\epsilon O_1/2 + \epsilon O_2 + \frac{\epsilon^2}{4}(O_1O_2 - O_2O_1) + \epsilon O_1/ 2 + [\epsilon O_1/2 + \epsilon O_2 + \frac{\epsilon^2}{4}(O_1O_2 - O_2O_1), \epsilon O_1/ 2] + \mathcal{O}(\epsilon^3)} 
$$

$$ 
\approx e^{\epsilon(O_1 + O_2)}
$$

Letting $O_1=L(T)$ and $O_2=L(V)$, and expanding $e^{L(T)}$ and $e^{L(V)}$ fully, we see that they are expressable as matrix multiplications (and hence linear maps) on a phase point. For instance:

$$
e^{L(T)}q = q + \{q, T\} + \frac{1}{2}\{\{q,T\}, T\}... 
$$

$$
= q + \{q,p^2/2\} + \frac{1}{2}\{\{q,T\}, T\}... 
$$

$$
= q + p
$$

Working out the other cases gives:

$$
e^{\frac{tL(T)}{2}} = \begin{pmatrix}
1 & \frac{t}{2} \\
0 & 1 
\end{pmatrix}
$$

and 

$$
e^{L(V)} = \begin{pmatrix}
1 & 0 \\
-\frac{\partial S}{\partial q}\frac{t}{q} & 1 
\end{pmatrix}
$$

The determinant of each map is 1, and so is their composition, which gives sympleticity. This is the leapfrog integrator.

There is a literature of clever attempts to find good higher-order integrators, by finding other low order approximations of $e^{L(H)}$. See e.g. [this paper](https://arxiv.org/pdf/hep-lat/0505020.pdf).

