Some brief but useful notational conventions typical in differential geometry, and a few important facts are provided below. 

## Notation

### For general vector spaces

For $i, j \in \mathbb{Z}$, and $X^{ij} \in \mathbb{R}$:

$$
X^{ij}Y^{jk} = \sum_{j}X^{ij}Y^{jk}
$$

### For manifolds in particular

To every point $p$ on the manifold, we associate a vector space $T_p(\mathcal{M})$, where $T_p(\mathcal{M})$ is of the same dimension $d$ as the manifold, and therefore isomorphic to $\mathbb{R}^d$. 

??? Addenda 

    We can (and do) also associate points in $T_p(\mathcal{M})$ with differential operators of type $(\mathbb{R} \to \mathbb{R}) \to \mathbb{R}$, which can take a function on the manifold $\mathcal{M}\to \mathbb{R}$ (or in coordinates: $\mathbb{R}\to\mathbb{R}$) to a real number, which we should think of as the velocity of the path at $p$.


When we talk about a vector field on the manifold, we mean a function of type:

$$
f : (p : \mathcal{M}) \to T_p\mathcal{M}
$$

??? Types

    Note that the type of a vector field is a [dependent](https://en.wikipedia.org/wiki/Dependent_type) type. This means that the **type** of the output of the function, namely $T_p\mathcal{M}$ depends on the **value** of the input $p$.

### Differential forms

Given a vector space $K$, let $\Lambda_n(K)$ be the space of n-linear antisymmetric maps on $K$.

A differential n-form is then a function of type $(p : \mathcal{M}) \to \Lambda_n(T_p\mathcal{M})$.

$n \leq m$, where $m$ is the dimension of the manifold. Forms of the maximum dimension $n=m$ are called top-forms.

The exterior derivative $d$ is a map from $n-$forms to $n+1$-forms.

??? More-Abstract-Perspective

    Manifolds form a category $\mathcal{M}$, with the maps being smooth functions between manifolds. Differential forms form a category too, given by a contravariant functor $F^k$ on the category of manifolds which takes a manifold $M$ to $\Omega^{k}(M)$, and acts on maps as follows:

    $$ F^kf\omega(H_1,\ldots,H_k) = \omega(df(H_1),\ldots,df(H_k)) $$

    <!-- Having a morphism $f$ between manifolds $M\to N$ gives us a natural definition of a morphism $f^{*}: \Omega^{k}(N) \to \Omega^{k}(M)$. Concretely: -->

    The image of a smooth map $f \in \mathcal{M}$, namely $F^kf$, is called the pullback, or rather, $f^*\omega =: F^kf(\omega)$ is called the pullback of $\omega$ along $f$.


    Forms are invariant with respect to integration, under the pullback, in the following sense:

    $$ \int_{\phi(M)}\alpha = \int_{M} \phi^*\alpha $$

    This means we can calculate integrals on an $n$-manifold $M$ by pulling back along a map $\mathbb{R}^n \to M$, and thus reducing the problem to one of integration on $\mathbb{R}^n$, which can be done by e.g. Riemann integration.

    For $F^k$ and $F^{k+1}$, there is a natural transformation $F^k \to F^{k+1}$ known as the exterior derivative $d$, to be defined concretely below. More precisely, the components of the natural transformation, which take $k$-forms to $(k+1)$-forms are called $d$. Being maps in a category of vector spaces, linearity is clearly obeyed, and we'll see a form of the product rule. Further, the naturality condition implies the commutativity of the exterior derivative and pullback ($d\circ \phi^* = \phi^* \circ d$).

    There is also a map from $k$-manifolds to $(k-1)$-manifolds, by mapping a manifold to its boundary. This operator is called $\partial$. Note that $\partial \circ \partial = \{\}$. Boundaries have no boundary.

    The culmination of all of this is Stokes' theorem, which establishes a relationship between $\partial$ and $d$:

    $$ \int_{\partial M} \alpha = \int_{M} d\alpha $$

    This immediately implies that $d^2 = 0$. The fundamental theorem of calculus is a special case of Stokes' theorem.

## Metrics on Manifolds

To be Riemannian, a manifold must have a positive definite two-form $\eta$ called the metric (since it provides an inner product $\langle v, u \rangle := \sum_j \eta^{ij}v_ju_i$ ), and notationally we write, for $v^i \in {T_p(\mathcal{M})}$:

$$
v_i := v^j\eta_{ij}
$$

Crucially, we write $v^iu_i := \sum_i v^iu_i$, but only if one index is superscript and one is subscript. This ensures that we keep track of the difference between vectors and dual vectors, which we convert between using the metric above.

### Derivatives

$$
X^i_{,j} := \frac{\partial x_i}{\partial x_j}
$$

and 

$$
\partial_\mu := \frac{\partial}{\partial \theta^\mu}
$$

Note that this requires that we know that $\mu$ is the index variable associated with coordinates $\theta$.

This notation is consistent with the fact that $\theta_\mu$ should combine with a vector in the tangent space $v^\mu$, not a dual vector $v_\mu$. 


### Divergence in coordinates

$$
\mathit{div}_{a(x)dx^1 \wedge ...\wedge dx^n} X = \frac{1}{a}(aX^i)_{,i}
$$

## Lie derivative

For a top form:

$$
(\mathit{div}_\eta X)η := d(i_X η) = ((d\circ i_X) + (i_X \circ d)) \eta = L_X \eta
$$
