Non-constructive Reasoning in Computational Dependent Type Theory (draft)
=========================================================================

We extend the observational calculus of constructions $CC_{obs}$ of Pujet and Tabareau by
a non-constructive modality `‖_‖ᶜ`: an idempotent monad into the universe of computationally
irrelevant types Ω allowing for non-constructive an ε operator in a fenced fragment of the
calculus without compromising its good computational properties. We extend the proofs
of normalization, canonicity and decidability of type- and proof checking by original authors
as well as their set theoretical model, so that $CC_{obs}$ + `‖_‖ᶜ` with $n$ universes can be
modelled and proven consistent in the set theory ZFC with $n$ inaccessables. We also show
that the a particular model $V_n$ of the set theory [Werner97] inside $CC_{obs}$ + `‖_‖ᶜ` with
$n + 1$ universes models ZFC with $n$ strong inaccessables.

We propose to refine the set theoretic model of $CC_{obs}$ + `‖_‖ᶜ` by modelling Ω with Heyting
algebra of sets of a Grothendieck universe $V_κ$, and propose an embedding of the type-theoretic
set model $V$ into `Ω` by Zakharyaschev subframe canonical formulae, so that sets in the
type-theoretic model $V$ are represented precisely by the very same sets in the set theoretic
model of $CC_{obs}$ + `‖_‖ᶜ`, proving $V$ to be a `‖_‖ᶜ`-faithful model of ZFC, and thus
$CC_{obs}$ + `‖_‖ᶜ` with countably infinite hierarchy of universes to be a conservative
extension of ZFC with countably many strong inaccessibles.


<!--

Let `V := Σ(\P : Ω -> Ω) ∃(\P' : (Ω -> Ω) -> Ω) ‖∀P'(P)‖ᶜ = ‖∀P`‖ᶜ

For every element of some s : V_i we have

Define globalize(s : Vᵢ) : V
sup(\f : Π(T : 𝒰ⁱ) (T -> Vᵢ)) => (
(\set : Ω ↦ ??),
(\pred : (Ω -> Ω) ↦ ??),
??
)

  --->

§ The Non-Constructive Modality
-------------------------------

The non-constructive modality is a type-former `‖_‖ᶜ : ∗ → Ω` with the following introduction rule

```
 Γ ⊢ T    Γ, εᵀ : ‖T‖ᶜ → T ⊢ prf : P
—————————————————————————————————————
         Γ ⊢ |prf|ᶜ : ‖P‖ᶜ
```

In type theories not validating the uniqueness of identifications (UIP) for all types (these
in particular include all univalent type theories), a refined definition is required:

```
 Γ ⊢ T    Γ, εᵀ : ‖T‖ᶜ → ‖T‖ᵁᴵᴾ ⊢ prf : ‖P‖
 ————————————————————————————————————————————
         Γ ⊢ |prf|ᶜ : ‖P‖ᶜ
```

where `‖_‖ : 𝒰ⁿ → 𝒰ⁿ` is the propositional truncation operator (NB: it does not move types into Ω!)
and `‖_‖ᵁᴵᴾ : 𝒰ⁿ → 𝒰ⁿ` is the 0-truncation operator.
This is necessary for two reasons: firstly, otherwise it would be possible to derive a contradiction
between choice and univalence in the context containing ε's, thus rendering the sub-universe `‖_‖ᶜ` of
non-constructively valid propositions inconsistent (every two of them would be equal than, including
`‖1‖ᶜ = ‖0‖ᶜ`). Secondly, so that is possible to derive
```
    prf : ∃(x̲ : X) P(x)
———————————————————————————
 |prf|ᶜ : ‖Σ(x̲ : X) P(x)‖ᶜ
```

Note that all proofs below hold for both the refined and the non-refined definition.

The non-constructive modality is defined precisely as to validate the axiom of choice. Let's
derive choice under the `‖_‖ᶜ` modality in its shortest form (The HoTT Book, Lemma 3.8.2). Assume
`X` satisfies `X = ‖X‖ᵁᴵᴾ` and `Y(x)` a type family over `X` satisfying `Y(x) = ‖Y(x)‖ᵁᴵᴾ` (the last
proposition can be only stated in this simple form because `X` satisfies UIP). Now observe
```
ac : ‖Π(x̲ : X) ‖Y(x)‖ → ‖Π(x̲ : X)Y(x)‖‖ᶜ
ac := | f̲ ↦ |x̲ ↦ ε (f x)| |ᶜ
```
Actually, we can either remove the requirement that `Y(x) = ‖Y(x)‖ᵁᴵᴾ` is or remove the
propositional truncation on the right side, but not both.

Consider another form of axiom of choice (`X` and `Y(x)` are again required to satisfy
UIP, `P` to be propositional)

```
∀(x̲ : X) ∃(y̲ : Y) P(x, y)   →   ‖∃(y̲ : X → Y) ∀(x̲ : X) P(x, y(x))‖ᶜ
```

which reads in words, “if for each `x : X` there is `y : Y` such that `P(x, y)` holds,
non-constructively there is a function `y : X → Y` that assigns to each `x` a value `y(x)`
such that `P(x, y(x))` holds”.

```
                        prf : Π(x̲ : X) ∃(y̲ : Y) P(x, y)
——————————————————————————————————————————————————————————————————————————————————————————
 |x̲ : X ↦ (ε |(prf x).fst|ᶜ), x̲ : X ↦ (prf x).snd|ᶜ : ‖Σ(y̲ : X → Y) ∀(x̲ : X) P(x, y(x))‖ᶜ
```

Let us denote the above term as `ac2`. Let
```
ttdi := |p̲r̲f̲ ↦ ε ac|ᶜ
ttdo : ‖Π(x̲ : X) ∃(y̲ : Y) P(x, y) → Σ(y̲ : X → Y) Π(x̲ : X) P(x, y(x))‖ᶜ
```

`ttdi` is the so called Type-theoretical Description Axiom from [Werner97]

Let us show that `‖_‖ᶜ` is idempotent and a monad:
```
    p : ‖X‖ᶜ
————————————————
 |p|ᶜ : ‖‖X‖ᶜ‖ᶜ

    p : ‖‖X‖ᶜ‖ᶜ
———————————————————
 |ε (ε p)|ᶜ : ‖X‖ᶜ

Since `‖X‖ᶜ : Ω`, these conversions are automatically inverses to each other
```


The map `(x̲ : T ↦ |x|ᶜ)` is the modal unit. The induction principle is given by
```
          f : Π(x̲ : A) ‖B(|x|ᶜ)‖ᶜ
———————————————————————————————————————————————
 (u̲ : ‖A‖ᶜ ↦ |f(ε u)|ᶜ ) : Π(u̲ : ‖A‖ᶜ) ‖B(u)‖ᶜ

Since ‖_‖ᶜ : Ω, unit and induction vacously satisfy monadic unit laws
```

It is quite easy to show that `‖_‖ᶜ` validates modus ponens and generalization inference rules:

```
 x : ‖X‖ᶜ    f : ‖X → Y‖ᶜ
————————————————————————— ‖_‖ᶜ-MP
   |ε(f) ε(x)|ᶜ : ‖X‖ᶜ

        f : Π(x̲ : T) ‖P(x)‖ᶜ
—————————————————————————————————————— ‖_‖ᶜ-Gen
 |x̲ : T ↦ ε(f x)|ᶜ : ‖Π(x̲ : T) P(x)‖ᶜ
```

Now it only remains to validate Hilbert axioms P1-I9 to show that `‖_‖ᶜ` implements the whole
classical first-order logic. Since all of them except P4 are valid intuitionistically, they
can be derived by plugging the λ-terms witnessing them into `|_|ᶜ`.

The only remaining axiom is P4. Its proof is essentially the Diaconescu's theorem:
```
———————————————————————————————P4
 TODO : ‖(¬Y → ¬X) → (X → Y)‖ᶜ

```

Let us additionally show that for propositional `P` (does not work for non-propositional `P`)
```
 ‖Σ(x̲ : T) ¬P(x)‖ᶜ
———————————————————QL
 ‖¬Π(x̲ : T) P(x)‖ᶜ

and

 ‖Π(x̲ : T) ¬P(x)‖ᶜ
 ——————————————————QR
 ‖¬Σ(x̲ : T) P(x)‖ᶜ

QL := (s̲ : ‖Σ(x̲ : T) ¬P(x)‖ᶜ) ↦
|(p̲ : Π(x̲ : T) P(x)) ↦
(ε s) match (x̲, y̲) ↦ y p(x)|ᶜ

QR := (p̲ : ‖Π(x̲ : T) ¬P(x)‖ᶜ) ↦
|(s̲ : Σ(x̲ : T) P(x)) ↦
s̲ match (x̲, p̲) ↦ (ε p) x p|ᶜ
```

By composing QL, QR and DNE (double negation elimination) we make the full circle,
thus both QL and QR are in fact isomorphisms.

Now we have all required results to state that for every theorem φ provable in ZFC
there is a proof `prf : ‖φⱽ‖ᶜ`, where is φⱽ the translation of the theorem φ into
the type-theoretic model of set theory model as proposed by Benjamin Werner in
[Werner97].

<!--
§ Completely impredicative model of sets
----------------------------------------

The universe of `Ω` has quite rich structure intuitionistically. In particular, it countably many
inequivalent undecided propostions:
```
WLEM₁ := ∀(\P : Ω) (¬P ∨ ¬¬P)
WLEMₙ := ∀(\P₁ ... \Pₙ : Ω) ⋁(\i ≠ \j) (¬Pᵢ -> ¬Pⱼ) ∨ ¬⋀(\k) ¬Pₖ
```

Each of the above propositions implies all propositions below, but the reverse implication not possible[1].
Let us define
```
WLEM₀ := ¬∀(\n : Nat) ¬(WLEM(n + 1) -> WLEM(n))
```
which is implied by any reverse implication and implies all other WLEMₙ.

Let us define the relation `(∈) : Ω -> Ω -> Ω` by
```
X ∈ Y := (X -> Y) ∧ ((Y -> X) -> WLEM₀)
```

Note that with this definition `(X ∈ X) -> WLEM₀`.

With this relation, the propositions LEMₙ model sets, representing finite ordinals.
```
WLEM₀ ⋍ {}
WLEM₁ ⋍ { {} }
WLEM₂ ⋍ { {}, { {} } }
```

We hope to extend this construction for infinite sets. Note that for any family `q : I -> Ω` of
propositions, the proposition `¬∀(\i : I) ¬q(i)` is implied by each single `q(i)`. Let
```
sup(q) := ∀(\P : Ω) (P ∨ P -> ¬∀(\i : I) ¬q(i))
```

`sup(q)` retains the property of being implied by each single `q(i)`, and has additional property
that implication `X -> sup((_ ↦ X))` is a constructive taboo, and that being applied to empty family
it also produces a constructive taboo. By adjusting the definition of `WLEM₀` (and thus (∈)) we
hope to achieve that supremum of empty family turns out to be the empty set, and supremum of the
constant family `(_ ↦ X)` and does not contain itself. Then we hope (possibly adjusting the definition
of `sup`) to achieve that the sub-universe of Ω generated by `sup` to be a model of ZFC set theory
with respect to the relation (∈). Then we want to characterize this universe as propositions generated
by classes satisfying (∈)-induction. We hope to provide the model of CC_obs + |_| such that such
propositions are modelled essentially by the sets they denote, and ultimately show concervativity.


§ Extracting equalities from non-constructive proofs
----------------------------------------------------

In the theory we propose so far, there is no way to extract anything from the fenced
non-constructive fragment. Conjecture: no problems would be introduced by the rule
```
Г, x y : I ⊢ prf : ‖x = y‖ᶜ
---------------------------
     ↡prf : (x = y)
```
for purely inductive types `I`. This rule is certainly not admissible for universes
including Ω without ruining canonicity, and probably also not admissible for function
types. But we don't yet see how it could ruin computational properties if only applicable
for the types for which we know in advance (that is can observe from the type definition)
they have decidable equality.
-->