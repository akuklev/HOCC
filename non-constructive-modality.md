Non-constructive Reasoning in Computational Dependent Type Theory (draft)
=========================================================================

We extend the observational calculus of constructions $CC_{obs}$ of Pujet and Tabareau by
a non-constructive modality ‖_‖ᶜ: an idempotent monad into the universe of definitionally
proof-irrelevant types Ω allowing for non-constructive an ε operator in a fenced fragment
of the calculus without compromising its good computational properties. We extend the proofs
of normalization, canonicity and decidability of type- and proof checking by original authors
as well as their set theoretical model, so that $CC_{obs}$ + ‖_‖ᶜ with $n$ universes can be
modelled and proven consistent in the set theory ZFC with $n$ inaccessables. We also show
that the a particular model $V_n$ of the set theory [Werner97] inside $CC_{obs}$ + ‖_‖ᶜ with
$n + 1$ universes models ZFC with $n$ inaccessables. Interleaving modellability establishes
equiconsistency of $CC_{obs}$ + ‖_‖ᶜ with a countably infinite hierarchy and ZFC with ω
inaccessables. By adapting the argument of [Rathjen-Tupailo06] we show that $V_n$ is
‖_‖ᶜ-conservative over ZFC for CC sentences which include all “(generalised) mathematical
formulae” as argued by Rathjen and Tupailo.

<!--

In Werner's model, sets are defined as
```
#Inductive Vᵢ
sup : Π(T : 𝒰ⁱ) (T -> Vᵢ) -> Vᵢ
```

Thus in $CC_{obs}$ with infinitely many universes we still don't have a model of set Theory
with infinitely many inaccessables, but only a family of models Vᵢ, that have (i-1) inaccessables.

Let us try to develop an entirely impredicative model V that has infinitely many inaccessables.

Let us imagine we could have subtypes of the impredicative type Ω, in particular the type
V ⊂ Ω of propositions that represent sets.

Let's define the relation (∈) : Ω -> Ω -> Ω:
```
X ∈ Y := (X ≠ Y) ∧ (Y -> X)
```

Every set defines a class C(S) : Ω -> Ω:
```
C(S : Ω) := _∈ X
```

Now if we have a collection of sets `col : I -> Ω` indexed by a type I. By sup(col) let us denote
set containing all sets from this collection. Then
```
X ∈ sup(col) := ∃(\i : I)(col(I) = X)
```

Now let's expand the left hand side:
```
(X ≠ sup(col)) ∧ (sup(col) -> X) := ∃(\i : I)(col(I) = X)
```

Can we find such combinations of universal quantifiers and logical operations
that `sup` can be represented?

For all `I` define
if i

The type `Ω -> Ω` will have two distinct roles in this model:
at the same time it will represent sets and predicate on sets.
If `P : Ω -> Ω` and `S : Ω -> Ω`




Now function S : Ω -> Ω` represents a collection of sets indexed by sets.
A set Y belongs to this collection precisely if ther is such a set X :

X ∈ Y is represented as X -> Y

  predicate on sets such
that for `P : Ω -> Ω` and `S : Ω -> Ω`

Let `V := Σ(\P : Ω -> Ω) ∃(\P' : (Ω -> Ω) -> Ω) ‖∀P'(P)‖ᶜ = ‖∀P`‖ᶜ

For every element of some s : V_i we have

Define globalize(s : Vᵢ) : V
sup(\f : Π(T : 𝒰ⁱ) (T -> Vᵢ)) => (
(\set : Ω ↦ ??),
(\pred : (Ω -> Ω) ↦ ??),
??
)


[X : Ω (as set) ∈ Y : (Ω -> Ω) (as class)] := Y(X)
[X : Ω (as set) ∈ Y : (Ω -> Ω) -> Ω (as set)]
:= ∃(set A : Ω) Eq ()

  --->

§ The Non-Constructive Modality
-------------------------------

The non-constructive modality is a type-former `‖_‖ᶜ : ∗ → Ω` with the following introduction rule

```
 Γ ⊢ T    Γ, εᵀ : ‖T‖ᶜ → T ⊢ prf : P
────────────────────────────────────────
         Γ ⊢ |prf|ᶜ : ‖P‖ᶜ
```

In type theories not validating the uniqueness of identifications (UIP) for all types (these
in particular include all univalent type theories), a refined definition is required:

```
 Γ ⊢ T    Γ, εᵀ : ‖T‖ᶜ → ‖T‖ᵁᴵᴾ ⊢ prf : ‖P‖
──────────────────────────────────────────
         Γ ⊢ |prf|ᶜ : ‖P‖ᶜ
```

where `‖_‖ : 𝒰ⁿ → 𝒰ⁿ` is the propositional truncation operator (NB: it does not move types into Ω!)
and `‖_‖ᵁᴵᴾ : 𝒰ⁿ → 𝒰ⁿ` is the 0-truncation operator.
This is necessary for two reasons: firstly, otherwise it would be possible to derive a contradiction
between choice and univalence in the context containing ε's, thus rendering the sub-universe ‖_‖ᶜ of
non-constructively valid propositions inconsistent (every two of them would be equal than, including
‖1‖ᶜ = ‖0‖ᶜ). Secondly, so that is possible to derive
```
    prf : ∃(\x : X) P(x)
----------------------------
 |prf|ᶜ : ‖Σ(\x : X) P(x)‖ᶜ
```

Note that all proofs below hold for both the refined and the non-refined definition.

The non-constructive modality is defined precisely as to validate the axiom of choice. Let's
derive choice under the ‖_‖ᶜ modality in its shortest form (The HoTT Book, Lemma 3.8.2). Assume
`X` satisfies `X = ‖X‖ᵁᴵᴾ` and `Y(x)` a type family over `X` satisfying `Y(x) = ‖Y(x)‖ᵁᴵᴾ` (the last
proposition can be only stated in this simple form because `X` satisfies UIP). Now observe
```
ac : ‖Π(\x : X) ‖Y(x)‖ -> ‖Π(\x : X)Y(x)‖‖ᶜ
ac := | \f ↦ |\x ↦ ε (f x)| |ᶜ
```
Actually, we can either remove the requirement that `Y(x) = ‖Y(x)‖ᵁᴵᴾ` is or remove the
propositional truncation on the right side, but not both.

Consider another form of axiom of choice (`X` and `Y(x)` are again required to satisfy
UIP, `P` to be propositional)

```
∀(x : X) ∃(y : Y) P(x, y)   →   ‖∃(y : X → Y) ∀(x : X) P(x, y(x))‖ᶜ
```

which reads in words, “if for each `x : X` there is `y : Y` such that `P(x, y)` holds,
non-constructively there is a function `y : X -> Y` that assigns to each `x` a value `y(x)`
such that `P(x, y(x))` holds”.

```
prf : Π(\x : X) ∃(\y : Y) P(x, y)
--------------------------------------------------------------------------------------------
|\x : X ↦ (ε |(prf x).fst|ᶜ), \x : X ↦ (prf x).snd|ᶜ : ‖Σ(y : X → Y) ∀(x : X) P(x, y(x))‖ᶜ
```

Let us denote the above term as `ac2`. Let
```
ttdi := |\prf ↦ ε ac|ᶜ
ttdo : ‖Π(\x : X) ∃(\y : Y) P(x, y) → Σ(y : X → Y) ∀(x : X) P(x, y(x))‖ᶜ
```

`ttdi` is the so called Type-theoretical Description Axiom from [Werner97]

Let us show that ‖_‖ᶜ is idempotent and a monad:
```
    p : ‖X‖ᶜ
----------------
 |p|ᶜ : ‖‖X‖ᶜ‖ᶜ

    p : ‖‖X‖ᶜ‖ᶜ
-------------------
 |ε (ε p)|ᶜ : ‖X‖ᶜ

Since `‖X‖ᶜ : Ω`, these conversions are automatically inverses to each other
```


The map `(\x : T ↦ |x|ᶜ)` is the modal unit. The induction principle is given by
```
            f : Π(\x : A) ‖B(|x|ᶜ)‖ᶜ
-------------------------------------------------
 (\u : ‖A‖ᶜ ↦ |f(ε u)|ᶜ ) : Π(\u : ‖A‖ᶜ) ‖B(u)‖ᶜ

Since ‖_‖ᶜ : Ω, unit and induction vacously satisfy monadic unit laws
```

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

Let us additionally show that for propositional `P` (does not work for non-propositional `P`)
```
 ‖Σ(\x : T) ¬P(x)‖ᶜ
 -------------------QL
 ‖¬Π(\x : T) P(x)‖ᶜ

and

 ‖Π(\x : T) ¬P(x)‖ᶜ
 --------------------QR
 ‖¬Σ(\x : T) P(x)‖ᶜ

QL := (\s : ‖Σ(\x : T) ¬P(x)‖ᶜ) ↦
|(\p : Π(\x : T) P(x)) ↦
(ε s) match (\x, \y) ↦ y p(x)|ᶜ

QR := (\p : ‖Π(\x : T) ¬P(x)‖ᶜ) ↦
|(\s : Σ(\x : T) P(x)) ↦
\s match (\x, \p) ↦ (ε p) x p|ᶜ
```

By composing QL, QR and DNE (double negation elimination) we make the full circle,
thus both QL and QR are in fact isomorphisms.

Now we have all required results to state that for every theorem φ provable in ZFC
there is a proof `prf : ‖φⱽ‖ᶜ`, where is φⱽ the translation of the theorem φ into
the type-theoretic model of set theory model as proposed by Benjamin Werner in
[Werner97].

§ Conservativity
----------------

A formula of set theory is called a CC formula if and only if whenever an unbounded quantifier
appears in an implication, it appears only in the conclusion of the implication. We can adapt
the argument

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
