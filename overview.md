Introduction to Higher Observational Construction Calculus
==========================================================

§ Brief History
---------------

The endavour of formalizing mathematical proofs on a computer is by no means new. The idea was
already envisioned in [[Leibniz1684]]. The first successful attempt to develop a formalised language
and a proof verifier on a computer was the Automath project by N. de Bruijn in 1967, while in 1973
A. Trybulec developed the Mizar system, a proof assistant still in use today. The Mizar Mathematical
Library was the largest coherent body of strictly formalised mathematics in existence as of 2009 [[Wiedijk2009]].

The evolution of proof-assisting languages largely mirrors the evolution of programming languages.
The Mizar system is based on standard set theory ZFC, but set theory is a very "low level" language
where any non-trivial statement, let alone proof, is essentially unreadable due to the exorbitant
length and a flood of "implementation details" that have nothing to do with the mathematical ideas
being conveyed. To address this problem, Mizar System developed an advanced layer of macros. As it
always the case for advanced meta-programming languages, a type system had to be introduced to
regulate composability and enforce abstraction. Eventually, high-level semantics for the macro
language emerged, allowing to “debug” the high-level code instead of the low-level assembly code
it elaborates into.

Languages of modern proof assistants are cross-platform high-level languages that can be certainly
compiled into the language of ZFC, but support other “target architectures”, known as topoi.

An elementary topos is a category endowed with just enough structure to faithfully model both
first-order theries (such as graphs, groups, rings, fields, vector spaces, partially ordered
sets, etc) and higher-order theories (such as topological spaces). Elementary topoi do not
necessarily have canonical mathematical objects such as natural or real numbers. Topoi
containing canonical natural numbers arithmetic topoi (or W-topoi). They admit construction
of all countable combinatorial objects, countable groups, countable fields etc. If they
additionally admit Cauchy-completions, they are called analytic topoi. Such topoi contain
canonical real numbers and admit constructions of all separable topological spaces).

The category of all ZFC sets and functions between them is a prime example of an analytic topos,
but not the minimal one. The race to develop an elegant and effective formalized languages for
all arithmetic topoi and all analytic topoi respectively took three decades and entered its
endgame in 2013 with the development of Homotopy Type Theory. Right now it seems to be in its
final phase.

§ Formalised Languages
----------------------

A formalised language is an artificial language for which there is a precise formal definition of
syntax [[EM43455]]. Expressions of a formalised language are formal combinations of some initial
symbols, formed by definite rules of formation of expressions in the given language. In contrast
to the theory of formal langauges, theory of formalised languages does not treat expressions as
linear strings of symbols taken from a fixed alphabet; expressions of formalised langauges are
taken to be abstract syntax trees, where every node is an application of some formation rule of
the respective language.

Formalised languages are in general multi-sorted. Expresions of the language belong to particular
sorts, and formation rules require the constituents to be of certain sort. For example:
```
 a : Attribute   n : Noun-Phrase
—————————————————————————————————
        a n : Noun-Phrase
```

While some formalised languages only have a finite number of sorts, complex formalised languages
have a whole infinite algebra of types, where types can be composed by such operations as cartesian
product or disjoint sum. In this case it becomes non-trivial to see if a particular expression has
a required type to be used as a constituent for some particular formation rule. Algebra of types
has to have a decidable relation `X ≼ Y`, called “conversion”, that specifies when an expression
of type `X` is allowed to be used where type `Y` is required. As it turns out, it is far from
being trivial to devise a formalized general-purpose typed language for mathematical proofs
and constructions with decidable conversion.

Formalised languages dealing with typed free variables are known as type theories. Formulae of
multi-sorted first-order theories involve typed quantifiers (∃ and ∀), thus their formalised
languages are type theories. The branch of mathematics studying syntax and semantics of type
theories is somewhat inconveniently also known as type theory.

§ Construction Calculi
----------------------

The term _Construction Calculi_ employed in the title of this paper refers to type theories that
are intended to serve as general purpose structuralist foundations of mathematics. At times we
will also mention type theories not striving to serve as general purpose foundational frameworks,
these will be referred to as _domain-specific type theories_.

Construction calculi must be able to express
* typed higher-order logic including proofs,
* formal axiomatic theories (such as Eucledian geometry) together with formal constructions on them,
* canonical mathematical objects such as natural and (analytic) real numers, lists, sequences,
functions, relations, etc. Such canonical objects are introduced there as inductive types.

In type theory, _inductive types_ are the types freely generated by a set of possibly recursive _generators_:
```
#Inductive Nat
0    : Nat
(_') : Nat → Nat
```

Here, the type `Nat` is introduced as the type freely generated by one non-recursive generator `0` satisfying
type `Nat` and a recursive generator `(_')` (writen as a postifx apostrophe) satisfying type `Nat → Nat`.
With this definition, the set of possible Nat-values is given by
&nbsp; `0, 0', 0'', 0''', ...`
The symbols `0` and `(_')` do not refer to some predefined entities, they are introduced right here as generators
of the type `Nat`. They represent nothing other than themselves and are irreducible. There is nothing else happening
“behind the schenes” - this definition of natural numers is entirely self sufficient.


[Leibniz1684]: Leibniz, G. W. (1684), “Meditationes de cognitione, veritate et ideis”, Acta Eruditorum

[Wiedijk2009]: Wiedijk, Freek (2009). "Formalizing Arrow's theorem". Sādhanā. 34 (1): 193–220. doi:10.1007/s12046-009-0005-1. https://hdl.handle.net/2066%2F75428

[EM43455]: V.N. Grishin, “Formalized language” Encyclopedia of Mathematics. URL: http://encyclopediaofmath.org/index.php?title=Formalized_language&oldid=43455