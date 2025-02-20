The HCCC Project
================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)

At JetBrains, we create the most effective, convenient, and pleasurable tools for engineers and researchers, allowing their the natural drive to develop to thrive and bear fruit. We dare to pursue ambitious visionary ideas. We wanted to use a programming language we'd enjoy using, so we created one, and now Kotlin is the language of choice for millions of software developers worldwide.

We want to make Kotlin fully amenable for certified programming, and we want a proof language and toolset we'd enjoy using. We believe that the best proof language for certified programming should be a general-purpose language suitable for stating results, definitions, conjectures, constructions, and proofs in all mainstream areas of mathematics and computer science. After 25 years of our own active research, aided by a number of breakthroughs sprouting from the Univalent Foundations Program, we finally have a blueprint for such a language.

§ Overview
----------

The Higher Categorical Construction Calculus (HCCC) is a powerful univalent computational type theory. As a proof calculus, it is capable of classical reasoning with choice, abstractness-aware (“parametric”) reasoning, structural induction over its own language, and higher categorical reasoning. It enjoys decidable proof checking and can be shown consistent relative to a strong form of classical set theory.

We call it a _construction calculus_, because, besides proofs, it can express canonical objects, structures, and algorithmic constructions involving them. Expressable canonical objects include the basic numerical datatypes such as (unbounded) integers, (Cauchy) reals, (pure) functions between and subsets of other expressable datatypes, and also datatypes of expressions/sentences for arbitrary formalized languages, allowing to provide interpretations with structural recursion and proofs by structural induction. Expressable structures include those defined by arbitrary axiomatic theories in any formalized language. 

We call it _higher categorical_, because these structures (models of axiomatic theories) come conveniently prepackaged in parametrized ω-categories, together with (towers of) structure-respecting correpsondences, expressing equivalence (one-to-one correspondences), relatedness (many-to-many correspondences), and homomorphisms (many-to-one correspondences), so that proofs and constructions can be specialized, generalized, and transferred along these correspondences. Proofs and constructions also transfer upwards, i.e., all constructions and proofs about ω-categories and any other theories also apply to their parametrized versions, which is crucial since models of an axiomatic theory, including ω-categories themselves, form a parametrized higher category, and parametrized ω-categories form a parametrized parametrized ω-category, and so on.

- The starting point of HCCC is the third-generation univalent type theory HOTT currently being developed by Shulman et al. 
- In [Type-theoretic approach to Reedy categories](reedy-types) we propose to extend Martin-Löf type theories by types representing Reedy categories, presheaves on them and functors between them. When applied to the Higher Observational Type Theory of Shulman et al. we would presumably obtain an autophagic computational type theory with native ω-categories still interpretable in an arbitrary ∞-topos. 
- In [“◇Classical and □Parametric Modalities for Martin-Löf Type Theories”](modalities) we propose to extend MLTTs by an S4-pair of modalities, allowing both abstractness-aware “parametric“ and classical reasoning with choice, and also vastly extending the available constructions by allowing all classically provable algorithms, without compromising its favorable computational properties and decidability of proof/type checking.
- The modalities also facilitate the introduction of reflective universes, resulting in a type-theoretic counterpart of M. Shulman's “Set theory for category theory” ZMC/𝕊, and making the underlying type theory eminently suitable for performing large constructions widely used in (higher) algebraic geometry. 

<center><b>HOTT + Reedy + ◇/□ + reflective universes = HOCC</b></center>
<br><br>

A sound theoretical foundation still needs to be put into shape. In a series of short proposals ([Literate Kotlin](https://akuklev.github.io/Literate_Kotlin/literate_kotlin.pdf), [Declarative Kotlin](https://akuklev.github.io/Literate_Kotlin/declarative_kotlin.pdf), [Academic Kotlin](https://akuklev.github.io/Literate_Kotlin/academic_kotlin.pdf), a few pages each) we develop a versatile syntax designed for excellent readability, conciseness, and typographic perfection. It is based on Kotlin, Python, Agda, and Lean, with some elements of Fortress and Scala. 
It's the culmination of over two decades of meticulous collection and evaluation of ideas, carefully assembled into a coherent system.

§ A proof language we'd enjoy using? A long way to go...
--------------------------------------------------------

Human readers understand implicit conversions immediately, forgive minor omissions, and think along with the author, so they are able to bridge nontrivial gaps and transform arguments "mutatis mutandis" once they grasp the idea. Any attempt at formalization is plagued by the pain to elaborate all of this explicitly.

To start with, known issues with known solution approaches have to be addressed:
- Most frustrating are the faulty type mismatch errors caused by obvious equalities and subsumptions not holding computationally. Every available solution approach should be taken into account: parallel reductions ([“The Taming of the Rew”](https://dl.acm.org/doi/10.1145/3434341)) and equations on neutral terms ([“New Equations for Neutral Terms”](https://dl.acm.org/doi/10.1145/2502409.2502411)), coercions along observational equality ([“Observational Equality meets CiC”](https://hal.science/hal-04535982v1)), the universe of explicitly propositional types ([“Definitional proof-irrelevance without K”](https://dl.acm.org/doi/10.1145/3290316)), limited predicate subtyping ([“Predicate Subtyping with Proof Irrelevance”](https://arxiv.org/abs/2110.13704)), and maybe also automated proof synthesis.
- The richness and flexibility of the type system lures into reinventing the wheel. Every library tends to use its own slightly different inventory of standard types and typeclasses, which massively hinders their interoperability. Luckily, this issue be addressed systematically by [algebraic ornaments](https://arxiv.org/abs/1212.3806) and [Dependent Interoperability](https://dl.acm.org/doi/abs/10.1145/2103776.2103779) in connection with a typeclass-based mechanism of contextual implicit coercions as in [Lean](https://lean-lang.org/functional_programming_in_lean/type-classes/coercion.html) and [Scala3](https://dotty.epfl.ch/docs/reference/contextual/conversions.html). Besides that, we need Fortress-style configurable inheritance (`Ring extends Monoid(::(·)), AbGroup(::(+))`) for sensible typeclass hierarchies, and a contextually configurable mechanism of instance resulution and derivation.
- Many other cases of excessive verbosity can be addressed with [liquid types](https://dl.acm.org/doi/10.1145/3632912), as well custom solvers and tactics, including the ones handling identification along canonical isomorphisms, elaboration of ”without loss of generality” and "mutatis mutandis" arguments, separation of “general position” arguments and corner case handling.
- Besides all that, we should look at, adopt and improve upon best practices from Lean, Agda, Coq, etc.


§ HCCC as a programming language
--------------------------------

Computational type theories can be used as functional programming languages, and HCCC is an exceptionally powerful one due to its ability to make use of classical termination proofs. Since the Turing-complete `Computation<T>`-monad is available as an inductive-inductive type, non-terminating computations can also be expressed and handled. In addition, the computational interpretation of ◇-modality is given by the recently introduced Verse Calculus, which adds the great expressiveness of deterministic functional logic programming. HCCC seems to have everything you could ever want from a language for _non-interactive programming_, but as great as it sounds in theory, programming in bare-bones intensional type theories demands for frustrating amounts of explicit proofs of termination, productivity, and convertibility. 

To make a decent programming language, HCCC would need indexed modalities for size-guarded recursion and clock-guarded corecursion, and SMT solver-based system of liquid types.

§ Embracing interactive programming
-----------------------------------

Non-interactive programming languages only deal with data. To go beyond that, we'll need a concept of environment the programs can interact with and references representing the fragments of environment available for interaction. As opposed to values, reference-valued variables are substructural (not invariant under substitution) and may have usage limitations (be non-discardable and/or non-sharable), properly described in terms of quantitative type theories first introduced by C. McBride. In a language with references, the contexts of expressions keep track of available references besides available values, leading to substructural cousins of ◇/□-modalities. The usual □-modality (written “const” in Kotlin) describes finite closed expressions which also can be understood as compile-time constants. Its substructural cousing is the `pure` modality: pure expressions are the ones not using any references even if these are available in the context. It's dual is the `interactive` modality, which contains expressions that might have captured references, i.e. they can make use of references not present in the context. The `interactive` modality is closely related to `suspend`-functions in Kotlin. In fact, if we only allow IO and other interactions with external shared mutable objects only happen non-blockingly via coroutines, `interactive` and `suspend` would coincide.

We'll understand Kotlinesque structured concurrency and Rustacean structured (lifetime-based) ownership-management as instances
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

§ What's missing?
-----------------

There are two major areas that are not yet covered:
- Quantum algorithms and interacting quantum automata on the programming side;
- The [affine logic for constructive mathematics](https://arxiv.org/abs/1805.07518) should be available on the reasoning side

Remarkably, these two are deeply related: the latter one is based on truncated Chu spaces, the general form of which is known to also describe the Hilbert spaces of quantum states. It resonates with types in univalent type theories being ∞-groupoids and propositions being the truncated ones.
