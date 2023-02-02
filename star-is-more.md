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

We will introduce typed version of unbounded quantifiers: their introduction rules correspond
to generalization and their elimination rules to specialization.

First recall that the bounded universal quantifier is represented in Martin-Löf type theory by
the dependent product type П:

```
 Γ ⊢ X : 𝒰    Γ, x : X ⊢ Y(x) : 𝒰
———————————————————————————————————
         П(x̲ : X)Y(x) : 𝒰

 Γ ⊢ X : 𝒰    Γ, x : X ⊢ y(x) : Y(x)
——————————————————————————————————————
     (x̲ : X ↦ y(x)) : П(x̲ : X)Y(x)
```

The typed unbounded quantifier `⋂(x̲ : X) Y(x)` will be defined the same way
with only difference that x is only allowed to be used only in the type
signatures of the terms of type `⋂(x̲ : X) Y(x)`, but not in their body:
```
 Γ ⊢ X : 𝒰ⁿ    Γ, x : X ⊢ Y(x) : 𝒰ᵐ
————————————————————————————————————
       ⋂(x̲ : X°)Y(x) : 𝒰ⁿᵔᵐ

      Γ, x : X ⊢ y : Y(x)
————————————————————————————————
  ([x̲ : X] y) : ⋂(x̲ : X°) Y(x)
```

Here, the `X°` is an expression that is allowed to contain a special symbol *, turns into
a wellformed expression of type `𝒰ⁿ⁺ᵏ` whenever `𝒰ᵏ` is substituted for *. Such expressions
will be also called signatures. Signatures of the form `K → *` (including `*` which is
equivalent to `1 → *`) including will be called kinds because they describe kinds of
polymorphic typeformers (see below).

How can such quantifiers be used? For example we may define the polymorphic identity
function `id : ⋂(T̲ : *) T → T` by
```
([T̲ : *] (x̲ : T) ↦ x)
```

Given a type `List : 𝒰 → 𝒰` one can define the function
```
map : ⋂(T̲ : 𝒰) (T → T) → (List T) → (List T)
```

While the inhabitants of `П(x̲ : X)Y(x)` are called (dependent) functions, the inhabitants of
`⋂(x̲ : K)Y(x)` will be called polymorphic constructions.

Bounds of the variables in ⋂ is not necessarly universes. We will often encounter examples
such as `⋂(T̲ : * → *)`, `⋂(T̲ : ℕ → *)`, `⋂(T̲ : X → X → *)` and so on.

Since native universes 𝒰 only have generators but no extractors, any expression of the type
`П(x̲ : X)Y(x)` with `Y(x) : 𝒰` is up to a syntactical conversion an expression of the type
`⋂(x̲ : X)Y(x)`. This metathoretical fact is the type theoretical counterpart of
the generalization rule.

> CAVEAT: without the restriction `Y(x) : 𝒰`, that is not true. If we only require `Y(x) : 𝒰⁺`,
we allow the expression `(t̲ : 𝒰 ↦ t) : П(t̲ : 𝒰) 𝒰` that uses its argument t not only in
type annotations but also in the body, and thus cannot be transformed into expression of the
form `([t̲ : 𝒰] expr) : ⋂(t̲ : 𝒰) 𝒰`.

Example 1.

Suppose you have a polymorphic type, endowing any type `T : *` with a desired mathematical structure
(see below, how polymorphic types can be handled), for example
```
Pointed[T̲]
  unit : T
```

Now, the universe 𝒰⁺ contains the type of all 𝒰-small pointed types
```
Σ(T̲ : 𝒰) Pointed[T] : 𝒰⁺
```

Likewise it has types of all 𝒰-small monoids, 𝒰-small groups, 𝒰-small categories, etc.
Now pretend you have proven something for all 𝒰-small groups:
```
prf : (G̲ : Σ(T̲ : 𝒰) Group[T]) → ...
```

You can uncurry the first argument to obtain
```
pr : (G̲-̲c̲a̲r̲r̲i̲e̲r̲ : 𝒰) → (G̲-̲s̲t̲r̲u̲c̲t̲u̲r̲e̲ : Group[G-carrier]) → ...
```

Now you can generalize to obtain the polymorphic construction proving it for all groups
instead of 𝒰-small ones:

```
p : ⋂(G̲ : *) (g̲ : Group[G]) → ...

# which also can be written as

p[G̲](g̲ : Group[G]) : ...
```

Now let us consider the elimination rule for ⋂-quantifiers. Specialization allows to apply
polymorphic constructions to both all native universes `𝒰ⁿ` and to the “user-defined” à la
Tarski universes `U : 𝒰`. Specialization $c : ⋂(x̲ : K) Y(x)$, and a type `U : 𝒰ⁿ` equiped
with a structure of a universe (that is, codes for types 0, 1, and 𝔹, operations Π, Σ, and Ẅ
on codes, coherency conditions and a decoding operator). Then it replaces the unbounded quantifier
`⋂` by `П` by replacing all instances of * in K by U and “compiling” the body of the polymorphic
construction term into primitives provided the inner unverse U is equipped with. Since native
universes 𝒰ⁿ also can be used for specialization (each of those is just a type 𝒰ⁿ : 𝒰ⁿ⁺¹, and
is equiped with required structure).

The interesting thing is that U does not actually have to be a universe: if U is a topos (or
rather ПW-pretopos, if we're working in a background theory without propositional resizing),
any definable expression can be compiled into U. In particular, for any universe U and index
type I (see below) the type I -> U is naturally a presheaf topos where all polymorphic
constructions can be interpreted as well and thus can be used for specialization.

For polymorphic constructions `c` not involving particular constructions we might relax the
requirements on the structure U has to be equiped with to be used as a microcosm `c` can be
specialized into. For instance, most constructions are interpretable in arbitrary locally
cartesian closed categories, most inductive types in arbitrary W-pretoposes and so on.

Let us denote the type theory without induction-recursion and with generalization and
specialization limited to native universes and presheaf topoi by `MLTT/S_{ps}`. The variant
where polymorphic constructions can be interpreted in user-defined á la Tarski universes
and various weaker microcosms depending on what primitives they actually require will be
denoted by `MLTT^{IR}/S_{ncatlab}`. We argue that `MLTT^{IR}/S_{ncatlab}` can be implemented
over `MLTT/S_{ps}` with PN-inductive-recursive types without extending its strength.

The goal of present project is to define the syntax of the theory `MLTT/S_{ps}` and its
interpretation into the set theory.

§ Inductive types as polymorphic constructions
----------------------------------------------

The type `List : 𝒰 -> 𝒰` defined by
```
#Inductive List[T̲]
  nil : List[T]
  cons : T → List[T] → List[T]
```
is not only an endomorphism on the universe 𝒰. It can be promoted to a polymorphic construction
on any cartesian closed category with W-types:

```
#Inductive List[T̲]
  nil : List[T]
  cons : T → List[T] → List[T]

List : ⋂(U̲ : 𝒰) (u̲ : W-CCC(U)) → U → U
```

The inductive definition above expands into a kind of macro that defines a list type for every
given universe endowed in terms of the W-operation the universe is supplied with.

Thus we can form the type of lists both for à la Tarski universes below 𝒰 and native universes
above 𝒰. Why stop there? We can actually go even further and allow to interpret polymorphic
types like not only in universes, but in any categories with enough structure, that is we allow
to override the meaning of (→)-operator in the definition. Categories with enough structure
to interpret simple W-types are actually the W-pretopoi, so we have
```
List : ⋂(U̲ : 𝒰, \(→): U → U → 𝒰) (u̲ : W-Pretopos(U, (→))) U → U
```

§ Structure Types as Polymorphic Constructions
----------------------------------------------

The structure W-pretopos is an extension of a more simple polymorphic structure, namely that
of category
```
$Structure Cat[O̲b̲ : 𝒰, M̲o̲r̲ : Ob → Ob → 𝒰]:
  id : (X : Ob) → Mor[X, X]
  compose : (X Y Z : Ob) → Mor[X, Y] → Mor[Y, Z] → Mor[X, Z]
  ... axioms

```
Ignoring polymorphism we simply have
```
Cat : П(O̲b̲ : 𝒰, M̲o̲r̲ : Ob → Ob → 𝒰) 𝒰
```

This way we can only define small categories, i.e. the ones where the type of objects
is a type inside 𝒰. But the definition above actually gives a lot more:

At the simplest, one can consider it to be a construction


For given types of objects and morphisms (doubly indexed over objects) it provides
id-morphisms for every object, composition operation for compatible morphisms as well
as witnesses for associativity of composition and cancellability of units.

In its definition it only uses Σ types, and identification types, thus it can be actually
interpreted not only in any universe but in any (∞,1)-category with enough pullbacks.

§ Internal Parametricity
------------------------

Show that for `a : ⋂(T̲ : *) T → T` holds `a = id`. Proof:
```
[T̲ : *](c̲ : T)
let P : (T → Ω°) := (x̲ : T) ↦ (x ~ c)
then a[P] : P[T](x) → P[T](a(x))
: Π(x̲ : T) (x ~ c) → (a(x) ~ c)

that is, the term pr1 := ([T̲ : *] (c̲ : T) ↦ a[(x̲ : T) ↦ (x ~ c))] has type
⋂(T̲ : *) Π(c̲ : T) Π(x̲ : T) (x ~ c) → (a(x) ~ c)

[T̲ : *] (x̲ : T) pr1[T](x)(x)(refl(x)) : ⋂(T̲ : *) Π(x̲ : T) a(x) = x

id[T] ~ a[T] = Π(x̲ : T) a(x) = id(x)
= Π(x̲ : T) a(x) = x
```

§ Internalization as Compilation Into a Weaker Language
-------------------------------------------------------

TODO: Few words that once an internal interpretation into ZFC + enough Grothendieck universes
will be extablished, one can see the whole Higher Observational Constructions Calculus as an
elaborate dependently typed language of macros over Mizar System. :-)