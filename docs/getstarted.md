## Installation

`pip install mclmc`

## Basic Usage

You can then run a sampler as follows:

```python
from sampling.sampler import Sampler, Target
import jax.numpy as jnp
import jax.random as jr

# define a 10 dim target distribution with negative log probability (nlogp) of a Gaussian
target = Target(d = 10, nlogp= lambda x: 0.5*jnp.sum(jnp.square(x)))
# sample from it
samples = Sampler(target).sample(
    num_steps=100,
    x_initial = jr.normal(shape=(10,), key=jr.PRNGKey(0)))
print(samples.shape)
# > (100, 10)
```

## More information

These Python [notebooks](https://github.com/JakobRobnik/MicroCanonicalHMC/tree/master/notebooks/tutorials), in particular [this one](https://github.com/JakobRobnik/MicroCanonicalHMC/blob/master/notebooks/tutorials/intro_tutorial.ipynb), show how to use the Python implementation of the MCLMC sampler.