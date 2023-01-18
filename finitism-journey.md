Why set theories? Programme for $IH_{<ω}$
=========================================

[author]: mailto:a@kuklev.com "Alexander Kuklev, JetBrains Research"
[Alexander Kuklev](mailto:a@kuklev.com), [JetBrains Research](https://research.jetbrains.org/researchers/alexander.kuklev/)  

This text briefly summarizes the history of set theories, explaining why and how they were used as
foundational system for the whole of mathematics and why they still matter when modern type theory
based systems take over their role as the foundations. At the end, I propose a programme to
establish a constructive theory of finitary pure sets that comprises the common core of all
set theories used to model mathematical objects, and can be used to carry out conditional
consistency proofs.

In structuralist foundations of mathematics, there are objects of very different types:
natural numbers, various combinatorial objects (e.g. finite strings of balanced brackets like
“()((())())”), real numbers, tuples, lists, functions between any types, relations on any types,
and so on. The vast zoo of types made structuralist foundations unatractive until 1990ies when
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

The theory of finitary sets only (where no infinite set can be shown to exist) was first
developed very recently [[Pakhomov2019]](https://arxiv.org/abs/1907.00877). It called $H_{<ω}$
and it has quite remarkable foundational properties. It is an excemption to the infamous Gödel
theorem that states that no strong enough mathematical theory can prove its own consistency.
Pakhomov's set theory $H_{<ω}$ does not fulfill the requirements of Gödel theorem (is not
“strong enough”) and does indeed prove its own consistency.

In this theory, the existence of infinitely growing functions cannot be shown unconditionally. For
example, one cannot show “for any two integers $a$ and $b$, there exists their product $a · b$”,
but one can prove all such statements conditionnaly on existence of what can be interpreted
computationally as “availability of enough memory to carry out the construction”. In particular
one can show that “for any two integers $a$ and $b$, given enough memory, the integer $a · b$
exists”. Metatheoretically, it can be shown that any true arithmetical or combinatorial statement
can be proven in this theory, if an upper bound on the memory required to carry out the
underlying constructions can be provided.

Finitary pure sets are not enough to model real numbers or functions on natural numbers, but
their infinite generalizations easily manage this task. Original idea due to G. Cantor was to
identify pure sets and predicates on them as it is usually done for sets of objects of fixed
type. Unfortunatelly, the recursivity of this definition leads to inconsistency of the theory.
For a period of time (1884–1899) the mathematical community was not aware of this inconsistency,
and learned how inconceivably effective it is for modelling all sorts of mathematical objects.
Now it is known as naïve set theory or “The Cantor Paradise”. After inconsistencies, such as
[Cantor's paradox](https://en.wikipedia.org/wiki/Cantor%27s_paradox),
[Burali-Forti paradox](https://en.wikipedia.org/wiki/Burali-Forti_paradox), and
[Russell's paradox](https://en.wikipedia.org/wiki/Russell%27s_paradox) were exposed,
the community did not abandon the set theory as a possible unified single-sorted (everything is
of type “pure set”) foundation for the whole body of mathematics, but found ways to get rid of
inconsistencies by compromises.

There are several ways to restrict the approach to generalizing finitary pure set theory to
infinite sets by allowing every predicate to define a set. One of such approaches, namely the
one developed mainly by Zermelo and von Neumann (ZN), leads to a foundationally attractive
class of set theories. There, one looses the ability to reflect all predicates on sets by sets,
but gains an insight into how all sets are “constructed from below”, while still being able to
provide models for essentially all objects of mathematical interest. These are the ZN-type set
theories, where one restricts the universe of discourse to well-founded pure sets `S` that are
not allowed to harbour infinite membership chains

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

The non-Gödelian theory of finitary sets is the basic the ZN-type set theory where only the
existance of finitary stages of the von Neumann hierarchy of sets is postulated. If an axiom
of infinity postulating existance of the set of all finitary sets $V_ω$ is added, it turns 
into a theory equivalent to the ordinary first order arithmetics.
Stronger variants of set theory are obtained by substituting this axiom by some strong axiom
of infinity. Strong axioms of infinity postulate existance of stages $V_κ$ of cumulative
hierarchy with specified closure properties which the class of all sets can be thought to
share as well.

ZN-type set theories are the canonical substrate for modelling various mathematical objects,
and that is precisely what makes them interesting for sturcturalists who do not treat them
as _the_ foundation of all mathematics anymore. (вставить кусок про ZFC и TG)

§ The constructive set theory CZF
---------------------------------

In 1978, Peter Aczel defined a constructive counterpart CZF of the standard set theory ZFC,
which is of a particular interest for those interested in foundations of mathematics.
CZF is a first order intuitionsitic theory highly suitable for modelling various mathematical
objects in constructive settings. To give an example, it is used to model constructive algebraic
model strucutres in category theory and predicative constructive type theories.
Being “constructive counterpart” of ZFC means the following: In presence of double negation
elimination, is equivalent to the the full to ZF set theory. If augmented by the axiom of
choice it turns equivalent the full ZFC, the standard set theory used as foundation for
ordinary mathematics for almost a century. At the same time CZF, is a weak predicative theory
that can be modelled in a rather weak Martin-Löf type theory and amenable to proof theoretic
analysis. Since CZF lacks the powerset operation (which is substituted by a much weaker subset
collection axiom), the von Neumann hierarchy in its original form cannot be defined for CZF.
However, recently [[Zieger2014]](https://core.ac.uk/download/pdf/30267838.pdf) one managed to define
a modified von Neumann hierarchy that shares the property that all sets eventually enters
the hierarchy, and thus gives the same foundational security CZF. Modified hierarchy
conicides with the usual one in presence of double negation elimination.
Infinity axioms, both strong and weak can be stated as existance of modified von Neumann hierarchy
stages being regular enough internal models of CZF (that is, hierarchy stages having specified closure
properties which the class of all sets can be thought to share as well). 

§ Towards Constructive Theory of Finitary Pure Sets
---------------------------------------------------

The only issue with foundational status of CZF we still have is that of the
constructive variant of the self-verifying finitary segment $IH_{<ω}$.
This work has to be done and might lead to unexpected enlightning results.
We conjecture that much like primitive recrusive arithmetic PRA, the intuitionistic theory
of finitary sets $IH_{<ω}$ can be axiomatized first in quantifier-free and then in
[logic-free manner](https://en.wikipedia.org/wiki/Primitive_recursive_arithmetic#Logic-free_calculus)
with (∈) and (⊃) being the only types of judgements, class being the only type of terms
and sets $S$ being classes that belong to some other class $C ∋ S$. This theory could turn
out to be replace PRA/EFA a the base theory where the consistincy proofs for other theories
(conditionally on existence of suitably large $V_κ$) are carried out.