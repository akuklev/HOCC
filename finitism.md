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
their elements' type: every predicate $φ(x)$ on the type $T$ defines a set { $x : T | φ(x)$ }, while
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
{ $n$, { $m$ } } which can be naturally extended to finite lists.

The theory of finitary pure sets (where no infinite set can be shown to exist) was first
developed very recently [[Pakhomov2019]](https://arxiv.org/abs/1907.00877). It is called $H_{<ω}$
and it has quite remarkable foundational properties. It is an excemption to the infamous Gödel's
second incompleteness theorem theorem that states that no strong enough mathematical theory can
prove its own consistency. Pakhomov's set theory $H_{<ω}$ is weak enough to evade Gödel's theorem
and does indeed prove its own consistency. Up to date, it is the only natural example of such a
theory.

Yet, finitary pure sets are not enough to model real numbers or functions on natural numbers.
How does one define infinite pure sets? The original Cantor's idea was to try the same
approach that works perfectly well for sets of integers, sets of reals and other “sets of $T$”
within a typed framework, namely to identify sets and predicates on $T$. But pure sets are
sets of pure sets, so the definition becomes recursive and leads to inconsistency of the theory.

For a period of time (1884–1899) the mathematical community was not aware of this inconsistency,
and learned how inconceivably effective it is for modelling all sorts of mathematical objects.
There were “enough” pure sets to model every mathematical object one could think out. In a sense,
there were too much of them: the universe of sets was too huge to be charted in any useful way.
Cantor himself compared it to an abyss way beyond comprehension.

After inconsistencies (cf. [Russell's paradox](https://en.wikipedia.org/wiki/Russell%27s_paradox))
were exposed, the community did not abandon the pure set theories as a possible unified
single-sorted (everything is of type “pure set”) foundation for the whole body of mathematics, but
found other approaches to define enough infinite pure sets without introducing inconsistency.

One of such approaches, namely the one developed mainly by Zermelo and von Neumann (ZN), leads
to a foundationally attractive class of set theories. There, one looses the ability to reflect
all predicates on pure sets by sets themselves, but gains an insight into how sets are “built
from below”. These are the ZN-type set theories, where one restricts the universe of discourse
to well-founded pure sets $S$ that are not allowed to harbour infinite membership chains

$$S ∋ S_1 ∋ S_2 ∋ ···$$

(From this point in text I will use the word “set” to mean wellfounded pure sets only.)

Wellfoundedness precludes existence of the set of all sets (as it would necessarily contain
itself, $V ∋ V ∋ V ∋ ···$, in infinite membership chain). In this theories every set still
defines a predicate (_ $∈ S$) on all sets and is completely defined by this predicate (two sets
containing the same elements are the same set), but it is not true anymore that every predicate
defines a set, since the predicate `true` would define a non-wellfounded set $V$ of all sets
However, in ZN-type set theories one can still form subsets { $x ∈ S | φ(x)$ } of sets $S$
already proven to exist by arbitrary predicates $φ(x)$.

As already mentioned, most appealing foundational feature of ZN-type set theories is that the
universe of all sets can be better understood: Sets $S$ can be ordered by their complexity
$rk(S)$, so that collection sets of complexity below $β$ forms a set $V_β$. Therefore one
has an infinite hierarchy of ever growing sets (known as the von Neumann hierarchy)
$$V_0 ⊂ V_1 ⊂ ··· ⊂ V_β ⊂ ···$$
with the property that every set eventually enters the hierarchy (namely at the stage $rk(S)$ + 1).
Therefore, the universe of all sets (which is not a set itself as it was discussed above) can be
seen as the limit of this hierachy. Furthermore, all stages of the hierarchy can be explicitly
described in terms of smaller stages.

Let us first consider its finite stages. The set $V_0$ of sets of complexity below zero should
be obviously empty:
$$ V_0 := ∅ $$

Given the set $V_β$ of sets of complexity below $β$ let
$V_{β + 1} := 𝓟(V_β)$
where $𝓟(V_β)$ denotes the power set $V_β$, i.e. set of all subsets of $V_β$ including $V_β$ itself.

![The first four stages of the von Neumann hierarchy](https://upload.wikimedia.org/wikipedia/commons/8/83/Von_Neumann_universe_4.png)

With these definitions one obtains an infinite sequence of sets satisfying
$$V_0 ⊂ V_1 ⊂ ··· $$
and
$$V_0 ∈ V_1 ∈ ··· $$

Each stage $V_β$ is a transitive set that is for every set it contains it does also contain all its elements:
$$(S ∈ V_β) ⇒ (S ⊂ V_β)$$
and closed under forming subsets of its elements by arbitrary predicates $φ(x)$:
$$∀(S ∈ V_β) ⇒ \{ $x ∈ S | φ(x)$ \} ∈ V_β$$

The sequence $(V_0, V_1, ...)$ can be continued transifinitely: one can form the union $V_ω$ of these sets
and show that it is also a transitive set closed under { $x ∈ S | φ(x)$ }, and contains all finite hierarchy
stages both as elements and subsets. $V_ω$ is the set of all finitary wellfounded pure sets, but it is not
end of the hierarhcy. One can iterate the powerset operation futher and to obtain $V_{ω + 1} := $𝓟(V_ω)$,
$V_{ω + 2} := 𝓟^2(V_ω)$. This sequence as well has a limit given by the union of all its elements.
This process can be iterated transinitely for all ordinal numbers:
$$$V_κ := ⋃_{β < κ} 𝓟(V_β)$$

Due to wellfoundness of all sets (in ZN-type set theories) one can now define the ordinal valued function
$rk(x) := ⋃_{y ∈ x}$ { $rk(y)$ } on sets that determines minimal von Neumann hierachy stage containing or
equal to $x$, proving the claim that every set eventually enters the hierarchy at some stage.

Note that being a transitive set closed under { $x ∈ S | φ(x)$ } each $V_β$ is a model of a weak set
theory where sets can only be made filtered by predicates. For each infinite ordinal $β$, $V_β$ models
the theory of finitary sets $H_{<ω}$. For each limit ordinal $β$, $V_β$ is also closed under forming
singleton sets ($x ↦ {x}$), ordered pairs ($(a, b) ↦ {{a}, {a, b}}$), and arbitrary unions, thus
modelling an even stronger set theory. A ZN-type set theory capable of expressing $V_β$ for $β < κ$
for some infinite $κ$ can be modelled (and thus shown consistent) in a ZN-type set theory capable of
expressing $V_κ$, but not the other way round as it would allow the latter theory to prove its own
consistency which violates the Gödel's theorem. Therefore the ability to exress $V_β$ up to certain
limit linearly orders ZN-type set theories by their modelling capacity.

The standard Zermelo-Fraenkel set theory ZFC is the canonical example of a ZN-type set theory, and
is being used as the foundation for the majority of mathematics since its develpment in 1925. For some
branches of mathematics such as category theory and algebraic geometry a slight extension of ZFC called
Tarski–Grothendieck (TG) set theory is used instead: there, one additionally assumes that for every set
$S$ is contained in a stage $V_κ ∋ S$ of von Neumann hierarhcy that is is closed under all possible set
operations of ZFC (this property is technically refered as $V_κ$ being an inner model of ZFC).

ZN-type set theories are the canonical substrate for modelling various mathematical objects,
and that is precisely what makes them interesting for sturcturalists who do not treat them
as _the_ foundation of all mathematics anymore. When a type theorist needs to prove consistency
of a strongly impredicative type theory (such as System F or the observational calculus
of constructions $CC_{obs}$), they resort to ZN-type theories to build models.

§ Weak set theories
-------------------

In the early days of set theory, everybody was primarily concerned about making theories general enough to
model everyting of mathematical interest. Initially, there was a hope that after such an ultimate system
is discovered, one would be able to prove its consistency using a weak system dealing only with
finitary objects by means of minimalistic list of obviously non-problematic axioms. It was a part of
a large [programm](https://en.wikipedia.org/wiki/Hilbert%27s_program) on developing secure foundations
for mathematics proposed by D. Hilbert. Soon it was proven by K. Gödel, that no powerful mathematical
theory could prove its own consistency let alone consistency of stronger systems. Around 1975,
H. Friedman founded the [Reverse Mathematics](https://en.wikipedia.org/wiki/Reverse_mathematics)
programm, that resorts to the second best option: one seeks to work in the weakest possible
system that allows to express and prove the quintessential theorems of a particular branch of
mathematics, and to gauge the strength of particular theorems and theories on a linear scale.

While some of the modern mathematics requires an extension of the standard ZFC (mostly TG), the
so called ordinary mathematics, which certainly includes all mathematics pursued until the WW2, except
the set theory inself, does not require the full power of ZFC. For ordinary mathematics,
it is sufficient to work in a subsystem of ZFC, where the existence
of von Neumann hierarchy stages beyond $V_{ω + n}$ (where $n$ is a fixed integer) cannot be shown.
An even weaker subsystem with no constructible stages beyond $V_{ω + 1}$ is sufficient to prove
theorems of algebra and combinatorics restricted to countable structures, and theorems of analysis
and topology restricted to separable spaces. It is sufficient for the entire body of real analysis.
Peano arithmetics is equivalent to a system where only $V_ω$ can be demonstrated to exist.

The theory of finitary pure sets $H_{<ω}$ mentioned above is the minimal ZN-type set theory
where only the existance of finitary stages of the von Neumann hierarchy of sets is postulated.
One can see all other classical ZN-type set theories as its extensions. Adding the axiom of
infinity (set $V_ω$ of all finitary sets exists) makes $H_{<ω} equivalent to Peano arithmetics.
Stronger ZN-type theories can be obtained by using stronger axioms of infinity, that is, postulating
existance of von Neumann hierarchy stages $V_κ$ with specified closure properties which the universe
of all sets can be thought to share as well.

When modelling a theory of interest $T$ using a set theory, instead of just using some strong enough
ZN-type set theory, one can identify the weakest set theory $H_{κ}$ or $H_{<κ}$ sufficient for such
modelling, where
$H_{κ} := H_{<ω}$ + there exists a von Neuman hierarchy stage with some specific closure propertes given by $κ$
$H_{<κ} := H_{<ω}$ + every set is an element of a von Neuman hierarchy stage with some specific closure properties  
given by $κ$. To ensure the chosen set theory is indeed the minimal one one has to construct the reverse model.

This way, one measures the minimal $κ$ required to show the consistency of $T$. It is called the consistency
strength of $T$.

§§ The theory of finitary pure sets $H_{<ω}$
--------------------------------------------

The theory of finitary pure sets by F. Pakhomov $H_{<ω}$ is stated in the language of single-sorted
first order logic with equality extended by the binary relation (∈) and the unary operator $V$.
To be specific, the language of the first order logic with equality consists of usual logical operators
(¬_), (⇒), (∧), (∨), the quantifiers ∀ and ∃, and the equality (=).
Being single sorted means that all variables have the same type (“well-founded pure set”, in this case).
Besides these basic parts of syntax, the language of $H_{<ω}$ has a binary relation $a ∈ b$ (“a is element of b”)
and a unary operator $V(x)$ “the smallest stage of von Neumann hierarchy containing or equal to $x$“.

The theory has all the axioms governing the logical elements of the language (¬_, ⇒, ∧, ∨, ∀, ∃, and =)
including double negation elimination $¬¬P ⇒ P$. In addition, it has a countably infinite list of axioms
postulating the existance of the sets $V_n$ for each fixed $n$, the axiom of extensionality specifying
equality between sets
$$(a = b) ⇔ ∀x ( x ∈ a ⇔ x ∈ b )$$
the axiom schema of separation that allows to filter any set $x$ by any predicate $φ(z)$
$$∃y ∀z (z ∈ y) ⇔ (z ∈ x ∧ φ(z)) $$
and the defining axiom for $V$
$$y ∈ V(x) ⇔ (∃z ∈ x)(y ⊆ V(z))$$

By Gödel's second incompletness theorem, if a theory $T$ is capable of modelling natural numbers so
that at least one strictly monotonly growing function on them can be proven injective, than $T$ can
not prove its own consistency. The theory $H_{<ω}$ is carefully formulated to be non-Gödelian:
it does not provide a way to show existence of infinitely growing functions unconditionally. For
example, one cannot show “for any two integers $a$ and $b$, there exists their product $a · b$”.
Yet, one can still prove all such statements conditionnaly on existence of what can be interpreted
computationally as “availability of enough memory to carry out the construction”. In particular,
one can show that “for any two integers $a$ and $b$, given enough memory, the integer $a · b$
exists”. Metatheoretically, it can be shown that any true arithmetical or combinatorial statement
can be proven in this theory, if an upper bound on the memory required to carry out the
underlying constructions can be provided.

To date, the theory $H_{<ω}$ the best candidate for the finitistic core for metamathematical
reasoning seeked by Hilbert in his program: a weak system dealing only with finitary objects by means
of minimalistic list of axioms. Due to Gödel's theorem it cannot be used to prove other theories
consistency unconditionally but it at least proves its own consistency. It can be used to prove other
theories consistency conditionaly on existence of suitable infinite stages of von Neumann hierarchy.
It is, however, unclear if it is suitable for more fine-grained conditional consistency proofs used
in reverse mathematics. There, one deals with theories with consistency strength below $H_{ω}$ and
between $H_{ω}$ and $H_{ω + 1}$. Their consistency proofs are currently carried out in a weak system
called ERA (elementary recursive arithmetic) and are conditional on termination of suitable recursive
algorithms rather than existence of suitable infinite sets.

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

The resulting theory (let us denote it by $CGB_{<ω}$) would be even more suitable for the role
of finitistic core.

When proving that two theories are equiconsistent or one theory being consistent conditionally
on certain ordinal notation being well-founded, one seeks to use the weakest system possible.
Today most metatheoretical proofs are carried out in the system known as Elementary Recursive
Arithmetic (ERA). Until recently it was the weakest known system that allows comfortable handling
of arithmetically encoded syntax. The conjectural theory $CGB_{<ω}$ should be a better replacement:
it is weaker, it only involves dealing with finitary objects and it seems to be at least equally
comfortable to work with.

ERA can be formulated not only above the language and basic rules of first order logic, but also
as a stanalone [quantifer-free and logic-free calculus](https://en.wikipedia.org/wiki/Primitive_recursive_arithmetic#Logic-free_calculus)
involving only functional symbols and judgements of the form “$P = Q$”. If one manages to state
the definitional axioms for $V$ in ∀∃-form, it will be also possible to formulate $CGB_{<ω}$ as
a standalone quantifier-free and logic-free calculus with “P ∈ Q“ and “P ⊃ Q“ being the only
types of judgements.

§ Machine-checkable metatheoretical proofs
------------------------------------------

Both ERA and conjectural $CGB_{<ω}$ are not directly suitable for carrying out metatheoretical
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
