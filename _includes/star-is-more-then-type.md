* is more than Type: A type theory for ncatlab
==============================================

In his seminal paper “Set Theory for Category Theory” Mike Shulman [Shulman08] introduces
a set theory ZMC/S, which is an extension of the standard set theory ZFC by a strong reflection
principle: in postulates existence of a Grothendick unverse 𝕊, with property that proposition
about sets φ is equivalent to its version $φ^𝕊$ relativized to the universe, i.e. with all
unbounded quantifiers bounded to 𝕊. It allows to show that there is both an infinite hierarhcy
of Grothendick universes above 𝕊 and _below_ (thus, inside) 𝕊.

In present paper we will seek to find the type-theoretical counterpart of ZMC/S. First of all
the situation with universes both above and inside 𝕊 reminds of the following setup which we'll
be using throughout this paper:
Let TT* be an intensional Martin-Löf type theory with cumulative hierarchy of universes
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

Bounds of the variables ⋂ is non necessarly universes. We will
often encounter encounters `⋂(\T : 𝒰⁺)`, `⋂(\T : 𝒰 → 𝒰)`, `⋂(\T : ℕ → 𝒰)`, `⋂(\T : C → C → 𝒰)`
for some fixed `T : 𝒰`, and so on. To make quantifiers ⋂(\X : K)Y(X) act as true unbounded
quantifiers we have to postulate that it is allowed to replace all instances of 𝒰 in K (or a
covariant subexpression of K) by 𝒰⁺.

Let us call terms of the type `⋂(\x : K)Y(x)` polymorphic constructions. The replacability of
𝒰 by 𝒰⁺ means that the same term (same polymorphic construction) defines a construction for
all native universes above at the same time.

In ZMC/S the specialization φ ↦ $φ^𝕊$ allows to turn a proof `pf : φ` of a proposition φ
with unbounded quantifiers into a proof $pf^𝕊 : φ^𝕊$ of a proposition where all quantifiers
are restricted to 𝕊.


generalization
part of the strong reflection property can be implemented by an operator that transfers
polymorphic constructions to higher universes.


Now let us consider
the polymorphic types. The type `List : 𝒰 -> 𝒰` is actually not limited to the fixed universe
𝒰. By means of the Ẅ operator it can be constructed in any universe supporting Ẅ-types.
Thus we can view `List` as a polymorphic construction as well:
```
List : ⋂(\U : 𝒰) (\u : Ẅ-closed-universe(U)) → U → U
```

Thus we can form the type of lists both for à la Tarski universes below 𝒰 and native universes
above 𝒰. Why stop there? We can actually go even further and allow to interpret polymorphic
types like not only in universes, but in any categories with enough structure, that is we allow
to override the meaning of (→)-operator in the definition. Categories with enough structure
to interpret simple W-types are actually the W-pretopoi, so we have

```
List : ⋂(\U : 𝒰, \(→): U → U → 𝒰) (\u : W-pretopos(U, (→))) U → U
```

The structure W-pretopos is an extension of a more simple polymorphic structure
(that of category):
```
Cat : П(\Ob : 𝒰, Mor: Ob → Ob → 𝒰) 𝒰
```

For given types of objects and morphisms (doubly indexed over objects) it provides
id-morphisms for every object, composition operation for compatible morphisms as well
as witnesses for associativity of composition and cancellability of units.

In its definition it only uses Σ types, and identification types, thus it can be actually
interpreted not only in any universe but in any (∞,1)-category with enough pullbacks.