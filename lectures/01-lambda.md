---
title: Lambda Calculus
headerImg: sea.jpg
---

## The Smallest Universal Language

![Alonzo Church](https://upload.wikimedia.org/wikipedia/en/a/a6/Alonzo_Church.jpg)

Developed in 1930s by Alonzo Church

- Studied in logic and computer science

Test bed for procedural and functional PLs

- Simple, Powerful, Extensible

## The Next 700 Languages

![Peter Landin](https://upload.wikimedia.org/wikipedia/en/f/f9/Peter_Landin.png)

> Whatever the next 700 languages
> turn out to be,
> they will surely be
> variants of lambda calculus.

Peter Landin, 1966

## Syntax: What Programs _Look Like_

**Three** kinds of expressions

- `e`

**Variables**

- `x`, `y`, `z`

**Function Definitions (Abstraction)**

- `\x -> e`

**Function Call (Application)**

- `e1 e2`

**Complete Description**

![The Lambda Calculus](/static/img/lambda-calculus.png)

## Syntax: Association

## Application Is Left Associative

We write

`e1 e2 e3 e4`

instead of

`(((e1 e2) e3) e4)`

## Examples

```haskell
\x -> x             -- The Identity function

\y -> \x -> x       -- A function that returns the Identity Fun

\f -> f (\x -> x)   -- A function that applies arg to the Identity Fun
```

## Semantics : What Programs _Mean_

We define the _meaning_ of a program with simple rules.

1. Scope
2. $\alpha$-step  (aka. _renaming formals_)
3. $\beta$-step   (aka. _function calls_)


## Semantics: Scope

The part of a program where a **variable is visible**

In the expression `\x -> e`

- `x` is the newly introduced variable

- `e` is **the scope** of `x`

- `x` is **bound** inside `e`

## Semantics: Free vs Bound Variables

`y` is **free** inside `e` if

- `y` appears inside `e`, and
- `y` is **not bound** inside `e`

Formally,

```haskell
free(x)       = {x}
free(\x -> e) = free(e)  - {x}
free(e1 e2)   = free(e1) + free(e2)
```

## QUIZ

Let `e` be the expression `\x -> x (\y -> x y z)`.

Which variables are *free* in `e` ?

**A.**  `x`

**B.**  `y`

**C.**  `z`

**D.**  `x`, `y`

**E.**  `x`, `y`, `z`


## Semantics: $\alpha$-Equivalence

$\lambda$-terms `E1` and `E2` are $\alpha$-equivalent if

- `E2` can be obtained be *renaming the bound variables* of `E1`

or

- `E1` can be obtained be *renaming the bound variables* of `E2`

## Semantics: $\alpha$-step

**Example:** The following three terms are $\alpha$-equivalent

```haskell
\x -> x   =a> \y -> y   =a> \z -> z
```

We write `E1 =a> E2` if `E1` is $\alpha$-equivalent to `E2`.  

- We can say `E1` takes an $\alpha$-step to `E2`.

## $\alpha$-step Makes Scope Clear

We often $\alpha$-rename to make **parameter names unique**

For example, instead of

```haskell
    \x -> x (\x -> x) x     -- Yucky scope

=a> \x -> x (\y -> y) x     -- Scope of bindings crystal clear
```


## Semantics: Function Calls

In the $\lambda$-calculus, a "function call" (application) looks like `(x -> E1) E2`


| **Function** | **Argument**  |
|:------------:|:-------------:|
| `\x -> E1`   | `E2`          |


How do we **evaluate** the _function_ with the given _argument_?

1. **Rename** parameters to make them unique
2. **Substitute** all occurrences of `x` in `E1` with `E2`!

If so, we say that

- `(\x -> E1) E2` $\beta$-steps to `E1[x := E2]`

and we can write it as

- `(\x -> E1) E2   =b>   E1[x := E2]`


## Function Calls: $\beta$-step Example

Replace occurrences of parameter `f` with argument

```haskell
(\f -> f (f x)) g        

   =b>     g (g x)
```

No need to rename, bindings already unique

## Normal Forms

An **redex** is a $\lambda$-term of the form

`(\x -> E1) E2`

A $\lambda$-term is in **normal form** if it contains no redexes.

## QUIZ

Is the term `x` in _normal form_ ?

**A.** Yes

**B.** No


## QUIZ

Is the term `x y` in _normal form_ ?

**A.** Yes

**B.** No


## QUIZ

Is the term `(\x -> x) y` in _normal form_ ?

**A.** Yes

**B.** No



## QUIZ

Is the term `x (\y -> y)` in _normal form_ ?

**A.** Yes

**B.** No



## Semantics: Evaluation

A $\lambda$-term `E` **reduces/evaluates to a normal form** `E'` if

there is a sequence of steps

```haskell
E =?> E_1 =?> ... =?> EN =?> E'
```

where each `=?>` is

- An $\alpha$-step `=a>` or
- A  $\beta$-step `=b>`.


## Examples of Evaluation

```haskell
(\x -> x) E
  =b> E
```

```haskell
(\f -> f (\x -> x)) (\x -> x)
  =a> (\f -> f (\x -> x)) (\y -> y)
  =b> (\y -> y) (\x -> x)
  =b> (\x -> x)
```

## Non-Terminating Evaluation

```haskell
(\x -> x x) (\y -> y y)
  =b> (\y -> y y) (\y -> y y)
  =a> (\x -> x x) (\y -> y y)
```

Oops, we can write programs that loop back to themselves...

- Self replicating code!

## $\lambda$-Calculus Review

**Super tiny language**

- `E ::= x | \x -> E | (E E)`

**Many [evaluation strategies](http://dl.acm.org/citation.cfm?id=860276)**

- Many steps possible, which to take?
- Call-by-name
- Call-by-value
- Call-by need

**Church Rosser Theorem**

- Regardless of strategy at most *one normal form*
- i.e. Programs can evaluate to a single result.

## $\lambda$-calculus and CSE 130?


> Whatever the next 700 languages
> turn out to be,
> they will surely be
> variants of lambda calculus.

Huh? What was that Landin fellow going on about?

## Programming with the $\lambda$-calculus

*Real languages have lots of features*

- Local Variables
- Booleans, Branches
- Records
- Integers
- Arithmetic
- Functions (ok, we got those)
- Recursion

Lets see how to _encode_ all of the above
with the $\lambda$-calculus.

**Hidden Motive**

- Free your mind
- Build intuition about **evaluation-by-substitution**

HEREHEREHERE

## THE END

Super brief notes, see details

## Booleans

```haskell
true  = \x y. x
false = \x y. y
ite   = \b x y. b x y
```

QUIZ
----

Given

```
foo = \b x y. b x y
bar = \p q. p
```

What does the following evaluate to?

```
foo bar apple orange
```

A. `foo bar apple orange`
B. `bar apple`
C. `apple`
D. `orange`
E. `bar`

QUIZ
----

Given

```
foo = \b x y. b x y
bar = \p q. p
baz = \b x y. b y x
```

What does the following evaluate to?

```
foo (baz bar) apple orange
```

A. `foo bar apple orange`
B. `bar apple`
C. `apple`
D. `orange`
E. `bar`


Boolean Operators
-----------------

We can develop the operators from their **truth tables**

```haskell
not = \b. ite b false true
and = \b1 b2. ite b1 b2 false
or  = \b1 b2. ite b1 true b2
```

Note that **for any** `a`, `b` and `c` we have:

```
ite a b c
  = (\b x y. b x y) a b c
  = a b c
```

hence we can simplify the above to

```haskell
not = \b.     b false true
and = \b1 b2. b1 b2 false
or  = \b1 b2. b1 true b2
```

Pairs
-----

What can we *do* with **pairs** ?

1. put **two** items into a pair.
2. get **first** item.
3. get **second** item.



Pairs : API
-----------

```haskell
(put v1 v2)  -- makes a pair out of v1, v2 s.t.

(getFst p)   -- returns the first element

(getSnd p)   -- returns the second element
```

such that

```haskell
getFst (put v1 v2) = v1
getSnd (put v1 v2) = v2
```

Pairs : Implementation
----------------------

```haskell
put v1 v2 = \b. ite b v1 v2

getFst p  = p true

getSnd p  = p false
```


QUIZ
----

Suppose we have

```haskell
one   = \f x. f x
two   = \f x. f (f x)
three = \f x. f (f (f x))
four  = \f x. f (f (f (f x)))
```

What is the value of

```
foo (mkPair true) false

foo (mkPair true) false =

mkPair 1 (mkPair 2 (mkPair 3 false))
```

A. `true`
B. `false`
C. `mkPair true false`
D. `mkPair true (mkPair true false)`
E. `mkPair true (mkPair true (mkPair true false))`


Naturals
--------

`n f s` means run `f` on `s` exactly `n` times

```haskell
-- | represent n as  \f x. f (f (f  ...   (f x)
--                         '--- n times ---'

1 = \f x. f x
2 = \f x. f (f x)
3 = \f x. f (f (f x))
4 = \f x. f (f (f (f x)))
```

QUIZ: Church Numerals
---------------------

Which of these is a valid encoding of `0` ?

```haskell
-- A
zero = \f x. x

-- B
zero = \f x. f

-- C
zero = \f x. f x

-- D
zero = \x. x

-- E
-- none of the above!
```

sub1 n = getSnd (n (\(flag, res) -> if (flag) (true, res + 1) (true, res))  (false, 0))


function sub1(n){
 var res = (false, 0);


  var flag = false;
 for (var i=0; i<n; i++){
   if (flag)
     res += 1;
   else
     flag = true;
 }
 return res;
}



Operating on numbers
--------------------

Lets implement a small API for numbers:

```haskell
isZero n = ... -- return `true` if n is zero and `false` otherwise
```

```haskell
plus1 n  = \f x. ?  -- ? should call `f` on `x` one more time than `n` does
```

```haskell
plus n m  = \f x. ?  -- ? should call `f` on `x` exactly `n + m` times
```

```haskell
mult n m  = \f x. ?  -- ? should call `f` on `x` exactly `n * m` times
```


```haskell
isZero = \n. n (\b. false) true
plus1  = \n. (\f s. n f (f s))
plus   = \n1 n2. n1 plus1 n2
mult   = \n1 n2. n2 (plus n1) zero
```

Recursion
---------
