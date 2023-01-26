Towards Higher Observational Construction Calculus (draft)
==========================================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)


Some 6 years ago, co-founder and former CEO of JetBrains Sergey Dmitriev told me about his
long-standing hope to establish a database of mathematical results, definitions, conjectures,
constructions and proofs. This database should allow content-based search and connect related
results into a web. Ideally, it should also connect results on mathematical objects of similar
structure even if their relatedness is not apparent and unknown to the respective authors.
These ideas got some traction inside the mathematical community, as exemplified by the
[Formal Abstracts Project](https://formalabstracts.github.io/) by Thomas C. Hales.

The prerequisite for such a database is a common formalized language for mathematics with a pleasing
syntax and enough expressivity to satisfy the needs of almost all mathematicians. Modern proof
assistants made huge progress in this direction in the last few years. In 2021, Fields medalist and
one of the top world mathematicians Peter Scholze, started a project on formalizing an entirely new
result in modern mathematics using Lean Proof Assistant. This project - the Liquid Tensor Experiment -
ran unexpectedly well and has indeed managed to subdue skepticism on proof formalization in substantial
parts of the mathematical community.

However, modern proof assistants are still afflicted with insufficient expressivity and excessive
verbosity.

§ The HOCC Project
------------------

Over the last five years in JetBrains Research, I've been intensely working on solving
the expressivity problems of the foundational systems underlying modern proof assistants.
In the present series of drafts, I outline the solutions to the most critical expressivity
problems.

§§ Backwards Compatibility: The Non-Constructive Modality
---------------------------------------------------------

Traditionally, set theories were used as the foundational system for mathematics. However, in late
1970s, the modern approach based on higher-order categorical logic started to gain momentum. This
approach was first plagued by issues handling equality and the universe of propositions.

A series of more than a dozen incremental improvements over the last 30 years recently culminated
in the Observational Calculus of Constructions $CC_{obs}$ [Pujet-Tabareau2022]. There, one extends
the core intuitionistic type theory with basic inductive types and a hierarchy of universes by an
impredicative universe of definitionally atomic types types `Ω° : 𝒰⁰` containing strict equality
types for every type. The universe `Ω°` fails to reflect all propositions, but it captures the
relation of “literal” equality between types and observational equality on their elements. In the
original formulation, `Ω°` is the subuniverse of all other universes, which makes possible to
require “literal equality” as a condition for theorems and constructions violating the Structure
Identity Principle (SIP). By removing `Ω° ⊂ 𝒰ⁿ` one restores the SIP, while retaining the comfort
of utilizing literal equality and type casts within proofs.

The type `Ω°` has a remarkable property that the logical power of impredicativity is locked inside.

* [The first draft](./non-constructive-modality) in the series utilizes this property to introduces
the non-constructive modality `‖_‖ᶜ`. This modality allows to employ non-constructive methods,
including the Axiom of Choice, inside a fenced fragment of the otherwise constructive framework.
Non-constructive modality does not compromise decidability of typechecking and effectiveness of
evaluation. That is, both the algorithm verifying proofs and the algorithm evaluating a closed
expression of the type `ℕ` (or any other purely inductive type) are guaranteed to terminate in
a finite time and produce a specific result. By adapting the results of B. Werner, M. Ratjen and
S. Tupailo it can be shown that the theory $CC_{obs}$ + `‖_‖ᶜ` has a model of the standard set
theory ZFC, that is faithful for all mathematical formulae in the sense of Rathjen and Tupailo:
a (generalized) mathematical formula φ can be proven in ZFC if and only if there is a proof of
`‖φ‖ᶜ` in $CC_{obs}$. We also describe how to define `‖_‖ᶜ` so that it no incompatibility with
univalence principle can arize.

§ Handling Large Categories and Internalization: Typed Unbounded Quantifiers
----------------------------------------------------------------------------

It is well known that the ZFC set theory alone poorly captures the category theory. For many
applications in modern mathematics (algebraic geometry, algebraic number theory, homological
algebra, etc.) ZFC has to be extended by additional axioms postulating the existence of enough
Grothendieck universes. But even then, the language of ZFC is incapable of expressing
large concrete categories (such as the category of all groups). This issue was first solved
by extended set theories ZFC/S and ZMC/S with appropriate reflection principles[Shulman2008]

* [The second draft](./star-is-more) in the series introduces the type-theoretic counterpart of
unbounded universal quantifiers with introduction and elimination rules reproducing the reflection
principle of ZMC/S. This extension allows to handle large concrete categories and similar objects.

We show how to extend the resulting theory to incorporate desired forms of internalization. Both
inductive types and mathematical structures defined polymorphically for all types, can be as well
defined internally to any category that admits all universal constructions used in the defintion.

In a form of type theory with “unbounded“ universal quantifiers, internalization can be mechanized:
with a usual definition of a group one gets the defintion of group objects in finitely complete
categories for free, with all proofs about groups and constructions on groups automatically
appliable to group objects, if all proof/construction elements are expressable in finitely
complete categories.

§ Reconciling Equality and Identifiability
------------------------------------------

Unfortunatelly, the theory $CC_{obs}$ fails to faithfully capture the equality between types, as
well as types endowed with some additional structure (e.g. groups, rings and topological spaces).
At the time observational equality underlying $CC_{obs}$ was introduced[Altenkirch-McBride2007],
everyone assumed that the respective notions of equivalence for various kinds of mathematical
structures have to be defined by hand for each kind: isomorphisms for groups, homeomorphsisms
for topological spaces, equivalences for categories, bisimularity for automata, etc.

In 2010 V. Voevodsky made a buffling discovery. If we are to define equality between any pair of
types `A : U` and `B : U` as effectively 1-to-1 correspondence (that is, a type `Corr : A × B → U`,
together with a left inverse function and a right inverse function), the natural notions of
equality (or, better to say, identifiability) for each type of mathematical objects is implied by
their respective definitions. There is no need to define and manually enforce isomorphisms,
homeomorphisms, equivalence, etc. The aforementioned definition of equality for types is known as
the univalence principle.

Discovery of this principle led to a mathematical revolution not only in the field of type
theories, but also in higher category theory and foundations of mathematics. As a result of
large collaborative program the so called Homotopy Type Theory was born in 2014. As a byproduct
one solved the longstanding problem of dealing constructively with analytic notion of real numbers.

It took over a decade to develop a constructive type theory incorporating univalence principle.
The resulting theory HOTT (in all capitals, Higher Observational Type Theory) was presented in
early 2022 in a series of talks M. Shulman, and then later by A. Kaposi at TYPES 2022.

* In the third draft I outline how to combine the HOTT, `Ω°`, `‖_‖ᶜ` and unbounded quantifiers.
The resulting system (with propositional resizing) deserves to be called Higher Observational
Construction Calculus and gives the name to this whole project.

§§ Expressing Dependently Typed Languages
-----------------------------------------

A structuralist foundational system has to be able to handle formalized languages both of other
theories and of its own, as inductive types. That is the only way to carry out metamathematical
proofs and construct models. The language of proof terms in any first-order theory is already
too complex to be naturally expressable by means of basic inductive types. The language of
__formulas__ of single-sorted first-order theories can be expressed as an inductive type family
`Formula(\n : Nat)`, where `n` is the number of free variables. However, even in this case the
index begs to be not just a type, but an inductively generated direct category `Δ⁺`, which
greatly simplifies the variable management[McBride2021]. For a multi-sorted first-order theory
one needs the type `Formula(\context : Δ⁺[S])`, where `S` is the type of sorts. The language of
proof terms needs to be indexed over both contexts (number of free variables in single-sorted
case) and formulas they prove: `ProofTerm(\ctx : Δ⁺[S], \statement : Formula(ctx))`. Now assume
we want to generalize this approach to first-order theories with dependent sorts (FOLDS). To
manage dependencies in the type of contexts, instead of `Δ⁺` one needs the type of indexes to be
an inductively generated Reedy category tracking both thinnings and dependencies:
`ProofTerm(\ctx : Δ[S], \statement : Formula(ctx))`, where `S : Δ⁻ → *` is the signature of the
respective FOLDS. The possibility to express sort signatures with dependencies (both finitary
and infinitary, like in case of theory of ω-categories) as presheaves `S : 𝒞 → *` for appropriate
inductively-generated Reedy category `𝒞` allow to define FOLDS-theories and H(igher)OLDS-theories
as polymorphic structures with a single parameter `\Carrier : S` (see unbounded quantifers above).

* [The fourth draft](./index-cats) is concerned by interpreting dependent type theories
inside dependent type theories. Continuing the line of research I pursued even before entering
JetBrains Research, I propose introducing the notion of index categories and inductive type
families indexed over them. Higher index types are the type-theoretical counterpart of Reedy categories,
functions defined on them are required to specify their actions not only on the indices themselves
but also on the arrows between them, thus being functors by definition. Inductive type families
over higher index types turn out to be presheaves.

By allowing index categories and functions on them to be defined simultaneously (and mutially
recursively) with inductive type family indexed by them one gains the ability to interpret dependent
type theories (in bi-directional presentation) as inductive types. This can be further generalized
two-fold:
* * by large induction-recursion to allow the theory to represent its own syntax by utilizing
“external” termination checking à la “The Gentle Art of Levitation”, or
* * by allowing higher constructors for the type family (but not for the index): this way we obtain
extended algebraic theories XAT (in case, the resulting family quotient-inductive) or higher XATs
otherwise. We conjecture that XATs have natural functorial semantics in the doctrine of weak model
categories, while higher XATs require ∞-categories. We speculate about possible generalization to
directed higher XATs with natural semantics in ω-categories.

§ Finitistic Core
-----------------

* Now that it is possible to represent languages of proofs directly, it makes sense to use the
resulting theory for metamathematical applications, e.g. to prove cut-elimination for specific
theories. Yet the theory is very far from being a weak finitistic core, one strives to use, when
deriving metamathematical results. Fortunatelly, with the approach mentioned in the draft on
unbounded quantifiers, proofs can be “compiled” into a weaker microcosm, given they don't use
any primitives not available there. [The fifth draft](./finitistic-core) in series is concerned by defining such a
microcosm, namely a finitistic core system metamathematical proofs are compiled into.
There I develop a constructive version of the non-Gödelian theory of hereditary finite sets by
F. Pakhomov, the only known example of a natural axiomatic system that is able to prove its
own consistency. I develop a logic-free (type-theory like) calculus for this theory to implement
its by-construction faithful model inside HOCC.
