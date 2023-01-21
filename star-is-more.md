Type-theoreitc Counterpart of Shulman's ZMC/S (draft)
=====================================================

In his seminal paper “Set Theory for Category Theory” Mike Shulman [Shulman08] introduces
a set theory ZMC/S, which is an extension of the standard set theory ZFC by a strong reflection
principle: in postulates existence of a Grothendick unverse 𝕊, with property that proposition
about sets φ is equivalent to its version $φ^𝕊$ relativized to the universe, i.e. with all
unbounded quantifiers bounded to 𝕊. It allows to show that there is both an infinite hierarhcy
of Grothendick universes above 𝕊 and _below_ (thus, inside) 𝕊.

In present paper we will seek to find the type-theoretical counterpart of ZMC/S. First of all
the situation with universes both above and inside 𝕊 reminds of the following setup which we'll
be using throughout this paper:
Let ITT* be an intensional Martin-Löf type theory with cumulative hierarchy of universes
`𝒰 : 𝒰⁺ : 𝒰⁺⁺ : ···` each containing types 𝟘, 𝟙 and 𝔹 := {tt, ff}, and closed under dependent
products Π, dependent sums Σ, and operator Ẅ forming inductive-recursive types by their codes.
The Ẅ operator allows to create the minimal à la Tarski universe `U : 𝒰` with the same closure
properties, the à la Tarski universe `U' : 𝒰` that additionally has the code for `U` and the
same closure properties, and so on.

To model the equivelence φ ⇔ $φ^𝕊$ let us first split it into two principles:
- generalization $φ^𝕊$ ⇒ φ
- specialization φ ⇔ $φ^𝕊$

We will also need a typed version of unbounded quantifiers. First recall that the bounded
universal quantifier is represented in Martin-Löf type theory by the dependent product type
П:

```
Γ ⊢ X : 𝒰    Γ, x : X ⊢ Y(x) : 𝒰
---------------------------------
       П(\x : X)Y(x) : 𝒰

Γ ⊢ X : 𝒰    Γ, x : X ⊢ y(x) : Y(x)
------------------------------------
  (\x : X ↦ y(x)) : П(\x : X)Y(x)
```

The typed unbounded quantifier ⋂(\x : X)Y(x) will be defined the same way
with only difference that x is only allowed to be used only in the type
signatures of the terms of type ⋂(\x : X)Y(x), but not in their body:
```
Γ ⊢ X : 𝒰    Γ, x : X ⊢ Y(x) : 𝒰
---------------------------------
       ⋂(\x : X)Y(x) : 𝒰

Γ ⊢ X : 𝒰    Γ, x : X ⊢ y : Y(x)
---------------------------------
  ([\x : X] y) : ⋂(\x : X)Y(x)
```

How can such quantifiers be used? For example we may define
the polymorphic identity function `id` by
```
([\T : 𝒰] (\x : T) ↦ x) : ⋂(\T : 𝒰) T → T
```

Given a type `List : 𝒰 → 𝒰` one can define the function
```
map : ⋂(\T : 𝒰) (T → T) → (List T) → (List T)
```

(Both of these types live in 𝒰⁺)

Bounds of the variables ⋂ is not necessarly universes. We will often encounter examples
such as `⋂(\T : 𝒰⁺)`, `⋂(\T : 𝒰 → 𝒰)`, `⋂(\T : ℕ → 𝒰)`, `⋂(\T : C → C → 𝒰)` for some fixed
`T : 𝒰`, and so on. To make quantifiers ⋂(\X : K)Y(X) act as unbounded quantifiers we have
to postulate that it is allowed to replace all instances of 𝒰 in K (or a covariant
sub-expression of K) by 𝒰⁺.

Let us call terms of the type `⋂(\x : K)Y(x)` polymorphic constructions. The replacability of
𝒰 by 𝒰⁺ means that the same term (same polymorphic construction) defines a construction for
all native universes above at the same time.

Since native universes 𝒰 only have generators but no extractors, any expression of the type
`П(\x : X)Y(x)` with `Y(x) : 𝒰` is up to a syntactical conversion an expression of the type
`⋂(\x : X)Y(x)`. That's the type theoretical counterpart of the generalization rule.
CAVEAT: without the restriction `Y(x) : 𝒰`, that is not true. If we only require `Y(x) : 𝒰⁺`,
we allow the expression `(\t : 𝒰 ↦ t) : П(\t : 𝒰) 𝒰` that uses its argument t not only in
type annotations but also in the body, and thus cannot be transformed into expression of the
form `([\t : 𝒰] expr) : ⋂(\t : 𝒰) 𝒰`.

Generalization guarantees that if we can prove some lemma about categories in the universe 𝒰,
it automatically generalizes into a polymorphic lemma that can be applied to categories in all
larger universes.

Type theoretic counterpart of specialization is more intricate. It would allow to apply the
same lemma (with unbounded quantifiers) to small à la Tarski universes `U : 𝒰`.

Specialization takes a polymorphic construction $c : ⋂(\x : K)Y(x)$, and a type `U : 𝒰` equiped
with a structure of a universe (that is, codes for types 0, 1, and 𝔹, operations Π, Σ, and Ẅ on
codes, coherency conditions and a decoding operator). Then it replaces the unbounded quantifier
by the bounded one replacing 𝒰 in K (either in the whole K or in a covariant subexpression thereof)
by U and “compiling” the polymorphic construction term into primitives provided with the inner
unverse U. The interesting thing is that U does not actually have to be a universe: if U is a
topos (or rather ПW-pretopos, if we're working in a theory without propositional resizing),
any definable expression can be compiled into U. Above we stated that unbounded quantifiers
are unbounded precisely because one is allowed to exchange 𝒰 for 𝒰⁺. Now we can see that this
is just a special case of specialization. Also note that for any universe U and index type I
(see below) the type I -> U is naturally a presheaf topos where all polymorphic constructions
can be interpreted as well.

For polymorphic constructions `c` not involving particular constructions we might relax the
requirements on the structure U has to be equiped with to be used as a microcosm `c` can be
specialized into. For instance, most constructions are interpretable in arbitrary locally
cartesian closed categories, most inductive types in arbitrary W-pretoposes and so on.

Let us denote the type theory with generalization and specialization limited to universes
(both native or internal á la Tarksi) and presheaf toposes by `ITT/S` and the variant
where polymorphic constructions can be interpreted in various weaker microcosms depending
on what primitives they actually require by `ITT/S_ncatlab`.

The goal of present project is to define the syntax of the theory `ITT/S` and its interpretation
into the set theory ZMC/S.

§ Inductive types as polymorphic constructions
----------------------------------------------

The type `List : 𝒰 -> 𝒰` defined by
```
#Inductive List[\T]
  nil : List[T]
  cons : T → List[T] → List[T]
```
is not only an endomorphism on the universe 𝒰.

is actually not
limited to the fixed universe 𝒰. By means of the W operator it can be constructed in
any universe supporting W-types.
Thus we can view `List` as a polymorphic construction as well:
```
#Inductive List[\T]
  nil : List[T]
  cons : T → List[T] → List[T]

List : ⋂(\U : 𝒰) (\u : Ẅ-closed-universe(U)) → U → U
```

The inductive definition above expands into a kind of macro that defines a list type for every
given universe endowed in terms of the W-operation the universe is supplied with.

Thus we can form the type of lists both for à la Tarski universes below 𝒰 and native universes
above 𝒰. Why stop there? We can actually go even further and allow to interpret polymorphic
types like not only in universes, but in any categories with enough structure, that is we allow
to override the meaning of (→)-operator in the definition. Categories with enough structure
to interpret simple W-types are actually the W-pretopoi, so we have
```
List : ⋂(\U : 𝒰, \(→): U → U → 𝒰) (\u : W-pretopos(U, (→))) U → U
```

§ Structure Types as Polymorphic Constructions
----------------------------------------------

The structure W-pretopos is an extension of a more simple polymorphic structure, namely that
of category
```
$Structure Cat[\Ob : 𝒰,  \Mor : Ob → Ob → 𝒰]:
  id : (X : Ob) → Mor[X, X]
  compose : (X Y Z : Ob) → Mor[X, Y] → Mor[Y, Z] → Mor[X, Z]
  ... axioms

```
Ignoring polymorphism we simply have
```
Cat : П(\Ob : 𝒰, Mor: Ob → Ob → 𝒰) 𝒰
```

This way we can only define small categories, i.e. the ones where the type of objects
is a type inside 𝒰. But the definition above actually gives a lot more:

At the simplest, one can consider it to be a construction


For given types of objects and morphisms (doubly indexed over objects) it provides
id-morphisms for every object, composition operation for compatible morphisms as well
as witnesses for associativity of composition and cancellability of units.

In its definition it only uses Σ types, and identification types, thus it can be actually
interpreted not only in any universe but in any (∞,1)-category with enough pullbacks.