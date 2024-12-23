## Contact Geometry

!!! References

    https://arxiv.org/pdf/1604.08266

Contact geometry is a counterpart to symplectic geometry on odd-dimensional manifolds.

<!-- In symplectic geometry, we start with a $(2n)$-dim manifold equipped with a 1-form $\eta$ satisfying certain properties, and a scalar-valued function $H$ on the manifold, and derive a vector field on the manifold (section of the tangent bundle) which respects 
TODO

In contact geometry, we start with a $(2n+1)$-dim manifold equipped with a 1-form $\eta$ satisfying certain properties, and a scalar-valued function $H$ on the manifold, and derive a dynamics on the manifold which respects 
TODO
$H^{-n}\eta\wedge (d\eta)^{n-1}$. -->

It turns out that isokinetic dynamics can be formulated in the language of contact geometry.

### Key points

Working on an $(2n+1)$-dimensional manifold $\mathcal{M}$, one starts by providing a contact form, which is a 1-form $\eta$ satisfying $\eta\wedge (d\eta)^n \neq 0$, and a Hamiltonian $H : \mathcal{M} \to \mathbb{R}$.

The Reeb vector field $\xi$ defined uniquely by $\eta(\xi) = 1$ and $d\eta(\xi)=\lambda k:0$.

Then the vector field $X_H$ is uniquely defined by:

$$i_{X_H}\eta = -H$$ 

and 

$$\mathcal{L}_{X_H}\eta = -\xi(H) \eta$$

Using Cartan's formula ($\mathcal{L}_X\omega = di_X\omega + i_Xd\omega$) then gives:

$$
\xi(H)\eta = dH - i_{X_H}d\eta
$$

<!-- 
     It can be shown that this implies:

    $$
    f_H = -\xi(H)
    $$

    so that, using Cartan's formula, we have

    $$
    dH = i_{X_H}d\eta - \mathcal{L}_{X_H}\eta
    $$

    $$
    = i_{X_H}d\eta + \xi(H)\eta
    $$ -->

#### Contact invariant

$$
V := |H|^{-(n+1)}(\eta \wedge (d\eta)^n)
$$

is preserved under the flow of $X_H$.

### Isokinetic contact form

Let our manifold be $S^n \times \mathbb{R}^{n+1}$, with coordinates $(\theta,x)$, and the contact form be $\eta = u_i(\theta)dx^i$, where $u_i$ is a function of the $\theta$ coordinates. 

Let $H(x,u) = p(x)^{1/n}$. We then recover isokinetic dynamics, up to a Sundman transform.

To see this, first note that $d\eta = \frac{\partial u_i}{\partial \theta_\mu}d\theta_\mu\wedge dx^i$, by the normal definition of the exterior derivative. Further, to satisfy the defining condition of the Reeb vector field, we must have $\xi(z)=u^i\frac{\partial}{\partial x^i}$.

<!-- TODO: why is $d\eta(\xi)$ 0? -->

Then note that $V = |H|^{-(n+1)}du\wedge dx$

where $du = \Lambda_{i=1}^n du_i$ and $dx = \Lambda_{i=1}^{n+1} dx_i$.


<!-- TODO: clarify this ^^^ - it seems off by a factor of $u$. -->

We can now put $\xi(H)\eta = dH - i_{X_H}d\eta$ in $x,
\theta$ coordinates directly as:

$$
-\partial_\mu u_i d\theta^\mu X_H^i+ \partial_\mu u_i dx^i  X_H^\mu - \partial_\mu H d\theta^\mu - \partial_i H dx^i = -u_ju_i\partial_i H dx^j
$$

From this we see that 

$$
\partial_\mu u_i   X_H^\mu = (\delta_{ji}-u_ju_i)\partial_i H 
$$

Moreover, $\partial_\mu u_i   X_H^\mu$ is precisely the $\theta$ components of the $X_H$ vector field pushed forward into the $u$ coordinates, so we have

$$
\dot u_j = (\delta_{ji}-u_ju_i)\partial_i H 
$$

or in vector notation:

$$
\dot u = (I - uu^T)\nabla H
$$

For $\dot x$, we use the condition $\eta(X_H)=u_iX_H^i = -H$ to solve and find $X_H^i(x,\theta) = -u^iH(x)$, so that

$$
\dot x = -uH
$$

From here, we perform a Sundman transform, with $a = -\frac{1}{H}$, to obtain the isokinetic dynamics:


$$
\dot x = u
$$

$$
\dot u = -(I - uu^T)\nabla \log H = -\frac{1}{d-1}(I - uu^T)\nabla \log p 
$$

where $d = n+1$


### Geometric picture

As in the symplectic case, we are composing two transforms on the bundle, which commute with each other.

```mermaid
 graph TD
      
      B["$$T^*M \cong M \times M$$, fibers = cotangent spaces. symplectic form exists"] -->|"pushforward by $$p \mapsto p/|p|$$"| C
     
A["$$M \times S^{d-1}$$, fibers = spheres; contact form exists"] -->|"pushforward by $$ \quad u(\theta)$$"| D
      D["$$M \times M$$, not contact any longer"] -->|"Sundman conformal factor: $$e^V$$ "| E["$$M\times M$$, isokinetic dynamics! What's the geometrical structure??"]
     
C["$$M \times M$$, not symplectic any longer"] -->|"Sundman conformal factor: $$e^K$$ "| E
```
      

      




      
