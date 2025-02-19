

For any family `q : I -> Ω` of propositions we can build:
```
sup(q) := ∀(\P : Ω) P ∨ (P -> ¬∀(\i : I) ¬q(i))   -- lem based candidate
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

Let us define TABOO by `¬∀(\X : Ω) ¬(sup((_ ↦ X)) -> X)`, that is
```
TABOO := ¬∀(\X : Ω) ¬((∀(\P : Ω) (P ∨ (P -> ¬¬X))) -> X)
```


Any instance of `(∀(\P : Ω) (P ∨ (P -> ¬¬X))) -> X`
Proves `(∀(\P : Ω) (P ∨ (P -> ¬¬X))) -> X`

∀(\P : Ω) (P ∨ (P -> ¬¬X))

Let us show that `X ∈ sup((_ ↦ X))`. We have already shown that `X -> {X}`, it only
remains to show that `({X} -> X) -> TABOO`.
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
