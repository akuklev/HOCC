Non-constructive Reasoning in Computational Dependent Type Theory (draft)
=========================================================================

We extend the observational calculus of constructions $CC_{obs}$ of Pujet and Tabareau by
a non-constructive modality ‖_‖ᶜ: an idempotent monad on the universe of definitionally
proof-irrelevant types Ω allowing for non-constructive an ε operator in a fenced fragment of the calculus
without compromising its good computational properties. We extend the proofs of normalization,
canonicity and decidability of type- and proof checking by original authors as well as their
set theoretical model, so that $CC_{obs}$ + ‖_‖ᶜ with $n$ universes can be modelled and proven
consistent in the set theory ZFC with $n$ inaccessables. We also show that the a particular
model $V$ of the set theory inside $CC_{obs}$ + ‖_‖ᶜ with $n$ universes models ZFC with $n$
inaccessables. By providing interleaving modellability we establish bi-interpretablity of
$CC_{obs}$ + ‖_‖ᶜ with a countably infinite hierarchy of universes and Tarski-Grothendieck
set theory. At least in theory we could develop mechanized translations between proofs in
a proof assistant based on $CC_{obs}$ + ‖_‖ᶜ and TG-based proof assistants Mizar and Metamath.

We are currently examining if bi-interpretablity can be strenthened to a conservativity result.

§ The Non-Constructive Modality
-------------------------------

The non-constructive modality is a type-former `‖_‖ᶜ : ∗ → Ω` with the following introduction rule

```
 Γ ⊢ T    Γ, εᵀ : ‖T‖ᶜ → T ⊢ prf : P
────────────────────────────────────────
         Γ ⊢ |prf|ᶜ : ‖P‖ᶜ
```

The non-constructive modality is defined precisely as to validate the axiom of choice:

```
∀(x : X) ∃(y : Y) P(x, y)   →   ‖∃(y : X → Y) ∀(x : X) P(x, y(x))‖ᶜ
```

which reads in words, “if for each `x : X` there is `y : Y` such that `P(x, y)` holds,
non-constructively there is a function `y : X -> Y` that assigns to each `x` a value `y(x)`
such that `P(x, y(x))` holds”. In type theory we interpret ∀ as dependent products and ∃
as (propositional truncation) of dependent sums.

Let us prove this claim:
```
prf : Π(\x : X) ∃(\y : Y) P(x, y)
-------------------------------------------
 TODO : ‖Σ(y : X → Y) ∀(x : X) P(x, y(x))‖ᶜ
```

In type theories not validating the uniqueness of identifications (UIP) for all types (these
in particular include all univalent type theories), a refined definition is required:

```
Γ ⊢ T    Γ, εᵀ : ‖T‖ᶜ → ‖T‖ᵁᴵᴾ ⊢ prf : ‖P‖
──────────────────────────────────────────
Γ ⊢ |prf|ᶜ : ‖P‖ᶜ
```

where `‖_‖` is the propositional truncation operator and `‖_‖ᵁᴵᴾ` is the 0-truncation operator.
This is necessary for two reasons: firstly, otherwise it would be possible to derive a contradiction
between choice and univalence in the context containing ε's. Secondly, so that is possible to derive
```
    prf : ∃(\x : X) P(x)
----------------------------
 |prf|ᶜ : ‖Σ(\x : X) P(x)‖ᶜ
```

Note that the proof of choice above holds for both the refined and the non-refined definition.
The same applies to all proofs below.

Let us show that ‖_‖ᶜ is idempotent and a monad:
```
    p : ‖X‖ᶜ
----------------
 |p|ᶜ : ‖‖X‖ᶜ‖ᶜ

    p : ‖‖X‖ᶜ‖ᶜ
-------------------
 |ε (ε p)|ᶜ : ‖X‖ᶜ

Since `‖X‖ᶜ : Ω`, these conversions are automatically inverses to each other

TODO: write down monad laws
```

TODO: Does it define a reflexive sub-universe?

It is quite easy to show that ‖_‖ᶜ validates modus ponens and generalization inference rules:

```
 x : ‖X‖ᶜ    f : ‖X → Y‖ᶜ
------------------------- ‖_‖ᶜ-MP
   |ε(f) ε(x)|ᶜ : ‖X‖ᶜ

          f : Π(\x : T) ‖P(x)‖ᶜ
---------------------------------------- ‖_‖ᶜ-Gen
 |\x : T ↦ ε(f x)|ᶜ : ‖Π(\x : T) P(x)‖ᶜ
```

Now it only remains to validate Hilbert axioms P1-I9 to show that ‖_‖ᶜ implements the whole
classical first-order logic. Since all of them except P4 are valid intuitionistically, they
can be derived by plugging the λ-terms witnessing them into |_|ᶜ.

The only remaining axiom is P4. Its proof is essentially the Diaconescu's theorem:
```
------------------------------P4
 TODO : ‖(¬Y → ¬X) → (X → Y)‖ᶜ

```

Let us additionally show that
```
 ‖¬Π(\x : T) ¬P(x)‖ᶜ
=====================
  ‖Σ(\x : T) P(x)‖ᶜ

```
