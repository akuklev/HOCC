Higher Categorical Construction Calculus
========================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)

At JetBrains, we create the most effective, convenient, and pleasurable tools for engineers and researchers, allowing their the natural drive to develop to thrive and bear fruit. We dare to pursue ambitious visionary ideas. We wanted to use a programming language we'd enjoy using, so we created one, and now Kotlin is the language of choice for millions of software developers worldwide.

We want to make Kotlin fully amenable for certified programming, and we want a proof language and toolset we'd enjoy using. We believe that the best proof language for certified programming should be a general-purpose language suitable for stating results, definitions, conjectures, constructions, and proofs in all mainstream areas of mathematics and computer science. After 25 years of our own active research, aided by a number of breakthroughs sprouting from the Univalent Foundations Program, we finally have a blueprint for such a language.

§ Overview
----------

The Higher Categorical Construction Calculus (HCCC) is a powerful univalent computational type theory. As a proof calculus, it is capable of classical reasoning with choice, abstractness-aware (“parametric”) reasoning, structural induction over its own language, and higher categorical reasoning. It enjoys decidable proof checking and can be shown consistent relative to a strong form of classical set theory.

We call it a _construction calculus_, because, besides proofs, it can express canonical objects, structures, and algorithmic constructions involving them. Expressable canonical objects include the basic numerical datatypes such as (unbounded) integers, (Cauchy) reals, (pure) functions between and subsets of other expressable datatypes, and also datatypes of expressions/sentences for arbitrary formalized languages, allowing to provide interpretations with structural recursion and proofs by structural induction. Expressable structures include those defined by arbitrary axiomatic theories in any formalized language. 

We call it _higher categorical_, because these structures (models of axiomatic theories) come conveniently prepackaged in parametrized ω-categories, together with (towers of) structure-respecting correpsondences, expressing equivalence (one-to-one correspondences), relatedness (many-to-many correspondences), and homomorphisms (many-to-one correspondences), so that proofs and constructions can be specialized, generalized, and transferred along these correspondences. Proofs and constructions also transfer upwards, i.e., all constructions and proofs about ω-categories and any other theories also apply to their parametrized versions, which is crucial since models of an axiomatic theory, including ω-categories themselves, form a parametrized higher category, and parametrized ω-categories form a parametrized parametrized ω-category, and so on.

§ HCCC = HOTT + Prototypes + ◇/□-Modalities + Reflective universes
------------------------------------------------------------------

In [Type-theoretic approach to Reedy categories](reedy-types) we propose to extend Martin-Löf type theories by types representing Reedy categories, presheaves on them and functors between them. When applied to the Higher Observational Type Theory of Shulman et al. we would presumably obtain an autophagic computational type theory with native ω-categories still interpretable in an arbitrary ∞-topos. 

In [“◇Classical and □Parametric Modalities for Martin-Löf Type Theories”](modalities) we propose to extend MLTTs by an S4-pair of modalities, allowing both abstractness-aware “parametric“ and classical reasoning with choice, and also vastly extending the available constructions by allowing all classically provable algorithms, without compromising its favorable computational properties and decidability of proof/type checking.

The modalities also facilitate the introduction of reflective universes, resulting in a type-theoretic counterpart of M. Shulman's “Set theory for category theory” ZMC/𝕊, and making the underlying type theory eminently suitable for performing large constructions widely used in (higher) algebraic geometry. 

HCCC is obtained by integrating all of these extensions.

§ Syntax and amenities
----------------------

While the aforementioned features make the purposed system an optimal for a proof assistaint, it does not automatically make such a proof assistaint a viable alternative to hand-written proofs. We need a pleasent and concise syntax, and a lot of additional mechanisms that minimize the formalization
pain: reduction of neutral terms and applications non-determinstic confluent reduction rules, support for
algebraic ornaments together with versatile subtyping, highly configurable implicit conversions, resolution
and derivation of implicit arguments, type inference, and proof inference (including but not limited to SMT
solvers and specialized solvers). In [Literate Kotlin series](litkot.md) we propose a versatile syntax based
on Kotlin, Python, Lean, and Agda simultaneously, and propose some machinery related to subtyping
and inference. The rest is a work in progress that probably only can really start after an initial
implementation of HCCC is available.

§ Towards Certified Kotlin
--------------------------

## Non-interactive programming

MLTTs can be understood as purely functional programming languages, and HCCC is an exceptionally powerful one due to its ability to make use of classical termination proofs. Since the Turing-complete `Computation<T>`-monad is available as an inductive-inductive type, non-terminating computations can also be expressed and handled. Furthermore, the computational interpretation of ◇-modality is given by the recently introduced Verse Calculus, adding the great expressiveness of functional logic programming. HCCC seems to have everything you could ever want, but as great as it sounds in theory, programming in bare-bones intensional type theories demands for frustrating amounts of explicit proofs of termination, productivity, and convertibility.

For practical use, MLTTs must be extended with indexed modalities for size-guarded recursion and clock-guarded corecursion, and SMT solver-based liquid types.

## Embracing interactive programming

To embrace interactive programming, we'll need to introduce substructural types for
non-discardable/non-sharable objects, and the substructural cousins of the ◇- and □-modalities:
`interactive` and `pure`, the `interactive` being closely related to `suspend`-modality of Kotlin
coroutines. We'll have to develop notions of (captured or standalone) objects, references and capabilities,
the linear, cartesian, and opaque variables related to the same entity. We'll understand Kotlinesque
structured concurrency and Rustacean structured (lifetime-based) ownership-management as instances
of structured ressource management, where managed shared objects are understood as addressable parts
of the state of their respective arena, which can be either an addressable part of an enclosing arena
a captured/standalone object in its own right. We'll show how objects and arenas can be defined as
in terms of algebraic effects/algebraic effects with parameters, and how the description of states of
those objects in terms of trace algebras (concurrent generalization of trace monoids, algebras over
the boolean rig) gives rise to separation logic embedded into the substructural part of the type theory.

if we launch two coroutines in parallel, and those coroutines perform actions on the same location
`ref` of an arena `A`, the state `s : ◇A` of the arena (an element of its trace algebra) is given
by a sum of possible outcomes for all possible on orders of the actions of those two coroutines.

A substructural ◇¹-modality arises in connection with concurrent programming and separation logic. There, 

The resulting system embraces mutability, concurrency and interactive programming, and can be used
as a type system for a strict variant of Kotlin, making it fully amenable for certified programming.

Similar to the case of guarded (co)recursion modalities, the system remains desugarable into plain
HCCC by encoding objects as paramatrized relative (co)monads and interpreting expressions involving
substructural types via do-notation, see [Paella: algebraic effects with parameters and their handlers](https://icfp24.sigplan.org/details/hope-2024-papers/7)

# Future Work

## Biconstructive logic and Chu spaces

Classical logic allows proofs involving double negation elimination and choice. Constructive logic limits application of those proof techniques so as to guarantee that the notion of existence is effective. That is, given a constructive proof of existence `pf : ∃(x : T) P(x)` it is possible to construct such an `x` satisfying `P(x)` algorithmically. It is possible limit proof techniques even more to ensure that a proof of non-universality `pf : ¬∀(x : T) P(x)` allows to algorithmically extract a counterexample `x` satisfying `¬P(x)`. Such a restriction could be called biconstructive logic and is only possible in presence of substructural types, see [“Affine logic for constructive mathematics” (M. Shulman)](https://arxiv.org/abs/1805.07518).

The universe `Chu` of biconstructive propositions turns out embrace peculiar cases of predicates (describing interacting non-deterministic automata)
which are nither connected by an implication, nor orthogonal. Such propositions satisfy Heisenbergian uncertainty principles, making them appear as if they would describe quantum systems. Yet, the non-deterministically interacting automata do not exhibit any real quantum behaviour as long as we assume them to have fixed states that can be measured simultaneously. If we take relativity of simultaneity (Einsteins relativity) into account, it turns out they _could_ exhibit genuinely quantum behaviour: a mathematically possible state of mutual interdependency of two interacting non-determinisitc automata, which cannot be physically prepared and is inconsistent with the assumption that both systems have a fixed internal state simultaneously.

## Quantum computing, quantum systems, and quantum logic

Entanglement in the sense of Verse Calculus/◇-modality is a perceived behavior emerging from the properties of underlying computational system that performs backtracking to find solutions to a set of constraints. It can only emerge where backtracking is available, and thus is incompatible with interactions as these are not reversible/undoable.

By making a wild conjecture that we can have arenas where interactions are somehow compatible with entanglement, we obtain a system where the impossible states from the previous section are attainable. We conjecture, that the resulting formalizm has a “computational” interpretation in the terms of quantum computation. We conjecture, that the resulting formalizm is a suitable dependently typed programming language for quantum computers, the spaces of the Chu-predicates on objects being the inner-product vector spaces of their quantum states.

We'll discuss how arenas allow describing interactive/interacting quantum systems and quantum fields, how the respective trace algebras turn into C*-algebras of those systems/fields, and how the combination of the separation logic generated by those C*-algebras and the notion of biconstructive proofs could shed some light on the nature of quantum reality.

We can also think about trace algebras over some rig R which is neither booleans, nor the complex numbers. Given an arena with a trace algebra over the rig
R, let us write down its nondeterministic states as `x : ◇ᴿT`. The types `◇ᴿT` can be understood as spectra over R, and R can be a generalized rig, that is algebras over the sphere spectrum, with 𝔽₁ being the initial generalized ring. For `R = 𝔽₁`, the type `◇ᴿT = T`. Connections to the absolute geometry (Connes) and to the type theory of parametrized spectra should be studied.


## Dialectica type theory: A size-graded type theory with internal ∂

It should be possible to develop a resource-aware (hopefully non-Gödelian) variant of non-interactive HCCC,
where only size-non-increasing functions on sized types can be shown to exist unconditionally. All the other
functions can be only shown to exist conditionally on the availability of enough “computational budget”, with
their derivatives (computed by an internal Dialectica translation ∂) describing how much computational budged they
need for arguments of given size.

It might turn out to be possible to provide a translation of this type theory into the self-verifying
Pakhomov set theory. It might be possible to provide an understanding of this type theory as a type theory
of Diophantine equations.
