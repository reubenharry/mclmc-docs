

## Why unadjusted methods are better in high dimensions

Empirically, it seems that unadjusted methods (tuned to minimize bias) are more statistically efficient than adjusted methods (which are unbiased). 

One can gain some insight into the reasons by considering Gaussians.

### Adjusted methods

We have, for a second order integrator:

$$
Var[E] \propto d\epsilon^4
$$

why???

and $Var[E] \propto d\epsilon^8$ for a fourth order integrator.

We know that for a Gaussian, 0.65 is the optimal acceptance rate for the Metropolis-Hastings algorithm (for HMC), and acceptance rate is proportional to the expected change in energy, which scales with variance:

TODO: why?

So, solving $d\epsilon^4 = k$, we have:

$$
\epsilon \propto d^{-1/4}
$$

for a second order integrator, and $\epsilon \propto d^{-1/8}$ for a fourth order integrator.

### Unadjusted methods

We are able to derive a relation:

$$
b^{\frac{3}{2}} \propto Var(E)/d
$$

where $b$ is a measure of bias. But since variance scales with $Var[E] \propto d\epsilon^4$, we see that $d$ cancels, and we have:

$$
b^{\frac{3}{2}} \propto \epsilon^4
$$

**which is constant in $d$**.

So it isn't necessary to shrink step size in high dimensions.