Towards Higher Observational Construction Calculus
==================================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)


Some 6 years ago, co-founder and former CEO of JetBrains Sergey Dmitriev told me about his
long-standing hope to establish a database of mathematical results, definitions, conjectures,
constructions, and proofs. This database should allow content-based search and connect related
results into a web. Ideally, it should also connect results on mathematical objects of similar
structure even if their relatedness is not apparent and unknown to the respective authors.
These ideas got some traction inside the mathematical community, as exemplified by the
[Formal Abstracts Project](https://formalabstracts.github.io/) by Thomas C. Hales.

The prerequisite for such a database is a common formalized language for mathematics with a pleasing
syntax and enough expressivity to satisfy the needs of almost all mathematicians. Modern proof
assistants made huge progress in this direction in the last few years. In 2021, Fields medalist and
one of the top world mathematicians Peter Scholze started a project on formalizing an entirely new
result in modern mathematics using Lean Proof Assistant. This project - the Liquid Tensor Experiment -
ran unexpectedly well and has indeed managed to subdue skepticism on proof formalization in substantial
parts of the mathematical community. However, modern proof assistants are still afflicted with
insufficient expressivity and excessive verbosity.

§ The HOCC Project
------------------

Over the last five years in JetBrains Research, I've been intensely working on solving
the expressivity problems of the foundational systems underlying modern proof assistants.
In the present series of drafts, I outline the solutions to the most critical expressivity
problems.

§§ Backwards Compatibility: The Non-Constructive Modality conservative over ZFC
-------------------------------------------------------------------------------

Traditionally, set theories were used as the foundational system for mathematics. However, in the
late 1970s, the modern approach based on higher-order categorical logic started to gain momentum.
Foundational systems of this kind are known as Construction Calculi in case they incorporate the
entire higher-order constructive logic, or intuitionistic type theories if they only incorporate
its predicative fragment. The common core of all those systems is the intuitionistic type theory
containing basic inductive types (at least 𝟘, 𝟙, 𝔹 = {ff, tt}, and ℕ of natural numbers), and a
cumulative hierarchy of universes 𝒰 : 𝒰⁺ : 𝒰⁺⁺ : ··· closed under forming dependent sums Σ and
products Π, so that every type (and every finite collection of types) lies inside some universe.

The Observational Calculus of Constructions $CC_{obs}$ [Pujet-Tabareau2022] extends the core ITT
by an impredicative universe of computationally irrelevant propositions `Ω° : 𝒰`, and a relation
of observational substitutivity `a ~ᵀ b : Ω°` for every type `T`. For plain types `T`, `a ~ᵀ b`
reflects indiscernibility of elements `\a \b : T` by any observations. For example, for functions
`\f \g : ℕ → ℕ` it is the pointwise equality: `(f ~ g) := ∀(\n : ℕ) f(n) ~ g(n)`. For the types
`\X \Y : 𝒰ⁿ` themselves `(X ~ Y)` reflects “literal” equality, e.g. `(X × Y) × Z ≁ X × (Y × Z)`.
Otherwise, one would be allowed to substitute `X × (Y × Z)` as the type for `((x, y), z) : (X × Y) × Z`,
which is syntactically incorrect.

The type `Ω°` has a remarkable property that the logical power of impredicativity is locked inside.

[The first draft](./choice) in the series utilizes this property to introduce the non-constructive
modality `‖_‖ᶜ`. This modality allows application of non-constructive methods, including the Axiom of
Choice, inside a fenced fragment of the otherwise constructive framework. Non-constructive modality
does not compromise the decidability of typechecking and effectiveness of evaluation. That is, both
the algorithm validating proofs and the algorithm evaluating closed expressions of a canonically
inductive type (such as the type `ℕ` of natural numbers) are guaranteed to terminate in a finite
time and produce a specific result.

By adapting the results of P. Aczel and B. Werner, we show that $CC_{obs}$ + `‖_‖ᶜ` with at least
$n + 1$ universes has a model `Vₙ` of the standard set theory ZFC($n$) with at least $n$
Grothendieck universes. We seek to prove that this model is faithful in the sense that a formula φ
can be proven in ZFC(n) if and only if there is a $CC_{obs}$ proof of `‖φ‖ᶜ` where all quantifiers
are interpreted as bounded in `Vₙ`.

The universe `Ω°` has the structure of Heyting algebra, and every Heyting algebra can be embedded
(via Kripke Representation) into the Heyting algebra of up-sets of some poset. It seems possible
to interpret $CC_{obs}($n + 1$)$ in ZFC($n + 1$) in such a way that the propositions `p : Ω°` will
be modeled by up-sets of the inaccessible cardinal $V_κ$ taken as a poset of sets ordered by
inclusion. On the type-theoretic side we'll have the type of Aczel sets `Vₙ` modeling ZFC($n$).
Utilizing the impredicative universal quantifier of `Ω°` to represent both finitary and infinitary
conjunctions, it seems possible to translate each `s : Vₙ` into the respective Zakharyaschev subframe
canonical formula `φₛ : Ω°` [[Bezhanishvili2022]] that will be in interpreted precisely by the set
$s ∈ V_κ$ in the model, finalizing the circle.

Intriguingly, this model seems to validate that non-constructive proofs `p : ‖a ~ b‖ᶜ` can be used
for term conversions without compromising the computational properties of the system.

[Bezhanishvili2022]: “Axiomatization Techniques for Intermediate Logics”

§§ Compatibility with Univalent Equality
----------------------------------------

Unfortunately, $CC_{obs}$ fails to qualify as the ultimate foundational system for two reasons:
* `Ω°` does not contain enough propositions to be a subobject classifier, that is the proposition
“`y : T` belongs to the image of the function `f`” is not always representable by a computationally
irrelevant `P : T → Ω°`, and conversely not every provably functional relation `R : X → Y → Ω°`can
be turned into a function;
* $CC_{obs}$ fails to respect the structural principle of equivalence: no constructions or theorems
should be able to tell apart isomorphic groups, homeomorphic spaces, equivalent categories, etc.
In the original formulation of $CC_{obs}$, the universe `Ω°` is not an isolated universe, but a
subuniverse of all other universes `Ω° ⊂ 𝒰ⁿ`, thus it is possible to state a theorem for, say a
group that is “literally” equal to ℤ₂ instead of being merely isomorphic to it.

At the time observational “equality” underlying $CC_{obs}$ was introduced [Altenkirch-McBride2007],
everyone assumed that the respective notions of equivalence for various mathematical structures
have to be defined by hand for each one separately: isomorphisms for groups, homeomorphisms
for topological spaces, equivalences for categories, bisimilarity for automata, etc.

In 2010 V. Voevodsky made a baffling discovery: if we are to define equality between any pair of
types `A : U` and `B : U` as effectively 1-to-1 correspondence (that is, a type `Corr : A × B → U`,
together with a left inverse function and a right inverse function), the natural notions of
equality (or, better to say, identifiability) for each type of mathematical objects is implied by
their respective definitions. There is no need to define isomorphisms, homeomorphisms, equivalences,
etc. manually, as they all arise as “generalized equality”. The aforementioned definition of
equality for types is known as the univalence principle.

The discovery of this principle led to a mathematical revolution not only in the field of type
theories but also in higher category theory and foundations of mathematics. As a result of a
large collaborative program, the so-called Homotopy Type Theory was born in 2014. As a byproduct,
one solved the longstanding problem of dealing constructively with the analytic notion of real
numbers and Turing-complete partial functions.

It took over a decade to develop a constructive system incorporating the univalence principle. It
was first presented in early 2022 in a series of talks by M. Shulman, and then later by A. Kaposi
at the TYPES 2022 conference, and goes by the name Higher Observational Type Theory HOTT in all
capitals.

HOTT solves the both problems mentioned in the first paragraph: it respects structural equivalence
and (in presence of dependent coinductive types and propositional resizing) also has a type of all
propositions `Ω` that contains all inductively and coinductively definable propositions and while
being closed under all logical connectives and quantifiers. It has enough propositions for all
purposes and is in particular a proper subobject classifier, i.e. subsets of a type `T` faithfully
defined as predicates `p : T → Ω`).

In this system, the notion of equality is substituted by that of identifiability. There, `a ~ᵀ b`
is in general not a proposition, but a type populated by identifications between `a` and `b`.
For example, for two groups `G` and `H` the type `(G = H)` is populated by their isomorphisms,
and there are often many inequivalent identifications between the same objects. Indeed, for any type
`T` we can define the type `T! = (T = T)` of its self-identifications, and for the finite types
`Fin(n)` with $n$ elements, the types `( Fin(n) )!` are the permutation groups containing exactly
$n!$ distinct elements.

The observational substitutivity `X ~ᵁ Y` of $CC_{obs}$ allows to treat terms of type `X` as if
they were also terms of type `Y`, but precludes the types `(X × Y) × Z` and `X × (Y × Z)` to be
equal. In HOTT we certainly have the identification `regroup : (X × Y) × Z = X × (Y × Z)`, but it
only allows to transport terms of the first type into the second type along the specified
identification `regroup`, resulting in cumbersome bookkeeping of identification paths even in the cases
where it is certainly avoidable. The necessity to track identifications even in case of equal
type expressions `Fin(n + m) ~ Fin(m + n)` and canonical isomorphisms `(X × Y) × Z = X × (Y × Z)`
is a deterrent annoyance.

Thus it is still desirable to augment HOTT by `Ω°` as an isolated universe harboring the relations
of observational substitutivity for all types. By removing the inclusion `Ω° ⊂ 𝒰ⁿ` one precludes
definitions violating the structural principle of equivalence. But since observational substitutivity
`X ~ Y` still can be used _inside_ the proofs, most path bookkeeping can be avoided.
In the next section, we'll also outline how to avoid it in the case of canonical isomorphisms. The
non-constructive modality can be also adapted to the univalent setting without causing any
issues. The resulting theory HOTT + `‖_‖ᶜ` is also capable of non-constructive reasoning
inside a fenced fragment and supposedly shares the attractive metamathematical properties of
$CC_{obs}$ + `‖_‖ᶜ`.

§§ Typed Unbounded Quantifiers: Internalization, Large Categories and Canonical Isomorphisms
--------------------------------------------------------------------------------------------

It is well known that the ZFC set theory alone poorly captures the category theory. For many
applications in modern mathematics (algebraic geometry, algebraic number theory, homological
algebra, etc.) ZFC has to be extended by additional axioms postulating the existence of enough
Grothendieck universes. But even then, the language of ZFC is incapable of expressing
large concrete categories (such as the category of all groups). This issue was first solved
by extended set theories ZFC/S and ZMC/S with appropriate reflection principles [Shulman2008].

[Our second draft](./star-is-more) in the series introduces the type-theoretic counterpart of
unbounded universal quantifiers with introduction and elimination rules corresponding to the
reflection principle of ZMC/S. This extension facilitates handling of concrete large categories
and also such objects as the large ordered field of surreal numbers.

We show how to extend the resulting theory to incorporate the desired forms of internalization.
Both inductive types and mathematical structures defined polymorphically for all types can be
also interpreted internally to any category that admits all universal constructions used in
their definitions.

In a form of type theory with “unbounded“ universal quantifiers, internalization can be mechanized.
With the usual definition of a group, one gets the definition of group objects in finitely complete
categories for free, with all proofs about groups and constructions on groups automatically
appliable to group objects, if all proof/construction elements are expressable in finitely
complete categories.

Together with the impredicative universe `Ω°` one can treat internal parametricity as a form of
internalization. For example, take any polymorphic function `f : ⋂(\T : *) T → T` and
specialize it to the predicate `Pₓ : (_~ x)`:
```
f[Pₓ] : ∀(\t : T) (t ~ x) → (f(t) ~ x)
```
thus
```
(\x : T ↦ f[Pₓ](x)(reflₓ)) : ∀(\x : T) f(x) ~ x
```
we have shown that every polymorphic function `f : ⋂(\T : *) T → T` is equal to the polymorphic
function `id[\T : *] := (\x : T ↦ x)`. Thus, the type `⋂(\T : *) T → T` is contractible and the
function `id` can be said to be the canonical `⋂(\T : *) T → T`. Analogously, we can show that
`regroup` is the canonical isomorphism of the type `⋂(\X \Y \Z : *) (X × Y) × Z = X × (Y × Z)`.
Now that the notion of canonical identification can be established, one can also provide the
machinery to handle transport over canonical identifications without cumbersome bookkeeping,
which used to be the primary hindrance to the widespread adoption of the univalent approach to
practical proof assistants.

§§ Expressing Dependently Typed Languages
-----------------------------------------

Higher algebra and higher category theory in particular are the branches of mathematics that could
probably benefit the most from HOTT-based proof assistants. In recent years, two higher algebraic
notions of extreme importance have been described as domain-specific type theories, namely the
Grothendieck-Maltsiniotis [weak ω-categories](https://arxiv.org/abs/1706.02866) [Finster-Mimram207]
and [virtual equipments](https://arxiv.org/abs/2210.08663) [New-Licata2022]. HOTT has to be extended
to enable the handling of such domain-specific type theories as internal objects. Optimally, it should
also be able to capture its own syntax as an inductive type internally, which is known under the
slogan [“Type Theory should eat itself”](https://homotopytypetheory.org/2014/03/03/hott-should-eat-itself/).

Proof assistants should also be able to handle metamathematical proofs which are carried out by
structural induction over formulae and derivations of theories being studied. Thus, those formulae
and derivations have to be expressable as inductive types.

The language of proof terms in any first-order theory is already too complex to be naturally
expressable with basic inductive types. The language of _formulae_ of single-sorted
first-order theories can be expressed as an inductive type family `Formula(\n : Nat)`, where
`n` is the number of free variables. However, it would be useful if the index is not just a type,
but an inductively generated direct category `Δ⁺`, as it would greatly simplify the management of
variables [McBride2021]. For a single-sorted theory, we should have `Formula(\n : Δ⁺)`, for a
multi-sorted first-order theory, one needs the type family `Formula(\context : Δ⁺[S])`, where
`S` is the type of sorts. The language of proof terms needs to be indexed over both contexts (or
just the number of free variables in single-sorted case) and formulae they prove:
`ProofTerm(\ctx : Δ⁺[S], \statement : Formula(ctx))`. Now assume we want
to generalize this approach to first-order theories with dependent sorts (FOLDS). To manage
dependencies in the type of contexts, instead of `Δ⁺` one needs the type of indexes to be
an inductively generated Reedy category tracking both thinnings and dependencies:
`ProofTerm(\ctx : Δ[𝓢], \statement : Formula(ctx))`, where the index type `𝓢 : *ᴵ` is the sort
signature of the respective FOLDS. The possibility to express sort signatures with dependencies
(both finitary and infinitary, like in the case of the theory of ω-categories) as Reedy inductive
types allow to define FOLDS-theories and H(igher)OLDS-theories as polymorphic structures with
a single parameter `\Carrier : S` (see unbounded quantifiers above).

Generalized algebraic theories without equations on sorts can now be presented as algebraic
theories with dependent sorts, while bi-directionally presentable type theories are
algebraic theories with infinitary dependent sort signatures inductively defined mutually with terms of the theory.
They include both domain-specific type theories CaTT of weak ω-categories and VETT of virtual
equipments as well as a predicative variant of HOTT with a fixed collection (𝟘, 𝟙, 𝔹, and ℕ) of
inductive types. We outline how to extend this presentation to the entire HOTT by utilizing large
induction-recursion to delegate termination checking of internal type definitions to the external
HOTT à la [“The Gentle Art of Levitation”](https://www.irif.fr/~dagand/papers/levitation.pdf).

In [our third draft](./reedy-types), we introduce the notion of Reedy inductive types and inductive
type families indexed over them. Reedy inductive types are the type-theoretic counterpart of Reedy
categories. Much like in the case of higher inductive types, functions defined on Reedy inductive
types are required to specify their actions not only on the values themselves but also on the arrows
between them, thus being functors by definition. Inductive type families over Reedy inductive types
turn out to be presheaves.

By allowing Reedy index types and functions on them to be defined simultaneously (and mutually
recursively) with inductive type family indexed by them, we gain the ability to interpret
bi-directionally presented dependent type theories as inductive types, yielding what we call
QRIITs, Quotient-Reedy-Inductive-Inductive-Types, or HRIITs if the resulting types are not required
to be set-truncated.

In HOTT, algebraic theories can be characterized as (non-dependent) elimination motives of quotient
inductive type families indexed over a canonically inductive type. We propose to address the
elimination motives of QRIITs as extended algebraic theories XAT, and these of HRIITs as higher
XATs respectively. We are developing natural functorial semantics for XATs in the doctrine of weak
model categories [Henry2018], and speculate that higher XATs have natural semantics in
the doctrine of (∞, 1)-categories. We speculate about a possible generalization to directed XATs
with natural semantics in ω-categories.

§§ Finitistic Core
-----------------

Now once it is possible to represent languages of proof terms directly, it makes sense to use the
resulting theory for metamathematical applications, e.g. to prove cut-elimination and establish
set-theoreitc models for specific theories. Yet our system is very far from being a weak finitistic
core one strives to use to derive metamathematical results. Fortunately, utilizing the approach
mentioned in [the draft on unbounded quantifiers](./star-is-more), proofs can be internalized in
(“compiled into”) any category with enough additional structure to interpret all constructions used
in those proofs. [The fourth draft](./finitistic-core) in the series is concerned with establishing a
finitistic core system that metamathematical proofs could be compiled into, namely a constructive
version of the non-Gödelian theory of hereditarily finite sets by F. Pakhomov, which is the only
known example of a natural axiomatic system that proves its own consistency. We develop
a logic-free calculus for this theory to implement its by-construction faithful
model in terms of canonically inductive types.

§ Conclusion and Future Work
----------------------------


The large body of theoretical work outlined in this summary addresses all critical expressivity
shortcomings of type theories known to the author. Higher Observational Construction Calculus
presented in this summary three anticipated directions for further generalization:
* [Directed higher inductive types](https://hott-uf.github.io/2022/HoTTUF_2022_paper_11.pdf)
which would make each type an ω-category rather than an ∞-groupoid and each universe an ω-cosmos
rather than an ∞-topos, presumably an ω-variant of virtual equipments with extra structure.
* Linear dependent types which make the system
capable of expressing [concurrency, quantum computations and entanglement](./lin-types).
* Resource-aware (hopefully non-Gödelian) variant of the theory, where strictly increasing
functions can be only shown to exist conditionally on the availability of enough “computational budget”.

While important in their own right, these extensions are not directly relevant to the purpose of
the project. On the other hand, the practical convenience of proof assistants critically depends
on several other refinements of the foundational system, including support for algebraic ornaments
[Dagand-McBride2013] necessary for code reusability and composability, and a tractable approach to
implicit conversions and subset types.

There are also more practical issues, the author and his colleagues were also working on, including
robust management of type classes (Arend and Scala3 provide almost satisfactory solutions), and a
rich, extensible, and versatile syntax ([view pdf](https://github.com/akuklev/akuklev/blob/master/Sketch_for_a_Common_Syntax%20(1).pdf)).
Together these solutions should provide a stable basis for the upcomming proof assistants.

There is plenty of work to do, but the goal of a satisfying general-purpose proof assistant is
within sight.

<!--
  Syntax: add lean-style comments /- -/, change “family” to “inductive” or “canonically inductive”,
  change (\_+_) to (\+), syntax for commands !check, !reduce, !eval, !print (instead of #check),
  use # for annotations (remove @on), allow to turn on autowhitespaces between capitals in Variables
  section for geometric Triangle(ABC) and Triangle(\A\B\C), if all caps names are to be used, use «GRP».

Numbered and nominal blocks:
Module (instead of namespace), Section, Defintion, Theorem, Lemma, Example, Structure, Property,
Inductive, Predicate, Instance (of a typeclass), Options (configurable defaults),

Module-wide and section-wide blocks:
Imports, Conventions (contains Variable, Universe, implicit conversions and other pragmata), Parameters

Inductive allows for annotations Canonical Inductive, Reedy Inductive, Quotient Inductive, Higher Inductive,
Directed Inductive, Large Inductive (large induction-recursion allowed). Mutual definitions as in Lean.

Unnumbered:
def, let, val, var

may be public, private, protected, protected[conf]; instead of “open”, use !import, hiding and renaming
allowed, also allowed #extend and #override.
--->