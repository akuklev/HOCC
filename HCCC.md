Towards Higher Categorical Construction Calculus
================================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)


Almost a decade ago, the co-founder and former CEO of JetBrains Sergey Dmitriev told me about his
long-standing ambition to establish a database of mathematical results, definitions, conjectures,
constructions, and proofs. This database should allow content-based search and connect related
results into a web. Ideally, it should also connect results on mathematical objects of similar
structure even if their relatedness is not apparent and unknown to the respective authors.
Independently, related ideas got traction inside the mathematical community over the last years,
as exemplified by the [Formal Abstracts Project](https://formalabstracts.github.io/) by Thomas C. Hales.

The prerequisite for such a database is a common formalized language for mathematics, which has to be
syntactically appealing, comfourtable to work with and most importantly expressive enough to satisfy
the needs of mathematicians. We made a substantial progress towards this goals by meticulously piecing
together existing ideas, and also by developing original ones.

§ The HCCC Project
------------------

Over almost a decade at JetBrains Research, I've been intensely working on expressivity the
foundational systems underlying modern proof assistants, and came to a vision of what we call
Higher Categorical Construction Calculus.

On the reasoning side, HCCC is a natural deduction calculus capable of classical reasoning with
choice, abstractness-aware reasoning, structural induction over its own language, and (higher)
categorical reasoning.

On the other side, HCCC is a construction calculus capable of expressing canonical objects,
structures, and algorithmic constructions involving them. Expressable canonical objects include
datatypes of integers, reals, formalized languages of any complexity, and datatypes of their
respective expressions, while expressable structures include the ones defined by arbitrary
axiomatic theories in any signature, i.e. a formalized language of underlying expressions. 

Those structures (models of axiomatic theories) come conviniently prepackaged into parametrized
higher categories together with structure-respecting correpsondences between them, expressing
equivalence (one-to-one correspondences), relatedness (many-to-many correspondences), and
homomorphism  many-to-one correspondences), and also correspondences between correspondences
themselves, so that proofs and constructions can be specialized, generalized, and transferred
along those correspondences. In fact, the theories themselves also come packed into categories,
considering theories that generate equivalent classes of models as equivalent (Morita-equivalence),
and taking relative interpretability of theories as homomorphisms. Moreover, the higher categories
also form a parametrized higher category, and parametrized higher categories form a parametrized
parametrized categories and so on, while all constructions and proofs about higher categories
(and any other theories) are also automatically applicable to their parametrized versions.

In the first paper we outline extending Martin-Löf Type Theories by types representing 
inductively-defined synthetic categories, and functors on those. When applied to the Higher
Observational Type Theory by Shulman et al., we would pressumably obtain a Higher Categorical
Type Theory, featuring native ω-categories and capability to express its own syntax as an
inductive datatype, but still interpretable in an arbitrary ∞-topos.

In [“◇Classical and □Parametric Modalities for Martin-Löf Type Theories”](modalities.md) we
outline extending MLTTs by a dual pair of modalities enabling both abstractness-aware and
classical reasoning with choice, and also hugely expanding available algorithmic constructions
by enabling all classically provable algorithms, without compromising its favorable computational 
properties and decidability of typechecking. The modalities also allow convinient introduction
of impredicative universe of propositions and reflective Mahlo universes of types, making the
underlying type theory excellently suitable for performing large constructions widely used in
(higher) algebraic geometry. We obtain HCCC by applying these extensions to HCTT.

By introducing these modalities we also greatly enhance expressivity of the underlying theory
taken as a programming language. While MLTTs can be understood as total functional programming
language, adding modalities makes it into a functional logic programming language. 

While not very relevant for mathematal applications, for practical usability as a functional programming language, the system has to be extended with
indexed modalities for size-guarded recursion and clock-guarded corecursion, which are known to
be eliminable towards of ordinary recursion and corecursion at cost of substantial complexity blowup.
