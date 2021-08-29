---
title: "Scala Intro"
subtitle: "Week 5"
date: 2021-03-03T22:47:50-05:00
draft: false
---

#### ================================
**Lecture 5.1: More Functions on Lists**
#### ================================
 
**Sublists and element access:**
- xs.head
- xs.tail (all elements except head)
- xs.length
- xs.last
- xs.init => list of all elements of xs exccept the last one (last)
- xs take n => list of first n elements
- xs drop n => remaining sublist after taking n elements
- xs(n) => element of xs at index n
 
**Creating New Lists:**
- `xs ++ ys`: concatenation;
- `xs.reverse`; reverse order
- `xs updated (n,x)`; replace index n with x
 **Finding Elements**
- `xs indexOf x`; yield index of first elem in xs equal to x, or yields -1 if x not in xs
- `xs contains x`; boolean
**implementation of List methods**
- **last** runtime is proportional to length of the list
- **init** runtime also proportional to len of list
- **concat** complexity proportional to |xs|
- **reverse** n for concat (:::) and n for reverse --> n*n (quadratic)
```
object Week5 {
 
 
   def last[T](xs: List[T]): T = {
 
       xs match {
           case List() => throw new Error("last of empty list")
           case List(x) => x
           case y :: ys => last(ys)
           }
       }
 
   def init[T](xs: List[T]): List[T] = {
 
       xs match {
           case List() => throw new Error("init of empty list")
           case List(x) => List()
           case y :: ys => List(y) ++ init(ys)
           }
 
       }
 
   def concat[T](xs: List[T], ys: List[T]): List[T] = {
 
       // depends on xs --> concatenation defined by ::: operator
       // xs ::: ys is basically a prepend of xs onto ys
       // thus makes sense to pattern match on xs
 
       xs match {
           case List() => ys
           case z :: zs => z :: concat(zs , ys)
       }
 
 
   def reverse[T](xs: List[T]): List[T] = {
 
       xs match {
           case List() => List()
           case y :: ys => reverse(ys) ::: List(y)
           }
       }
 
} 
```
 
 
#### ================================
**Lecture 5.2: Pairs and Tuples**
#### ================================
 
Introduce pairs and tuples, how they can help in **program composition and decomposition**
 
**Sorting Lists Faster**
- a more efficient sorting function than insertion sort
- **merge sort**
 - if the list consists of 0 elems, already sorted
 - else
   - Separate the list into 2 sub-lists, each of about equal length
   - Sort the 2 sub-lists
   - Merge the two sorted sub-lists into a single sorted list
 
**see test5_1**
 
**Pair and Tuples**
- `val pair = ("answer",42)` => pair: (String, Int)
- type of val pair is (String, Int)
- pairs can be used as **patterns**
 - `val (label, value) = pair`
- works analagously with **tuples**, with more than 2 elems
- a tuple type (T1,...,TN) is an abbreviation of the parameterized type: scala.Tuple*n*[T1,...,Tn]
- values can be accessed with `pair._1`, `pair._2`, etc.
 
 
**Re-writing the merge function**
- the current implementation uses a nested pattern match
- does not reflect inherent symmetry of the merge alg
- rewrite using a pattern matching over pairs
**see test5_1**
 
 
#### ================================
**Lecture 5.3: Implicit Parameters**
#### ================================
 
How can we write a version of merge sort so that it can be used **not just for a singular argument type**
- **parameterization by either functions or objects** helps here
- want to achieve `def msort[T](xs: List[T]): List[T] = ...`
- currently can't work because `<` in merge is **not defined for arbitrary types, T**
- **can parametrize merge with the necessary comparison function**
- see test5_1.scala
 
 
There is already a class in standard lib that represents ordering: **scala.math.Ordering[T]**
- provides ways to compare elements of type T
- instead of using user defined, lt, we can use Ordering instead
 
**Implicit Parameters**
- passing around lt or ord values is cumbersome
- can make it appear that way by not passing ord or lt explicitly --> **implicit parameter**
- `(implicit ord: Ordering)`
- when subsequent functions are called without explicitly passing the parameter, `msort(fst)`, the **compiler** will figure out right implicit to pass based on the **demanded type**
 
 
**Rules for Implicit Parameters**
- compiler will search for an implicit definition that:
 - is marked implicit
 - has a type comparible with T
 - is visible at the point of the function call, or is defined in a companion object associated with T
- if there is a single (most specific) definition, it will be taken as the actual arg for the implicit param
- otherwise, error
 
 
#### ========================================
**Lecture 5.4: Higher Order List Functions**
#### ========================================
 
We've seen that functions on lists have similar structures, display patterns:
- **transformeing (map)** each element in a list in a specified way
- **retrieving (filter)** a list of all elements satisfying a criterion
- **combining (reduce)** the elements of a list using an **operator**
 
**Apply a function to elements of a list**
- see test5_2.scala
- this scheme can be generalized to the **method map** of the List class
- see test5_2.scala for a simplified version of the map method
 
**Filtering**
- besides `filter`, there are the followig methods for extracting sublists based on predicate
 - xs filterNot p
 - xs partition p
 - xs takeWhile p
 - xs dropWhile p
 - xs span p
 
**Exercise: Pack func, packs consecutive duplicates of a list into sublists**
- see test5_2.scala
 
**Encode**
 
#### ========================================
**Lecture 5.5: Reduction of Lists**
#### ========================================
 
Introducing **fold/reduce** combinators
- several variants
- all insert an operator between adjacent elements of a list
 
**Reduction of Lists**
- see test5_3.scala
- implemented with **recursion/pattern matching scheme**
 
**ReduceLeft**
- `ReduceLeft` inserts a given binary operator btween adjacent elems of a list
  - `List(1,2,3) reduceLeft ((x,y) => x+y)`
  - this yields `(((x1 op x2) op x3)...) op xn`
 
 
 **Shorter way to write functions**
 - `((x,y) => x*y)` can be rewritten as `(_*_)`
- `List(1,2,3) reduceLeft ((x,y) => x+y)` can be rewritten `List(1,2,3) reduceLeft (_+_) `


More General Version of ReduceLeft -> **FoldLeft**
- like reduceLeft, but takes an **accumulator**, z, as an additional parameter
- accumulator **returned when FoldLeft is called on an empty list, the stop condition**
- whereas Reduce produces an outcome as the linear combination of the elements of the list, Fold "folds" the elements of the list into the accumulator using the operator provided
  - `def sum(xs: List[Int]) = (xs foldLeft 0) (_+_)`
  - `def product(xs: List[Int]) = (xs foldLeft 1) (_*_)`
- note that reduceLeft can be defined in terms of foldLeft

**implementation of ReduceLeft and FoldLeft**
- note that in FoldLeft, the type of the accumulator, z, can be different than the type of the elements of the list

**FoldRight and ReduceRight**
- applications of foldLft and reduceLeft unfold on trees that lean to the left 
- dual functions, **foldRight** and **reduceRight**
- **reduceLeft** yields `(((x1 op x2) op x3)...) op xn`
- **reduceRight** yields `x1 op (x2 op (...(x{n-1} op xn)...)`

**Differences between FoldLeft and FoldRight**
- for operators that are **associative and commutative**, FL and FR are equivalent 
- sometimes, only one type is appropriate

**Revisiting concat**
- previously defined as 
```
def concat[T](xs: List[T], ys: List[T]): List[T] = {

    // depends on xs --> concatenation defined by ::: operator
    // xs ::: ys is basically a prepend of xs onto ys
    // thus makes sense to pattern match on xs

    xs match {
        case List() => ys
        case z :: zs => z :: concat(zs , ys) 
        }

    }
```
- a new formulation **using FR**
```
    def concat[T](xs: List[T], y: List[T]): List[T] = {

        (xs foldRight ys) ( _ :: _ )

    }
```
- note that `ys` here is being treated as the **accumulator**
- in this case, FL could not replace FR in the implementation
  - using FL, **the types would not work out**

**More Examples of Folds**
```
    def concat[T](xs: List[T], ys: List[T]): List[T] = {

        (xs foldRight ys) ( _ :: _ )

    }


    def mapFun[T, U](xs: List[T], f: T => U): List[U] = {
     
        (xs foldRight List[U]())( f(_)::_ )
        }
    // ListReduce.mapFun(List(1,2,3), (i: Int) => i*i )


    def lengthFun[T](xs: List[T]): Int = {
        
        (xs foldRight 0)( ((x,y) => 1+y) )
    }
    // ListReduce.lengthFun(List(1,2,3))
```


#### ========================================
**Lecture 5.6: Reasoning about Concat**
#### ========================================

**What does it mean for FP to be correct?**
- use **concat** as example, `++` for lists
  - would like to verify that concatenation is **associative**
  - and that it admits the empty list Nil as netrual element to the lef and to the right
```
(xs ++ ys) ++ zs == xs ++ (ys ++ zs)

xs ++ Nil == xs

Nil ++ xs == xs

```
- how can we prove these properties? **what criteria can we use?**
- **Structural Induction** on lists

**Reminder: Natural Induction**
- principle of proof by natural induction
- *To show that a property P(n) for all the integers n >= b*
  - Show that we have P(b) *(base case)*
  - for all integers n >=b, **show the induction step**
    - if one has p(n), then one **also** has p(n+1)
- in FP, we can **apply reduction steps as equalities to some part of a term**
  - works because pure FP does not have side effects, so a term is equivalent to the term to which it reduces
  - **referential transperancy**


**Structural Induction**
- analogous to natural induction
- To prove p(xs) for all lists, xs
  - **show** that p(nil) holds, **base case**
  - for a list xs and **some element x**, show the **induction step**
    - *if p(xs) holds, then p(x :: xs) also holds*

**review content for concat**




