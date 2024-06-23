Strictly-Typed Programming Languages: Where are we now?
=======================================================

First let's clarify what I mean by strictly-typed programming languages. If you're reading this, it is safe to assume, you came across a programming language like C or Java at some point of your life, a statically typed programming language where it is possible to introduce custom data types. In all of those languages are defined

macroassembler.

, static type system is a leaky abstraction piggybacked onto operational 

type signatures variables, functions and their arguments are more of a “declaration of intent” kind of thing; it is possible and sometimes unavoidable to “look under the hood” ignoring types.

“Under the hood” there is another, rudimentary type system: in case of C it's roughly “everything is an int”, in Java and other JVM-languages its several primitive types like `int`, “array” or “object” that cont

one can forcefully “cast” values and objects from 

Already in C it is possible to define finite types (`enum Direction {LEFT, RIGHT}`), record types (``)

struct S {
  char myLetter;       // Member (char variable)
};
Many people, by the way, not only software developers, sometimes have to learn to program. And sometimes they come across such programming languages as C++ or Java - strictly typed, in which you can define your own types. Then these people learn about algebraic data types, and sometimes they start googling and reading something. And they learn that it turns out that there is some powerful mathematical theory behind type systems, and there are some mega-expressive type systems where you can strictly type printf.
Then it turns out that mainstream languages can't do such things, and if they can, the abstraction leaks, and therefore it is disproportionately more troublesome than useful. So they leave to write in some Clojure or Python, because their rudimentary type system does not get in the way.
And where are normal programming languages based on type theory? There are none yet, though there have been several attempts. And no, because you can't jump over your head, as with Maslow's pyramid - you need water, food and a roof over your head first, and then aesthetic needs. That's the pyramid we have here:



Principled
