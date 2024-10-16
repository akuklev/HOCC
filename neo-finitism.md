Hilbert's Program
=================

In 1920s David Hilbert proposed a vision that every formal system to be used im mathematics should be proven consistent within a trusted finitistic core system. However, quite soon Kurt Gödel (1930) has proven that impossible: non-finitistic systems require strictly stronger systems to prove their consistency.

The idea of Hilbert transformed into a somewhat arbitrary “second best” practice. Instead of proving formal systems (in particular an axiomatic theories) consistent, mathematicians “establish their meaningfulness” by providing a set-theoretic model within the standard set theory ZFC or an establised large cardinal extension thereof. For constructive systems the constructive set theory CZF and its extensions are used. 

# Finitistic core and standard modelling hierarchy

It took almost a century to come up with a worthy candidate for the role of a trusted finitistic core system: the theory H<sub><ω</sub> of hereditarily finite iterative sets developed by Fedor Pakhomov in 2019. This theory has a remarkable property: it proves it own consistency. At the moment it's the only known example of a natural self-justifying theory. H<sub><ω</sub> evades the curse of Gödel theorem by being a finitistic theory: it cannot prove existence of any infinite set.

Of course, it is still not capable of proving consistency of any stronger systems, but it can be used as a base for a linear hierarchy of regular extensions H(ɑ) to build models for stronger systems and thus establish their consistency relative to some H(ɑ) and measure minimal modelling capacity ɑ sufficient to provide a model. However, we want to consider constructive and even computational models for formal systems that admit such models, which requires a slightly refined form of H<sub><ω</sub>.

H<sub><ω</sub> = minimalistic base theory H₀ = {extensionality, separation, and weak hierarchy} + a countably infinite family of axioms ∃0, ∃1, ∃2,... extablishing existence of all finite ordinals. We conjecture it is possible to develop a constructive set theory CZF₀, such that theories CZF(ɑ) = {CZF₀ + ∃β for all recursive ordinals β < ɑ} has following properties:
- CZF(ω) proves its own consistency
- CZF(ω) proves the same facts about hereditarily finite sets as H<sub><ω</sub>
- CZF(ɑ) does not prove wellfoundness of ɑ, i.e. the proof-theoretic ordinal |CZF(ɑ)| = ɑ
- CZF(ɑ) have direct computational interpretations up to the second order arithmetic, i.e. if ɑ ≤ |PA₂|  
- CZF(|CZF|) = CZF, where |CZF| is the Bachman-Howard ordinal
- CZF(ɑ) has models of all CZF(β) with β < ɑ, moreover CZF(ɑ) = CZF₀ + Con(CZF(β)) for all β < ɑ
- For limit ordinals ɑ, CZF(ɑ) satisfies reflection principle: has inner models for each of its finite subtheories
- The standard set theories ZFC/CZF and their large cardinal extensions fall into the CZF(ɑ)-hierarchy

We also conjecture that it is possible to establish a quantifier-free logic-free presentation for its conservative extension CZF<sup>ω</sup>(ɑ), very similar to the logic-free presentation of primitive-recursive arithmetic PRA developed by Goodstein.

# Formalising metamathematics

Метаматематические доказательства очень tedious и никто их не проверяет. Часть, где арифметизируется синтаксис формальных систем, вообще принимается на веру. Нам необходима end-to-end формализация. В теориях типов мы можем работать не с арифметизацией синтаксиса, а с самим синтаксисом. Более того, близка к завершению концепция синтетических типов данных, охватывающая естественное описание синтаксиса сколь угодно сложных формальных систем. Недавно также было выявлено, как ограничивать теории типов так, чтобы они содержали лишь примитивно-рекурсивные функции — достаточно прямолинейные чтобы все верили, что они завершаются, и достаточно мощные, чтобы описывать трансформацию синтаксиса — нас интересует построение модели, то есть трансформацию синтаксиса произвольной формальной системы в систему CZF(ɑ), и наоборот. Нам необходимо разработать теорию типов, содержащую все финитарные синтетические типы данных и примитивно-рекурсивные функции*, создать практичный theorem-prover на её базе. Доказать что её собственный синтаксис обоюдоинтерпретируем с CZF(ω^ω), запустить community project по формализации всевозможных метаматематических результатов.

Нужна стандартная библиотека для формализации метаматематических результатов, начать которую следует с разработки logic-free calculus для CZF(ɑ), что сильно упрощает трансляцию, и обобщённую CNF-нотацию для ординалов до Бахманна-Говарда и далее докуда получится (https://akuklev.livejournal.com/1312249.html), по построению генерирующую функции, тотальность которых равносильна существованию каких ординалов.

(* Для ультраслабых теорий, |T| < ω^ω, нам нужны особо ограниченные варианты, запрещающие использовать элиминаторы в произвольном контексте, а в случае |T| = ωk + c < ω^2 следящие чтобы переменные в них повторялись не больше k раз и использовалось не больше c + n раз, где n количество конструкторов в аргументе.)
