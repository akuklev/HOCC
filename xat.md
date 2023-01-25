In type theory, single-sorted algebraic theories can be defined as theories that arize as
recursion motives of primitive quotient inductive types. By “primitive” we mean that all
generators have to have the type `Tⁿ → T`. The simplest non-trival example is given by the type
```
#Inductive ℕ:
  zero : ℕ
  succ : ℕ → ℕ
```

Its recursor is given by `ℕ-rec[\T] : ℕ → ℕ-Algebra[T] → T`, where `ℕ-Algebra` is
```
#Structure ℕ-Algebra[\T]:
  base : T
  step : T → T
```

Now let's provide an example, where the word “quotient” comes into play. Consider the definition of
monoids in type theory:
```
#Structure Monoid[\M]
  unit : M
  (∘) :  M → M → M
  unitor-l : ∀(\x : M) unit ∘ x = x
  unitor-r : ∀(\x : M) x ∘ unit = x
  associator : ∀(\x \y \z : M) (x ∘ y) ∘ z = x ∘ (y ∘ z)
```
It is a single sorted equational theory, i.e.
1. It is a polymorphic record type with one type parameter `\M` called “carrier”.
2. All field of the record are either operators of finite arity, i.e. have types of the form
`M^n → M`, or
3. Equational axioms, i.e. have the type `∀(\x ... \z : M) equation`.

It arizes as the recursion motive for the following inductive type:
```
#Inductive ProtoMonoid
  unit : ProtoMonoid
  concat : ProtoMonoid → ProtoMonoid → ProtoMonoid
  unitor-l : ∀(\x : ProtoMonoid) (concat unit x) = x
  unitor-r : ∀(\x : ProtoMonoid) (concat x unit) = x
  associator : ∀(\x \y \z : ProtoMonoid)
   (concat (concat x y) z) = (concat x ∘ (concat y z))
```

There are several extensions of algebraic theories. One of them is to allow infinitary operations,
that is operations of the signature `T^κ -> T` with $κ$ infinite. These can be represented by quotient
inductive types without the primitivity restriction, say
```
#Inductive Infinitely-Branching-Tree
  leaf : Infinitely-Branching-Tree
  node : (ℕ → Infinitely-Branching-Tree) → Infinitely-Branching-Tree
```
This extension also allows to have infinitely many operations:
```
#Inductive Variably-Branching-Tree
  node : ∀(\n : ℕ) → (n → Variably-Branching-Tree) → Variably-Branching-Tree
```
In the above example we have a countably-infinite family of constructors, one for each finite arity.

Multi-sorted algebraic theories arize as recursion motives for quotient inductive type families
indexed by a finite type. Generalized algebraic theories without equations on sorts (GAT⁻) arize
as recursion motives of closed quotient inductive type families indexed by a purely inductive type.

Usage of types without decidable equality as indexes of types with any recursive constructors
leads to undecidability of applicability of constructors, except for rare special cases
(unless there are no recursive constructors: no recursive constructor, or type is representable as
a simpler indexed type[Zhang2021], or the conditions for “Definitional Proof-Irrelevance without K” apply)

see работа про индуктивные строгие предикаты и работа нашего интерна
про индуктивные типы, где индукцию можно заменить на паттерн-матчинг). Note that by prohibiting usage of
non-decidable indexes in these cases, we do not preclude the definition of Cauchy real numbers (there,
the relation of ε-closedness is indexed by reals which don't have decidable equality, but it does not
lead to any problems, because ε-closedness does not have any recursive constructors).

Can we generalize beyond GAT⁻ without ruining decidability of constructors? We are still allowed
to define indices the inductive type family indexed over them simultaneously and also simultaneously
define functions on the types being defined. As long as the definitions are closed, we get a so
called extended algebraic theory. If we allow to map outside of the theory (for example into the
types of the surrounding theory) and use functions from outside of the theory in constructors (that is,
reuse the external termination checker) we get “large XAT”s which may include the language of the surrounding
theory itself.

We'll study what is the right doctrine to interpret all XATs and obtain sound functorial semantics
for them. ATM, it seems that weak model categories are precisely what one needs to interpret XATs.

Note that since we can have both large XATs and the finitistic core set theory, we can build set
theoretic model of the theory in the theory itself.

Annotations:

@Decidable
@Inductive Nat

@Algebraic
@Inductive GroupPrototype -- decidable index, but is allowed to be quotient type, thus have verifiable but
not falsifieable equality

Operation on morphism uses its shadow operation on objects
how can we state the requirement for shadow operation to be functorial?

MonoidalCategory[\Ob : *, (⊗) : Monoid[Ob], ...]