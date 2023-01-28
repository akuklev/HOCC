Towards Higher Observational Construction Calculus
==================================================

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
parts of the mathematical community. However, modern proof assistants are still afflicted with
insufficient expressivity and excessive verbosity.

§ The HOCC Project
------------------

Over the last five years in JetBrains Research, I've been intensely working on solving
the expressivity problems of the foundational systems underlying modern proof assistants.
In the present series of drafts, I outline the solutions to the most critical expressivity
problems.

§§ Backwards Compatibility: The Non-Constructive Modality concervative over ZFC
-------------------------------------------------------------------------------

Traditionally, set theories were used as the foundational system for mathematics. However, in the
late 1970s, the modern approach based on higher-order categorical logic started to gain momentum.
Foundational systems of this kind are known as Construction Calculi in case they incorporate the
entire higher-order constructive logic, or intuitionistic type theories if they only incorporate
its predicative fragment. The common core of all those systems is the intuitionistic type theory
containing basic inductive types (at least 𝟘, 𝟙, 𝔹 = {ff, tt}, and ℕ of natural numbers), and a
cummulative hierarchy of universes 𝒰 : 𝒰⁺ : 𝒰⁺⁺ : ··· closed under forming dependent sums Σ and
products Π, so that every type (and every finite collection of types) liese inside some universe.

The Observational Calculus of Constructions $CC_{obs}$ [Pujet-Tabareau2022] extends the core ITT
by an impredicative universe of definitionally atomic types `Ω° : 𝒰`, and a relation of strict
observational equality `a ~ᵀ b : Ω°` for every type `T`. For plain types `T`, `a ~ᵀ b` reflects
observational equality between elements `\a \b : T`. For example for functions `\f \g : ℕ → ℕ`,
observational equality is the pointwise equality `(f ~ g) := ∀(\n : ℕ) f(n) ~ g(n)`. For types
`\X \Y : 𝒰ⁿ` themselves `(X ~ Y)` reflects “literal” equality, e.g. `(X × Y) × Z ≁ X × (Y × Z)`.

The type `Ω°` has a remarkable property that the logical power of impredicativity is locked inside.

[The first draft](./choice) in the series utilizes this property to introduce the non-constructive
modality `‖_‖ᶜ`. This modality allows to employ non-constructive methods, including the Axiom of
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
(vie Kripke Representation) into the Heyting algebra of up-sets of some poset. It seems possible
to interpret $CC_{obs}($n + 1$)$ in ZFC($n + 1$) in such a way that the propositions `p : Ω°` will
be modeled by up-sets of the inaccessible cardinal $V_κ$ taken as a poset of sets ordered by
inclusion. On the type-theoretic side we'll have the type of Aczel sets `Vₙ` modelling ZFC($n$).
Utilizing the impredicative universal quantifier of `Ω°` to represent both finitary and infinitary
conjuntions, it seems possible to translate each `s : Vₙ` into the respective Zakharyaschev subframe
canonical formula `φₛ : Ω°` [Bezhanishvili2022] that will be in interpreted precisely by the set
$s ∈ V_κ$ in the model, finalizing the circle.

[Bezhanishvili2022]: “Axiomatization Techniques for Intermediate Logics”  Since

§ Reconciling Equality and Identifiability
------------------------------------------

Unfortunatelly, $CC_{obs}$ fails to qualify as the ultimate foundational system for two reasons:
* `Ω°` does not contain enough propositions to be a subobject classifier, that is the proposition
“`y : T` belongs to the image of the function `f`” is not always representable by a definitionally
atomic `P : T → Ω°`, and conversely not every provably functional relation `R : X → Y → Ω°`can be
turned into a function;
* $CC_{obs}$ fails to respect the structural principle of equivalence: no constructions or theorems
should be able to tell apart isomorphic groups, homeomorphic spaces, equivalent categories, etc.
In the original formulation of $CC_{obs}$, the universe `Ω°` is not an isolated universe, but a
subuniverse of all other universes `Ω° ⊂ 𝒰ⁿ`, thus it is possible to state a theorem for, say a
group that is “literally” equal to ℤ₂ instead of being merely isomorphic to it.

At the time observational equality underlying $CC_{obs}$ was introduced [Altenkirch-McBride2007],
everyone assumed that the respective notions of equivalence for various mathematical structures
have to be defined by hand for each one separately: isomorphisms for groups, homeomorphsisms
for topological spaces, equivalences for categories, bisimularity for automata, etc.


§ Handling Large Categories and Internalization: Typed Unbounded Quantifiers
----------------------------------------------------------------------------

It is well known that the ZFC set theory alone poorly captures the category theory. For many
applications in modern mathematics (algebraic geometry, algebraic number theory, homological
algebra, etc.) ZFC has to be extended by additional axioms postulating the existence of enough
Grothendieck universes. But even then, the language of ZFC is incapable of expressing
large concrete categories (such as the category of all groups). This issue was first solved
by extended set theories ZFC/S and ZMC/S with appropriate reflection principles [Shulman2008].

[Our second draft](./star-is-more) in the series introduces the type-theoretic counterpart of
unbounded universal quantifiers with introduction and elimination rules corresponding to the
reflection principle of ZMC/S. This extension allows to handle large concrete categories and
such objects as the large ordered field of surreal numbers.

We show how to extend the resulting theory to incorporate the desired forms of internalization.
Both inductive types and mathematical structures defined polymorphically for all types, can be
also interpreted internally to any category that admits all universal constructions used in
their defintions.

In a form of type theory with “unbounded“ universal quantifiers, internalization can be mechanized.
With a usual the definition of a group one gets the defintion of group objects in finitely complete
categories for free, with all proofs about groups and constructions on groups automatically
appliable to group objects, if all proof/construction elements are expressable in finitely
complete categories.

§ Reconciling Equality and Identifiability
------------------------------------------

Unfortunatelly, the theory $CC_{obs}$ fails to faithfully capture the equality between types, as
well as types endowed with some additional structure (e.g. groups, rings, and topological spaces).
At the time observational equality underlying $CC_{obs}$ was introduced [Altenkirch-McBride2007],
everyone assumed that the respective notions of equivalence for various mathematical structures
have to be defined by hand for each one separately: isomorphisms for groups, homeomorphsisms
for topological spaces, equivalences for categories, bisimularity for automata, etc.

In 2010 V. Voevodsky made a buffling discovery. If we are to define equality between any pair of
types `A : U` and `B : U` as effectively 1-to-1 correspondence (that is, a type `Corr : A × B → U`,
together with a left inverse function and a right inverse function), the natural notions of
equality (or, better to say, identifiability) for each type of mathematical objects is implied by
their respective definitions. There is no need to define and manually enforce isomorphisms,
homeomorphisms, equivalence, etc as “generalized equality”. The aforementioned definition of
equality for types is known as the univalence principle.

Discovery of this principle led to a mathematical revolution not only in the field of type
theories, but also in higher category theory and foundations of mathematics. As a result of
large collaborative program the so called Homotopy Type Theory was born in 2014. As a byproduct,
one solved the longstanding problem of dealing constructively with analytic notion of real numbers.

It took over a decade to develop a constructive type theory incorporating the univalence principle.
The resulting theory HOTT (in all capitals, Higher Observational Type Theory) was presented in
early 2022 in a series of talks by M. Shulman, and then later by A. Kaposi at TYPES 2022
conference.

In the third draft we outline how to combine the HOTT, `Ω°`, `‖_‖ᶜ`, and unbounded quantifiers.
The resulting system (together with propositional resizing and dependent coinductive types) deserves
to be called Higher Observational Construction Calculus, the namesake for this project.

§§ Expressing Dependently Typed Languages
-----------------------------------------

A structuralist foundational system has to be able to handle formalized languages both of other
theories and of its own. inductive types That is the only way to carry out metamathematical
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
`ProofTerm(\ctx : Δ[𝓢], \statement : Formula(ctx))`, where the index type `𝓢 : *ᴵ` is the signature of the
respective FOLDS. The possibility to express sort signatures with dependencies (both finitary
and infinitary, like in case of theory of ω-categories) as Reedy index types allow to define
FOLDS-theories and H(igher)OLDS-theories as polymorphic structures with a single parameter `\Carrier : S` (see unbounded quantifers above).

[Our fourth draft](./reedy-types) is concerned by interpreting dependent type theories
inside dependent type theories. Continuing the line of research I pursued even before entering
JetBrains Research, I propose introducing the notion of Reedy index types and inductive type
families indexed over them. Higher index types are the type-theoretical counterpart of Reedy categories,
functions defined on them are required to specify their actions not only on the indices themselves
but also on the arrows between them, thus being functors by definition. Inductive type families
over higher index types turn out to be presheaves.

By allowing Reedy index types and functions on them to be defined simultaneously (and mutially
recursively) with inductive type family indexed by them one gains the ability to interpret dependent
type theories (in bi-directional presentation) as inductive types. This can be further generalized
two-fold:
* Extension by large induction-recursion to allow the theory to represent its own syntax by
utilizing “external” termination checking à la “The Gentle Art of Levitation”.
* Extension by path constructors for the type family (but not for the index) makes recursion
motives for those types form an extension of GAT⁻ (Generalized Algebraic Theories without
equations on sorts) we'd like to call XATs (Extended Algebraic Theories) or higher XATs depending
on their set-truncatedness. We conjecture that XATs have natural functorial semantics in the
doctrine of weak model categories, while higher XATs require ∞-categories. We speculate about
possible generalization to directed higher XATs with natural semantics in ω-categories.

§ Finitistic Core
-----------------

Now that it is possible to represent languages of proofs directly, it makes sense to use the
resulting theory for metamathematical applications, e.g. to prove cut-elimination for specific
theories. Yet the theory is very far from being a weak finitistic core, one strives to use, when
deriving metamathematical results. Fortunatelly, with the approach mentioned in the draft on
unbounded quantifiers, proofs can be “compiled” into a weaker microcosm, given they don't use
any primitives not available there. [The fifth draft](./finitistic-core) in series is concerned
by defining such a microcosm, namely a finitistic core system metamathematical proofs are compiled
into. There I develop a constructive version of the non-Gödelian theory of hereditary finite sets
by F. Pakhomov, the only known example of a natural axiomatic system that is able to prove its
own consistency. I develop a logic-free (type-theory like) calculus for this theory to implement
its by-construction faithful model inside HOCC.
