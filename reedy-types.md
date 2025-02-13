Type-theoretical approach to Reedy categories  (draft)
======================================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)

Building on the unpublished ideas of C. McBride, we propose a novel extension for Martin-Löf Type Theories (MLTTs) we'll call inductive prototypes, a type-theoretic internalization of Reedy categories. Inductive type families indexed over prototypes provide effective machinery for dealing with syntaxes that include binding, and become indispensable when internalizing the syntax and semantics of type theories themselves. Bidirectional presentations of dependent type theories turn out to be inductive-inductive-recursive definitions. There, the inductive type families of expressions and normal forms are defined simultaneously with inductive prototypes representing their contexts and reduction rules framed as recursive functions on expressions.

Semantically, fibered quotient inductive-inductive type definitions (FQIITs) are effective presentations of weak model categories whose structure-preserving functors correspond to elimination motives. In strong analogy to the functorial semantics of Lavwere algebraic theories, these functors themselves form a category of models, with their natural transformations serving as model homomorphisms.

In a subsequent paper, we hope to establish the existence of initial models in an arbitrary (∞,1)-topos, thus providing a semantics for FQIITs. We assume it to be conditional on the existence of an inaccessible/Mahlo cardinal for small/large FQIITs, respectively, and unconditional for finitary FQIITs. It would also uniformly solve the initiality conjecture for structures admitting an effective bidirectionally algebraic definition, including [weak ω-categories](https://arxiv.org/abs/1706.02866), [virtual equipments](https://arxiv.org/abs/2210.08663), and (∞,1)-toposes once the Higher Observation Type Theory (HOTT) is complete.

§ Introduction
--------------

Natural deduction, an inference rule-based proof calculus can be seen as a precursor of Martin-Löf type theories.
The inference rules of natural deduction serve as a typed generative grammar for proof terms, where the propositions being
proven serve as types of the respective proofs:
```
  a : P     b : Q
————————————————————
 (a, b) : (P and Q)
```
— ‘given a proof term `a` which proves the proposition `P` and a proof term `b` which proves the proposition `Q`, we can construct a proof term denoted `(a, b)` which proves the proposition `P and Q`’. This notation for inference rules still universally used today was first introduced for natural deduction.

Another forerunner of Martin-Löf type theories is Gödel's System T: it's a Simply Typed λ-calculus `STLC(ℕ)` over the inductive data type of natural numbers. In fact, it's a fragment of all Martin-Löf type theories. There, the types of expressions are data types.

The other constituent of Martin-Löf type theories comes from the first type theory ever introduced, the Russell's theory of types introduced at the very beginning of XX century as a way to deal with Russell's paradox: the type universes. It was long assumed that the universes of data types in Martin-Löf type theories can be seen as data types themselves, but due to a discovery made by Martin Hofmann and Thomas Streicher in 1996 it began to become apparent that it's not the case: the universe of datatypes neccesarily carries additional higher structure, and ignoring this structure makes the theory either undecidable or unsound. Datatypes can be equivalent in inequivalent ways: for instance, the functions `id := { x ↦ x }` and `negate = { tt ↦ ff ; ff ↦ tt}` are nonequivalent automorphisms of the datatype `𝔹 = {tt, ff}`. Starting with the universe `U₀` of datatypes we can construct other “non-data” types, e.g. the universe of pointed data types `Σ(T : U₀) T` or the universe of `U₀`-polymorphic datatypes `U₀ → U₀`. The next type universe `U₀⁺` is obviously also a “non-data” type. Ultimately, Voevodski understood that the types of Martin-Löf type theories represent anima (also known as ∞-groupoids), with data types and propositions being special cases of 0th and -1th truncation levels respectively.

Long before this realization of Voevodski's, enthusiasts of the category-theoretic approach to the foundations of mathematics wondered whether it was possible to generalize MLTTs to accomodate categories as types. Subsequently, approaches were indeed found, but as often happens in the absence of applied motivation, this developments remain rather obscure and hard to grasp. The situation changed in 2021 when C. McBride presented a number of (still unpublished as I'm writing this) ideas in his Topos Institute Lecture [“Cats and Types: Best Friends?“](https://youtu.be/05IJ3YL8p0s). It turns out, it is very tempting to use types with directed higher structure as indexes for inductive data type families. This idea immediatelly resonated to our decade-long endeavour to conceptualize dependent type theories as an advanced form of algebraic theories  



§ Introduction
--------------

The term _Construction Calculi_ employed in the title of this paper refers to type theories that are intended to
serve as general purpose structuralist foundations of mathematics. At times we will also mention type theories not striving to serve as general
purpose foundational frameworks, these will be referred to as _domain-specific type theories_.

Construction calculi must be able to express typed higher-order intuitionistic logic including proofs, formal
axiomatic theories (such as Eucledian geometry) together with formal constructions on them,
and, last but not least, canonical mathematical objects such as natural and (analytic) real numers, lists, sequences,
functions, relations, etc. Such canonical objects are introduced there as inductive types.

In type theory, _inductive types_ are the types freely generated by a set of possibly recursive _generators_:
```
#Inductive Nat
  0    : Nat
  (_') : Nat → Nat
```

Here, the type `Nat` is introduced as the type freely generated by one non-recursive generator `0` satisfying
type `Nat` and a recursive generator `(_')` (writen as a postifx apostrophe) satisfying type `Nat → Nat`.
With this definition, the set of possible Nat-values is given by
&nbsp; `0, 0', 0'', 0''', ...`
The symbols `0` and `(_')` do not refer to some predefined entities, they are introduced right here as generators
of the type `Nat`. They represent nothing other than themselves and are irreducible. There is nothing else happening
“behind the schenes” - this definition of natural numers is entirely self sufficient.

Inductive types may be _polymorphic_, i.e. have typal _parameters_ (enclosed in square brackets):
```
#Inductive List[\T]
  Empty : List[T] 
  (::)  : T → (List[T] → List[T])
```

This definition introduces the polymorphic type `List[T]` parametrized by the type `T` of its elements.
The backslash in `\T` in the first line is the so-called _freshness marker_, it is there to express that `T`
is not a constant that was defined somewhere else, but a variable being introduced at this very spot.
The second line introduces a rather boring generator `Empty` of `List[T]`, much like 0 in the definition
of `Nat`. The third line introduces an infix operator `::`, which is a family of recursive generators:
For each inhabitant `\head : T` it defines a recursive generator
&nbsp; `(head ::) : List[T] → List[T]`
that appends `head` to the right hand side argument `\tail : List[T]` so that non-empty lists can be
generated by succesively appending elements to the `Empty` list. Note, that the arrow operator is
right-associating, so `T → (List[T] → List[T])` can be written simply as `T → List[T] → List[T]`.
The `Empty` together with the infix operator `::` defines the following set of possible values of `List[T]`:
&nbsp; `Empty, x :: Empty, x :: y :: Empty, x :: y :: z :: Empty, ...`
for `x, y, z, ...` being of type `T`.

There is also a notion of _inductive type family_ indexed over an other inductive type. For example,
consider the type family of vectors of fixed length:
```
#Inductive Vec[\T] : Nat → *
  Empty : Vec[T](0)
  (::)  : T → Vec[T](\n) → Vec[T](n')
```

Here `Vec[\T]` is not one type, but a whole _family of types_, one for each natural number `n`.
The “type” `Nat → *` of `Vec[T]` means “a function mapping natural numbers to types“ modulo issues stemming from
the fact that the class of all types `*` is not really a type itself and requires somewhat careful handling to avoid
running into the [barber issues](https://en.wikipedia.org/wiki/Barber_paradox).
The generator `Empty` satisfies the type `Vec[T](0)`, vector of `T`s of zero length.
Generators `(head ::)` take a vector `\tail : Vec[T](\n)` of _some_ length `\n` (notice the
freshness marker) and generate a vector of length `n' = n + 1`.

In this work, we propose to introduce inductive types indexed over more general entities than
inductive types themselves. We will call these entities index types. Index types are inductive types
endowed with a (correct by construction, inductively generated) structure of a Reedy category
with connections.
We built upon an unpublished idea of Conor McBride presented in his Topos Institute Lecture
[“Cats and Types: Best Friends?“](https://youtu.be/05IJ3YL8p0s). The inductive type famlies of
the kind `I → *` for an index `I` will semantically correspond to type-valued presheaves on
`I` as Reedy category.

Using this notion, we will be able to construct various very useful indices:
- The index CatCarrier so that `[\C : CatCarrier → *] ≡ [\Ob : *, \Mor : Ob → Ob → *]`;
- For each notion of n-categories (globular, simplicial, cubical, etc) the index `nCatCarrier[n]` so that
```
  [\C : CatCarrier → *] ≡ [
    \C.Cell(0) : *
    \C.Cell(1) : Ob → Ob → *
    \C.Cell(2) : ···
    ...
    \C.Cell(n) : ···
  ]
```   
  allowing to define n-categories and n-functors between them generically for all n. 
- The indices Δ⁺ and Δ such that inductive type families of the kind `Δ⁺ → *` and `Δ → *` will correspond
precisely to semi-simplicial and simplicial types respectively, which allows to define ω-categories and
various other interesting structures. In particular, one covers the notion of _very-dependent types_ as
introduced by Jason J. Hickey and A. Kopylov, cf. [“Formal objects in type theory using very dependent
types.”](https://www.cs.cornell.edu/jyh/papers/fool3/paper.pdf).

§ Construction Calculi: An Overview
-----------------------------------

Before proceeding to our contributions let us breefly chart the landscape of types and principles of
Construction Calculi. This overview should be skipped by readers versed in type theory.

***

The introduction only mentions inductive types. These are used in construction calculi to represent canonical
mathematical objects such as natural numbers, real numbers, various combinatorial objects (e.g. graphs), and
finitary collections of objects of known type, e.g. pairs of integers, couples (unordered pairs) of rationals,
(finite) lists of reals, etc. Inductive types are “built from below”, i.e. it is exactly knows what their values
are and how are they built from ground up.

However, inductive types are insufficient to describe everything of mathematical interest; one also needs the
dual notion of behavioral types.

§§ Behavioural types
--------------------

Behavioural types are described not in terms of generators, but in terms of _extractors_. For behavioral types
one remains completely agnostic about nature of their values as long as they provide a given “interface“.
Consider the type of possibly infinite streams:
```
#Structure Stream[\T]
  head : T
  tail : Maybe[ Stream[T] ]

 where
#Inductive Maybe[\T]
  Nothing      : Maybe[T]
  Value(t : T) : Maybe[T]
```

Here the inhabitants of the type `Stream[T]` are defined as values with distinguished extractors `head`, yielding
a value of the type `T`, and `tail` yielding either the rest of the stream or the value `Nothing` if the stream
ends there.

Values of behavioural types can be defined by specifying actions of their extractors (recursion is allowed). To give
an example, let us define the Fibonacci sequence. Fibonacci sequence is a sequence, in which each number is the sum
of the two preceding ones. A Fibonacci sequence can be defined for any two initial elements, while _the_ Fibonacci
sequence is a Fibonaccy sequence starting with 0 and 1:
```
(0,) 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, ...
```

Here is how to define them:
```
#Defintion fibonacci-sequence(\current \previous : Nat) : Stream[Nat]
  head ↦ current
  tail ↦ fibonacci-sequence((current + previous), current)

#Default the-fibonacci-sequence
  fibonacci-sequence(1, 0)
```

The `fibonacci-sequence(n, m)` yields `n` when asked for a `head` and `fibonacci-sequence(n + m, n)` when asked for
a tail.

In contrast to values of purely inductive types (which are exhausted by ones that can be explicitly written down),
a value `s : Stream[Nat]` might be a thing “living outside of the computer” being “measured” by `head` and
`tail`: it can be shown constructively that the type `Stream[Nat]` is not exhausted by values that can be written
down inside the theory: the type of `Nat`-streams is uncountably infinite. Note that equality of two streams
`s g : Stream[Nat]` cannot be in general verified in finite time. Verifying such equality would in general require
checking equality for an infinite number of terms.

§§ Functions
------------

Functions `f : X → Y` can be seen as a trivial case of behavioral types: behavioral types with a family
of non-recursive extractors parametrized by values `x : X`:

```
#Structure Function[\X \Y]
  apply : X → Y
```

While it is customary in computer science to use the word “function” for partial functions (that might sometimes
fail to yield a result), in mathematics and type theories by “functions” one means total and deterministic
functions.

That's how one defines a function on `Nat`:

```
#Define double : Nat → Nat
  0 ↦ 0
  (\n)' ↦ ( double(n) )''
```

Functions on inductive types can be defined by exhaustive pattern matching on their generators. In case of recursive
generators, definitions are allowed to be inductive. In the above example one performs an exhaustive pattern matching
on natural numbers: natural number is either a zero `0`, or a successor `n'` of a natural number `n`. Thus one
defines the value of the function `double` case by case first for zero `0` and then for successor `n'`, in the latter
case the value `double(n)` is already “known”. The ability to define functions (terminating, total functions) on
inductive types by pattern matching on their generators reflects the defining property of inductive types: their
values are guaranteed to be finitary compositions of their generators.

§§ Mixed Inductive Types and Purely Inductive Types
---------------------------------------------------

Types are not either inductive or behavioral, but can be mixed. For example the types `List[ Stream[Nat] ]` and
`Maybe[Nat → Nat]` are not purely inductive any more. Now let us define purely inductive types because of their
remarkable properties.

§Definition
  Purely inductive types are inductive types that either a finite number of generators (like `Nat`) or
  only generator families parametrized by other purely inductive types. For example, the types `Nat`,
  `List[Nat]` and `List[List[Nat]]` are purely inductive, while the type `List[Nat → Nat]` is not pure.

From within the type theory purely inductive types can be shown to be either finite, or countably infinite,
that is, their values can be explicitly numbered by natural numbers `n : Nat` in one-to-one fasion.
Equality of values of purely inductive types is decidable, that is checkable algorithmically in finite number
of steps.

Metatheoretically, that is from outside of the respective type theory, one strives to show two desirable
computational properties of the type theory: normalization and canonicity. Normalization means that any
expression of the type `T` eventually reduces to the normal form, while canonicity means that for purely
inductive types `T` the only normal forms in empty context are given by finite compositions of respective
generators.

§§ Propositions
---------------

Consider the degenerate inductive type

```
#Inductive Empty : *
  /- no constructors -/
```

This type has no constructors, and in a type theory enjoing normalization and canonicity there can be no expressions
of the type `Empty` in empty context. At the same time, a function on this type can be defined (as it is the case
for all inductive types) by pattern matching. Yet there are no constructors to match, such function has no body
and still exists. In fact there is a unique function from `Empty` to any type `X`. Fortunatelly in cannot be ever
applied because the type `Empty` has no inhabitants.

In Construction Calculi logical propositions are represented by types inhabited by proofs of respective
propositions. A function `P → Q` between two propositions by definition turns proofs of `P` into proofs of `Q` and
therefore is a proof that `P` implies `Q`. The type `Empty` encodes the canonical false proposition: it has no
proofs and if there were any proofs, (remember, there is a function form `Empty` into any type `X`) it would allow
prove all propositions `P` (together with their negations `¬P`, because they are propositions as well) therefore
trivializing the theory, which is known in logics as “ex falso quodlibet”. By the way, negation of an arbitrary
proposition `P` can be defined as `P → Empty` (`P` is false exactly we we can derive contradiction by assuming it
to be true).

All provably false propositions can be shown to be equivalent to `Empty`. One direction (`X -> Empty`) comes from
the definition of negation, the other direction (`Empty -> X`) comes from the fact that there is a function from
`Empty` into any type. The same way, any true propoistion can be shown to be equivalent to the type `Unit` with
exactly one element. Yet, one cannot show constructively that the class of all propositions `Ω` is exhausted by this
two variants: besides provably false propositions and provably true ones there can be unsettled ones. To give a
concrete example, existance of sets strictly larger than the set of natural numbers and strictly smaller than the
set of natural number sequences is an example of such an unsettled proposition in the standard set theory. It is known
as Continuum Hypothesis. Existence of such sets are neither implied nor ruled out by axioms of the set theory.
There are models of set theory without such sets and there are other perfectly valid models where such sets exist.
Futhermore, one cannot settle all propostions by adding more axioms. Gödel's incompleteness theorem states that any
axiomatic theory (with finitely generated system of axioms) has unsettled propositions at least if is powerful enough
to express natural numbers and multiplication on them as an operation.

While all true propoistions are alike and all false propositions are alike, unsettled propositions are unsettled in
their own way. It might be that one unsettled proposition is implied by another (e.g. Generalized Continuum
Hypothesis implies the Continuum Hypothesis, yet both are independent of standard set theory), or they can be
completely independent. Constructively, the class of all propositions `Ω` is bounded lattice ordered by implication
`P → Q` with bottom element `Empty` (the false proposition) and top element `Unit` (the true proposition).

The “class” of canonical propositions `Ω` is seen a as a type in most Construction Calculi. It is neither an
inductive type, nor a behavioural one, and its inhabitants are themselves types (propositions are considered
to be types inhabited by their respective proofs). Such types (types of types) are known as universes and form
a third large group of types besides inductive and behavioral ones.
For any type `X` there is a proposition “`X` is non-empty” that we'll denote `⁰X : Ω`.

As it was already mentioned, the class of canonical propositions `Ω` contains `Empty` and `Unit` and is closed
under forming functions: for `P Q : Ω`, `(P → Q) : Ω`. It is also closed under forming pairs: a pair of proofs,
one for `P` and one for `Q` is precisely the proof of `P ‹and› Q`. Via implication and negation one can also define
the logical conjunction `(‹or›)`. Together with these two logical conjunctions the type `Ω` forms a Heyting algebra,
moreover a complete one as we will see in the next section.

All constructive calculi of concern are compatible with axiom of double negation elimination `¬¬P → P` that
trivializes all structure mentioned above to classical logic where every proposition has a definite truth value
`true` or `false`, yet this axiom is not derivable constructively in general. Double negation elimination can be
derived constructivly only for a very narrow class of propositions: namely, for decidable ones (these are the ones
that can be at least in theory checked algorithmically).


§§ Predicates, Relations and Dependent Functions
------------------------------------------------

By means of indexed inductive types one can define predicates on inductive types:

```
#Inductive (_-is-even) : Nat → Ω
  zero-is-even : (0)-is even
  suc-of-suc-is-even(\n : Nat) : (n)-is even → (n'')-is even
```

Here, the predicate `-is-even` on natural numbers is defined. Equivalently, for each natural number `n`
a type `(n)-is-even` is defined, which can be either empty (which stands for false) or have an inhabitant
(which stands for true). Here the generator `zero-is-even` populates `(0)-is even`, and the generator
`suc-of-suc-is-even(\n : Nat)` populates `(n + 2)-is even` whenever `(n)-is even`.

While this particular predicate is decidable (one can easily check if a given number is even), in general
inductively defined predicates are not. For example it is still unknown (cf. Collatz conjecture) if the following
predicate is decidable:
```
#Inductive Collatz-Terminating : Nat → Ω
  ct-zero : Collatz-Terminating(0)
  ct-one : Collatz-Terminating(1)
  ct-half(\n) : Collatz-Terminating(n) → Collatz-Terminating(2 · n)
  ct-triple-plus-one(\n) : Collatz-Terminating(3 · n + 1) → Collatz-Terminating(n)
```

In dependent type theories there are so called dependent function types:
For a type `X` and an inductive type family `Y : X -> *` indexed over it, there is a type
`∀(\x : X) Y(x)`, read “for each `x` of `X` a value of the type `Y(x)`”. For example the function
&nbsp; `f : ∀(\n : Nat) Vec[Nat](n)`
must yield a vector of length `n` for each number `n`.

When applied to predicates, dependent functions reassemble universal quantifier. The type
```
∀(\n : Nat) Collatz-Terminating(n)
```

is populated precisely by functions yielding a proof of `Collatz-Terminating(n)` for each `n`,
that is precisely by constructive proofs of the proposition `∀(\n : Nat) Collatz-Terminating(n)`.

To give an example how such proofs might look like consider an easier proposition:
```
∀(\n : Nat) ( double(n) )-is-even
```

Functions are defined by pattern matching, and we are allowed to unfold the definition of `double` while matching:
```
#Define prf : ∀(\n : Nat) ( double(n) )-is-even
  0 ↦ zero-is-even : ( double(0) )-is-even
  (\n)' ↦ suc-of-suc-is-even( double(n) )(prf(n) : ( double(n) )-is-even) : ( double(n') )-is-even
```

The class of all propositions `Ω` is closed under forming arbitrary conjunctions:
For any family of propositions `P : X → Ω` one can form `∀(\x : X) P(x) : Ω`, which means Ω is a complete Heyting
algebra. Since `Ω` a complete Heyting algebra and contains any proposition definable within HOCC, subsets of any type
`T` can be identified with predicates `P : T → Ω`. In fact, we can even introduce special syntax for subset types
`{\t : T | P(t)}`, for example `{\n : Nat | (n)-is-even ‹and› (10 ≤ n) }`.

Relations on a type can be defined as twice indexed inductive types:
```
#Inductive (≤) : Nat → Nat → Ω
  ≤-refl(\n) : (n ≤ n)
  ≤-succ(\n \m) : (n ≤ m) → (n ≤ m')
```

Each inductive type automatically comes with an indictively defined equality relation that exactly follows the pattern
of its generators. For example, for the type of natural numbers the equality relation can be given as:
```
#Inductive (=) : Nat → Nat → Ω
  0= : (0 = 0)
  (\n)'= : (n' = n')
```

For behavioral types, equality follows the pattern of their extractors and represents observational equivalence,
that is two “things” are equal precisely if applying same extractors yields same values. In particular, for two
functions `\f \g : X → Y` the equality is given by
```
pointwise : ∀(\x : X) f(x) = f(y)
```

In particular, in type theory two implementations of the same function are considered equal as functions if they
yield equal results on equal arguments.

<!--
§§ Quotient inductive types and Identification types
----------------------------------------------------


Quotient inductive types are the types given in terms of generators and _relations_.
Here is an example of unordered pair:
```
#Inductive UPair[\T]
⦃_,_⦄ : T → T → UPair[T]
swap(\a \b : T) : ⦃a, b⦄ = ⦃b, a⦄
```

decidable type checking -> decidable proof checking

Correspondence `A → B → *` with given left and right inverses `lt : B -> A, rt ; A -> B`.

-->
§ Index types
-------------

As we've seen, some inductive types have only finite number of generators (like `Nat`) while some have
generator families (like `(head ::) : List[T] → List[T]`) that can be infinite, possibly even uncountably infinite.

Purely inductive types are inductive types that either a finite number of generators (like `Nat`) or only generator
families parametrized by other purely inductive types. For example, the types `Nat`, `List[Nat]` and `List[List[Nat]]`
are purely inductive, while the type `List[Nat → Nat]` is not pure.

Purely inductive types have decidable equality and either finite or countably infinite. As types with decidable
equality, they satisfy the UIP-condition `∀(a b : T) ∀(e o : a = b) e = o`, which is known as Hedberg's theorem.
**[UIP Dicussed above]**
This property is very useful for indexes. Consider a type `I` and an inductive type family
`P : I -> *` indexed over it. If `I` is satisfies UIP by definition, and one establishes for two its elements
`\a \b : I` that `\e : (a = b)`, values of `\x : P(a)` uniquely correspond to values `P(b)`. This correspondence is
given by operator called `coerce[P](e, x) : P(b)`. Without UIP, one can show that `P(a) = P(b)` and ...


Purely inductive types have one more important metatheoretical propery we'll make use of: inhabitants of each purely
inductive type are naturally ordered lexicographically, and this order is guaranteed to be well-founded. That is,
metatheoretically we have a function `deg(i)` that maps inhabitants to a countable ordinal.

... несколько слов о том, зачем следующее определение

An index type `I : Idx` is a purely inductive type `|I|` with additional structure of inductively generated
correct-by-construction Reedy category, that is for each inhabitant `\i : I`,
- a purely inductive type `⬇(i)` of outbound “dependency arrows“ together with a function `target : ⬇(i) → I`,
dependency arrows are required to lower degree `deg(i)` of inhabitants.
- for each arrow generator `g : ⬇(i)` a function `g^ : ⬇( target(g) )` such that application `^g(x)` and composition
`^g ∘ ^x` agree definitionally. This way we ensure provide computation rules for composition of depency arrows that
are definitionally associative.
- a purely inductive type `⬆(i)` of  inbound “inclusion  arrows” together with a function `source : ⬆(i) → I`.
Inclusion arrows are required to raise the degree.
- Dually we require composition rules for inclusion arrows.


§ The case of degeneracies
--------------------------

One type one often encounters is the type of number sequences containing only finite number of nonzero terms.
For many practical applications one needs to have an upper bound on the number of nonzero entries, because it
cannot be calculated in predictable time if unknown even in case of integer number sequences; in case of real
number sequences it is not computable at all since the check an unknown decimal fraction represents a zero
requires an infinite number of steps. So we need an inductive type family
`FiniteVec[\T : *, \zero : T] : NatBound → *` of sequences parametrized by the type `T` of its terms, marked
`zero : T` element of that type and indexed over an upper bound to a number of non-zero terms.

The index `NatBound` ist not just a natural number. We want `FiniteVec[T,0][n]` to be a subtype of
`FiniteVec[T,0][m]` whenever `n < m` and we want trailing zeroes to be ignored, i.e. sequences `(x :: y :: z)`
and `(x :: y :: z :: 0)` to be equal.


**WARNING: THE REST OF THIS SECTION HAS TO BE REWRITTEN AFTER THE PREVIOUS SECTION HAS BEEN COMPLETED**

<!--

  In order to get there let us define the following index:
  ```
  #Index NatBound
  0    : NatBound
  (_') : NatBound → NatBound

(\n : NatBound) Extend(\m : Nat)⟩ : (n.rec m (_'))

|Extend(\m : Nat)⟩ |Extend(\p : Nat)⟩ ↦ |Extend(n + m)⟩
```

The indices itself are freely generated by generators `0` and `(_')` just as natural numbers. But additionally
there are embedding arrows `|Extend(m)⟩` going _from_ to each element `(\n)`. Together with arrow source `(n)`
the argument `m` of the arrow generator `|Extend(m)⟩` allows to compute which element the arrow goes _to_.
Embedding arrows are required to go from structurally smaller elements to structurally larger ones, that is
they comply with natural lexicographic ordering on the inhabitants of `NatBound`. For each pair of composable
arrow generators we have to provide the computation rule for the composition (last line of the definition above),
which has to be strictly associative. All in all, while it might be unclear which set of arrows goes into
a particular element, but the set of right arrows going _from_ an element is a freely generated (inductive) type,
and each arrow computes its target.

When one defines a type indexed over an index with embeddings, one has to provide degeneracy maps for each embedding:
```
#Inductive FinNatVec : NatBound → *
Empty : FinNatVec(0)
(::)  : Nat → FinNatVec(\b) → (b')

Empty            |Expand> ↦ (Empty :: 0)
(\head :: \tail) |Expand> ↦ (\head :: (\tail |Expand>))
```
-->

§ The case of dependencies
--------------------------

Simple mathematical structures are defined above a carrier which is simply a type:
```
#Structure Monoid[\T : *]
   unit : T
   (∘)  : T → T → T
   
   associator(\x \y \z : T)
   : (x ∘ y) ∘ z = x ∘ (y ∘ z)
   
   lt-unitor(\x : T) :
   : unit ∘ x = x
   
   rt-unitor(\x : T) :
   : x ∘ unit = x
```

More advanced mathematical structures seem not to have a single carrier:
```
#Structure Cat[\Ob : *, \Mor : Ob → Ob → *]
   id[\T]        : T
   (∘)[\A \B \C] : Mor[B][C] → Mor[A][B] → Mor[A][C]

   lt-unitor[\A \B](\f : Mor[A][B]) :
   : id[A] ∘ f = f
   
   rt-unitor[\A \B](\f : Mor[A][B]) :
   : f ∘ unit = f

   associator[\A \B \C \D](\f : Mor[A][B], \g : Mor[B][C], \h : Mor[C][D])
   : (h ∘ g) ∘ f = h ∘ (g ∘ f)
```

For a multitude of reasons it is desirable to represent the whole parameter
list `[\Ob : *, \Mor : Ob → Ob → *]` as a single carrier `[C : CatCarrier → *]`.

For this purpose we'll need an index type `CatCarrier` with dependency arrows:
```
#Index CatCarrier:
  Ob  : CatCarrier
  Mor : CatCarrier
  
  Mor [Src⟩ Ob
  Mor [Tgt⟩ Ob
```

Dependency arrows are exactly the same as embedding arrows except that they comply with inverse lexicographic
ordering on the index, i.e. they are well-founded, all composable strings of dependency arrows are finite.
It is precisely this finiteness that allows us introduce a special restriction operator (↾): any indexed
typeformer `T : I → *` and index `\i : I` the restriction (T ↾ i) denotes a dependent record type:
its extractors are given by dependency arrows form `i` and map to their respective types as given by `T` and
previous entries of the dependent record. Now given an indexed typeformer `T : I → *` we have `T(i) : (T ↾ i) → *`.
For indices with no dependent arrows, (T ↾ i) is the unit type, thus one has simply `T(i) : *`.

In particular, given a typeformer `T : CatCarrier → * `, we have
```
T.Ob : *
T.Mor : (T ↾ Mor) → *
 ⇕
T.Mor : {src : Ob, tgt : Ob} → *
```

Here is how one defines and an inductive type indexed by CatCarrier:
```
#Inductive SmallPointedTypes[\I : CatCarrier]
  PointedType(\T : 𝒰, \p : T) : SmallPointedTypes[Ob]
  PointedFunction(\X \Y : 𝒰, \x : X, \y : Y, f : (X → Y), pointedness : ( f(x) = y ))
  : SmallPointedTypes[Mor][(Src ↦ PointedType(X, x); Tgt ↦ PointedType(Y, y)]
```

So far we only considered finite dependent indexes, let us consider an infinite one to illustrate the concept better.

The canonical example of an infinite index is given by `Natⱽ`:
```
#Index Natⱽ:
  0    : Natⱽ
  (_') : Natⱽ → Natⱽ → Natⱽ
  
  (\n)' [Field(\m : Fin[m])⟩ m
  
  [Field(\m)⟩^: (_ ↦ [Field(m)⟩)
```

For a given `T : Natⱽ → *` the restriction `T ↾ n` is the dependent record
```
field(0) : T(0)
field(1) : T(1)[ field(0) ]
field(2) : T(2)[ field(0) ][ field(1) ]
...
field(n - 1) : T(n - 1)[ field(0) ]...[ field(n - 2) ]
```

Thus, `T : Natⱽ → *` gives us precisely the so called very dependent types. A very-dependently typed sequence
of such type `T` can be defined as follows:

```
#Coinductive DepSequence[\T : Natⱽ → *]
  head : T(0)
  tail : DepSequenceTail[T, 1, (field(0) ↦ head)]

 where

#Coinductive DepSequenceTail[\T : Natⱽ → *][\i : Nat][\prefix : (T ↾ n)]:
  head : T(i)[prefix]
  tail : DepSequenceTail[T, i', prefix ++ (field(i) ↦ head)]
```

Analogously one can define the index Δ⁺, so that `Δ⁺ → *` are semi-simplicial types. By combining dependencies
and degeneracies one can also define the simplicial types. In general, we'll be able to form presheaves over any
explicitly definable Reedy-categories, and with those, we can provide definitions for any higher categorical objects.

It still remains to be worked out which additional machinery is required to work with dependencies and embeddings
properly: for example we used the operator `(++)` to extend a restriction `(T ↾ n)` to a restriction `(T ↾ n')`.

It also remains to be worked our how to provide codes for indexes and type families indexed over them so that we
can mutually define type universes and index universes closed under them, and ensure the metatheoretical property
that any type of our extended system eventually lands in a sufficiently large universe.

§ Functions on Indices and Functoriality
----------------------------------------

... The example of monoidal structure on semi-simplicial category, using CAST operator

§ Kinds and Universes
---------------------

Above we used a special type definition language to define inductive types, including polymorphic and indexed ones.
As a result of type definition we obtain a typeformer (e.g. `Nat` and `List`) of a specific kind that looks like
a type but is not exactly a type:
```
Nat : *
List : * → *
Vec : * → Nat → *
```

Unfortunatelly, in type theory we are not allowed to have the type `*` of all types, as it would necessarily
contain itself which leads to a contradiction. There is no better alternative to having an separate “type system”
of kinds for typeformers.

A type former is either a simple type, a polymorphic one (parameters might themselves be of any kind),
or an indexed one therefore valid typeformer kind is given by:
- `*`
- `index → kind`
- `kind → kind`

Here the signatures on the right hand side may depend on paramerers on the left hand side, and any normal type
can be used as an index. Here are some examples of valid signatures:
```
  *
  Nat → *
  * → *
  * → (Δ⁺ → *)
  (\T : *) → Monoid[T] → *
```

While we cannot speak about _the type of all types_, one can very well speak about types containing some other
types without pretending to contain all of them. Such types are called type universes.
In particular, HOCC we postulate a distinghished type universe `Prop` that contains an isomorphic copy of each
type `P` satisfying `∀(\x y\ : P) x = y`, it is each type with at most one element. Subsets of any type `T` have
the form `{\x : T | P(x)}` for some `P : T → Prop`, i.e. the type `(T → Prop)` can be said to classify subsets of
any type `T`. The universe `Prop` is said to be subset (or subobject) classifier. They type `Prop` has at least
two distinct elements: the empty type 𝟘 and the unit type 𝟙 (the type with the unique inhabitant denoted `• : 𝟙`),
and thus does not belong to itself.

For each universe `𝒰` we define `𝒰⁺` to be the universe containing
1) all inhabitants of `𝒰`,
2) the type `𝒰` itself
and is closed under
3) forming a certain class† of inductive types, including dependent pairs `Σ(\x : X) Y(x)` for `X Y(_) : 𝒰⁺`,
4) forming a certain class† of behavioral types, including dependent functions `∀(\x : X) Y(x)` for `X Y(_) : 𝒰⁺`,
5) forming types of identifications `(a = b)` between inhabitants `a b : T` of their types `T : 𝒰`.

Now one can generate a cumulative hierarchy of universes
```
Prop ⊂ Prop⁺ ⊂ Prop⁺⁺ ⊂ ···
```

The universe Prop⁺ and all higher universes in this hierarchy taken together with maps between their types as
categories are in fact elementary toposes*: (2) guarantees them to have subobject classifier, (3) guarantees them
to have a natural number object, (3 + 5) guarantees them to have all finite limits `Σ(\x : X, \y : Y) f(x) = g(y)`,
(5) makes them locally cartesian closed (`∀(\x : X) Y(x)`).
All purely inductive types including `List[Nat]`, `List[List[Nat]]` and similar much more complicated ones land
in the very first nontrivial universe `Prop⁺` since they are countable and discrete by construction which makes
them isomorphic either to the type of natural numbers `Nat` or to a finite type `Fin(n)` which are already present
in `Prop⁺`.

[Except for the principle of Unique Choice]

We can develop a system of codes for all inductive and coinductive types definable in our type definition language
and adjust the closedness requirements (3) and (4), to ensure the following metatheoretic property:
> For any finite number of definable typeformers, there is a universe in the above hierarhcy, that contains
them/is closed under applying them.

Now we can give metatheoretic meanings to kinds of type formers:
`SomeType : *` means that `SomeType : 𝒰` for a sufficiently large universe 𝒰 and all universes above it.
`SomeOtherType : * → *` means that `SomeOtherType` acts as function `𝒰 → 𝒰` on sufficiently large universes.

> In fact, `*` can be interpreted even more broadly. A polymorphic type definition makes sense not only in
a universe `Prop⁺⁽ⁿ⁾` but in and elementary topos that includes all types mentioned in the definition of the
polymorphic type being interpreted. Since (higher) topos structure is preserved by forming presheaves, any
kind can be generalized by replacing a particular strictly-positive occurence of `*` by `(ϰ → *)`, where `ϰ`
is an arbitrary kind, which corresponds to transferring a construction from a universe to a presheaf valued
in this universe.

After we design a system of codes for inductive and behavioral types we actually do not need separate type
definitions at all. They do not exist in the core HOCC. One can define types and type formers as usual expressions
involving generators of the types `Prop⁺⁽ⁿ⁾` without referring explicitly to kinds and polymorphism. We seemingly
do not need any kinds at all, everything involves only bona fide types.

This is not entirely true: to emulate fully polymorphic typeformers (i.e. w/o restriction to a particular fixed
universe), we need lifting operators indexed by kinds. Consider the typeformer `List`, which now has the bona fide
type `List : 𝒰 → 𝒰`. How do we apply it to a type living in a higher universe 𝒰⁺? We need a lifting operator (↑)
that turns `List : 𝒰 → 𝒰` into `↑List : 𝒰⁺ → 𝒰⁺`.

The lifting operator has four parameters besides the expression being lifted:
- the assumed kind ϰ of the typeformer being lifted,
- a universe 𝒰,
- a kind η that generalizes ϰ,
- a larger universe 𝒱 ⊃ 𝒰

Lifting operator checks that the expression being lifted satisfies the type obtained by specializing ϰ to 𝒰,
and yields an expression of the type obtained by specializing η to 𝒱. The ability of lifting to replace any `*`
in a strictly-positive position of kind by `(ϰ → *)` for any kind ϰ is precisely what we mean by “`*` is more
than Type”. We'll discuss that in depth in the section about handling categories below.


§ Parametric quantifiers
------------------------

We've already discussed how fully polymorphic type constructors are just a syntactic sugar that can be faithfully
emulated by the cummulative universe hierarchy and lifting operators. Now what about polymorphic constructions /
theorems / functions / lemmas? To emulate them in full generality one needs an additional quantifier and another
lifting operator.

In addition to the usual universal quantifier “for each” `∀(\x : X) Y[x]` HOCC has parametric quantifiers “for
all (independently of their values)” written as `⋂(\x : X) Y[x]`. These were introduced under various names at
least a dozen of times. In particular by Miquel as implicit dependent products, by ?? as dependent intersection
types, by C. McBride in “I Got Plenty o’ Nuttin’”.

Expressions of that type are inhabited by a special kind of lambda-abstractions:
```
 (\x :⁰ X) expr
```
The superscript zero 0 reflect to the fact that `x` is allowed to be used in the `expr` exactly zero times not
counting usages in type annotations.

When regarding to values, the ⋂-quantifier has more of an existential flavour:
```
c : ⋂(\length : Nat) Vec[T][length]
```
means that `c` is a `T`-vector of some length. The length exists, that's all we know, and it cannot be extracted.

There are not much definable expressions of the type `∀(\T : U) List[T]`. Since `T` is an unknown type we cannot
excibit any of its inhabitants. In fact it might be empty. So the only definable expression of this type is
`(\T : U) ↦ Empty[T]` that produces an empty list of the given type. The variable `T` has only usages in type
annotations, therefore this expression also typechecks against the type `⋂(\T : U) List[T]`.

Exactly for that reason it might be tempting to think that for any universe `U` the types `∀(\T : U) Y[T]`
and `⋂(\T : U) Y[T]` are contain the same definable values, i.e. the type `∀(\T : U) Y[T]` can contain only
constants of the type `Y[T]`. It can be shown to be false if we take `Y[T]` to be `U`. In this case the function
`(\T : U) ↦ U` satisfies to the type `∀(\T : U) U`, but not `⋂(\T : U) U`.

Now we're ready to state that fully polymorphic functions/lemmas (i.e. the ones where polymorphism is not restricted
to a particular universe) are not a part of the core HOCC, but are emulated by functions/lemmas parametrically
quantified on universes and lifting.

We'll need a lifting operator for such functions/lemmas, that will be also signature indexed. Whenever we prove
a statement
```
p : ⋂(\T : U) ∀(\m : Monoid[T]) something
```
we would be able to transport it to larger universes U'.

Working directly with polymorphic typeformers and functions/lemmas is a syntactic sugar over the core HOCC in
the very same way as the von Neumann-Gödel-Bernays set-and-class theory NBG is a syntactic sugar on (conservative
extension of) its core set theory ZFC. Being able to speak about polymorphic typeformers and functions / lemmas
corresponds to being able to speak about classes. In particular, both provide a language to express constructions
applicable to all groups, all categories and so on.


§ Handling Categories
---------------------

Let's consider categories as structures of the form
```
#Structure Cat[\Ob : *, \Mor : Ob → Ob → *]:
  id[\T : Ob] : Mor[T][T]
  compose[\X \Y \Z : Ob] : Mor[X][Y] → Mor[Y][Z] → Mor[X][Y]
  ... axioms
```

As we explained in depth above, we can define an index `CatCarrier` so that `Cat` can be seen as a structure
endowing a carrier adhering to the signature `C : CatCarrier → *`:
```
#Structure Cat[\C : CatCarrier → *]:
  id[\T : C.Ob] : C.Mor[Src ↦ T; Tgt ↦ T]
  compose[\X \Y \Z : C.Ob]
  : C.Mor[Src ↦ X; Tgt ↦ Y] → C.Mor[Src ↦ Y; Tgt ↦ Z] → C.Mor[Src ↦ X; Tgt ↦ Y]
  ... axioms
```

In mathematics we frequently work with seemingly large categories of structured types, like category of all groups or
all rings. In simple categories, `Mor[\src : Ob, tgt : Ob] : *` are just types, doubly indexed by `Ob`. In categories
of structured objects they are parametrized by carriers and indexed by structures of their source and target:
```
Mor[\X \Y : *][\src : Group[X], \tgt : Group[Y]]
```

It looks quite monstrously, but we really can define categories of structured types without without any
special gadgets:
```
#Structure CatOfStructuredTypes[
  \Ob : * → *,
  \Mor : (SrcCarrier : *) → (TgtCarrier : *)
   → (SrcStructure : S[SrcCarrier])
   → (TgtStructure : S[TgrCarrier])
   → *]
... realization
```

Now we could define the category of all groups as an instance of
`CatOfStructuredTypes[\Ob: Group, \Mor: GroupHomomorphism]`.
If one specializes the quite complicated object to a fixed universe `U`, one obtains type isomorphic to the simple
category `Cat[\Ob: Σ(T : U) Group[T], \Mor : SmallGroupHomomorphism]` of `U`-small groups. Thus, such complicated
objects are in the fact quite amenable to work with considering specialization and lifting.

By using indexes (CatCarrier in particular), we wrap up the quite monstrous signature of `CatOfStructuredTypes`:
```
#Structure CatOfStructuredTypes[\С : CatCarrier → (* → *)].
```

In fact, with generalized (presheaf) lifting we can dispense with defining `CatOfStructuredTypes` altogether.
We can just plug the typeformer of the group structure `Group : * → *` into the spot where `Ob : *` is required,
and the whole signature and everything else adjusts automagically. In fact we could use structures of more
complicated carriers than just a type. For instance we could define the category of all categories.. well, almost.
Because categories don't actually form a category, they form a 2-category.

Thankfully, our approach opens the road for defining carriers of n-categories uniformly:
```
#Index nCatCarrier(\n : Nat)
   Cell(\m : Fin[n]) : nCatCarrier
   ... dependency arrows
```

Then one can uniformoly define the n-categories themselves:
```
#Structure nCat[\n : Nat][\С : CatCarrier(n) → *] 
  ... realization
```

so that `Cat ≅ nCat[1]`. Then one can also define n-functors between n-categories
```
#Structure nFunctor[\n : Nat][\С : CatCarrier[n']]
  ... realization
```

so that
```
  Cat ≅ nFunctor[1][Cell(0)]
  Functor ≅ nFunctor[1][Cell(1)]
  NatTrans ≅ nFunctor[1][Cell(2)]
```

Finally, for each `n` we can define the (n + 1)-category of n-categories and (n, m)-functors between them.

```
#Define nCAT(\n) : nCat[n'][nFunctor[n]]
  ... realization
```

We are unaware of any other foundational framework able to handle categories that naturally. We conjecture that
within this approach one would eventially be able to formalize the whole corpus of http://ncatlab.org.

***

PART II
=======

§ Why do we care about algebraic theories?
------------------------------------------

Algebraic theories are very well understood. Definition of an (possibly multisorted) algebraic theory is
a description of a finite-product category in terms of generators and relations, models are given by functors
on that category, homomorphisms between models are given by natural transformations. Thus, models and
homomorphisms between them form a category themselves.

For a single-sorted algebraic theory **Alg** one can always construct the purely inductive type `FreeAlg[T : *]`
of free algebras on the set `T` of generators, and its quotients, i.e. models of **Alg** given in terms of
generators and relations. The `FreeAlg[Fin[n]]` is guaranteed to be countable and have verifiable equality,
that is this type is the syntactic model of the theory and consists of valid exressions with variables `n`
in the language of the theory `Alg`. The free algebra on empty set of generators is the initial object in
the category of models. This carries over to multi-sorted algebraic theories.

For every algebraic theory **Alg** one can define a derived theory called cosmification of **Alg**.
One can define a notion of generalized models (and their homomorphisms) of **Alg** in every model of
its cosmification, the notions of model homomorphism . For example, a monoid object in a multicategory.
The category of models of **Alg** and carries structure of the cosmification of **Alg** and the same
applies to categories of generalized models in any cosmification of **Alg** which is known as
microcosm-macrocosm principle.

Algebraic theories as presented are uncapable of embracing such structures as categories, toposes,
polycategories, higher categories and so on, while there are descriptions of those theories entirely
algebraic in their spirit. There are two known generalizations of algebraic theories (essentially
algebraic theories EAT and generalized algebraic theories GAT) which are almost but not completely
satisfactory. EATs have very nice semantics, but depart from algebraicity, GATs are very algebraic
in their nature but don't have a straightforward functorial semantics in terms of natural structures.

We think it is possible to develop a theory of extended algebraic theories (XAT) that takes best of
both worlds and embraces bidirectionally presented dependent type theories, which can be seen as
descriptions of weak model categories in terms of generators and relations.


§ Bidirectional presentations of dependent type theories
--------------------------------------------------------

...

<!--
Чисто синтетические типы, такие как
```
#Synthetic Nat:
   Zero
   PosInt(\predecessor : Nat)
```
это типы, свободно порождённые не более чем счётным набором (вообще говоря рекурсивных) образующих. 

Чисто синтетические типы могут быть использованы, чтобы описать формализованные языки, вот например описание языка выражений “как на калькуляторе со скобками”:
```
Synthetic Expr:
   Neg(\a : Expr)
   Add(\a \b : Expr)
   Const(\c : Float)
```
Итого выражение это либо константа (число с плавающей запятой), либо комбинация выражений при помощи сложения Add и функции Neg. Обратите внимание, что выражение это тут нет ничего о порядке операторов и расстановке скобок: выражение в интересующем нас смысле — это уже дерево, Abstract Syntax Tree.

Тут надо подчеркнуть разницу между формальными языками и формализованными. Теория формальных языков (в информатике) занимается как раз выражениями как последовательностями букв, цифр, значков и пробелов: описание формального языка арифметических выражений — это как раз про сбалансированность скобок и порядок операций, про то какие последовательности символов являются корректными выражениями, как их распарсить в синтаксическое дерево и как ещё на этапе дизайна языка выражений исключить двусмысленности. Теория формализованных языков — это уже на уровень выше, это про работу с уже готовыми синтаксическими деревьями.

В узком смысле type theory studies это в точности теория типизированных формализованных языков. Что такое типизированный формальный язык? Это когда для при сборке выражений из подвыражений у нас есть ограничения на то, что куда можно втыкать. Допустим, в нашем языке значений появится два типа выражений:
```
#Synthetic ExprType
  Numeric
  Boolean
```
Теперь тип выражений будет “индексирован” типом ExprType:
```
#Synthetic Expr[\type : ExprType]:
   Const(\c : Float) : Expr[Numeric]
   Neg(\a : Expr[Numeric]) : Expr[Numeric]
   Add(\a \b : Expr[Numeric]) : Expr[Numeric]

   Not(\a : Expr[Boolean]) : Expr[Boolean]
   And(\a \b : Expr[Boolean]) : Expr[Boolean]

   CheckEquals(\a \b : Exp[Numeric]) : Expr[Boolean]
   CheckLess(\a \b : Exp[Numeric]) : Expr[Boolean]

   IfThenElse(\cond : Exp[Boolean], \a \b : Exp[Numeric])
    : Expr[Numeric]
```

... теперь надо сделать STLC, на его примере ввести редукции и бидирекциональность
-->

§ Extended Algebraic Theories and their Functorial Semantics
------------------------------------------------------------

...

§ Conclusion and Future Work
----------------------------

We came incredibly near to having a Construction Calculus expressive enough to accomodate not only the discrete
mathematics, but also all of undergraduate mathematics, analysis, differential geometry, topology, homotopy theory,
advanced algebra, algebraic geometry, and category theory, while retaining
desirable foundational properties:
- univalence (transportability of theorems and constructions between identifiable
objects together with structural identity principle, lack of non-univalent “strong” equality),
- decidability of type checking,
- computability,
- compatibility with the Axiom of Choice,
- relative consistency wrt to ZFC + large cardinal (CZF + large cardinal for the predicative version).

In the future, we hope to establish the language of this construction calculus as purely inductive type family
inside of itself and bulid a hierarchy of inner models a la [“The Gentle art of levitation“](https://www.irif.fr/~dagand/papers/levitation.pdf)
by J. Chapman, P.-E. Dagand, C. McBride and P. Morris, completing the theory.

Ultimately, we would also like to provide an apparatus to work with the Axiom of Choice or its weaker versions (double
negation elimination and ultrafilter lemma) in a fenced fashion, without compromising computational properties of the
parts outside of the scope where a constructive taboo was involved.
Possibly, it would be a subtask of a larger goal to provide support for modal extensions
like the [real-cohesive homotopy type theory](https://arxiv.org/abs/1509.07584) of Mike Shulman, as embedded DSLs.

In our opinion, the next step would be to make the formal system comfortable to work with, in particular by developing
machinery for ν-rules, ornaments, subtyping, and derived implicits that include SMT-assisted proof term search.

That being done, one could consider adressing linear dependent types and quantum computations, as well as
generalizing inductive and behavioral types to accomodate the quantum analogon of algebraic theories, namely
[Partial Algebraic Theories](https://arxiv.org/pdf/2011.06644.pdf), and develop a notion of their models over
a partial field restoring the classical algebraic theories for the case of the “field with one element“ 𝔽₁.
