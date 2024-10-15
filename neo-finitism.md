Hilbert's Program
=================

In 1920s David Hilbert proposed a vision that every formal system to be used im mathematics should be proven consistent within a thrusted finitistic core system. However, quite soon Kurt Gödel has proven that impossible: non-finitistic systems require strictly stronger systems to prove their consistency.

The idea of Hilbert transformed into a somewhat arbitrary “second best” practice. Instead of proving formal systems (in particular an axiomatic theories) consistent, mathematicians “establish their soundness” by constructing a set-theoretic model within the standard set theory ZFC or an establised large cardinal extension thereof. For constructive systems the constructive set theory CZF and its extensions are used. 

In 2019 Fedor Pakhomov has developed a theory H<sub><ω</sub> of hereditarily finite iterative sets with a remarkable property: H<sub><ω</sub> proves it own consistency. H<sub><ω</sub> evades the curse of Gödel theorem by being a finitistic theory: it cannot prove existence of any infinite set.
H<sub><ω</sub> = minimalistic base theory H₀ = {extensionality, separation, and weak hierarchy} + a countably infinite family of axioms ∃0, ∃1, ∃2,... extablishing existence of all finite ordinals. We conjecture it is possible to develop a constructive set theory CZF₀, such that CZF(ɑ) = CZF₀ + ∃β for all β < ɑ has following properties:
- CZF(ω) proves its own consistency
- CZF(ω) proves the same facts about hereditarily finite sets as H<sub><ω</sub>
- CZF(ɑ) does not prove wellfoundness of ɑ, i.e. proof-theoretic ordinal |CZF(ɑ)| = ɑ
- CZF(|CZF|) = CZF, the standard constructive set theory, where |CZF| is the Bachman-Howard ordinal
- CZF(ɑ) has models of all CZF(β) with β < ɑ, moreover CZF(ɑ) = CZF₀ + Con(CZF(β)) for all β < ɑ
— For limit ordinals ɑ, CZF(ɑ) satisfies reflection principle: has inner models for each of its finite subtheories

The standard set theories ZFC/CZF, possibly with some large cardinal hypotheses, also belong to the CZF(ɑ)-family, which makes it a canonical substrate for constructing models of other formal systems and precisely measuring thir modelling capacity.
