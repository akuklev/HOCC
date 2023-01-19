Towards Constructive Theory of Finitary Pure Sets
=================================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)  

> This text briefly summarizes the history of set theories, explaining why and how they were used as
foundational system for the whole of mathematics and why they still matter when modern type theory
based systems take over their role as foundations. At the end, I sketch a programme to establish a
constructive theory of finitary pure sets that comprises the common core of all set theories used
to model mathematical objects, and can be used to carry out conditional consistency proofs instead
of elementary recursive arithmetic ERA used for such proofs today.

In structuralist foundations of mathematics, there are objects of very different types:
natural numbers, various combinatorial objects (e.g. finite strings of balanced brackets like
“()((())())”), real numbers, tuples, lists, functions between any types, relations on any types,
and so on. The vast zoo of types made structuralist foundations unattractive until 1990ies when
the burden of type bookkeeping could be delegated to computers. Instead of dealing with a complex
type theory underlying structuralist foundations, one could take a theory $S$ of one particular
type of objects that are rich enough to model all types of intrest (natural numbers, functions,
relations, etc) and substitute all other types by their $S$-models.

That is precisely how Cantorian set theories were used as foundation for the whole mathematics
for more than a century. Let me briefly remind what Cantorian set theories are and how they
came to existence.

§ From Cantor's Naïve Set Theory to ZFC
---------------------------------------

Any kind of foundations necessarily deals with sets of natural numbers, sets of reals, and other
sets of objects of specified type. Finite sets are just finite lists ignoring order and multiplicity.
That is one takes finite lists, defines the predicate (_ $∈ l$) «_ is an element of $l$», and defines
finite sets as lists modulo equality relation $(l_1 = l_2) := ∀(x) (x ∈ l_1) ⇔ (x ∈ l_2)$.
How could infinite sets be defined? The conventional approach is to identify them to predicates on
their elements' type: every predicate $P(x)$ on the type $T$ defines a set { $x : T | P(x)$ }, while
every set $S$ defines a precidate (_ $∈ S$) on the type $T$, and sets with same elements are equal
( $(a = b) := ∀(x) (x ∈ a) ⇔ (x ∈ b)$, which is known as extensionality).
Thus, sets just reflect predicates.

Already in the 18th centory, it was recognized that one can also study pure sets, i.e sets
consistung of sets, consisting of sets, and so on. It might appear that this recursion has
to lead ultimately to some fixed non-set type, but that is a false impression because there is
at least one object that surely exists and qualifies as a pure set: the empty set.

Finitary pure sets are simple combinatorial objects built by wrapping the empty set in various
ways. For example one can consider the set containing the empty set as element {∅} and the
set containing this set {{∅}} or the set containing both of them {{∅}, {{∅}}}. Finitary
sets can be seen as unlabeled trees of finite width and depth, where the only leaves are
the empty sets and order and multiplicity of branches is ignored. Equivalently, they can be
seen as finite strings of balanced brackets modulo reordering and duplication of balanded
substrings.

Finitary pure sets turn out to be convinient substrate to model any other finitary objects
including natural numbers, finite sets of natural numbers, finite lists of natural numbers,
rational numbers, graphs, finite groups, finite geometries, etc. For example, natural numbers
can be modelled by sets containing models of all smaller natural numbers. Thus, zero is modelled
as the empty set ∅, the number one by the set {∅}, the number two by {∅, {∅}}, etc. Given two
natural numbers modelled by sets $n$ and $m$, one can model their ordered pair by the set
{$n$, {$m$}} which can be naturally extended to finite lists.

The theory of finitary pure sets (where no infinite set can be shown to exist) was first
developed very recently [[Pakhomov2019]](https://arxiv.org/abs/1907.00877). It called $H_{<ω}$
and it has quite remarkable foundational properties. It is an excemption to the infamous Gödel's
second incompleteness theorem theorem that states that no strong enough mathematical theory can
prove its own consistency. Pakhomov's set theory $H_{<ω}$ is weak enough to evade Gödel's theorem
and does indeed prove its own consistency. Up to date it is the only natural example of such a
theory.

Yet, finitary pure sets are not enough to model real numbers or functions on natural numbers.
How does one define infinite pure sets? The original idea due to G. Cantor was to try the same
approach that works perfectly well for sets of integers, sets of reals and other “sets of $T$”
within a typed framework, namely to identify sets and predicates on $T$. But pure sets are
sets of pure sets, so the definition becomes recursive and leads to inconsistency of the theory.

For a period of time (1884–1899) the mathematical community was not aware of this inconsistency,
and learned how inconceivably effective it is for modelling all sorts of mathematical objects.
There were “enough” pure sets to model every mathematical object one could think out. In a sense
there were too much of them: the universe of sets was too huge to be charted in any useful way.
Cantor himself compared it to an abyss way beyond comprehension.

After inconsistencies (cf. [Russell's paradox](https://en.wikipedia.org/wiki/Russell%27s_paradox))
were exposed, the community did not abandon the pure set theories as a possible unified
single-sorted (everything is of type “pure set”) foundation for the whole body of mathematics, but
found other approaches to define enough infinite pure sets without introducing inconsistency.

One of such approaches, namely the one developed mainly by Zermelo and von Neumann (ZN), leads
to a foundationally attractive class of set theories. There, one looses the ability to reflect
all predicates on pure sets by sets themselves, but gains an insight into how sets are “built
from below” These are the ZN-type set theories, where one restricts the universe of discourse
to well-founded pure sets `S` that are not allowed to harbour infinite membership chains

$$S ∋ S_1 ∋ S_2 ∋ ···$$

Wellfoundedness precludes existence of the set of all sets (as it would necessarily contain
itself, $V ∋ V ∋ V ∋ ···$, in infinite membership chain). In this theories every set still
defines a predicate (_ $∈ S$) on all sets and is completely defined by this predicate (two sets
containing the same elements are the same set), but it is not true anymore that every predicate
defines a set, since the predicate `true` would define a non-wellfounded set $V$ of all sets
However, in ZN-type set theories one can still filter sets already proven to exist by arbitrary
predicates.

As already mentioned, most appealing foundational feature of ZN-type set theories is that the
universe of all sets does not seem to be an uncharted abyss anymore; instead it is a limit of
a cummulative hierarchy of sets that are iteratively “made” from the sets lying below in the
hierarchy. The von Neumann hierarchy $V_κ$ is a cummulative hierarchy of transitive sets indexed
by ordinals (transfinite generalization of natural numbers)
$$V_0 ∈ V_1 ∈ ··· ∈ V_ω ∈ ···$$
with the property that every set eventually enters the hierarchy at some stage. This hierarchy is
obtained by iterating the powerset operation $V_κ := 𝓟^κ(∅)$ finitely and transfinitely, that is
for zero $V_0 := ∅$, for successor ordinals $V_{β + 1} := 𝓟(V_β)$, and for limit ordinals
$V_κ = ⋃_{β < κ} V_β$.

The standard Zermelo-Fraenkel set theory ZFC is the canonical example of a ZN-type set theory, and
is being as the foundation for the majority of mathematics since its develpment in 1925. For some
branches of mathematics such as category theory and algebraic geometry its slight extension called
Tarski–Grothendieck set theory: there one additionally assumes that for every set $S$ there is a
stage $V_κ ∋ S$, such that $V_κ$ is closed under all possible set operations of ZFC (i.e. $V_κ$
is an inner model of ZFC).

ZN-type set theories are the canonical substrate for modelling various mathematical objects,
and that is precisely what makes them interesting for sturcturalists who do not treat them
as _the_ foundation of all mathematics anymore. When a type theorist needs to prove consistency
of an impredicative type theory (such as System F or the observational calculus
of constructions $CC_{obs}$, they resort to ZN-type theories to build models.

§ Weak set theories
-------------------

In early days of set theory everybody was primarily concerned about making theories general enough to
model everyting of mathematical interest. Initially there was a hope that after such ultimate system
would be discovered, one would be able to prove its consistency using a weak system dealing only with
finitary objects by means of minimalistic list of obviously non-problematic axioms. It was a part of
a large [programm](https://en.wikipedia.org/wiki/Hilbert%27s_program) on developing secure foundations
for mathematics proposed by D. Hilbert. Soon it was proven by K. Gödel, that no powerful mathematical
theory could prove its own consistency let alone consistency of stronger systems. Around 1975
H. Friedman founded the [Reverse Mathematics](https://en.wikipedia.org/wiki/Reverse_mathematics)
programm, that resorts to the second best option: one seeks to work in the weakest possible
system that allows to express and prove the quintessential theorems of a particular branch of
mathematics, and to gauge the strength of particular theorems and theories on a linear scale.

While some of the modern mathematics requires an extension of the standard ZFC (mostly TG), the
so called ordinary mathematics (which certainly includes all mathematics pursued until WW2) does
not require the full power of ZFC; it is sufficient to work in a subsystem of ZFC, where existence
of von Neumann hierarchy stages beyond $V_{ω + n}$ (where $n$ is a fixed integer) cannot be shown.
An even weaker subsystem with no constuctible stages beyond $V_{ω + 1}$ is sufficient to prove
theorems of algebra and combinatorics restricted to countable structures, and theorems of analysis
and topology are restricted to separable spaces, it is in particular sufficient for the whole
real analysis. Peano arithmetics is equivalent to a system where only $V_ω$ can be demonstrated
to exist.

The theory of finitary pure sets $H_{<ω}$ mentioned above is the basic the ZN-type set theory
where only the existance of finitary stages of the von Neumann hierarchy of sets is postulated.
One can see all other ZV-type set theories as its extensions. If one additionally postulates
existance of the set of all finitary sets $V_ω$ (this is known as axiom of infinity), $H_{<ω}$
turns into a theory equivalent to the Peano arithmetics. Stronger ZV-theories can be obtained by
substituting this axiom by some strong axiom of infinity, that is an axiom postulating existance
of stages $V_κ$ of von Neumann hierarchy with specified closure properties which the class of all
sets can be thought to share as well.

Instead of simply modelling a theory of interest `T` in some strong enough ZN-type set theory, one
can measure its consistency strength exactly by modelling it the theory “$H_{<ω} + ∃V_κ$ with
specific closure properties” or “$H_{<ω} + (∀S)(∃V_κ ∋ S)$ with specific closure properties”
and than modelling the used set theory inside of `T` and thus proving their equiconsistency.

§§ The theory of finitary pure sets $H_{<ω}$
--------------------------------------------

The first-order theory of finitary pure sets by F. Pakhomov $H_{<ω}$ stated in the language of
single-sorted first order logic with equality extended by the relation (∈) and unary operator V with
intended meaning «$V(x)$ is the smallest stage of von Neumann hierarchy being superset of $x$».
To be specific, the language it has the usual logical operators (¬_), (⇒), (∧), (∨), as well as
quantifiers ∀ and ∃ binding variables of the single type (being “wellfounded pure sets” in this
case) as well as equality (=) and the theory has all axioms governing these elements of the
language including double negation elimination $¬¬P ⇒ P$. In addition it has a countably infinite
list of axioms postulating existance of the sets $V_n$ for each fixed $n$, the axiom of
extensionality specifying equality between sets
$$(a = b) ⇔ ∀x ( x ∈ a ⇔ x ∈ b )$$
the axiom schema of separation that allows to filter any set `x` by any predicate `φ(z)`
$$∃y ∀z (z ∈ y) ⇔ (z ∈ x ∧ φ(z)) $$
and the defining axiom for $V$
$$y ∈ V(x) ⇔ (∃z ∈ x)(y ⊆ V(z))$$

By Gödel's second incompletness theorem, if a theory $T$ is capable of modelling natural numbers so
that at least one strictly monotonly growing function on them can be proven injective, than $T$ can
not prove its own consistency. The theory $H_{<ω}$ is carefully formulated to be non-Gödelian:
it does not provide a way to show existence of infinitely growing functions unconditionally. For
example, one cannot show “for any two integers $a$ and $b$, there exists their product $a · b$”.
Yet one still can prove all such statements conditionnaly on existence of what can be interpreted
computationally as “availability of enough memory to carry out the construction”. In particular
one can show that “for any two integers $a$ and $b$, given enough memory, the integer $a · b$
exists”. Metatheoretically, it can be shown that any true arithmetical or combinatorial statement
can be proven in this theory, if an upper bound on the memory required to carry out the
underlying constructions can be provided.

To date, the theory $H_{<ω}$ the best candidate for the finitistic trusted core theory seeked by
Hilbert in his program: a weak system dealing only with finitary objects by means of minimalistic
list of axioms. It cannot be used to prove other theories consistent unconditionally but it at
least recognizes its own consistency. It can be used to prove other theories consistent if extended
by suitable infinity axioms, but not fine-grained enough for gauging consistency strength of weak
predicative theories. Already the weakest infinity axiom $∃V_ω$ makes it as strong as Peano
arithmetic, which is way stronger than two of the basic systems used in reverse mathematics, the
next strongest infinity axiom $∃V_(ω + 1)$ makes it stronger than all predicative systems studied
in reverse mathematics and proof theory.

§§ Weak set theories: The constructive set theory CZF
-----------------------------------------------------

Other weak set theory extensively used for modelling mathematical objects is the constructive
set theory CZF. It was introduced in 1978 by P. Aczel as the constructive counterpart CZF of
the standard set theory ZFC. It is formulated in the language of single-sorted first order
theories with equality extended by the relation (∈), but it is an intuitionsitic first order
theory: axioms governing logical operators to not include double negation elimination (DNE)
$¬¬P ⇒ P$. CZF is a weak predicative theory that can be modelled in a rather weak Martin-Löf
type theory and amenable to proof theoretic analysis, but if one extends it by an axiom
enabling DNE, its power skyrockets; CZF + DNE = ZF, CZF + Axiom of Choice = ZFC.

CZF designed to be perfectly suitable for modelling mathematical objects in the constructive
setting. To give an example, it is used to model constructive algebraic model strucutres in
category theory and predicative constructive type theories.

Since CZF lacks the powerset operation (which is substituted by a much weaker subset collection
axiom), the von Neumann hierarchy in its original form cannot be defined for CZF. Recently
[[Zieger2014]](https://core.ac.uk/download/pdf/30267838.pdf) one managed to define a modified
von Neumann hierarchy that shares the property that all sets eventually enters the hierarchy,
and thus gives the same foundational security CZF. Modified hierarchy conicides with the usual one
in presence of double negation elimination. With this result one can consider CZF to be ZN-type
set theory. Much like in the case of $H_{<ω}$, the strength of CZF itself is not always sufficient
for modelling all objects of interest, but the strengh can be enhanced by postulating existence of
modified von Neumann hierarchy stages having specified closure properties which the class of all
sets can be thought to share as well.

§ Towards Constructive Theory of Finitary Pure Sets
---------------------------------------------------

All true propositions $H_{<ω}$ of about finitary sets are provable in CZF as well, but $H_{<ω}$
is not a subtheory of CZF because it is defined over first-order logic with double negation
elimination, which is not present in CZF. Adapting $H_{<ω}$ to constructive setting is a nontrivial
and intriguing task. The axiom of extensionality and axioms postulating existence of all individual
finite level stages of von Neumann hierarchy can be taken over literally. The axiom schema of
separation can be substituted by [predicative separation](https://en.wikipedia.org/wiki/Axiom_schema_of_predicative_separation)
(which is known to be implementable by a finite number of axions instead of a schema). The tricky
part is the definitional axiom for $V$. It is far from obvious how to restate it to obtain all the
desired consequences without DLE. It is surely even less clear how to adapt it to represent the
modified von Neumann hierarchy so that the theory does not get trivialized when extended by strong
infinity axioms.

Following the approach of [NBG](https://en.wikipedia.org/wiki/Von_Neumann%E2%80%93Bernays%E2%80%93G%C3%B6del_set_theory),
such theory can be conservatively (i.e. without introducing new theorems or increasing strength)
extended to include not only sets but also classes of sets. The resulting theory remains
single-sorted, with every variable being of type “class of sets”, and sets being recovered as
classes $S$ that belong to at least one other class $C ∋ S$. Such modification allows to handle
all objects of interest without resorting to second-order extensions used in the original
paper by F. Pakhomov.

The resulting theory (let us denote it by $CNG_{<ω}$) would be even more suitable for the role
of trusted finitistic core.

When proving that two theories are equiconsistent or one theory being consistent conditionally
on certain ordinal notation being well-founded, one seeks to use the weakest system possible.
Today most metatheoretical proofs are carried out in the system known as Elementary Recursive
Arithmetic (ERA). Until recently it was the weakest known system that allows comfortable handling
of arithmetically encoded syntax. The conjectural theory $CNG_{<ω}$ should be a better replacement:
it is weaker, it only involves dealing with finitary objects and it seems to be at least equally
comfortable to work with.

ERA can be formulated not only above the language and basic rules of first order logic, but also
as a stanalone [quantifer-free and logic-free calculus](https://en.wikipedia.org/wiki/Primitive_recursive_arithmetic#Logic-free_calculus)
involving only functional symbols and judgements of the form “$P = Q$”. If one manages to state
the definitional axioms for $V$ in ∀∃-form, it will be also possible to formulate $СNG_{<ω}$ as
a standalone quantifier-free and logic-free calculus with “P ∈ Q“ and “P ⊃ Q“ being the only
types of judgements.

§ Machine-checkable metatheoretical proofs
------------------------------------------

Both ERA and conjectural $CNG_{<ω}$ are not directly suitable for carrying out metatheoretical
proofs because the syntax of the respective theories is only accessible via arithmetical/set
theoretical encoding, rules of which external to the theories. In sturcturalist foundational
systems known construction calculi, languages generated by syntactic rules are accessible directly
as purely inductive types (indexed purely inductive types in case of languages with variables, in
particular languages of first-order theories). Thus the next task after defining $CNG_{<ω}$ will
be to provide faithful and transparent encodings for complex purely inductive types and operations
on them.

Consistency proofs of predicative (and generalized predicative) theories are conditional on
well-foundness of certain ordinal notations. These notations are also available only in form
of their arithmetical encoding, but can be defined directly in suitable construction calculi,
which also begs for unified tranlation rules.
