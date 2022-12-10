Towards Higher Observational Construction Calculus
==================================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"

We propose (a sketch of) an extension HOCC of the Higher Observational Type Theory (HOTT) by parametric quantifiers,
a first-class notion of Reedy categories and inductive type families indexed and fibered above them. This combination
enables flawless handling of higher categorical objects and naturally occuring large categories. In particular, HOCC
provides simplicial types and other Reedy presheaf types without reccuring to any kind of non-univalent equality.

Purely inductive type families indexed and fibered over simultaneously defined Reedy categories correspond precisely
to bidirectional presentations of dependent type theories and allow to develop natural functorial semantics of their
models. This justifies viewing type-theoretic presentations of weak ω-categories (by E. Finster, S. Mimram et al.)
and virtual equipments (by M. New, D. Licata) as extended algebraic definitions.

This work heavily builds upon (mostly unpublished) ideas of C. McBride.


§ Introduction
--------------

We'll use the term _Construction Calculi_ for type theories intended to serve as general purpose structuralist
foundations of mathematics and capable of expressing polymorphic constructions and theorems, i.e. constructions
and theorems applying to all mathematical objects of certain kind (say, Groups or Categories) w/o size limitations.  
Type theories not striving to serve as general purpose foundational frameworks will be referred as _domain-specific
type theories_.

Construction calculi must be able to express typed higher-order intuitionistic logic, constructive proofs in natural
deduction style, formal axiomatic structures (such as Eucledian geometry) together with formal constructions on them,
and, last but not least, canonical mathematical objects such as natural and (analytic) real numers, lists, sequences,
functions, relations, etc. Such canonical objects are introduced there as inductive types.

In type theory, inductive types are the types freely generated by a set of possibly recursive generators:
```
#Inductive Nat
  0    : Nat
  (_') : Nat → Nat
```

Here, the type `Nat` is introduced as the type freely generated by one non-recursive generator `0` satisfying
type `Nat` and a recursive generator `(_')` (writen as a postifx apostrophe) satisfying type `Nat → Nat`.
With this definition, the set of possible Nat-values is given by
&nbsp; `0, 0', 0'', 0''', ...`
Readers more versed with general purpuse programming languages than with type theory might assume that 0 is
a literal inbuilt into the language and `(_')` is a function defined behind the scenes that acts on some
technical representation of natural numbers, yet that's not what happening here. Both `0` and `(_')` are
introduced right here as generators of the type `Nat`. They are “static symbols” that represent nothing
other than themselves and are irreducible.

Inductive types may have typal parameters (these are enclosed in square brackets):
```
#Inductive List[\T]
  Empty : List[T] 
  (::)  : T → (List[T] → List[T])
```

This definition introduces the polymorphic type `List[T]` parametrized by the type `T` of its elements.
The backslash in `\T` in the first line is the so-called “freshness sigil”, it is there to express that `T`
is not a constant that was defined somewhere else, but a fresh variable being introduced at this very spot.
The second line introduces a rather boring generator `Empty` of `List[T]`, much like 0 in the definition
of `Nat`. The third line introduces an infix operator `::`, being a family of recursive generators:
For each inhabitant `\head : T` it defines a recursive generator
&nbsp; `(head ::) : List[T] → List[T]`
that appends `head` to the right hand side argument `\tail : List[T]` so that non-empty lists can be
generated by succesively appending elements to the `Empty` list. The set of possible values of `List[T]`
is therefore given by  
&nbsp; `Empty, x :: Empty, x :: y :: Empty, x :: y :: z :: Empty, ...`
for `x, y, z, ...` being of type `T`.  
Note that the arrow operator is right-associating, so `T → (List[T] → List[T])` can be written simply
as `T → List[T] → List[T]`.

There is also a notion of inductive type family indexed over an other inductive type. For example,
consider the type family of vectors of fixed length:
```
#Inductive Vec[\T] : Nat → *
  Empty : Vec[T](0)
  (::)  : T → Vec[T](\n) → Vec[T](n')
```

Here `Vec[\T]` is not one type, but a whole family of types, one for each natural number `n`.
The signature `: Nat → *` means roughly “a function mapping natural numbers to types“.
The generator `Empty` satisfies the type `(Vec[T] 0)`. Generators `(head ::)` take
a vector `\tail : (Vec[T] \n)` of _some_ length `\n` (notice the freshness sigil) and
generate a vector of length `n' = n + 1`.

In this work we propose to introduce inductive types indexed over more general entities than
inductive types themselves. We will call these entities indexes. Indexes are inductive types
endowed with a (correct by construction, inductively generated) structure of a Reedy-category.
We built upon an unpublished idea of Conor McBride presented in his Topos Institute Lecture
[“Cats and Types: Best Friends?“](https://youtu.be/05IJ3YL8p0s). The typeformers of the kind
`I → *` for an index `I` will semantically correspond to type-valued presheaves on `I` as
Reedy-category.

We be able to define various very useful indices:
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
- The indices Δ⁺ and Δ such that typeformers satisfying the signatures `Δ⁺ → *` and `Δ → *` will correspond
precisely to semi-simplicial and simplicial types respectively, which allows to define ω-categories and
various other interesting structures. In particular one covers the notion of very-dependent types as
introduced by Jason J. Hickey and A. Kopylov, cf. [“Formal objects in type theory using very dependent
types.”](https://www.cs.cornell.edu/jyh/papers/fool3/paper.pdf).

§ The case of degeneracies
--------------------------

One type one often encounters is the type of number sequences containing only finite number of nonzero terms.
For many practical applications one needs to have an upper bound on the number of nonzero entries, because it
cannot be calculated in predictable time if unknown even in case of integer number sequences; in case of real
number sequences it is not computable at all since the check an unknown decimal fraction represents a zero
requires an infinite number of steps. So we need an inductive type family
`FiniteVec[\T : *, \zero : T] : NatBound → *` of sequences parametrized by the type `T` of its terms, marked
`zero : T` element of that type and indexed over an upper bound to a number of non-zero terms.

The index `size-bound` ist not just a natural number. We want `FiniteVec[T,0][n]` to be a subtype of
`FiniteVec[T,0][m]` whenever `n < m` and we want trailing zeroes to be ignored, i.e. sequences `(x :: y :: z)`
and `(x :: y :: z :: 0)` to be equal.

In order to get there let us define the following index:
```
#Index NatBound
  0    : NatBound
  (_') : NatBound → NatBound
  
  (\n : NatBound) |Extend(\m : Nat)⟩ : (n.rec m (_'))
  
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

For a multitude of reasond it is desirable to represent the cast the whole parameter
list `[\Ob : *, \Mor : Ob → Ob → *]` as a single carrier `[C : CatCarrier → *]`.

For this purpose we'll need an index type `CatCarrier` with dependency arrows:
```
#Index CatCarrier:
  Ob  : CatCarrier
  Mor : CatCarrier
  
  Mor.|Src⟩ : Ob
  Mor.|Tgt⟩ : Ob
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
  
  (\n)'.|Field(\m : Fin[m])⟩ : m
  
  |Field(_)⟩|Field(\n)⟩ : n
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
categories are in fact elementary toposes: (2) guarantees them to have subobject classifier, (3) guarantees them
to have a natural number object, (3 + 5) guarantees them to have all finite limits `Σ(\x : X, \y : Y) f(x) = g(y)`,
(5) makes them locally cartesian closed (`∀(\x : X) Y(x)`).
All purely inductive types including `List[Nat]`, `List[List[Nat]]` and similar much more complicated ones land
in the very first nontrivial universe `Prop⁺` since they are countable and discrete by construction which makes
them isomorphic either to the type of natural numbers `Nat` or to a finite type `Fin(n)` which are already present
in `Prop⁺`.

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
mathematics (that has been there for decades), but also all of undergraduate mathematics, analysis, differential
geometry, topology, homotopy theory, advanced algebra, algebraic geometry and category theory, while retaining
desirable foundational properties: univalence (transportability of theorems and constructions between identifiable
objects together with structural identity principle, lack of non-univalent “strong” equality), decidability of type
checking, computability, compatibility with axiom of choice, relative consistency wrt to CZF/ZFC + suitable large
cardinal axioms (CZF for the predicative intuitionistic version, ZFC for the version with AC). In future we hope to
establish the language of this construction calculus as purely inductive type family inside of itself and bulid
a hierarchy of inner models a la [“The Gentle art of levitation“](https://www.irif.fr/~dagand/papers/levitation.pdf)
by J. Chapman, P.-E. Dagand, C. McBride and P. Morris, which would round up the journey.

Ultimately, we would also like to provide machinery to work with axiom of choice or its weaker versions (double
negation elimination and ultrafilter lemma) in a fenced fashion, without compromising computational properties
of the surroundings. Possibly, it would be a subtask of a larger goal to provide support for modal extensions
like the [real-cohesive homotopy type theory](https://arxiv.org/abs/1509.07584) of Mike Shulman, as embedded DSLs.

In our view, the next step would be to make the formal system comfortable to work with, in particular by developing
nice machinery for ν-rules, ornaments, subtyping and derived implicits that include SMT-assisted proof term search.

That being done, one could consider adressing linear dependent types and quantum computations, as well as
generalizing inductive and behavioral types to accomodate the quantum analogon of algebraic theories, namely
[Partial Algebraic Theories](https://arxiv.org/pdf/2011.06644.pdf) and develop notion of their models over
a partial field restoring the classical algebraic theories for the case of the “field with one element“ 𝔽₁.