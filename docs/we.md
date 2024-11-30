
$$
\newcommand{\R}{\mathbb{R}}
$$

Let $M$ be the configuration manifold, i.e. the manifold on which $x$ is valued. Then the cotangent bundle $P=T^*M$ is the phase space, so the flow field $F$ of an ODE on the phase space lives on $T(T^*M)$.

We are concerned with two transformations, $S$, and $p \mapsto \frac{p}{|p|}$, which commute:

$$
\begin{CD}
T(T^*M) @>{S(F) = |p|F}>> T(T^*M)\\
@VVV @VV{B(p)=\frac{p}{|p|}}V \\
T(S(M)) @>{}>> T(S(M))
\end{CD}$$

Here, $S$ maps the field $F(z)$ to $|p|F(z)$. The second transform $B$ is a bundle morphism on the inner bundle, which maps $p$ to $\frac{p}{|p|}$, and can be lifted to the full bundle as $B^*$, in the usual way, by a pushforward map.

Suppose our original ODE, from a Hamiltonian with $K = \log |p|$ is:

$$
\frac{d}{dt}\begin{bmatrix} x \\ p \end{bmatrix} = \begin{bmatrix} F_x \\ F_p \end{bmatrix} = \begin{bmatrix} \frac{p}{|p|^2} \\ -\nabla V(x) \end{bmatrix}
$$

Then the ODE for $S(F)$ is:

$$
\frac{d}{dt}\begin{bmatrix} x \\ p \end{bmatrix} = \begin{bmatrix} S(F)_x \\ S(F)_p \end{bmatrix} = \begin{bmatrix} \frac{p}{|p|} \\ -\nabla V(x)|p| \end{bmatrix}
$$

and the ODE for $B^*(S(F))$ will be a (matrix) multiplication of $S(F)$ by the Jacobian of the transform $B$. In particular, for the $u$ coordinates: $\frac{du}{dv} = \frac{1}{|p|}(I - \frac{vv^T}{|p|^2})$, so we get:

$$
\frac{d}{dt}\begin{bmatrix} x \\ u \end{bmatrix} = \begin{bmatrix} B^*(S(F))_x \\ B^*(S(F))_u \end{bmatrix} = \begin{bmatrix} \frac{p}{|p|} \\ -(I-\frac{pp^T}{|p|^2})\nabla V(x) \end{bmatrix} = \begin{bmatrix} u \\ -(I-uu^T)\nabla V(x) \end{bmatrix}
$$

### General form of stationary distribution

Let $G$ be a vector field, or in more general terms, a section of a vector bundle, and define $z$ by $\dot z = G(z)$. Then any distribution of the form $\rho(z) \propto \sqrt{g(z)}f(\Lambda)$ is stationary. In particular, we have a (microcanonical) stationary distribution of the form $\rho(z) \propto \sqrt{g(z)}\delta(\Lambda(z) - c)$, where $\Lambda$ is a conserved quantity, and $g$ is a metric on phase space given by $g(z)=e^{-w(z)}$, where $w$ is defined by $\frac{d}{dt}w = \nabla \cdot G$.

??? Derivation

    Recall that $\partial_t (\sqrt{g}p) = \nabla \cdot (\sqrt{g}pG)$.

    Suppose that $\rho(z) \propto f(\Lambda(z))$, where $\frac{d}{dt}\Lambda(z) = 0$ and $f$ is any function. Then:

    $$
    \nabla \cdot (\sqrt{g}pG) = \sqrt{g}p\nabla \cdot G + \nabla (\sqrt{g}p) \cdot G =  e^{-w} p\frac{dw}{dt} + G\cdot (p\nabla e^{-w}  +  e^{-w} \nabla p)
    $$

    $$
    = -p\dot z \nabla e^{-w} + \dot z p \nabla e^{-w} + e^{-w} \dot z \nabla p
    $$

    Now since $0 = \frac{dC}{dt} = \nabla C\dot z$, and $\nabla p \propto \frac{d f}{d C}\nabla C$, we see that the last term vanishes, and $0 = \nabla \cdot (\sqrt{g}pG) = \partial_t (\sqrt{g}p)$. So $\rho$ is stationary. 

#### Summary of results

Let $F$ be the vector field for Hamiltonian dynamics with $K=\log |p|$, and $S(F) = |p|F$, $B^*(S(F))$ be the vector field of isokinetic dynamics, and $T_{1,2}$ being the two terms of $W$. $\Delta E$ is the change in total energy along the integral curve of the vector field. $F_x$ is the first $\R^n$ components of $F$, and $F_p$ is the rest (so  $F_p$ is the force in the typical sense of $F = \dot p$ (ignoring mass here)).

| Field | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- |  ------- | ------- | ------- | ------- |
| $F$  |  $\Delta E$    | 0 | $\Delta E$ |  0
| $S(F)$ | $\Delta K$     | $-\Delta K$, so $\Delta V$  | $\Delta E$ | 0
| $B^*(S(F))$  | $\Delta V$    | $\Delta K$ so $-\Delta V$ | $\Delta E$ | 0
| $F_x$   | $\Delta V$    | $0$ | $\Delta V$ | $\Delta V$
| $F_p$  | $\Delta K$    | $0$ | $\Delta K$ | $\Delta K$
| $S(F)_x$   | $\Delta V$    | $0$ | $\Delta V$ | 
| $S(F)_p$   | $2\Delta K$    | $-\Delta K$ | $\Delta K$ | 
| $B^*S(F)_x$   $\Delta V$    | $0$ | $\Delta V$ | $???$
| $B^*S(F)_u$   $0$    | $\Delta K$ | $\Delta K$ | $???$

## Derivations

### F

| Field    | $\rho \propto e^{-H}$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $F$  | canonical or micro of $F$ | $\Delta E$    | 0 | $\Delta E$ |  0

This case is easy. The flow is incompressible, i.e. $\nabla \cdot F = 0$, so $T_2=0$. $T_1$, by inspection, is energy change. And energy change is $0$. So $W = \Delta E$.

If the stationary distribution is $\rho \propto \delta (H-c)$, this also works.

## S(F)

| Field    | $\rho \propto \frac{1}{\|p\|}\delta(H-c)$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $S(F)$ | canonical or microcanonical of $S(F)$ | $\Delta K$     | $-\Delta K$, so $\Delta V$  | $\Delta E$ | 0


Now we consider the field $S(F)(z) = a(z)F(z)$, specifically for $a(z)=|p|$. Let us calculate $\sqrt{g}$ (see definition above). We observe that $\frac{dw}{dt} = \nabla w \cdot \dot z = \nabla w \cdot F$, so the property that $w$ must satisfy is, in the case of $S(F)$ is:

$$
S(F) \cdot \nabla w = \nabla \cdot S(F)
$$



$S(F)(z)=a(z)F(z)$, so we find that or 

$$
aF \cdot \nabla w = \nabla \cdot (aF) 
$$

$$
= a\nabla\cdot F + F \cdot \nabla a = F \cdot \nabla a  = aF \cdot \frac{\nabla a}{a} = aF \cdot \nabla (\log a)
$$

This demonstrates that $w = \log a = \log |p|$.

We then immediately see that $T_1 = -\log\frac{p(T)}{p(0)} = \log\frac{p(0)}{p(T)} = \log\frac{e^{-\log |p(0)|}}{e^{-\log|p(t)|}} = \log|p(t)| - \log |p(0)| = \Delta K$.

(Note that if our original kinetic energy were quadratic, and $a=e^{-|p|^2}$, we would obtain a similar result.)

Meanwhile, we know that the effect of the transformation $S$ is to rescale time in a momentum dependent way, which does not alter integral curves, so $H$ is preserved, and $\Delta E = 0$.

Putting this together, we see that $\rho$ is given as above.

$T_2$ is immediately seen to be $-(e^{-w(t)} - e^{-w(0)}) = -\Delta K$.


So again, $W = \Delta E$.

### B(S(F))

| Field    | $\rho \propto e^{-(d-1)V(x)}\delta(\|u\|^2 - c)$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $B^*(S(F))$  | microcanonical dist of $B^*S(F)$ | $\Delta V$    | $\Delta K$ so $-\Delta V$ | $\Delta E$ | 0

Recall that $\dot u \propto -(I-uu^T)\nabla V(x) = -\nabla V(x) + (u\cdot \nabla V(x))u$, so for $B^*S(F)(u) =  -\nabla V(x) + (u\cdot \nabla V(x))u$, we calculate that $\nabla\cdot B^*S(F) = (d-1) u\cdot \nabla V(x)$, so that $w \propto V(x)$, since $\frac{d}{dt}V(x) = \nabla V(x) \cdot \dot x = \nabla V(x) \cdot u$.(Note that the factor of $d-1$ can easily be included in the potential.)

Noting that $|u|$ is constant, we obtain $\rho$ as above.

$T_1$ is also easily obtained, given $w$, as $\Delta V$.

For $T_2$, the reasoning is more subtle. Using $z'=(x', u')$ to label the variables after the time rescaling, with $z'(t) = z(s(t))$, we have:

$$-\int_0^T \nabla \cdot F(t)dt = -\int_0^T u(t)\cdot \nabla V(x'(t))dt = - \int_0^{T} \frac{p'}{|p'|}\cdot \nabla V(x')dt
$$

$$
= -\int^T_0 \frac{p(s(t))}{|p(s(t))|}\cdot \nabla V(x(s(t)))dt = -\int^{s(T)}_0 \frac{p(s)}{|p(s)|^2}\cdot \nabla V(x(s))ds
$$



$$
= \int_0^{s(T)} \frac{p}{|p|^2} \cdot \dot p = \int_0^{s(T)}\frac{d}{dt}\log |p| = \Delta K := K(s(T))-K(0)
$$


<!-- $$\int_0^{S(T)} u(s(t)) \cdot F_u(s(t))dt = \int_0^T \frac{1}{|p|} v(s) \cdot F_u(s)ds = \int_0^{T} v \cdot F_u = \Delta K$$ -->

<!-- But we know that $\Delta E$, defined for the dynamics of $S(F)$ is $0$, so $W = \Delta E$ and also $T_2 = -\Delta V$. -->

### x and p updates of F

| Field    | $\rho \propto e^{-H}$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $F_x$  | canonical of $F$ | $\Delta V$    | $0$ | $\Delta V$ | $\Delta V$
| $F_p$  | canonical of $F$ | $\Delta K$    | $0$ | $\Delta K$ | $\Delta K$ 

Here, we are concerned with a splitting-style integrator, so calculate the effects of $F_x$ and $F_p$ separately. The sum of $W(F_x)$ and $W(F_p)$ will be the quantity of interest, but we want to stationary distribution to remain the stationary distribution of $F$.

To be precise, we view $F_x$ as in $\R^{2n}$, but having $0$ for all the $p$ components. Likewise for $F_p$.

The flow from $F_x$ obviously only changes $x$, so in $T_1$ we get $\Delta V$. In $T_2$, we get $0$ because the $x$ part of $F_x$ is only a function of $p$ and the rest is $0$, so $\nabla_z \cdot F_x = 0$.

A similar argument applies to $F_p$, where $T_1$ is $\Delta K$ and $T_2$ is $0$.

So $W(F_x) + W(F_p) = \Delta V + \Delta K = \Delta E \neq 0$.

### x and p updates of S(F)

| Field    | $\rho \propto \frac{1}{\|v\|}e^{-H}$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $S(F)_x$  | canonical of $S(F)$ | $\Delta V$    | $0$ | $\Delta V$ | $???$
| $S(F)_p$  | canonical of $S(F)$ | $2\Delta K$    | $-\Delta K$ | $\Delta K$ | $???$

This is the most interesting case. Recall that in this case, the ODE is given by $\begin{bmatrix} x \\ p \end{bmatrix} = \begin{bmatrix} \frac{p}{|p|} \\ -\nabla V(x)|p| \end{bmatrix}$, and the stationary distribution is $\rho \propto \frac{1}{\|v\|}e^{-H}$.

For $S(F)_x$, only $x$ changes, so we get $\Delta V$ for $T_1$. For $T_2$, we get $0$ because the $x$ part of $S(F)_x$ is only a function of $p$ and the rest is $0$, so $\nabla_z \cdot S(F)_x = 0$.

For $S(F)_p$, we observe that $\rho \propto \frac{1}{|p|}e^{-H} = \frac{1}{|p|^2}e^{-V(x)}$, so that $T_1 = 2\Delta K$, recalling that $\log |p|$ is the kinetic energy.

On the other hand, $T_2$ is $-\Delta K$. To show this, we proceed as follow, using $z'=(x', p')$ to label the variables after the time rescaling, with $z'(t) = z(s(t))$. Then:

$$\int_0^T \nabla\cdot F(z'(t))dt = -\int \nabla \cdot (|p'(t)|F(z'(t))) = -\int F(z'(t)) \cdot \nabla |p'(t)|dt$$

$$ 
-\int \frac{p'(t)}{|p'(t)|} \cdot F_p(z'(t))dt = -\int \frac{p'(t)}{|p'(t)|} \cdot \dot p(z'(t))dt
$$

$$
=
-\int_0^{T} \frac{p(s)}{|p(s)|} \cdot \dot p(z(s))\frac{dt}{ds}ds = -\int_0^{s(T)} \frac{p(s)}{|p(s)|^2} \cdot \dot p(z(s))ds 
$$



<!-- 
$$
= \int_0^T p(s(t)) \cdot \nabla V(x(s(t)))dt = \int_0^{s(T)} |p(s)|p(s) \cdot \nabla V(x(s))ds
$$ 
-->

$$
= -\int_0^{s(T)}\frac{d}{dt}\log |p| = -\Delta K := K(0)-K(s(T))
$$


So $W(S(F)_x) + W(S(F)_p) = \Delta V + \Delta K = \Delta E$.

### x and p updates of B(S(F))

| Field    | $\rho \propto e^{-V(x)}\delta(\|u\|^2 - c)$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $B^*S(F)_x$  | micro of $B^*S(F)$ | $\Delta V$    | $0$ | $\Delta V$ | $???$
| $B^*S(F)_u$  | micro of $B^*S(F)$ | $0$    | $\Delta K$ | $\Delta K$ | $???$

The $B^*S(F)_x$ update is straightforward, and we get the above using the same reasoning as in previous cases.

For $B^*S(F)_u$, we see that the flow stays on the sphere, and so doesn't change the probability. Note that this allows us to use the microcanonical ensemble. So $T_1 = 0$. For $T_2$, we have already shown above that we get $\Delta K$.

So $W(B^*S(F)_x) + W(B^*S(F)_u) = \Delta V + \Delta K = \Delta E$.

<!-- ### Understanding $w$ for arbitrary transforms on phase space

Suppose we start with a vector field $G$, and have a transformation $B$ on the base manifold of the ODE, which in our case is the phase space $T^*M$. Then the resulting change in the vector field on $T(T^*M)$ is a matrix multiplication by the Jacobian $\frac{\partial B(z)}{\partial z}$, so $B^*(G) = \frac{\partial B(z)}{\partial z}G$.

For $B(p) = p/|p|$, the Jacobian is $\frac{1}{|p|}(I - \frac{pp^T}{|p|^2})$.

We are interested in calculating $w := \int \nabla \cdot B^*G$, because in this case we can identity the stationary distribution of $B^*G$.

Using [this formula](https://math.stackexchange.com/questions/2625185/divergence-of-matrix-vector-product), we find that $\nabla \cdot B^*G = (\nabla \cdot \frac{\partial B(z)}{\partial z})G + tr(\frac{\partial B(z)}{\partial z}\nabla G)$ (with definitions of the matrix divergence and vector gradient given on that page).

Noting that $\nabla \cdot B^*G = \frac{dw}{dt} = \nabla w \cdot B^*G$, we see that **if** $tr(\frac{\partial B(z)}{\partial z}\nabla G)=0$, $w = (\nabla \cdot \frac{\partial B(z)}{\partial z})(\frac{\partial B(z)}{\partial z})^{-1} = \nabla \cdot \log \frac{\partial B(z)}{\partial z}$, otherwise it is not obvious that it exists.

For $B(p) = p/|p|$, we can do a sanity check to see that we recover the desired result:

$$
(A + bc^T)^{-1} = A^{-1} - \frac{A^{-1}bc^T A^{-1}}{1+c^T A^{-1} b}
$$

so

$$
(\frac{I}{|p|} - \frac{pp^T}{|p|^3})^{-1} = |p|I + \frac{|p|pp^T/|p|^3}{1 - p^Tp/|p|^3}$$

$$
 = |p|I + \frac{pp^T/|p^2|}{1-p^Tp/|p|^3} = |p|I + \frac{uu^T}{1-u^Tu/|p|}
$$




Note that $\dot u = -(\frac{I}{|p|} - \frac{pp^T}{|p|^3})|p|\nabla V(x)$, so that $\nabla V(x) = -\dot u \frac{1}{|p|}(\frac{\partial B(z)}{\partial z})^{-1} =-\dot u(|p|I + \frac{uu^T}{1-u^Tu/|p|})\frac{1}{|p|} = -\dot u(I + \frac{uu^T}{|p|-u^Tu})$.

**Todo**: keep working on this... 


 -->
