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


A sound theoretical foundation still needs to be put into shape. In a series of short proposals ([Literate Kotlin](https://akuklev.github.io/Literate_Kotlin/literate_kotlin.pdf), [Declarative Kotlin](https://akuklev.github.io/Literate_Kotlin/declarative_kotlin.pdf), [Academic Kotlin](https://akuklev.github.io/Literate_Kotlin/academic_kotlin.pdf), a few pages each) we develop a versatile syntax designed for excellent readability, conciseness, and typographic perfection. It is based on Kotlin, Python, Agda, and Lean, with some elements of Fortress, Scala. 
It's the culmination of over two decades of meticulous collection and evaluation of ideas, carefully assembled into a coherent system.

§ HCCC as a programming language
--------------------------------

Computantional type theories can be used as functional programming languages, and HCCC is an exceptionally powerful one due to its ability to make use of classical termination proofs. Since the Turing-complete `Computation<T>`-monad is available as an inductive-inductive type, non-terminating computations can also be expressed and handled. Furthermore, the computational interpretation of ◇-modality is given by the recently introduced Verse Calculus, adjoining the great expressiveness of functional logic programming. HCCC seems to have everything you could ever want from a language for _non-interactive programming_, but as great as it sounds in theory, programming in bare-bones intensional type theories demands for frustrating amounts of explicit proofs of termination, productivity, and convertibility. 

To make a decent programming language, HCCC would need indexed modalities for size-guarded recursion and clock-guarded corecursion, and SMT solver-based system of liquid types.

§ Embracing interactive programming
-----------------------------------

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

# What's missing?

There are two possible deeply related extensions we have not addressed so far:
- HCCC the programming language could be extended to embrace quantum algorithms and interacting quantum automata.
- HCCC the proof calculus could be extended to embrace the [“Affine logic for constructive mathematics” (M. Shulman)](https://arxiv.org/abs/1805.07518).

# A long way to go

Human readers understand implicit conversions immediately, forgive minor omissions, and think along with the author, so they are able to bridge nontrivial gaps and transform arguments "mutatis mutandis" once they grasp the idea. Any attempt at formalization is plagued by the pain to elaborate all of this explicitly.
- Convincing the machine that an obvious equality holds is especially frustrating, so we need to use every approach available to eliminate unneccesary type conversions: parallel reductions (“The Taming of the Rew”) and equations on neutral terms (New Equations for Neutral Terms), coersions along observational equality (“Observational Equality meets CiC”), the universe of explicitly propositional types (“Definitional proof-irrelevance without K”), and limited predicate subtyping (“Predicate Subtyping with Proof Irrelevance”).
- The other problem are the cases where we have to redefine a construction multiple times from scratch, because the same thing is implemented by different authors using slightly different types. This issue can and should be addressed systematically by algebraic ornaments together with configurable implicit conversions. To handle the zoo of typeclasses, we need highly configurable subtyping, resolution and derivation of implicit arguments.
- Many other cases of excessive verbosity can be addressed with an advanced system deriving implicit arguments, occasional type inference, and proof inference (including but not limited to SMT solvers and specialized solvers), and applying Liquid types.
- We should peek, adopt and improve best practices from Lean, Agda, Coq, etc.
