## Numerical Integration

The solution of an ODE is a function $f : \mathbb{R} \to \mathbb{R}^n$. The solution of an SDE is a distribution over such functions.

In the absence of an analytic solution to a given ODE, computing $f(t)$ is not possible. However, we can transform the original ODE into a difference equation (the discrete equivalent of a differential equation) which we can solve by computation for a given number of steps.

### For linear operators

Suppose we have an ODE of the form:

$$
\frac{d}{dt}f = \mathcal{O}f
$$

Understand $\mathcal{O}$ as some operator *on the space of functions*. If $\mathcal{O}$ is a linear operator, then the usual approach from linear algebra applies, namely:

$$
f(t+\epsilon) = (e^{\epsilon \mathcal{O}}f)(t)
$$

This is true for *any* $\epsilon$, not just small. The approach is to then approximate $e^{\epsilon \mathcal{O}}$ to some finite order, find the closed form, and then discretize time in units of $\epsilon$.

### Example: leapfrog integrator

As a concrete example, consider $\mathcal{O} = \mathcal{O}_1 + \mathcal{O}_2$.

We can obtain a second order approximation by expedient use of the [Baker–Campbell–Hausdorff formula](https://en.wikipedia.org/wiki/Baker%E2%80%93Campbell%E2%80%93Hausdorff_formula), which describes multiples of (potentially non-commutative) exponentials via a commutator series:

$$
e^{\epsilon X}e^{\epsilon Y} = e^{\epsilon X+\epsilon Y+\frac{\epsilon^2}{2}[X,Y] + O(\epsilon^3)}
$$

In particular, we can show that 

$$ e^{\epsilon \mathcal{O}_1/2}e^{\epsilon \mathcal{O}_2}e^{\epsilon \mathcal{O}_1/2}$$

$$ 
= e^{\epsilon \mathcal{O}_1/2 + \epsilon \mathcal{O}_2 + \frac{\epsilon^2}{4}(\mathcal{O}_1\mathcal{O}_2 - \mathcal{O}_2\mathcal{O}_1)}e^{\epsilon \mathcal{O}_1/2} + \mathcal{O}(\epsilon^3)
$$

$$ 
\approx e^{\epsilon \mathcal{O}_1/2 + \epsilon \mathcal{O}_2 + \frac{\epsilon^2}{4}(\mathcal{O}_1\mathcal{O}_2 - \mathcal{O}_2\mathcal{O}_1)}e^{\epsilon \mathcal{O}_1/2} 
$$

$$= e^{\epsilon \mathcal{O}_1/2 + \epsilon \mathcal{O}_2 + \frac{\epsilon^2}{4}(\mathcal{O}_1\mathcal{O}_2 - \mathcal{O}_2\mathcal{O}_1) + \epsilon \mathcal{O}_1/ 2 + [\epsilon \mathcal{O}_1/2 + \epsilon \mathcal{O}_2 + \frac{\epsilon^2}{4}(\mathcal{O}_1\mathcal{O}_2 - \mathcal{O}_2\mathcal{O}_1), \epsilon \mathcal{O}_1/ 2] + O(\epsilon^3)} 
$$

$$ 
\approx e^{\epsilon(\mathcal{O}_1 + \mathcal{O}_2)}
$$


So $e^{\epsilon \mathcal{O}_1/2}e^{\epsilon \mathcal{O}_2}e^{\epsilon \mathcal{O}_1/2}$ is a good approximation of $e^{\epsilon\mathcal{O}}$, and if we have closed forms for $e^{\epsilon \mathcal{O_1}}$ and $e^{\epsilon \mathcal{O_2}}$, we can use this operator in our discretization.

There is a literature of clever attempts to find good higher-order integrators, by finding other low order approximations. See e.g. [this paper](https://arxiv.org/pdf/hep-lat/0505020.pdf).


### Symplectic integrators

!!! Note

    This section assumes familiarity with the Hamiltonian formulation of classical physics, in particular the Poisson bracket.
    
One case where we can compute closed form updates is for Hamilton's equation of motion. Here, 

$$
\mathcal{O}f = \{f, H\} = \{f, T\} + \{f, V\}  := (L(T) + L(V))f
$$


Expanding $e^{L(T)}$ and $e^{L(V)}$ fully, we see that they are expressable as matrix multiplications (and hence linear maps) on a phase point. For instance:

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

The determinant of each map is 1, and so is their composition, which gives sympleticity.


