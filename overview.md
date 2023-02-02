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

Construction calculi generally feature three classes of types: inductive types, behavioral
(co-inductive) types, and universes.


§ Inductive types
-----------------

In type theory, _inductive types_ are the types freely generated by a set of possibly recursive _generators_:
```
#Inductive Nat
  0    : Nat
  (_') : Nat → Nat
```

Here, the type `Nat` is introduced as the type freely generated by one non-recursive generator `0` of
type `Nat` and a recursive generator `(_')` (writen as a postifx apostrophe) of type `Nat → Nat`.
With this definition, the set of possible Nat-values is given by
&nbsp; `0, 0', 0'', 0''', ...`
The symbols `0` and `(_')` do not refer to some predefined entities, they are introduced right here as generators
of the type `Nat`. They represent nothing other than themselves and are irreducible. There is nothing else happening
“behind the schenes” - this definition of natural numers is entirely self sufficient.

Inductive types may be _polymorphic_, i.e. have typal _parameters_ (enclosed in square brackets):
```
#Inductive List[T̲]
  Empty : List[T]
  (::)  : T → (List[T] → List[T])
```

This definition introduces the polymorphic type `List[T]` parametrized by the type `T` of its elements.
The underline under `T̲`, in the first line is the so-called _freshness marker_, it is there to express
that `T` is not a constant that was defined somewhere else, but a variable being introduced at this
very spot. The second line introduces a rather boring generator `Empty` of `List[T]`, much like 0 in
the definition of `Nat`. The third line introduces an infix operator `::`, which is a family of
recursive generators:
For each inhabitant `head : T` it defines a recursive generator
&nbsp; `(head ::) : List[T] → List[T]`
that appends `head` to the right hand side argument `tail : List[T]` so that non-empty lists can be
generated by succesively appending elements to the `Empty` list. Note, that the arrow operator is
right-associating, so `T → (List[T] → List[T])` can be written simply as `T → List[T] → List[T]`.
The `Empty` together with the infix operator `::` defines the following set of possible values of `List[T]`:
&nbsp; `Empty, x :: Empty, x :: y :: Empty, x :: y :: z :: Empty, ...`
for `x, y, z, ...` being of type `T`.

There is also a notion of _inductive type family_ indexed over an other inductive type. For example,
consider the type family of vectors of fixed length:
```
#Inductive Vec[T̲] : Nat → *
  Empty : Vec[T](0)
  (::)  : T → Vec[T](n̲) → Vec[T](n')
```

Here `Vec[T̲]` is not one type, but a whole _family of types_, one for each natural number `n`.
The “type” `Nat → *` of `Vec[T]` means “a function mapping natural numbers to types“ modulo issues
stemming from the fact that the class of all types `*` is not really a type itself and requires
somewhat careful handling to avoid running into the [barber issues](https://en.wikipedia.org/wiki/Barber_paradox).
The generator `Empty` satisfies the type `Vec[T](0)`, vector of `T`s of zero length.
Generators `(head ::)` take a vector `tail : Vec[T](n)` of _some_ length `n` and generate
a vector of length `n' = n + 1`.

§§ Behavioural types
--------------------

Behavioural types are described not in terms of generators, but in terms of _extractors_. For behavioral types
one remains completely agnostic about nature of their values as long as they provide a given “interface“.
Consider the type of possibly infinite streams:
```
#Structure Stream[T̲]
  head : T
  tail : Maybe[ Stream[T] ]

where
#Inductive Maybe[T̲]
  Nothing      : Maybe[T]
  Value(t̲ : T) : Maybe[T]
```

Here the inhabitants of the type `Stream[T]` are defined as values with distinguished extractors `head`, yielding
a value of the type `T`, and `tail` yielding either the rest of the stream or the value `Nothing` if the stream
ends there.

Values of behavioural types can be defined by specifying actions of their extractors (recursion is allowed). To give
an example, let us define the Fibonacci sequence. Fibonacci sequence is a sequence, in which each number is the sum
of the two preceding ones. A Fibonacci sequence can be defined for any two initial elements, while _the_ Fibonacci
sequence is a Fibonaccy sequence starting with 0 and 1:
```
(0,) 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, ...
```

Here is how to define them:
```
#Defintion fibonacci-sequence(c̲u̲r̲r̲e̲n̲t̲ p̲r̲e̲v̲i̲o̲u̲s̲ : Nat) : Stream[Nat]
  h̲e̲a̲d̲ ↦ current
  t̲a̲i̲l̲ ↦ fibonacci-sequence((current + previous), current)

#Default the-fibonacci-sequence
  fibonacci-sequence(1, 0)
```

The `fibonacci-sequence(n, m)` yields `n` when asked for a `head` and `fibonacci-sequence(n + m, n)` when asked for
a tail.

In contrast to values of purely inductive types (which are exhausted by ones that can be explicitly written down),
a value `s : Stream[Nat]` might be a thing “living outside of the computer” being “measured” by `head` and
`tail`: it can be shown constructively that the type `Stream[Nat]` is not exhausted by values that can be written
down inside the theory: the type of `Nat`-streams is uncountably infinite. Note that equality of two streams
`s g : Stream[Nat]` cannot be in general verified in finite time. Verifying such equality would in general require
checking equality for an infinite number of terms.

§§ Functions
------------

Functions `f : X → Y` can be seen as a trivial case of behavioral types: behavioral types with a family
of non-recursive extractors parametrized by values `x : X`:

```
#Structure Function[X̲, Y̲]
  apply : X → Y
```

While it is customary in computer science to use the word “function” for partial functions (that might sometimes
fail to yield a result), in mathematics and type theories by “functions” one means total and deterministic
functions.

That's how one defines a function on `Nat`:

```
#Define double : Nat → Nat
  0 ↦ 0
  (n̲)' ↦ ( double(n) )''
```

Functions on inductive types can be defined by exhaustive pattern matching on their generators. In case of recursive
generators, definitions are allowed to be inductive. In the above example one performs an exhaustive pattern matching
on natural numbers: natural number is either a zero `0`, or a successor `n'` of a natural number `n`. Thus one
defines the value of the function `double` case by case first for zero `0` and then for successor `n'`, in the latter
case the value `double(n)` is already “known”. The ability to define functions (terminating, total functions) on
inductive types by pattern matching on their generators reflects the defining property of inductive types: their
values are guaranteed to be finitary compositions of their generators.

[Leibniz1684]: Leibniz, G. W. (1684), “Meditationes de cognitione, veritate et ideis”, Acta Eruditorum

[Wiedijk2009]: Wiedijk, Freek (2009). “Formalizing Arrow's theorem”. Sādhanā. 34 (1): 193–220. doi:10.1007/s12046-009-0005-1. https://hdl.handle.net/2066%2F75428

[EM43455]: V.N. Grishin, “Formalized language” Encyclopedia of Mathematics. URL: http://encyclopediaofmath.org/index.php?title=Formalized_language&oldid=43455