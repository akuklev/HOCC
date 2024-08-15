## Motivation

We have a dream to make Kotlin a programming language suitable for every purpose in any context. In this memo, I explore what can be done to design a Kotlin flavour suitable and appealing to replace various ad hoc flavours of pseudocode used in academic and educational contexts.

## Significant indentation and the block-structure-first approach

Owing to the universal popularity of Python in the scientific community, significant indentation should be used for multiline code blocks. When reading the code, people see the indentation-based block structure before anything else. Thus, contrary to Python, we want block structure to have priority over comments, string literals, and brackets. **Such an approach massively improves the performance of incremental parsing: code blocks can be delineated extremely quickly without prior parsing and parsed independently.** 

We propose to fix block indentation to two whitespaces once and for all, while treating any other number of indenting whitespaces (1 or >2) as a continuation of the previous line:

```Kotlin
fun example(files : List<File>,
            target : File)
  ...
  return something + somethingElse + somethingOther +
   yetSomething + rest
```

In case of consequent dedents, black square `■` may be used as a visual aid:

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

This way every coding line either starts with an `@`, or is an indented line following a coding line (with possibly one or more blank lines in between). Let us require the compiler to skim all the lines that do not meet this specification. These other lines now can be used for accompanying text written as is in any desired flavour of Markdown or TeX without fencing.

Freely interleaving the code and accompanying text, without fencing either, is the perfect fit for literate programming (hence the title). The very same file can either be fed into a Kotlin compiler to produce a binary or into a Markdown/TeX processor to produce a paper.

## Interactive Literate Programming

It was far from obvious in 1984, but the expository paper can (and should) contain runnable code samples to illustrate usages of the code being explained and test cases for each non-trivial function.

One could introduce a couple of new directives for that: 

```Kotlin
Unnamed runnable sample:
@run sampleFunction(1, 3)

Named one:
@example `Some name`
  val a = 1 + 2
  a + 3

Named with expected output, i.e. test case:
@example Addition1
  1 + 2 + 3
@expect
  6

Unnamed with display parameters:
@run(collapsed: true, autoexec: false)
  someLenghtyComputation()
```

Such blocks should be rendered as in internal REPL in the IDE so the user can edit and rerun them to play with the context of the source defined so far. The IDE should support rich (visual, animated and even interactive) output of such blocks. Wait, have we reinvented Kotlin-notebooks? 

## Compliance with mathematical notation

To be compliant with standard mathematical notation, we should display the multiplication operator as `·`, comparison operators as `≤`, `≥`, `=`, `≠`, logical operators as `¬`, `∧`, `∨`, and use `↦` in lambda-expressions, e.g. `{x ↦ x + 1}` . 

We should consider using `#` followed by a whitespace and displayed as `■` for end-of-line-comments instead of `//`. The latter should be reserved for integer division as in Python.

As it is customary in mathematics to have whitespaces around relation symbols and symbolic symmetric binary operators such as `+` (except for `a·b`), we require that for all such operators including the typing relation as in `n : Int`.

The assignment operator should be then displayed as `≔` when introducing a fresh name
(e.g. `val a ≔ 5`) and for default argument values, or by an immediate colon `key: value` for named arguments and other “key-value” cases.

In mathematics and functional programming, it's fairly common to use the right pointing black triangle for inverse application, i.e. `x ▸ f ≔ f(x)`. Thus we propose to display `x.let f` as `x ▸ f` and `x?.let f` as `x?▸ f`.

In Kotlin, operators are always referred to by their verbatim name, like `minus`, `unaryMinus`, and `dec`. We propose to allow an alternative notation: operator symbols enclosed into parentheses with no whitespaces around for infix operators, whitespace before for postfix ones and whitespace after for prefix ones. Thus, one can use `(-)` for `minus`, `(- )` for `unaryMinus`, and `( —)` for `dec`rement.

The other way around, any binary (or vararg) function should be allowed to be used as an infix operator by surrounding it by chevron quotation marks, e. g. `a ‹and› b` , `2 ‹Nat.plus› 3`.

## Dual naming: verbose names and concise names

Naming things is hard both in programming and in mathematics. Objects and operations should have readable and self-explanatory names. However, verbose names may heavily obstruct readability in formulas. Compare the following three variants of the same formula:
	`n·(n + 1) / 2`,

	`elementCount * (elementCount + 1) / 2`, and

	`div(times(elementCount, plus(elementCount, 1)), 2)`

Dual naming `` `verbose name` conciseName `` is a possible way to reconcile contradictory requirements.

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

fun `⌊·⌋`floor(x : Float) = ... # round
```

With those definitions, one can use `𝔹` for `Boolean`, `X × Y` for `Pair<X, Y>`, `n!` for `factorial(n)`, `+c` for `conjugate(c)` and  `⌊0.6⌋` for `floor(0.6)`.

#### Tightness

Expressions like `+n!` can be parsed both as `(+n)!` and `+(n!)`. With definitions as above, it is not a valid expression, it's a `syntax error: ambiguous expression`. However, one can specify tightness for operators. If `( !)` binds tighter than `(+ )`, `+n!` resolves into `+(n!)` and the other way around.

Infix operators may have different right and left tightness. For example, `(-)` binds tighter on the right than on the left: `a - b - c` resolves into `(a - b) - c`.

Tightness strengths must form a poset (actually, even less: a DAG), but to not form a set: they do not have to be pairwise comparable. We introduce abstract labels called Operator Categories and declare them to be tighter or weaker than some other labels.

Actually, an `OperatorCategory` is a bit more than a label: it specifies how to deal with respective homogeneous operator chains. For example, there is a large operator category `EqRel` that contains all comparison operators and resolves chains of the form `a < b < c`  into`a < b ‹and› b < c` .

#### Inner parameters

Operators may have inner parameters, e.g. the indexed access operator `arr[i]` is a postfix operator with an inner parameter `( [·])` . In mathematics, many binary operators, including tensor product and semidirect product, have optional parameters rendered as subscripts or superscripts.

Using parser techniques developed for the Agda programming language, we can embrace this complexity without any considerable problems.

By combining custom `OperatorCategory` and operators with inner parameters, one can even embrace the notorious example of insane operator complexity: the METAPOST path notation.

```Kotlin
draw z0 ·· z1 ·· z2 ·· z3 -- z4 --cycle
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

Since triangles are not present on the standard keyboard, so we assume the IDE to convert the dots into such triangles when appropriate.

## Semantic considerations

Academic pseudocode languages do, in general, assume the default integer type to be overflow-free (as in Python), the operator `/` to be the true division operator even when both operands are integer. For integer division, an additional operator should be introduced.

In accordance with their mathematical semantics, expressions like `1 + 1` are interpreted as `Int.plus(1, 1)` rather than `1.plus(1)`, i.e. arithmetical operators are considered to be functions belonging to companion objects of the given numeric type rather than methods of number objects themselves.

Since we mentioned companion objects containing operators like “plus”, we should also mention that the notion of type-classes is indispensable in many academic contexts. In Kotlin, one can define
both nested classes and extension functions. The type-classes are, in a sense, extension nested data classes with quite a bit of additional syntactic sugar.

Consider the following definition of a monoid-structure on a type `T`:

```Kotlin
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

```Kotlin
fun<T : Monoid> square(x : T)
  x ‹T.compose› x
    
or, equivalently
    
fun<T : Monoid<(∘)>> square(x : T)
  x ∘ x                  // generics resulution implicitly imports (∘) into the scope
```
