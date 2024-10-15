Hilbert's Program
=================

In 1920s David Hilbert proposed a vision that every formal system to be used im mathematics should be proven consistent within a thrusted finitistic core system. However, quite soon Kurt Gödel has proven that impossible: non-finitistic systems require strictly stronger systems to prove their consistency.

The idea of Hilbert transformed into a somewhat arbitrary “second best” practice. Instead of proving formal systems (in particular an axiomatic theories) consistent, mathematicians “establish their soundness” by constructing a set-theoretic model within the standard set theory ZFC or an establised large cardinal extension thereof. For constructive systems the constructive set theory CZF and its extensions are used. 

In 2019 Fedor Pakhomov has developed a theory H<sub><ω</sub> of hereditarily finite iterative sets with a remarkable property: H<sub><ω</sub> proves it own consistency. H<sub><ω</sub> evades the curse of Gödel theorem by being a finitistic theory: it cannot prove existence of any infinite set.
H<sub><ω</sub> = minimalistic base theory H₀ = {extensionality, separation, and weak hierarchy} + a countably infinite family of axioms ∃0, ∃1, ∃2,... extablishing existence of all finite ordinals. We conjecture it is possible to develop a constructive set theory CZF₀, such that CZF(ɑ) = CZF₀ + ∃β for all β < ɑ has following properties:
- CZF(ω) proves its own consistency
- CZF(ω) proves the same facts about hereditarily finite sets as H<sub><ω</sub>
- CZF(ɑ) does not prove wellfoundness of ɑ, i.e. the proof-theoretic ordinal |CZF(ɑ)| = ɑ
- CZF(|CZF|) = CZF, where |CZF| is the Bachman-Howard ordinal
- CZF(ɑ) has models of all CZF(β) with β < ɑ, moreover CZF(ɑ) = CZF₀ + Con(CZF(β)) for all β < ɑ
- For limit ordinals ɑ, CZF(ɑ) satisfies reflection principle: has inner models for each of its finite subtheories

The standard set theories ZFC/CZF and their large cardinal extensions belong to the CZF(ɑ)-family, which makes it a canonical substrate for constructing models of other formal systems and precisely measuring thir modelling capacity.


# Formalisation program
-----------------------

Метаматематические доказательства очень tedious и никто их не проверяет. Часть, где арифметизируется синтаксис формальных систем, вообще принимается на веру. Нам необходима end-to-end формализация. В теориях типов мы можем работать не с арифметизацией синтаксиса, а с самим синтаксисом. Более того, близка к завершению концепция синтетических типов данных, охватывающая естественное описание синтаксиса сколь угодно сложных формальных систем. Недавно также было выявлено, как ограничивать теории типов так, чтобы они содержали лишь примитивно-рекурсивные функции — достаточно прямолинейные чтобы все верили, что они завершаются, и достаточно мощные, чтобы описывать трансформацию синтаксиса — нас интересует построение модели, то есть трансформацию синтаксиса произвольной формальной системы в систему CZF(ɑ), и наоборот. Нам необходимо разработать теорию типов, содержащую все финитарные синтетические типы данных и примитивно-рекурсивные функции, создать практичный theorem-prover на её базе. Доказать что её собственный синтаксис обоюдоинтерпретируем с CZF(ω^ω), запустить community project по формализации всевозможных метаматематических результатов.

Нужна стандартная библиотека для формализации, начать которую следует с разработки logic-free calculus для CZF(ɑ), что сильно упрощает трансляцию, и обобщённую CNF-нотацию для ординалов до Бахманна-Говарда и далее докуда получится (https://akuklev.livejournal.com/1312249.html), по построению генерирующую функции, тотальность которых равносильна существованию каких ординалов.
