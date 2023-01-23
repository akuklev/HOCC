

For any family `q : I -> Ω` of propositions we can build:
```
sup(q) := ∀(\P : Ω) (P ∨ P -> ¬∀(\i : I) ¬q(i))   -- lem based candidate
sup(q) := ∀(\P : Ω) (¬P ∨ ¬P -> ¬∀(\i : I) ¬q(i))   -- wlem based candidate
sup(q) := ¬∀(\i : I) ¬q(i)   -- wlem based candidate2

```

Intuitionistically we can show each of `q(i)`'s implies `sup(q)`:
```
∃(\i : I) q(i) -> ¬∀(\i : I) ¬q(i), therefore also sup(q)
```

In the case of empty family we get LEM. In case of the constant family with
element S we get
```
sup((_ ↦ X)) := ∀(\P : Ω) (P ∨ (P -> ¬¬X))
```

Let us denote this case by `{X}`. Note that we get
```
{LEM} = ∀(\P : Ω) (P ∨ (P -> ¬¬∀(\P2 : Ω) (P2 ∨ ¬P2)))
```

Let us show that `X ∈ sup((_ ↦ X))`. We have already shown that `X -> {X}`, it only
remains to show that `({X} -> X) -> LEM`.
```
Assume we have
p : (∀(\P : Ω) (P ∨ (P -> ¬¬X))) -> X
```

empty

```
LEM₁(P) := ¬P ∨ ¬¬P
LEM₂(Pₙ) := (¬P₁ ∧ ¬P₂) ∨ ¬(¬P₁ ∧ ¬P₂) ∨ ((¬P₁ -> ¬P₂) ∧ (¬P₂ -> ¬P₁))

sup(q) := ∀(\P : Ω) (¬P ∨ ¬P -> ¬¬(∀(\P' : Ω) ¬P' ∨ ¬¬P'))

LEMₙ(Pₙ) := (⋀(\k) ¬Pₖ) ∨ ¬(⋀(\k) ¬Pₖ) ∨ ⋁(\i ≠ \j) (¬Pᵢ -> ¬Pⱼ)


```

[1]: GENERALIZATIONS OF THE WEAK LAW OF THE EXCLUDED MIDDLE ANDREA SORBI AND SEBASTIAAN A. TERWIJN
