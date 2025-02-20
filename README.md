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
- In [“◇Classical and □Parametric Modalities for Martin-Löf Type Theories”](modalities) we propose to augment MLTTs with modal operators □ of neccesity and ◇ possibility on types, mapping datatypes `T` to the type `□T` of its finite closed expressions and the type `◇T` of its hypothetically possible inhabitants. This extension allows classical reasoning with choice under the ◇ modality, and introduction of parametric quantifiers `∀<X : T> Y` that only allow using `X` as typal parameters (parameters that are only allowed to be used in type annotations), allowing abstractness-aware “parametric“ reasoning onder the □ modality. Lastly, it vastly extends the computational power of the system by allowing all classically provable algorithms without compromising its favorable computational properties and decidability of proof/type checking.
- The modalities also facilitate the introduction of reflective universes, resulting in a type-theoretic counterpart of M. Shulman's “Set theory for category theory” ZMC/𝕊, and making the underlying type theory eminently suitable for performing large constructions widely used in (higher) algebraic geometry. 

<center><b>HCCC = HOTT + Reedy types + S4 modalities + reflective universes</b></center>
<br>

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

Computational type theories are functional programming languages capable of exhaustive internal reasoning about the programs' behavior. HCCC is an exceptionally powerful one due to its ability to make use of classical termination proofs. Since the Turing-complete `Computation<T>`-monad is available as an inductive-inductive type, non-terminating computations can also be expressed and handled. In addition, the computational interpretation of ◇-modality is given by the recently introduced Verse Calculus, which adds the great expressiveness of deterministic functional logic programming. HCCC seems to have everything you could ever want from a language for _non-interactive programming_, but as great as it sounds in theory, programming in bare-bones intensional type theories demands for frustrating amounts of explicit proofs of termination, productivity, and convertibility. 

To make a decent programming language, HCCC would need indexed modalities for size-guarded recursion and clock-guarded corecursion, and SMT solver-based system of liquid types.

§ Embracing interactive programming
-----------------------------------

Non-interactive programs only deal with data. Interactive programs interact with the environment and deal with references in addition to values. References point to some kind of locations or objects within environment, and all interactions involve one or more references. To deal with references, the notion of expression contexts has to be generalized: in addition to available variables and their types, they have to track available references and their types. The types of references may be dependent both on other values and on other references from the context, for instance we can have such context:
```
T : Type, size : Nat, h : Heap, r : h.MutableArray<T>(size)
```
Here both `h` and `r` are references, and the type of `r` depends on the type `T`, value `size` and reference `h`. Dependence of a reference type on other reference `h` signalizes that the referred object exists within the object referred by `h` (the mutable array `r` is an entity within the heap `h` in our case) and guarantees that those references can never escape the scope where their parent objects are available. 

References can annotated singular and/or unique, see [Linearity and Uniqueness: An Entente Cordiale](https://granule-project.github.io/papers/esop22-paper.pdf). “Singular” annotation forbids to create secondary references, while “unique” annotation guarantees that no secondary references were created previously outside. Dually, references can be annotated obligatory and/or obligated. “Obligatory” references have to be explicitly consumed, while “obligated” references are guaranteed to be eventually consumed outside. These are crusial for suspended coroutines (that have to be eventually resumed) and resource locks (that have to be eventually released).

Bifurcation of contexts into the value and reference part, introduces reference-level cousins of ◇/□-modalities. The usual □-modality describes finite closed expressions which also can be understood as compile-time constants (written “const” in Kotlin). Its substructural cousin is the `pure` modality: pure expressions are the ones not using any references even if these are available in the context. It's dual is the `interactive` modality, which contains expressions that might have captured references, i.e. they can make use of references not present in the context. References captured as typal parameters are known as capabilities, and essentially reproduce the [Capture Checking mechanism recently proposed for Scala3](https://docs.scala-lang.org/scala3/reference/experimental/cc.html). The `interactive` modality is closely related to `suspend`-functions in Kotlin. In fact, if we only allow IO and other interactions with external shared mutable objects only happen non-blockingly via coroutines, `interactive` and `suspend` would coincide.

**Dependent reference types and the `interactive` modality allowing to capture references = structured ressource management, a robust common generalization for Rustacean lifetime-based ownership management and Kotlinesque structured concurrency.**

On the other hand, interactivity can be incapsulated: for instance, inside a pure function we can create an instance of a persistent data structure emulating a mutable one (perhaps even a heap where new mutable objects of arbitrary signature can be allocated), and run non-pure imperative programm requiring a heap against it, while still being pure from the external point of view. The same surely applies to exceptions and algebraic effects in general: as long as we handle all exceptions/effects, we're pure from the external point of view. Similar to the case of guarded (co)recursion modalities, the whole system is desugarable into plain HCCC by encoding objects as paramatrized relative (co)monads interpreting expressions involving references types via do-notation, see [Paella: algebraic effects with parameters and their handlers](https://icfp24.sigplan.org/details/hope-2024-papers/7)

§ Emergent separation logic
---------------------------

So far we have only described a sound type system for interactive programs themselves, but have not addressed how to achieve exhaustive internal reasoning about the interactive programs' behavior.

Extrinsic state of objects can be described in terms of trace monoids. When concurrency comes into play, we have to deal with incomplete information on the operational state of objects: instead of a fixed state, we have a set of possible states. Thus for the prupose of reasoning, state of an object is described by an element of a trace algebra, a unital associative algebra the boolean rig that takes the role of the trace monoid in the concurrent setting. If we launch two coroutines in parallel, and those coroutines perform actions on the same object, its algebraic state is given by a sum of possible outcomes for all possible on orders of the actions performed on the object by these two coroutines.

Object arenas such as heaps, where fresh mutable objects and arrays can be created, and coroutine contexts where jobs can be launched, and theatres where interacting actors can be launched, have very particular trace monoids generated by a partial commutative monoid of creation and substitution operators for objective and ghost states giving rise to the associated concurrent separation logic withing the type theory.

The resulting system would embrace mutability, concurrency and interactive programming in their in their entirety, and thus can be used
as the foundation for the aspired Certified Kotlin.

§ What's missing?
-----------------

There are two major areas that are not yet covered:
- The [affine logic for constructive mathematics](https://arxiv.org/abs/1805.07518) on the reasoning side;
- Quantum algorithms and interacting quantum automata on the programming side;

Trace algebras extrinsically describing states of objects can be defined in terms of their duals, the algebras of obserables, which are commutative as observations are independent of each other. A non-commutative generalization where observations are not neccesarily independent anymore would allow describing quantum where the coexistence of multiple pure states is inherent rather than due to a lack of knowledge. The non-commutative generalization of state creation and substitution algebras would respectively allow to describe quantum arenas where separated objects might be entangled. There, the objects created separately do not in general remain separable objects, and the picture of arenas being collections of distinct objects becomes fuzzy. We're cannot reason in terms of “distinct particles“ anymore, and have to reason in terms of “field quanta” with quantum arenas being the quantum fields.

Remarkably, the affine logic for constructive mathematics we mentioned above is also deeply related to quantum systems being based on truncated Chu spaces, the general form of which is known to describe the Hilbert spaces of quantum states. It resonates with the situation in univalent type theories, where the propositions are truncated types, while the types in general are ∞-groupoids.
