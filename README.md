Towards Higher Observational Construction Calculus (draft)
==========================================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)


Some 6 years ago, co-founder and former CEO of JetBrains Sergey Dmitriev told me about his long-standing hope to
establish a database of mathematical results, definitions, conjectures, constructions and proofs.
This database should allow content-based search and connect related results into a web. Ideally,
it should also connect results on mathematical objects of similar structure even if their
relatedness is not apparent and unknown to the respective authors.
These ideas got some traction inside the mathematical community, as exemplified by the
[Formal Abstracts Project](https://formalabstracts.github.io/) by Thomas C. Hales.


The prerequisite for such a database is a common formalized language for mathematics with a pleasing
syntax and enough expressivity to satisfy the needs of almost all mathematicians. Modern proof
assistants made huge progress in this direction in the last few years. In 2021, Fields medalist and
one of the top world mathematicians Peter Scholze, started a project on formalizing an entirely new
result in modern mathematics using Lean Proof Assistant. This project - the Liquid Tensor Experiment -
ran unexpectedly well and has indeed managed to subdue skepticism on
proof formalization in large parts of the mathematical community.

However, modern proof assistants are still afflicted with insufficient expressivity and excessive
verbosity.

§ The HOCC Project
------------------

Over the last five years in JetBrains Research, I've been intensely working on solving
the expressivity problems of the foundational systems underlying modern proof assistants. In the present
series of drafts, I outline the solutions to the most critical expressivity problems.

Traditionally, set theories were used as the foundational system for mathematics. However, in late
1970s, a new approach based on higher-order categorical logic started to gain momentum. This
approach was first plagued by problems handling equality and the universe of
propositions. A series of more than a dozen incremental improvements over the last 30 years
recently culminated in the Observational Calculus of Constructions $CC_{obs}$ [Pujet-Tabareau2022].
This remarkable foundational system is the first one to flawlessly handle both the universe `Ω`
reflecting all propositions and the equality between inhabitants of setlike types.

* [The first draft](./non-constructive-modality) in the series introduces the non-constructive
modality `‖_‖ᶜ` for $CC_{obs}$. This modality allows to employ non-constructive methods, including
the Axiom of Choice, inside a fenced fragment of the otherwise constructive framework of $CC_{obs}$.
Non-constructive modality does not compromise decidability of typechecking and effectiveness of evaluation.
That is, both the algorithm verifying proofs and the algorithm evaluating a closed expression of the type `ℕ`
(or any other purely inductive type) are guaranteed to terminate in a finite time and produce
a specific result. By adapting the results of B. Werner, M. Ratjen and S. Tupailo it can be shown
that the theory $CC_{obs}$ + `‖_‖ᶜ` has a model of the standard set theory ZFC, that is faithful
for all mathematical formulae in the sense of Rathjen and Tupailo: a (generalized) mathematical
formula φ can be proven in ZFC if and only if there is a proof of `‖φ‖ᶜ` in $CC_{obs}$.

It is well known that the ZFC set theory alone poorly captures the category theory. For many
applications in modern mathematics (algebraic geometry, algebraic number theory, higher
homological algebra) ZFC has to be extended by additional axioms postulating the existence of
enough Grothendieck universes. But even then, the language of ZFC is incapable of expressing
large concrete categories (such as the category of all groups). This issue was first solved
by extended set theories ZFC/S and ZMC/S with appropriate reflection principles[Shulman2008]

* [The second draft](./star-is-more) in the series introduces the type-theoretic counterpart of unbounded universal
quantifiers with introduction and elimination rules reproducing the Shulman reflection principle.
This extension allows to handle large concrete categories and similar objects. If used in
conjunction with inductive-recursive types, it also captures the categorical approach to
mathematical structures. That is, proofs and constructions regarding objects of particular kind
(say, groups) can be automatically applied to group objects in arbitrary microcosms (say, finitely
complete categories) as long as both the definitions of objects and the elements of the proofs and
constructions use only the primitives available in the respective microcosms.

Unfortunatelly, the theory $CC_{obs}$ fails to faithfully capture the equality between types, as
well as types endowed with some additional structure (e.g. groups, rings and topological spaces).
At the time observational equality underlying $CC_{obs}$ was introduced[Altenkirch-McBride2007],
everyone assumed that the respective notions of equivalence for various kinds of mathematical
structures have to be defined by hand for each kind: isomorphisms for groups, homeomorphsisms
for topological spaces, equivalences for categories, bisimularity for automata, etc.

In 2010 V. Voevodsky made a buffling discovery that a concise principle governing equality for
types only, implies the natural notion of equality for topological spaces, groups, rings,
categories, graphs, automata, functions and even propositions. Discovery of this principle,
known as Univalence Axiom, led to a mathematical revolution not only in the field of type
theories, but also in higher category theory and foundations of mathematics. As a result of
large collaborative program the so called Homotopy Type Theory was born in 2014. As a byproduct
one solved the longstanding problem of dealing constructively with analytic notion of real numbers.

Unfortunatelly, the Homotopy Type Theory as defined in 2014 lacked good computational properties,
in particular it lacks effective evaluation. After a multitude of developments (in particular,
by Valery Isaev) an exceptionally elegant computational variant of the theory was presented in
early 2022 in a series of talks M. Shulman. They author coin the name Higher Observational Type
Theory (HOTT). While still unpublished and not yet implemented in any proof assistant, HOTT
seems perfectly feasible.

* In the third draft I outline how to combine the ideas of HOTT and $CC_{obs}$ by using the
aforementioned type theoretical counterparts to unbounded quantifiers. The supposed resulting
theory should be called Higher Observational Construction Calculus and gives the name to this
whole project.

There is a longstanding problem of interpreting the syntax of type theories within themselves.

* [The fourth draft](./higher-index-types) is concerned by interpreting dependent type theories inside dependent
type theories. Continuing the line of research I pursued even before entering JetBrains
Research, I propose introducing the notion of higher index types and inductive type families
indexed over them. Higher index types are the type-theoretical counterpart of Reedy categories,
functions defined on them are required to specify their actions not only on the indices themselves
but also on the arrows between them, thus being functors by definition. Inductive type families
over higher index types turn out to be presheaves.

The canonical usecase for such inductive type families are typed languages with variable-binding
operators, such as quantifiers in predicate logic. There it is not sufficient to define closed
propositions only, one has to define propositions with free variables as well. Let `T` be the
type of sorts, than the type family `Proposition(l : List[T])` will be the type of propositions with
`length l` free variables, sorts of which comprize the list `l`. The quantifier `∀ᵀ` reduces the number
of free variables by 1:
```
∀ : Proposition[\l :: \T] -> Proposition[l]
```
However, if we consider the language of proofs for the predicate logic, we run into a problem.
The language of proofs has to be an inductive type family `Proof(P)` , where `P` represents is the
proposition being proven, of the type `Proposition(l : List[T])`. We're dealing with a dependently
typed language. But with `T` carrying structure of a Reedy category, dependencies and degeneracies
can be taken into account, facilitating definitions of dependently-typed languages. By allowing
the index type to be defined mutually with the type family indexed over it, one allows to define
languages of dependent type theories where types are terms. By allowing small induction-recursion,
one allows to define type-theories with computational rules using the doctrine of bi-directional
typing. If one allows large induction-recursion, it become possible for the theory to represent
its own syntax internally, as pioneered by the “The Gentle Art of Levitation”.

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
