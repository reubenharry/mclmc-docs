
$$
\newcommand{\R}{\mathbb{R}}
$$

#### Summary

Let $F$ be the vector field for Hamiltonian dynamics with $K=\log |v|$, and $S(F) = |v|F$, $B^*(S(F))$ be the vector field of isokinetic dynamics, and $T_{1,2}$ being the two terms of $W$. $\Delta E$ is the change in total energy along the integral curve of the vector field. $F_x$ is the first $\R^n$ components of $F$, and $F_v$ is the rest (so  $F_v$ is the force in the typical sense of $F = \dot p$ (ignoring mass here)).

| Field    | $\rho$ | $T_1$ | $T_2$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $F$  | canonical or micro of $F$ | $\Delta E$    | 0 | $\Delta E$ |  0
| $S(F)$ | micro of $S(F)$ | $\Delta K$     | $-\Delta K$, so $\Delta V$  | $\Delta E$ | 0
| $B^*(S(F))$  | micro of $B^*S(F)$ | $\Delta V$    | $\Delta K$ so $-\Delta V$ | $\Delta E$ | 0
| $F_x$  | canonical of $F$ | $\Delta V$    | $0$ | $\Delta V$ | $\Delta V$
| $F_v$  | canonical of $F$ | $\Delta K$    | $0$ | $\Delta K$ | $\Delta K$
| $S(F)_x$  | canonical of $S(F)$ | $\Delta V$    | $0$ | $\Delta V$ | $???$
| $S(F)_v$  | canonical of $S(F)$ | $2\Delta K (?)$    | $-\Delta K$ | $\Delta K$ | $???$
| $B^*S(F)_x$  | micro of $B^*S(F)$ | $\Delta V$    | $0$ | $\Delta V$ | $???$
| $B^*S(F)_u$  | micro of $B^*S(F)$ | $0$    | $\Delta K$ | $\Delta K$ | $???$

### Derivations

| Field    | $\rho \propto e^{-H}$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $F$  | canonical or micro of $F$ | $\Delta E$    | 0 | $\Delta E$ |  0

This case is easy. The flow is incompressible, i.e. $\nabla \cdot F = 0$, so $T_2=0$. $T_1$, by inspection, is energy change. And energy change is $0$. So $W = \Delta E$.

If the stationary distribution is $\rho \propto \delta (H-c)$, this also works.

##


| Field    | $\rho \propto \frac{1}{\|v\|}\delta(H-c)$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $S(F)$ | canonical or microcanonical of $S(F)$ | $\Delta K$     | $-\Delta K$, so $\Delta V$  | $\Delta E $ | 0

Let $G$ be a vector field, or in more general terms, a section of a vector bundle, and define $z$ by $\dot z = G(z)$. Then we have a (microcanonical) stationary distribution of the form $\rho(z) \propto \sqrt{g}(z)\delta(\Lambda(z) - c)$, where $\Lambda$ is a conserved quantity, and $g$ is a metric on phase space given by $g(z)=e^{-w(z)}$, where $w$ is defined by $\frac{d}{dt}w = \nabla \cdot F$.

??? Derivation

    Suppose that $\rho(z) \propto f(C(z))$, where $\frac{d}{dt}C(z) = 0$ and $f$ is any function. Then:

    $$
    \nabla \cdot (\sqrt{g}pF) = \sqrt{g}p\nabla \cdot F + \nabla (\sqrt{g}p) \cdot F =  e^{-w} p\frac{dw}{dt} + F\cdot (p\nabla e^{-w}  +  e^{-w} \nabla p)
    $$

    $$
    = -p\dot z \nabla e^{-w} + \dot z p \nabla e^{-w} + e^{-w} \dot z \nabla p
    $$

    Now since $0 = \frac{dC}{dt} = \nabla C\dot z$, and $\nabla p \propto \frac{d f}{d C}\nabla C$, we see that the last term vanishes, and $0 = \nabla \cdot (\sqrt{g}pF) = \partial_t (\sqrt{g}p)$. So $\rho$ is stationary. 

Now we consider the field $S(F)(z) = a(z)F(z)$, specifically for $a(z)=|v|$. Let us calculate $\sqrt{g}$. We observe that $\frac{dw}{dt} = \nabla w \cdot \dot z = \nabla w \cdot F$, so the property that $w$ must satisfy is, in the case of $S(F)$ is:

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

This demonstrates that $w = \log a = \log |v|$.

We then immediately see that $T_1 = -\log\frac{p(T)}{p(0)} = \log\frac{p(0)}{p(T)} = \log\frac{e^{-\log |v(0)|}}{e^{-\log|v(t)|}} = \log|v(t)| - \log |v(0)| = \Delta K$.

(Note that if our original kinetic energy were quadratic, and $a=e^{-|v|^2}$, we would obtain a similar result.)

Meanwhile, we know that the effect of the transformation $S$ is to rescale time in a momentum dependent way, which does not alter integral curves, so $H$ is preserved, and $\Delta E = 0$.

Putting this together, we see that $\rho$ is given as above.

$T_2$ is immediately seen to be $-(e^{-w(t)} - e^{-w(0)}) = -\Delta K$.


So again, $W = \Delta E$.


| Field    | $\rho \propto e^{-V(x)}\delta(\|u\|^2 - c)$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $B^*(S(F))$  | microcanonical dist of $B^*S(F)$ | $\Delta V$    | $\Delta K$ so $-\Delta V$ | $\Delta E$ | 0

Recall that $\dot u \propto -(I-uu^T)\nabla V(x) = -\nabla V(x) + (u\cdot \nabla V(x))u$, so for $B^*S(F)(u) =  -\nabla V(x) + (u\cdot \nabla V(x))u$, we calculate that $\nabla\cdot B^*S(F) \propto u\cdot \nabla V(x)$, so that $w \propto V(x)$, since $\frac{d}{dt}V(x) = \nabla V(x) \cdot \dot x = \nabla V(x) \cdot u$.(Note that the factor of $d-1$ can easily be included in the potential $V$.)

Noting that $|u|$ is constant, we obtain $\rho$ as above.

$T_1$ is also easily obtained, given $w$, as $\Delta V$.

For $T_2$, note that $-\int \nabla \cdot F \propto -\int u\cdot \nabla V = \int_0^{S(T)} u(s(t)) \cdot F_u(s(t))dt = \int_0^T \frac{1}{|v|} v(s(t)) \cdot F_u(s(t))dt = \int_0^{T} v \cdot F_u = \Delta K$.

But we know that $\Delta E$, defined for the dynamics of $S(F)$ is $0$, so $W = \Delta E$ and also $T_2 = -\Delta V$.

| Field    | $\rho \propto e^{-H}$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $F_x$  | canonical of $F$ | $\Delta V$    | $0$ | $\Delta V$ | $\Delta V$
| $F_v$  | canonical of $F$ | $\Delta K$    | $0$ | $\Delta K$ | $\Delta K$ 

Here, we are concerned with a splitting-style integrator, so calculate the effects of $F_x$ and $F_v$ separately. The sum of $W(F_x)$ and $W(F_v)$ will be the quantity of interest, but we want to stationary distribution to remain the stationary distribution of $F$.

To be precise, we view $F_x$ as in $\R^{2n}$, but having $0$ for all the $v$ components. Likewise for $F_v$.

The flow from $F_x$ obviously only changes $x$, so in $T_1$ we get $\Delta V$. In $T_2$, we get $0$ because the $x$ part of $F_x$ is only a function of $v$ and the rest is $0$, so $\nabla_z \cdot F_x = 0$.

A similar argument applies to $F_v$, where $T_1$ is $\Delta K$ and $T_2$ is $0$.

So $W(F_x) + W(F_v) = \Delta V + \Delta K = \Delta E \neq 0$.



| Field    | $\rho \propto \frac{1}{\|v\|}e^{-H}$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $S(F)_x$  | canonical of $S(F)$ | $\Delta V$    | $0$ | $\Delta V$ | $???$
| $S(F)_v$  | canonical of $S(F)$ | $2\Delta K (?)$    | $-\Delta K$ | $\Delta K$ | $???$

This is the most interesting case. Recall that in this case, the ODE is given by $\begin{bmatrix} x \\ v \end{bmatrix} = \begin{bmatrix} \frac{v}{|v|} \\ -\nabla V(x)|v| \end{bmatrix}$, and the stationary distribution is $\rho \propto \frac{1}{\|v\|}e^{-H}$.
TODO: check!

For $S(F)_x$, only $x$ changes, so we get $\Delta V$ for $T_1$. For $T_2$, we get $0$ because the $x$ part of $S(F)_x$ is only a function of $v$ and the rest is $0$, so $\nabla_z \cdot S(F)_x = 0$.

For $S(F)_v$, we observe that $\rho \propto \frac{1}{|v|}e^{-H} = \frac{1}{|v|^2}e^{-V(x)} = 2\Delta K$, recalling that $\log |v|$ is the kinetic energy.

On the other hand, $T_2$ is $-\Delta K$, because $-\int \nabla \cdot (|v|F) = -\int F \cdot \nabla |v|  = -\int \frac{1}{|v|} F \cdot v$, and here we perform the change of variables as above and use the work energy theorem, which applies even for this $K$.

So $W(S(F)_x) + W(S(F)_v) = \Delta V + \Delta K = \Delta E$.


| Field    | $\rho \propto e^{-V(x)}\delta(\|u\|^2 - c)$ | $\log\frac{P(z(0))}{P(z(t))}$ | $-\int \nabla \cdot F$ | $W$ | $\Delta E$
| -------- | ------- | ------- | ------- | ------- | ------- |
| $B^*S(F)_x$  | micro of $B^*S(F)$ | $\Delta V$    | $0$ | $\Delta V$ | $???$
| $B^*S(F)_u$  | micro of $B^*S(F)$ | $0$    | $\Delta K$ | $\Delta K$ | $???$

The $B^*S(F)_x$ update is straightforward, and we get the above using the same reasoning as in previous cases.

For $B^*S(F)_u$, we see that the flow stays on the sphere, and so doesn't change the probability. Note that this allows us to use the microcanonical ensemble. So $T_1 = 0$. For $T_2$, we have $-\int \nabla \cdot F = -\int u \cdot \nabla V = \int \frac{1}{|v|} v \cdot F_v = \Delta K$.

So $W(B^*S(F)_x) + W(B^*S(F)_u) = \Delta V + \Delta K = \Delta E$.
