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

