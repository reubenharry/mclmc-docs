The speed of the mode finding behavior suggests that MCLMC would work well in concert with annealing, and in particular with [Sequential Monte Carlo](smc.md). The idea is that an initial high temperature (which here means an inverse multiplicative factor on $\log p(x)$ ) allows for mode exploration, which is subsequently lowered to the target temperature.


