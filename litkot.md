We have a dream to make Kotlin a programming language suitable for every purpose in any context. In this memo, I explore what can be done to design a Kotlin flavour suitable and appealing to replace various ad hoc flavours of pseudocode used in academic and educational contexts.

## Significant indentation and the block-structure-first approach

Following Python, Scala 3 and Markdown, we propose significant indentation for multi-line blocks of code. Since the indentation-based block structure sticks out above everything else, we want to go a step further: unlike in Python, block structure takes precedence over comments, quoted literals and brackets. **Such an approach massively improves the performance of incremental parsing: code blocks can be delineated extremely quickly without prior parsing and parsed independently.** 

We propose to fix block indentation to two whitespaces once and for all, while treating any other number of indenting whitespaces (1 or >2) as a continuation of the previous line:

```Kotlin
fun example(files : List<File>,
            target : File)
  ...
  return something + somethingElse + somethingOther +
   yetSomething + rest
```

IDEs should provide visual reading aid in case of consequent dedents by displaying end marks (`■`). At the end of large indentation regions, IDEs should display labeled end marks (e.g. `■ main`).

```Kotlin
fun main(args : List<String>)
  for (arg in args)
    println(arg)
  ■
```

## Bracket-free text literals

In Kotlin, trailing code block arguments enjoy special treatment: `a.map({ println(it) })` can be written as `a.map { println(it) }`. We think trailing string arguments deserve special treatment too. Bracket-free text literals are opened by `~ `and closed by the next line or dedent:

```Kotlin
fun greet(name : String)
  println~ Hello, $name!
```

Those would also work nicely with key-value lists:

```Kotlin
address: Address
  house:~
    Olaf Taanensen 
    Tordenskiolds 24
  city:~ Oslo
```

## Literate Programming

In 1984, Donald Knuth introduced literate programming, a practice of working not just on the source code but on a well-written and well-structured expository paper from which the source code can be extracted. The ultimate result should be the expository paper, which carefully walks through all the nooks and crannies of the source code, explaining the ideas and documenting the reasoning behind certain decisions. It is both an essay interspersed with code snippets and a source code interleaved by accompanying text: code and text are equally important.

Programming languages treat accompanying text (“comments”) as second-class citizens, bashfully fenced by freakish combinations of special characters like `/* … */`. There is a way to do better!

#### Treating code and text equally

Our proposal from the first section implies mandatory indentation for all non-inline blocks. Thus, all remaining unindented lines are top-level definitions (`class …`, `object …`, …) or directives (`package …`, `import …`). These necessarily begin with an annotation or a keyword. Annotations readily begin with an `@`, and it won't be too much pain to prepend `@` to top-level keywords: `@import` already looks familiar from CSS, `@data class` and `@sealed class` make perfect sense anyway: most modifier keywords are nothing but inbuilt annotations.

This way every coding line either starts with an `@`, or is an indented line following a coding line (with possibly one or more blank lines in between). Let us require the compiler to skim all the lines that do not meet this specification. These other lines now can be used for accompanying text written as is without fencing. We suggest using LaTeX hybrid-mode Markdown (`\usepackage[hybrid]{markdown}`) as it has excellent readability while providing access to all features used for writing technical papers.

Freely interleaving the code and accompanying text, without fencing either, is the perfect fit for literate programming (hence the title). The very same file can either be fed into a Kotlin compiler to produce a binary or into a Markdown/TeX processor to produce a paper.

## Interactive Literate Programming

It was far from obvious in 1984, but the expository paper can (and should) contain runnable code samples to illustrate usages of the code being explained and test cases for each non-trivial function. For that purpose let's introduce `@run`-blocks that should be rendered as in internal REPL in the IDE so the user can edit and rerun them to play with the context of the source defined so far.

```Kotlin
@run sampleFunction(1, 3)

@run 1 + 2 + 3
@expect 6

@run `Named sample`:
  val a = 1 + 2
  a + 3

@run(collapsed: true, autoexec: false)
  someLenghtyComputation()
```

The IDE should properly display rich output of `@run`-blocks: visual, animated, maybe interactive. Wait, have we reinvented Kotlin-notebooks? 

## Compliance with mathematical notation

To be compliant with standard mathematical notation, we should display the multiplication operator as `·`, comparison operators as `≤`, `≥`, `=`, `≠`, logical operators as `¬`, `∧`, `∨`, and use `↦` in lambda-expressions, e.g. `{x ↦ x + 1}` . 

As it is customary in mathematics to have whitespaces around relation symbols and symbolic symmetric binary operators such as `+` (except for `a·b`, `a..b`, and `a..<b`), we require that for all such operators including the typing relation as in `n : Int`. We want to allow `//` and `#` both as infix operators and as end-of-line comment markers. It is indeed possible if we require end-of-line comments to be separated from the preceding text by at least two whitespaces or to start directly at the indentation level of the previous line.

The assignment operator should be then displayed as `≔` when introducing a fresh name
(e.g. `val a ≔ 5`) and for default argument values, or by an immediate colon `key: value` for named arguments and other “key-value” cases.

In mathematics and functional programming, it's fairly common to use the right pointing black triangle for inverse application, i.e. `x ▸ f ≔ f(x)`. Thus we propose to display `x.let f` as `x ▸ f` and `x?.let f` as `x?▸ f`.

In Kotlin, the method invocation `method(args)` is a complex syntactic entity, supporting optional arguments, named arguments, and variable number of tail arguments, as well as special handling for the last argument if it is of the function type. As we proposed in the second section, special handling could be also introduced for strings and string templates (values of the type `AdditionalContext.()-> String`). Further, one could introduce positional-only and keyword-only arguments and \*\*kwargs as in Python. In method invocations, parentheses can be omitted in some cases (while invocation is implied!), so methods as entities cannot be referred to by their name, the notation `::method` (`class::method` in fully qualified case) is used instead. Application of functions (values of the type `(args)-> R`) mimics method invocation, yet with several intransparent limitations: parentheses are mandatory, sometimes manual `.invoke()` has to be used.

Mimicking method invocation does not comply with the usual mathematical practice, where it is customary to write `sin x` instead of `sin(x)` and `f a b` for `( f(a) )(b)`. We propose to use `import FunctionalNotation` to introduce a new type `X -> Y` (without parens around `X`) to introduce functions like `sin` that can be used as customary in mathematics and functional programming languages. Additionally, we propose `import CoefficientNotation` to interpret `2x` for `2·x`, and `import SegmentsNotation` to interpret sequences of uppercase letters with optional indices (`AB`, `ABC`, `ABCD`, `X1X2`) as `Segments(A, B)`, `Segments(A, B, C)`, `Segments(A, B, C, D)`, `Segments(X1, X1)`. In the latter case uppercase identifiers are available with backticks (`` `ABC` ``).

In Kotlin, operators are always referred to by their verbatim name, like `minus`, `unaryMinus`, and `dec`. We propose to allow an alternative notation: operator symbols enclosed into parentheses with no whitespaces around for infix operators, whitespace before for postfix ones and whitespace after for prefix ones. Thus, one can use `::(-)` for `::minus`, `::(- )` for `::unaryMinus`, and `::( --)` for `::dec`rement.

The other way around, any binary (or vararg) function should be allowed to be used as an infix operator by surrounding it by chevron quotation marks, e. g. `a ‹and› b` , `2 ‹Nat.plus› 3`.

## Avoiding redundant type annotations

As in Coq, Agda, and Lean, variables with certain names can be declared to have default types

```Kotlin
variables n : Int, z : Point
... now variables n, n1,… will have default type Int; z, z1,… default type Point
```

and two or more consecutive variables of the same type can be separated by whitespaces:

```Kotlin
fun plus(x y : Int) : Int
```

## Let blocks

We suggest introducing let-blocks. Let-block header contains a list of vals being defined, the following block contains a list of conditions those have to satisfy.  

```
let x y : Float
  x + 2y = 5
  x - y = 4
```

A let-block compiles if there is a compiler solver-plugin that supports given condition forms and succeeds iff there is a unique or a preferred solution.

We envision at least two solvers: Linear solver precisely as in Knuth's METAPOST (in particular, solves the example above) and, in the distant future, a deep unification solver as defined in [The Verse Calculus paper](https://simon.peytonjones.org/assets/pdfs/verse-icfp23.pdf) by Simon Peyton Jones, Guy Steele et al., that possesses enormous expressive power, elegantly subsuming both Prolog and Datalog.

## Dual naming: verbose names and concise names

Naming things is hard both in programming and in mathematics. Objects and operations should have readable and self-explanatory names. However, verbose names may heavily obstruct readability in formulas. Compare the following three variants of the same formula:
	`n·(n + 1) / 2`,

	`elementCount * (elementCount + 1) / 2`, and

	`div(times(elementCount, plus(elementCount, 1)), 2)`

Dual naming `` `verbose name`conciseName `` is a possible way to reconcile contradictory requirements.

```Kotlin
val `element count`n = ...
val (`height`x, `width`y) = o.getDimensions()
...

class List<`element type`T>
...
```

## Unicode names and custom operators 

It should be allowed to use non-ASCII characters and custom operators as `conciseName`s. Readable `verbose name` is strictly necessary (so one knows how to read those symbols aloud) and ASCII-only if `conciseName` contains characters not available on a standard keyboard.

```Kotlin
enum class `Boolean`𝔹 {`true`, `false`}

data class `Pair`(×)<out X, out Y>(val first : X, val second : Y)

val `factorial`( !) = fun(n : ℕ) {
  when(n) {0 -> 1; p⁺ -> n · p!}
}

val `conjugate`(+ ) = {c : ℂ -> Complex(c.re, -c.im)}
```

With those definitions, one can use `𝔹` for `Boolean`, `X × Y` for `Pair<X, Y>`, `n!` for `factorial(n)`, `+c` for `conjugate(c)` and  `⌊0.6⌋` for `floor(0.6)`.

The other way round, the verbose name can be a “closed operator”:

```Kotlin
fun<T> `if $c then $a else $b`ifelse(a b : T, c : 𝔹) : T

fun `⌊$x⌋`floor(x : Float)
```

#### Tightness

Expressions like `+n!` can be parsed both as `( +n )!` and `+( n! )`. With definitions as above, it is not a valid expression, it's a `syntax error: ambiguous expression`. However, one can specify tightness for operators. If `( !)` binds tighter than `(+ )`, `+n!` resolves into `+(n!)` and the other way around.

Infix operators may have different right and left tightness. For example, `(-)` binds tighter on the right than on the left: `a - b - c` resolves into `(a - b) - c`.

Tightness strengths must form a poset (actually, even less: a DAG), but to not form a set: they do not have to be pairwise comparable. We introduce abstract labels called Operator Categories and declare them to be tighter or weaker than some other labels.

Actually, an `OperatorCategory` is a bit more than a label: it specifies how to deal with respective homogeneous operator chains. For example, there is a large operator category `EqRel` that contains all comparison operators and resolves chains of the form `a < b < c`  into`a < b ‹and› b < c` .

#### Inner parameters

Operators may have inner parameters, e.g. the indexed access operator `arr[i]` is a postfix operator with an inner parameter `( [$idx])` . In mathematics, many binary operators, including tensor product and semidirect product, have optional parameters rendered as subscripts or superscripts.

Using parser techniques developed for the Agda programming language, we can embrace this complexity without any considerable problems.

By combining custom `OperatorCategory` and operators with inner parameters, one can even embrace the notorious example of insane operator complexity: the METAPOST path notation:

```Kotlin
draw z0 ·· z1 ·· z2 ·- z3 -- z4 --cycle
draw z0 ·· z1 ··[tension: 1.5, 1]·· z2 ·· z3
draw z0 [curl: c]·· z1 ··[curl: c] z2
draw z0 ·· z1 [up]·· z2 [left]·· z3 ·· z4.
draw (0,0) ··[controls: (26.8,-1.8), (51.4,14.6)]··
 (60,40) ··[controls: (67.1,61.0), (59.8,84.6)]·· (30,50)
```

## Disambiguating methods and properties

In Kotlin `obj.name(...)` can mean invocation of the method `name` of `obj` or extraction
of its property `name` of a callable type and its application. One cannot find out which one is used without looking up the definition of the class of `obj`, and this is ambiguity presents a major problem in an academic context.

To resolve this ambiguity and to improve typographical properties of the code, we suggest reserving dots `.` per se are reserved for properties only and use `▸` in case of method calls following the long tradition of using arrows for method calls started by PL/I in the late 60s:

```Kotlin
files ▸dropLast(n) ▸withIndex ▸last  
```

```Kotlin
fun example(files : List<File>,
            target : File)
  files ▸filter
    it.size > 0 &&
    it.type = "image/png"
  ▸map { it.name }
  ▸withIndex ▸map fun(idx, item) 
    ...
  ...
```

Since triangles are not present on the standard keyboard, we assume the IDE to convert the dots into such triangles when appropriate.

## Semantic considerations

Academic pseudocode languages do, in general, assume the default integer type to be overflow-free (as in Python), the operator `/` to be the true division operator even when both operands are integer. For integer division, an additional operator `//` should be introduced.

In accordance with their mathematical semantics, expressions like `1 + 1` are interpreted as `Int.plus(1, 1)` rather than `1.plus(1)`, i.e. arithmetical operators are considered to be functions belonging to companion objects of the given numeric type rather than methods of number objects themselves.

Since we mentioned companion objects containing operators like “plus”, we should also mention that the notion of type-classes is indispensable in many academic contexts. In Kotlin, one can define
both nested classes and extension functions. The type-classes are, in a sense, extension nested data classes with quite a bit of additional syntactic sugar.

Consider the following definition of a monoid-structure on a type `T`:

```
data class <T>.Monoid(val compose : (vararg xs : T)-> T)
  val unit ≔ compose() // unit is the nullary composition
  
  contracts {
    unit ‹compose› x = x
    x ‹compose› unit = x
    x ‹compose› y ‹compose› z = x ‹compose› (y ‹compose› z)

    compose(x, *xs) = x ‹compose› compose(*xs)
  }
```

With such a definition, we now can write functions like this:

```
fun<T : Monoid> square(x : T)
  x ‹T.compose› x
    
or, equivalently
    
fun<T : Monoid<::(∘)>> square(x : T)
  x ∘ x                  // generics resulution implicitly imports (∘) into the scope
```

Support for higher-kinded type classes and proper inheritance for them can be directly imported from Arend. Eventually, one should carefully introduce full-blown dependent types, following the defensive approach to dependent types as pioneered in Haskell.

Amusingly, adding dependent types to Kotlin immediately allows embedding SQL-type queries almost verbatim:

```
fun Table.select(cols : this.colsCtx.()-> List<t.Col>) : LazyTable
fun LazyTable.where(clause : this.ctx.()-> 𝔹) : LazyTable

users ▸select {name, age, address as "userAddress"} 
      ▸where {age > 18}
```

## Public properties in coroutines

Last but not least, we want to propose allowing to declare top-level vals and vars in coroutines `public val`s and `public var`s, in that case jobs generated by `launch f()` would have a subtype of `Job` having those vals and vars as read-only properties which can be used, for example, to track the progress of job completion. Introduce `public get val`s for computed properties in coroutines. 

## Strong object typing

Eventually, structured concurrency should be generalized to structured ownership, with a general notion of managed object and managing scopes. Kotlinesque coroutine scopes and Rustacean lifetimes are managing scopes, jobs and shared mutable variables are respective managed objects, governed by separation logic. Redistributable references to managed objects can be faithfully treated as values, types of which are path-dependent (in Scala sense) on their respective managing scopes (cs.Job, lt.Var). Thus, to handle them, it would suffice to support full-blown PDTs and allow passing objects (coroutine scopes, lifetimes, etc.) not only as arguments, but alternatively as parameters, e.g. `fun <cs : CoroutineScope> example(v : cs.MutRef<Int>)`.

Besides managed objects, there are exclusively-owned objects (cf. uniqueness typing). References to those cannot be copied or passed arbitrarily, so they must be marked syntactically as being non-values. When a method gets them as arguments, the respective arguments must be annotated either `my obj` or `borrow obj` in case the object is returned back to the call site after completion. A local “variable” containing an exclusively-owned object should be declared `my obj` instead of `val obj`, e.g. `my job = lunch someCoroutine(…)` or `my o = object : SomeInterface {…}`. Exclusively-owned objects most frequently appear as receivers (`this`). Owing to smart casts, strong typing for exclusively-owned objects can be piggybacked on the existing Kotlin type system by extending the syntax and semantics for interfaces. The resulting type system fragment would closely reassemble the system by F. Pfennig and A. Das from “[Verified Linear Session-Typed Concurrent Programming](https://www.cs.cmu.edu/~fp/papers/ppdp20.pdf)”, see also <https://www.cs.cmu.edu/~fp/papers/lmcs22a.pdf> for a primer on possible concise syntax.

The third possibility is the external/standalone objects (resources), such as filesystem and database: those are properly handled by a capability system like that in Scala 3.
