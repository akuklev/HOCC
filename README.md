Towards Higher Observational Construction Calculus (draft)
==========================================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)

Since at least a decade, the co-founder of JetBrains and president of JetBrains Research Sergey
Dmitriev was envisioning a project to establish a database of mathematical results, definitions,
conjectures, constructions and proofs that would in particular allow content-based search, connect
related results into a web, and automatically search for mathematical objects of equivalent or
simular structure even when their relatedness is not apparent and unknown to the respective authors.
These ideas got some traction inside the mathematical community, as exemplified by the
[Formal Abstracts Project](https://formalabstracts.github.io/) by Thomas C. Hales.


The prerequisite for such a database is a common formalized language for mathematical results, that
has to have a pleasing syntax and satisfy needs of the vast majority of mathematicians. Modern proof
assistants made a huge progress in this direction in the last few years. In 2020-2021 fields medalist
and one of the top world mathematicians initiated a large community effort project of formalizing an
entirely new result in modern mathamatics by means of Lean Proof Assistant. This project, known as
the Liquid Tensor Experiment, ran unexpectedly well, and has indeed managed to subdue scepticism on
proof formalization in large parts of mathematical community.

However, modern proof assistants are still afflicted with unsufficient expressivity and excessive
verbosity, the latter being not only the problem of syntax but also of their foundational system.

Over the course of the last five years in JetBrains Research I've been intensely working solving
the expressivity problems of the foundational systems modern proof assistants are based on, as well
as on their syntax while taking considerations of most flexible extensibility and interoperability
with computer algebra systems and STM solvers into account. In present series of drafts I present
the solutions to the most important expressivity problems.

Traditionally, set theories were used as the foundational system for mathematics. However, in late
1970s a new approach based on higher-order constructive logics started to gather momentum. This
approach was first plagued by problems handling equality and the universe reflecting all
propositions. A series of more than a dozen incremental improvements over the last 30 years
recently culminated into the Observational Calculus of Constructions $CC_{obs}$ [Pujet-Tabareau2022].
This remarkable foundational system is the first one to perfectly handle the universe `Ω`
reflecting all propositions, and equalities between elements of setlike types.

- My first contribution is the development the non-constructive modality `‖_‖ᶜ` for $CC_{obs}$.
This modality allows to make use of non-constructive principles including the Axiom of Choice
inside a fenced fragment of the language, without compromising decidability of typechecking
and effectiveness of evaluation. That is, both the algorithm verifying proofs and the algorithm
evaluating a closed expression of the type `ℕ` (or any other purely inductive type) are
guaranteed to terminate in a finite time and produce a specific result. Additionally I adapt
results of B. Werner, M. Ratjen and S. Tupailo to show that the theory $CC_{obs}$ + `‖_‖ᶜ`
has a model of the standard set theory ZFC, that is faithful for all (generalized) mathematical
formulae in the sense of Rathjen and Tupailo: a  (generalized) mathematical formula φ can be
proven in ZFC if and only if there is a proof of `‖φ‖ᶜ` in $CC_{obs}$.

It is well known that the ZFC set theory alone poorly captures category theory. For many
applications in modern mathematics (algebraic geometry, algebraic number theory, higher
homological algebra) one has to extend ZFC by additional axioms postulating existence of
enough Grothendieck universed. But even then the language of ZFC is incapable of expressing
large concrete categories (such as the category of all groups). This issue was first solved
by extended set theories ZFC/S and ZMC/S with appropriate reflection principles[Shulman2008]

- The next contribution is the development of polymorphic quantifiers exactly mimicking
the Shulman reflection principle. This extension that allows to handle large concrete
categories and similar objects. If used in conjunction with inductive-recursive types
it also captures the categorical approach to mathematical structures. That is, proofs
and constructions about objects of particular kind (say, groups) can be automatically
applied to group objects in arbitrary microcosms (say, finitely complete categories) as
long as both definitions of objects and steps of proofs use only the primitives available
in the respective microcosms.

TODO: Metamathematics


Unfortunatelly, the theory $CC_{obs}$ fails to faithfully capture the equality between types,
as well as types endowed with some additional structure (e.g. groups, rings and topological
spaces). It was the great achievement of V. Voevodsky to discover the very possiblity to
capture typal equality formally. (...)

TODO: HOCC

TODO: Higher index types

gained tracktion