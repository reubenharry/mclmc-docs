# Next Generation Sampling Algorithms

Fully automated sampling from a differentiable density is needed in many fields. For the last decade, that black box solution has been the *No-U-Turn Sampler (NUTS)*. Recently, there has been work by many groups to replace NUTS. This website documents the next-generation samplers from Uroš Seljak's group[^1].

[^1]: This work was funded by the [NSF](https://www.nsf.gov/awardsearch/show-award/?AWD_ID=2311559)

# **Is there a sampler I can use today that is better than the No-U-Turn Sampler?**

Yes! The answer depends on whether you have a high dimensional problem where you can only run a few chains in parallel, or a lower dimensional problem where you can run *as many chains as the number of independent samples you want*.

**For the few-chain setting**, we recommend [Microcanonical Langevin Monte Carlo](https://blackjax-devs.github.io/sampling-book/algorithms/mclmc/), without Metropolis-Hastings. More information about the algorithm is available [here](mclmc.md).


**For the many-chain setting**, we recommend the [Late Adjusted Parallel Sampler](https://blackjax-devs.github.io/sampling-book/algorithms/laps/), which you can read about more [here](laps.md).

Both should work out of the box, but if you have problems or questions, you are welcome to reach out and we can advise.

# FAQ

### I heard that MCLMC doesn't use Metropolis-Hastings (MH): does this mean that it is inaccurate?

The idea that samplers without MH are inaccurate is widespread, but misleading. Samplers are used to calculate expectations, and even with MH, there is error from the finite sample size[^2]. Not using MH introduces an extra error term, but this doesn't mean that the total error at a given chain length has to increase. What we show is that one can trade-off between these two source of error.

[^2]: If you have collected an infinite number of samples, you may disregard this comment.

### Is MCLMC faster than NUTS?

In general, yes. This is an empirical claim, so should be taken with a pinch of salt. But we find that on every density we have tried, the number of gradient calls to reach 100 effective samples (taking bias into account) is lower for MCLMC than NUTS. Notably, the improvement gets better as the dimension increases.

### Is LAPS faster than just running chains in parallel?

Yes, quite a bit. You can also see comparison to other proposed parallel schemes in the paper.




<!-- 
two developments:

- a better understanding of how to use HMC without the Metropolis-Hastings acceptance step: this leads to large improvements in **single chain performance**
- a better understanding of how to take advantage of multiple chains: this leads to large improvements in **parallel performance**

The algorithm we recommend 




[Microcanonical Hamiltonian Monte Carlo](references/#microcanonical-hamiltonian-monte-carlo) (MCHMC) and its cousin [Microcanonical Langevin Monte Carlo](references/#microcanonical-hamiltonian-monte-carlo) (MCLMC) constitute a new sampling algorithm for distributions with differentiable log likelihoods, introduced with the goal of replacing the current state of the art differentiable sampler, NUTS.

A Python implementation is available in Blackjax: https://blackjax-devs.github.io/sampling-book/algorithms/mclmc.html

## Overview

![](/img/github_poster.png)

![](/img/rosenbrock.gif)

## Contents of this website

This website exists as a supplement to those papers, in order to explain [the theory behind the algorithm](tutorial.md) in more detail, and document [a variety of applications](applications.md) to which it is presently being applied.


 -->
