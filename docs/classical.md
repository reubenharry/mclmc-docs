## Classical Mechanics

The movement of a particle is given by an ODE:

$$
\frac{d}{dt}\begin{bmatrix} x \\ p \end{bmatrix} = \begin{bmatrix} \frac{\partial }{\partial p}H(x,p) \\ -\frac{\partial }{\partial x}H(x,p) \end{bmatrix}
$$

### Harmonic oscillator

$$
H(x,p) = \frac{p^2}{2} + \frac{x^2}{2}
$$

## Statistical Mechanics

We are interested in stationary distributions on phase space (i.e. fixed points on the information manifold under the flow of the dynamics).

### Canonical distribution

$$
p(x, v) \propto e^{-H(x, v)}
$$

#### Example of harmonic oscillator

$$
p(x,v) \propto e^{-\frac{1}{2}v^2 - \frac{1}{2}x^2}
$$

i.e a Gaussian distribution!

[illustration]

### Microcanonical distribution

$$
p(x, v) \propto \delta(H(x, v) - E)
$$

for some fixed $E$.

#### Example of harmonic oscillator

[illustration]