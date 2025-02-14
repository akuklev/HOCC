Modalities for ♢Classical and □Parametric Reasoning within Martin-Löf Type Theories
===================================================================================

# Overview

In 2016 C. McBride [introduced](https://link.springer.com/chapter/10.1007/978-3-319-30936-1_12) graded type theories, where we can have exact control over the number of usages of variables (usages in type annotations not being counted). Original version used three modes, but can be extended to the following five:
```
x : X -- as often as desired
x :⁻X -- use at most once
x :¹X -- use exactly most once
x :⁺X -- use at least once
x :⁰X -- use only in type annotations
```

In intutionistic type theories, negation `¬T` is encoded as the function type `T → ⊥`. A proof that requires an assumption `nT : ¬T` can employ
this assumption to cut excluded cases when performing case analysis. For instance, a value `v : P + T` can be matched by `{ inl(p) ↦ ... ; inr(t) ↦ nT(t) }`.
Due to ex-falso-quodlibet (the recursion operator of the empty type ⊥), the excluded branch `nT(t)` can typecheck against any type.

Proofs by contradiction assume a negative premise `nT : ¬T` to derive a contradiction `⊥`. One way to arrive at a contradiction is to construct a counter-example
`t : T` and feed it into `nT(t)` thus yielding a contradiction. In general, a proof can require multiple negative premises, and use only one of them, so it is
not guaranteed that every negative premise is used. On the other hand, such a premise can be used inside an internal recursive subproof to cut branches, so it can
be used unbounded number of times. However, if we require such a premise to be used exactly once, we can extract a counterexample from a proof by contradiction:
```
 prf : ¬(nT :¹¬T)
——————————————————
   εᵀ(prf) : T
```

Otherwise, the extraction will be non-determinstic. However, we can introduce non-deterministic modalities ♢⁺, ♢⁻ and ♢, meaning “one or more possible outcome”, ”one or less outcomes” and “any number of possible outcomes”. With this modalities we have:
```
 prf : ¬(nT :⁺¬T)      prf : ¬(nT :⁻¬T)        prf : ¬¬T
——————————————————    ——————————————————    ——————————————
  εᵀ(prf) : ♢⁺T         εᵀ(prf) : ♢⁻T        εᵀ(prf) : ♢T
```

For the rest of this paper will not consider the substructural modes (⁻, ¹, ⁺) and modal operators (♢⁺, ♢⁻), and only consider the truly remarkable third rule above. It's the classical choice operator, which means we can presummably use classical reasoning (with excluded middle and choice) under the ♢-modality! We will argue that it has a perfect computational interpretation in terms of the Verse Calculus recently introduced by S. Peyton Jones et al. We will add the dual IS4 □-modality which is known to have computational interpretation of staged computation. Terms `t : □T` have the intended meaning of being closed terms of type `T`. Dually to classical reasoning under the ♢-modality we will obtain parametric reasoning under the □-modality. By establishing a set-theoretic interpretation of types under ♢-modality, we will show admissibility of the rule allowing to use closed non-constructive proofs can be used in computational context as virtual arguments:
```
 f : (x :° X) → Y      x : □♢X
———————————————————————————————
          f(x) : Y
```

We will show that this rule implies (and essentially is) the computational Markov principle allowing to evaluate Turing-complete computations given a closed classical proof of their non-divergence: 
```
 c : (Computation T)   nonDivergencePrf : □♢(c ≠ ⊥)
————————————————————————————————————————————————————
          eval(c, nonDivergencePrf) : T
```

In presence of °- and □-modality we can also introduce a reflective virtial universe 𝕊 and postulate existence of universes closed under any finite number of closed-definable typeformers, yielding a type-theoretic counterpart (and pressumably an equiconsistent conservative extension) of M. Shulman's “Set theory for category theory” ZMC/𝕊. These features render the proposed system an optimal candidate for the foundation of a proof assistaint. For practical usability as a functional programming language, the system has to be extended with indexed modalities for size-guarded recursion and clock-guarded corecursion, which can be shown to be eliminable towards of ordinary recursion and corecursion at cost of substantial complexity blowup.

In a companion paper, we outline how to extend Martin-Löf type theories (including the system proposed in this paper) to allow structural induction over their own languages, extending the notion of types from ∞-groupoids to ω-categories as a natural by-product.

# The closed term modality □ and virtual arguments
```
   □Г ⊢ x : X
————————————————
 □Г, Δ ⊢ x : □X

 Г ⊢ x : □X     Г, u :□ X ⊢ y : Y
———————————————————————————————————
     Г ⊢ (let u = x; y) : Y 

 □Г, x : X ⊢ y : □Y(x)
=======================
 Г, x :° X ⊢ y : Y(X)
```

# The verse modality ♢ and perceived entanglement

# Versal quantifiers

# The Markov principle

# Unary parametricity

# Interpreting classical logic

# Reflective universes

# Embedding of the type-theoretic model of ZF-sets into ♢ by Zakharyaschev subframe canonical formulae

# Set-theoretic model a la Pujet-Tabareau and conservativity via back-and-fourth argument

# Normalization for the modal-free fragment and admissibility of Markov principle

# Future work: Canonicity for the □-fragment, productivity for the ♢-fragment
