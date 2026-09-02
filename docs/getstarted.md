## Installation

### Blackjax

MCLMC are LAPS are available in Blackjax, so first install it:

`pip install blackjax`

Then see https://blackjax-devs.github.io/sampling-book/algorithms/mclmc/ and https://blackjax-devs.github.io/sampling-book/algorithms/laps/ for instructions on how to run the algorithms.

### NumPyro and PyMC

Both NumPyro and PyMC have support for extracting their densities into functions, which can then be used with Blackjax. See [here](https://blackjax-devs.github.io/blackjax/examples/howto_use_numpyro.html) and [here](https://blackjax-devs.github.io/blackjax/examples/howto_use_pymc.html).

NumPyro now also supports external samplers. See [here](https://github.com/juanitorduz/numpyro/blob/21ebd13f1cf18500f73639301f96c0c18bdc2069/notebooks/source/other_samplers.ipynb) for an example.