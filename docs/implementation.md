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

🚧 Under construction: add explanations  🚧

The leapfrog integrator described in the [supplementary material](integrators.md) can be defined like so (with some extra values passed along like the change in log likelihood):

```python
def leapfrog(d, grad_nlogp, sigma):
      def step(u,x,g, eps):
        """leapfrog"""

        # half step in momentum
        uu, delta_r1 = update_momentum(d, eps * 0.5)(u, g * sigma)

        # full step in x
        xx, l, gg = update_position(eps, uu*sigma, grad_nlogp)(x)

        # half step in momentum
        uu, delta_r2 = update_momentum(d, eps * 0.5)(uu, gg * sigma)
        kinetic_change = (delta_r1 + delta_r2) * (d-1)

        return xx, uu, l, gg, kinetic_change
      return step
```

This is the core of the code (except that the minimal norm integrator is the default).Most of the rest of the code (and there's quite a lot) is setting up initial conditions, tuning, and more elaborate variations of the algorithm.