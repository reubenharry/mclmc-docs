## A stochastic differential equation

On [this page](diffgeo.md), we showed how to use the tools of differential geometry to show that a distribution $\rho_\infty$ is stationary under the ODE:

$$
\frac{d}{dt}\begin{bmatrix}
x \\
u
\end{bmatrix}
=
\begin{bmatrix}
u \\
-P(u)(\nabla S(x)/(d − 1))
\end{bmatrix}
$$

This isn't ergodic, so we add a stochastic term, upgrading our equation to a *stochastic* differential equation (SDE).

$$
\frac{d}{dt}\begin{bmatrix}
x \\
u
\end{bmatrix}
=
\begin{bmatrix}
u \\
-P(u)(\nabla S(x)/(d − 1)) + \eta P(u)dW
\end{bmatrix}
$$

!!! Note 
    In the paper [introducing MCHMC](https://arxiv.org/pdf/2212.08549.pdf)
     stochastic momentum updates were added to the discrete process induced by a (slightly different version of the) ODE. But no SDE was directly discussed which when discretized would give rise to those updates. This is the topic of the [follow up paper](https://arxiv.org/pdf/2303.18221.pdf). 


## Stationarity

🚧 Under construction 🚧

## Ergodicity

🚧 Under construction 🚧


