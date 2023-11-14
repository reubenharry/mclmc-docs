There is a Python implementation of the code [here](https://github.com/JakobRobnik/MicroCanonicalHMC). 

For example, this is the momentum update, calculated in an efficient way:

```python
def update_momentum(d, eps):
    
  def update(u, g):
      g_norm = jnp.sqrt(jnp.sum(jnp.square(g)))
      e = - g / g_norm
      ue = jnp.dot(u, e)
      delta = eps * g_norm / (d-1)
      zeta = jnp.exp(-delta)
      uu = e *(1-zeta)*(1+zeta + ue * (1-zeta)) + 2*zeta* u
      delta_r = delta - jnp.log(2) + jnp.log(1 + ue + (1-ue)*zeta**2)
      return uu/jnp.sqrt(jnp.sum(jnp.square(uu))), delta_r
  
  return update
```

Recall from the [tutorial](choice_of_integrator.md) that the momentum update should be:

$$
    u \mapsto 
    \frac{u + (\sinh{(\delta)}+ {e} \cdot u (\cosh (\delta) -1)) {e} }{\cosh{(\delta)} + {e} \cdot u \sinh{(\delta)}}   
$$

where $\delta = \epsilon \vert \nabla E(x) \vert / d$ and ${e} = - \nabla E(x) / \vert \nabla E(x) \vert$.

The above code gives this result.

The leapfrog integrator described in the [supplementary material](integrators.md) can be defined like so (with some extra values passed along like the change in log likelihood):

```python
def leapfrog(d, T, V):

  def step(x, u, g, eps, sigma):

    # V T V
    uu, r1 = V(eps * 0.5, u, g * sigma)
    xx, l, gg = T(eps, x, uu*sigma)
    uu, r2 = V(eps * 0.5, uu, gg * sigma)
    
    # kinetic energy change
    kinetic_change = (r1 + r2) * (d-1)

    return xx, uu, l, gg, kinetic_change
  
  return step, 1 # number of gradient calls per step
```

This is the core of the code (except that the minimal norm integrator is the default). Most of the rest of the code is setting up initial conditions, tuning, and more elaborate variations of the algorithm (multiple chains, interacting chains, annealing, SMC).