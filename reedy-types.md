Type-theoretic approach to Reedy categories
===========================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)

Building on the unpublished ideas of C. McBride, we propose a novel extension for Martin-Löf Type Theories (MLTTs) we'll call inductive prototypes, a type-theoretic internalization of Reedy categories. Inductive type families indexed over prototypes provide effective machinery for dealing with syntaxes that include binding, and become indispensable when internalizing the syntax and semantics of type theories themselves. Bidirectional presentations of dependent type theories turn out to be inductive-inductive-recursive definitions. There, the inductive type families of expressions and normal forms are defined simultaneously with inductive prototypes representing their contexts and reduction rules framed as recursive functions on expressions.

Semantically, fibered quotient inductive-inductive type definitions (FQIITs) are effective presentations of weak model categories whose structure-preserving functors correspond to elimination motives. In strong analogy to the functorial semantics of Lavwere algebraic theories, these functors themselves form a category of models, with their natural transformations serving as model homomorphisms.

In a subsequent paper, we hope to establish the existence of initial models in an arbitrary (∞,1)-topos, thus providing a semantics for FQIITs. We assume it to be conditional on the existence of an inaccessible/Mahlo cardinal for small/large FQIITs, respectively, and unconditional for finitary FQIITs. It would also uniformly solve the initiality conjecture for structures admitting an effective bidirectionally algebraic definition, including [weak ω-categories](https://arxiv.org/abs/1706.02866), [virtual equipments](https://arxiv.org/abs/2210.08663), and (∞,1)-toposes once the [Higher Observation Type Theory (HOTT)](https://ncatlab.org/nlab/show/higher+observational+type+theory) is complete.

# Overview

Natural deduction, an inference rule-based proof calculus can be seen as a precursor of Martin-Löf type theories.
The inference rules of natural deduction serve as a typed generative grammar for proof terms, where the propositions being
proven serve as types of the respective proofs:
```
  a : P     b : Q
————————————————————
 (a, b) : (P and Q)
```
— ‘given a proof term `a` which proves the proposition `P` and a proof term `b` which proves the proposition `Q`, we can construct a proof term denoted `(a, b)` which proves the proposition `P and Q`’. This notation for inference rules still universally used today was first introduced for natural deduction.

Another forerunner of Martin-Löf type theories is Gödel's System T: it's a Simply Typed λ-Calculus `STLC(ℕ)` over the inductive data type of natural numbers. In fact, it's a fragment of all Martin-Löf type theories. There, the types of expressions are data types.

The other constituent of Martin-Löf type theories comes from the first type theory ever introduced, the Russell's theory of types introduced at the very beginning of XX century as a way to deal with Russell's paradox: the type universes. It was long assumed that the universes of data types in Martin-Löf type theories can be seen as data types themselves, but due to a discovery made by Martin Hofmann and Thomas Streicher in 1996 it began to become apparent that it's not the case: the universe of datatypes neccesarily carries additional higher structure, and ignoring this structure makes the theory either undecidable or unsound. Datatypes can be equivalent in inequivalent ways: for instance, the functions `id := { x ↦ x }` and `negate = { tt ↦ ff ; ff ↦ tt}` are nonequivalent automorphisms of the datatype `𝔹 = {tt, ff}`. Starting with the universe `U₀` of datatypes we can construct other “non-data” types, e.g. the universe of pointed data types `Σ(T : U₀) T` or the universe of `U₀`-polymorphic datatypes `U₀ → U₀`. The next type universe `U₀⁺` is obviously also a “non-data” type. Ultimately, Voevodski understood that the types of Martin-Löf type theories represent anima (also known as ∞-groupoids), with data types and propositions being special cases of 0th and -1th truncation levels respectively. Advanced forms of MLTT correctly accomodating higher structure of type universes are now known as univalent (Martin-Löf) type theories. Currently there are several implementations, each with its own minor issues, while the mature one (the Higher Observational Type Theory) is being developed. There, universes come with a recursively defined operation `(≃) : ∀{T : U} ∀(x y : T) U` that computes the type of equivalences of elements inside the types `T` inside the universe.

In fact, type universes are not just ∞-groupoids: taking function types `X → Y` between their elements makes them into ∞-categories. They also come with a recursively defined operation `( ᵈ) : U → U` that computes displayed types representing binary relations `Xᵈ` from the type `X` into other types. These binary relations make type universes into ∞-procategories. As I will show below, for every inductive type `I` we also obtain a ∞-procategory `I-Mod` of its models.^[Technically, the polymorphic type `I-Mod<T : *>` of I-model structures on a type `T` can be itself equipped with a natural structure of a displayed ∞-procategory, yielding a hierarchy of ∞-procategories of its U-small localizations `I-Modᵁ := Σ(T : U) I-Mod<U>`.]

Long before this realization of Voevodski's, enthusiasts of the category-theoretic approach to the foundations of mathematics wondered whether it was possible to generalize MLTTs to accomodate categories as types. Subsequently, approaches were indeed found (cf. [Type Theory for Synthetic ∞-categories](https://rzk-lang.github.io)), but as often happens in the absence of applied motivation, this developments remain rather obscure and hard to grasp. The situation changed in 2021 when C. McBride ([“Cats and Types: Best Friends?“](https://youtu.be/05IJ3YL8p0s)) presented the idea that representing syntaxes with binding begs for inductive data type families indexed over types with directed higher structure. Complementarily, representing dependently-sorted syntaxes requires very-dependent types_ introduced by Hickey and Kopylov [“Formal objects in type theory using very dependent types.”](https://www.cs.cornell.edu/jyh/papers/fool3/paper.pdf) which can be seen as families indexed over types with inverse higher structure.

To make the MLTTs capable of performing structural induction over their own language (i.e. representing their own syntax in terms of inductive types, also known as “eating themselves”), we need to introduce yet another kind of types not fitting into the landscape set by Voevodski, namely types with both directed and inverse higher structure, type-theoretic internalization of Reedy categories we call inductive prototypes.

The raison d'être for inductive prototypes are the inductive families indexed over them. We'll show that these are given by ( ᵈ)-coinductive types recently introduced by Kolomatskaia and Shulman. These are a type-theoretic internalizations of Reedy presheaves, and will turn out to include (semi-)simplicial types and other (weak) test categories commonly used to provide models for higher-categorical structures. We'll show how definition of type families indexed over prototypes can be generalized to functors from prototypes to arbitrary ∞-procategories, including other prototypes which can be seen as simplest synthetic procategories. We expect the universes of Reedy prototypes, as well as functors and families over them to carry structure of iterated ∞-procategories, that is, yet-to-be-defined, ω-procategories.

We conjecture that it would be possible to reproduce and advance developments related to the type theory for synthetic ∞-categories, and ultimately embrace synthetic ω-(pro)categories^[We expect that the approach developed in “Types are Internal ∞-Groupoids” by Allioux, Finster, and Sozeau to extend to show that all HCTT types would turn out to be internal ω-procategories.], a long sought-after category-theoretic foundational framework what turns to be emergent if one seeks for a natural proof calculus capable of structural induction over its own language.

# Inductive types and ∞-procategories of their models

Every inductive type comes with a ∞-procategory of its models, but let's use the inductive type of natural numbers to have an illustrative example:
```
inductive ℕ
  0 : ℕ
  ( ⁺) : ℕ → ℕ
```

An inductive definition does not only generate the type (ℕ) itself, but also coinductive dual, the type of infinite sequences:
```
structure (ℕ→ )<T : *>
  head : T
  tail : ℕ→ T
```
and its dependent form
```
structure (ℕ→ )<Ts : ℕ→ *>
  head : Ts.head
  tail : ℕ→ Ts.tail
```

These can be seen as the definitions for function and dependent function types on ℕ respectively:
```
(ℕ → T) := (ℕ→ T)
∀(n : ℕ) T(n) := ℕ→ { n ↦ T(n) }
```

The inductive definition of ℕ also generates the structure of a ℕ-model on an arbitrary type `T`.
```
structure ℕMod<T : *>
  base : T
  step : T → T

ℕModᵁ := Σ(T : U) ℕMod<T>
```
and its canonical instance
```
instance ℕobj : ℕMod<ℕ>
  base: 0
  step: ( ⁺)
```

The displayed models are given by the following structure:
```
structure ℕModᵈ (src : ℕAlg) <T : |src| → *>
  base : T(src.base)
  step : ∀{n : |src|} T(n) → T(src.step n)
```
allowing do define the type of induction motives and the induction operator:
```
def ℕᴹ : ℕ → *
  ℕModᵈ ℕobj

ℕind : ∀(P : ℕ → *) ℕᴹ → ∀(n : ℕ) P(n)
```

Inhabitants of the type `Σ(src : ℕMod) (pm : ℕModᵈ src)` are promorphisms (weak homomorphisms) with source `src : ℕMod` and target given by
```
def target (src : ℕMod) (pm : ℕModᵈ src) : ℕMod<Σ(n : |src|) |pm| src>
  base: pm.base
  step: { n : |src|, x : |pm| n ↦ (src.step n, |pm| (src.step n))}
```

We can define (strong) homomorphisms as the weak ones with contractible fibers `Σ(src : ℕMod, pm : ℕModᵈ src) ∀(n) inContr (|pm| n)`, making the type of ℕ-algebras into a ∞-precategory (Segal type), which turns out to be a ∞-category (Complete Segal type) as it is well-known that the equivalences `(≃) {ℕMod}` of ℕ-algebras correspond to their isomorphisms.

The presented construction generalizes to all inductive types, quotient inductive types and (quotient) inductive(-inductive-recursive) type families. We expect them to work mutatis mutandis for familes over inductive prototypes and positive fibered induction-recursion into arbitrary procategories.

# The motivating example

A homogeneous pair `p : T × T` can be equivalently described as a function `p : 𝔹→ T` on the type with two values.
Heterogeneous pairs `p : X × Y` correspond to dependent functions `f : 𝔹→ { ff ↦ X; tt ↦ Y }`. What about dependent pairs?

With inductive prototypes we can do that! We'll need the following one:
```
prototype 𝔻
  fst : 𝔻
  snd : 𝔻
  snd [dep⟩ fst
```

Its coinductive dual `(𝔻→ )` requires the target to be a procategory `U : *̃`, a mere type `T : *` is not enough:
```
structure (𝔻→ ) <U : *̃>
  fst : U
  snd : (dep : this.fst) →ᵁ U
```

(The operator (→ᵁ) referes to hom-types of the procategory U.)

And here is the dependent coinductive dual:
```
structure (𝔻→ ) <Ts : 𝔻→ *̃>
  fst : Ts.fst
  snd : (dep : this.fst) →ᵀˢ Ts.snd
```

Now we dependent pairs `p : (x : X) × Y(x)` can be expressed as very dependent functions:
```
f : 𝔻→ { fst ↦ X, snd ↦ Y(f(fst)) }
```

Using this approach, we can define a carrier prototype for very dependent sequences:
```
prototype Δ⁻
  0    : Δ⁻
  ( ⁺) : Δ⁻ → Δ⁻

  (n⁺) [dep⟩ n

structure (Δ⁻↦ )<U : *̃>
  head : U
  tail : (dep : this.head) →ᵁ (Δ⁻↦ U)

structure (Δ⁻↦ )<Ts : Δ⁻↦ *̃>
  head : Ts.head
  tail : (dep : this.head) →ᵁ (Δ⁻↦ Ts.tail)
```

We have just defined the very-dependent function types initially introduced by Kopylov et al.

# Handling the contexts and telescopes

Definition of a prototype `I` automatically generates its truncations, the prototypes `↓n` for `n : I`.
In the particular case of `n : Δ⁻`, truncations can be used to describe dependent tuples `t : (↓n)→ Ts`
of length `n`, where `Ts : (↓n)→ *` are the type telescopes of length `n`.

When describing dependent syntaxes with bindings, we introduce types `Expr<ctx : Ctx>` of expressions
in a given context. Contexts are almost the same as telescopes `Ts : (↓n)→ *`, with a minor but very
important difference: an expression `e : Expr<ctx : Ctx>` should be liftable to a larger context
`ctx' : Ctx`, along thinnings `thn : ctx ⊂ ctx'`.

We will define a prototype Δ that extends Δ⁻ by inverse arows representing thinnings `m ⟨thn] n`, generating
embeddings `thn : ↓n ⊂ ↓m` and hence `thn : (ctx : (↓n)→ *) ⊂ (ctx' : (↓n)→ *)`, and also
`thn : Expr<ctx> ⊂ Expr<ctx'>`.
```
prototype Δ
  0    : Δ
  ( ⁺) : Δ → Δ
 
  (n⁺) [dep⟩ n

  (n⁺) ⟨t(f : ↓n)] (f.target)⁺
  (n⁺) ⟨f(f : ↓n)] f.target
```
(Differently from McBride, we only provide constructors for non-identity morphisms.)

```
structure (Δ→ )<T : *̃>
  ..TODO

structure (Δ→ )<Ts : Δ→ *̃>
  ..TODO
```  

Presheaves over Δ, i.e. families `Δ°→*` over the opposite prototype, are known as simplicial types. If we only take the face maps (opposite of thinnings), we get semi-simplicial types, which can be expressed using displayed types as follows
```
structure (Δ⁺°→ )<U : *̃>
  head : U
  tail : this.Z →ᵁ (Δ⁺°→ )ᵈ this
```

With simplicial types and enough combinatorics, we can derive `(P→ )` structures for any prototypes.

**TODO** Say how to understand prototypes as procategories themselves, and how the types ↓n also form
procategories, show examples of functors and show how they form procategories, define products of prototypes
and show how bifunctors are compatible with currying.

* * *

Prototypes are type-theoretical counterparts of Reedy categories.

A prototype `T` is an inductive type `|T| : *` defined mutually with two following inductive-recursive types:
```
Reductions[T] : *, .to : T,   .precompose  : (x : Reductions[.to])  ->  Reductions[T] with .to ≡ x.to
Extensions[T] : *, .from : T, .postcompose : (x : Extensions[.from]) -> Extensions[T] with .from ≡ x.from
```
The definitional equalities must be checked. It is only possible if `t.to` and `t.from` are structurally smaller
then `t`. It ensures that the arguments `x` of the functions precompose and postcompose come from a type that
has already been defined, its constructors are known and values of .to and .from on resulting values can be
explicitly computed. Since compositions of reductions and extensions are represented by composition of functions,
it is definitionally associative. The involution T° on prototypes simply exchanges reductions and extensions.
The prefix operator (↓ ) generates a derived downward prototype for each element `t : |T|` consisting only of
those elements of `|T|` that t can be reduced to, and their respective reductions. The universe of derived
downward prototypes comes for a fixed prototype `T` comes with a prototype structure induced by reductions
and extensions in `T` acting elementwise.

# Inductive prototypes and algebraic theories

## Lavwere algebraic theories

School algebra is about transforming expressions. We learn basic operations such as addition and multiplication and basic identities they satisfy such as associativity, commutativity and distributivity. We compose basic operations to form expressions (= derived operations), and learn how to transform them by composing identities. 

High school algebra only deals with operations on some kind of numbers, in propositional logic we deal with operations (conjunction, disjunction, negation) on propositions. Actually, one can study operations abstractly, i.e. without specifying what they are intended to act on. That's precisely what universal algebra does: it studies finitely-generated sets of operations satisfying finitely-generated sets of identities.

Classical universal algebra usually deals with the case when operations are generated by constants, unary, and binary operations, operands being of the same sort as values. It is possible to generalize to the case with multiple sorts, e.g. vectors and scalars.

To generalize from propositional logic to predicate logic, we need an infinite number of sorts, namely the sort 0 of propositions, the sort 1 of propositions with one variable (unary predicates) and so forth for propositions with any finite number of variables (`n`-ary predicates). Quantifiers `∀` and `∃` bind one variable and thus decrement the sort of their operand. To state it precisely, quantifiers are not just two operations but an infinite families of operations: for any natural numbers `n < m` there is a specific variant of that accept m-ary predicates binding their `n`th hole. Binary operations like the conjunction and disjunction also turn into an infinite family. For unary predicates `R(x)` and `Q(x)` each binary operation * has two variants: it can either join the variables of predicates `R(x) * Q(x)` or leave them distinct `R(x) and Q(y)`. In the general case of an n-ary and and m-ary predicate, there will be many ways to join variables, but it is not hard to derive them all algorithmically. Thus, the set of operations is not finitely-generated anymore, but effectively generated (can be generated by an algorithm). The same applies to the set of identities, since we finite number of identities cannot govern an infinite number of primitive operations.

The gadget to deal with predicate logic algebraically is known as effectively generated Lawvere algebraic theory: it has a subcountable set of sorts, and effectively generated sets of operations and identities. As opposed to the case of finitely-generated theories, presenations of effectively generated ones include algorithms producing operations and identities, which have to be checked for termination and producing expressions of matching sorts, which requires some heavy computational machinery and is almost impossible to do by hand for nontrivial theories.

## Dependent sorts

We already discussed operations acting on numbers, propositions, and predicates. Let us now consider operations acting on even more complex entities: proofs and programs.

A proof calculus is a two-level system somewhat like vector spaces having the sort of vectors and the sort of scalars. The first layer is the layer of propositions and predicates we have already described as a Lawvere theory. In addition there will be the layer of proofs. Similar to propositions, we'll have sorts of proofs with `n` variables for each `n`. But it is more complicated than that. The sort of proof has to include the proposition or predicate being proven! Otherwise we cannot state the most basic operator on proofs `p ▸ q` that composes a proof of `A` and a proof of `A implies B` into a proof of  `B`. Sorts of proofs with `n` variables have to be of the form `Prf(p)`, where `p`is an `n`-ary predicate. We need to have dependent sorts: a feat Lawvere theories cannot accomodate.

In Lawvere algebraic theories, sorts are mere labels governing which expressions can be plugged together. An expression can be plugged into a hole of another expression if the sorts of the expression and the hole are identical. Sorts are syntactical entities, so it's OK to talk about them being identical, after all syntactical entities can be faithfully numbered and “identical” simply means “the same number”.

When generalising to allow value-dependent sorts `S(x)` we run into a problem. Values are semantic entities, so they can be equal without being represented by identical expressions. How can we even talk about sorts being identical or not if they depend on values?

Fortunately, values `x` occurring as sort arguments `S(x)` rules are not just any values, but values given by expressions of the very same theory, so it might be possible to provide an algorithm that computes canonical forms `|x|` so that equal values are mapped to identical normal forms. The operation `|_|` is where the computational aspect really comes into play.

For many sorts of interest it is is not possible to algorithmically extract canonical forms from values. For instance this is impossible for `n`-ary predicates with `n > 0`: word problem for their sorts is semiundecidable in the theory of predicate logic. If two predicates are indeed equal, systematic application of all possible identities will eventually find a path from one to another, but if they are distinct, this process will simply proceed indefinitely; we cannot in general prove distinctness of two predicates due to halting problem, which precludes existence of canonical forms. In such cases we can assign non-canonical normal forms `|_|` to values and find or introduce a (doubly dependent) sort `Id(a, b)` inhabited by identification paths from `a` to `b`. In our case we luckily already have one: two predicates can be shown equivalent precisely if we have a proof of the sort `Prf((A implies B) and (B implies A))`. This sort has to have property that it allows to plug expressions of the sort `Prf(A)` into holes of the sort `Prf(B)` if we have `p : Id(x, y)`, that is `p : Prf((A implies B) and (B implies A))`.

In simple cases can have an algorithm that syntactically transforms expressions of the sort `Prf(A)` into expressions of the sort `Prf(B)`. This is unfortunatelly not alwasy possible. What turns out to be possible is an algorithm that syntactically transform the recipient expression with hole of the sort `Prf(A)` into an equivalent expression with the corresponding hole of the sort `Prf(B)` owing to additional flexibility that this syntactic transform can also change the resulting sort of the expression into an equivalent but non-identical one. Lifting expressions along identification paths gets really tricky when we work with systems where sorts can depend on values themselves being of a dependent sort. Luckily there is a whole well-developed area of mathematics dealing with lifting equivalences over equivalences — the abstract homotopy theory. Below we will establish that effective algebraic theories with dependent sorts correspond to weak model categories the same way as Lawvere algebraic theories correspond to finite-product categories.

## Internal types

The proof calculus we outlined above is internally untyped, i.e. the variables bound by quantifiers do not have type declarations. Requiring a variable to have certain data type can be implemented by logical predicates, like this `(∀(n) isNat(n) implies P(n) )`. 

If we want our proof calculus to support proofs by induction, we have to use typed variables instead:
```
 P : Prop(ℕ)    base : P(0)    step : (P(n) implies P(n + 1))
——————————————————————————————————————————————————————————————
  ℕind(P, step, base) : ∀(n : ℕ) P(n)
```

To accomodate induction we need a much more complicated system of dependent sorts featuring a sort of (internal) data types and a substantial amount of infrastructure. Fortunatelly, the flexibility provided by the notion of dependent sorts and normalization maps `|_|` turns out to be flexible enough even to internalize itself. (Apart from algorithm totality-checking. If a system contains a total “programming language” inside, it cannot be powerfull enough to encode the normalization maps used for defining the system. The only way for a system striving to define its own syntax inside, is to use “levitating” algorithms for the normalization maps, which are interpreted (and termination-checked) by the ambient system and look as black boxed from inside the system, as first introduced in “The Gentle Art of Levitation” by J. Chapman.)

# Preliminaries



* * *

In type theories featuring modal internal parametricity, inductive definitions also come with relational parametricity principles
```
Ipar : (n : □Iᵁ) → (R : IAlgᵈ Iobjᵁ) → (|R| n)
```

where `Iᵁ := ∀(T : U) ℕAlg<T> → T` is the type of Church-implementations, `( ᵁ) : ℕ → ℕᵁ` the recursion operator, and `Iobjᵁ` the Church-encoding like
```
instance ℕobjᶜ : ℕAlg(ℕᶜ)
  base: 0ᶜ
  step: ( ⁺)ᶜ
```

that can be used for instance to derive the classical
```
def m : 𝟙Algᵈ 𝟙objᵁ {id : 𝟙ᵁ ↦ (id ≃ { x ↦ x } }
  point: refl

Theorem ∀(id : □∀(T : *) T → T) id ≃ { x ↦ x }
  𝟙par(m)
```

# Introducing prototypes

To present algebraic theories as above we'll have to define inductive type families like `P(context)`,
where context is list of types of variables the predicate uses. Typically, we'll have to define the
internal type `Ty` of variable types together with an evaluation function `|_| : Ty -> *`, both defined
mutually with type family `P` and what not.

That's how we can define the type of contexts from the type `Ty` naïvely:  
For `n : ℕ` let us use the notation `↓n` for the type of size n usually known as `Fin n`. Given a vector of
types `Con : ↓n -> Ty` we can define a tuple of values of the respective types as `vals : ∀(i : ↓n) |Con(i)|`.

Below we'll introduce the notion of prototypes and introduce a prototype Δ⁺ so that a telescope of types
can be defined as `Con : ↓n -> Ty` for `n : Δ⁺` and type of respective contexts as `∀(i : ↓n) |Con(i)|`.
Moreover, it will be possible to introduce the prototype Δ that additionally keeps track of context
extensions `ext : Con ⊂ Con'` and allows extending functions on `∀(i : ↓n) |Con(i)|` to functions on
`(∀(i : ↓n) Con'(i))` along `ext` automatically.

Prototypes are type-theoretical counterparts of Reedy categories.

A prototype `T` is an inductive type `|T| : *` defined mutually with two following inductive-recursive types:
```
Reductions[T] : *, .to : T,   .precompose  : (x : Reductions[.to])  ->  Reductions[T] with .to ≡ x.to
Extensions[T] : *, .from : T, .postcompose : (x : Extensions[.from]) -> Extensions[T] with .from ≡ x.from
```
The definitional equalities must be checked. It is only possible if `t.to` and `t.from` are structurally smaller
then `t`. It ensures that the arguments `x` of the functions precompose and postcompose come from a type that
has already been defined, its constructors are known and values of .to and .from on resulting values can be
explicitly computed. Since compositions of reductions and extensions are represented by composition of functions,
it is definitionally associative. The involution T° on prototypes simply exchanges reductions and extensions.
The prefix operator (↓ ) generates a derived downward prototype for each element `t : |T|` consisting only of
those elements of `|T|` that t can be reduced to, and their respective reductions. The universe of derived
downward prototypes comes for a fixed prototype `T` comes with a prototype structure induced by reductions
and extensions in `T` acting elementwise.

The type of functions `X -> Y` from a prototype is undefined unless `Y` is a universe (thus a category),
in which case `X -> Y` is a universe as well. Now assume `F[t : T]` is a type dependent on prototype `T`.
Values of type `x : F[t : T]` come with actions of reductions `rx : F[r.to]`, where `r : Reductions[t]`.

Functions `f` on `F[t : T]` have to provide action on “higher constructors” `e : Extensons[x]` yielding
either a path `f(x) = f(e.from)` or an extension `Extension[f(x)]` with .from ≡ f(e.from).

Type-valued functions `F[t : T]` on prototypes are defined using the following form of elimination:
Constructors `c : |T|` should map to types dependent on `((r : Reductions[c]) -> F[r.to])`, with inner occurences
of `T` being transfored to `Tᵈ t`. Higher constructors `e : Extensons[t]` should evaluate to maps `F[e.from] -> F[e]`.

**TODO:** Explicitly state induction motives for prototypes using ( ᵈ)-transformation and show definitions of inductive type families over prototypes work. Describe functors (functions A (-> B -> ··· ) -> Z, where A..Z are prototypes), and universes of prototypes.

**TODO:**
Ask Nikolai Kudasow how this stuff fits with rzk-lang.github.io, that  essentially deals with what we call “universes” here, i.e. synthetic categories.
