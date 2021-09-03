---
title: "Scala Intro"
subtitle: "Week 3"
date: 2021-03-03T20:58:52-05:00
draft: false
---


## Week 3: Data and Abstraction
 
### Lecture 3.1: CLass Hierarchies:

**Class Hierarchies: CLasses that extend eachother**
- model of evluation changes
  - method calls might depend on runtime type of receiver of a given method --> **dynamic binding** 
  - revisit family writing

**Abstract Class**
- Abstract classes can contain members which are missing an implementation
```
abstract class IntSet { 
    def incl(x: Int): IntSet
    def containes(x: Int): Boolean 
    }
```
- IntSet itself cannot be instantiated, must be suplemented by classes that **extend it**

**References for Sets represented as binary trees:** 
- http://ethanp.github.io/blog/2014/05/16/scala-union-beauty/
  - "how the union method retains the sortedness for the BST. Short answer... `this union other` --> each element of `this` is passed through `incl` to be inserted into `other`


**Class Extension**
- implement sets as binary trees
  - a tree for the empty set
  - a tree consisting of an integer and two sub-trees
- want to maintain invariant, property that rhs values are greater than the node, and LHS values are less than node
- purely functional, no mutation --> **persistent data structures**

See code for `IntSet` [here]()

- `Empty` and `NonEmpty` extend and **conform** to type `IntSet`
  - an object of type Empty or NonEmpty can be used wherever an object of type IntSet is required
- IntSet is the **Superclass**, EMpty and NonEmpty are the **sub classes**
- standard class **object** is the superclass of any class --> base class

**Object Definitions**
- defining EmptySet as an **Object rather than a class**
- defines a **singleton object**
  - cannot create multiple instances of this object with `new`
  - singelton objects are values 

**Exercise: Functional Union Method between two IntSets**
- for Empty set, the union of itself with an **other** set is always the equivalent to the other set
- for NonEmpty Sets, 
  - first split the LHS set into left and right, and take union
    - yields IntSet excluding `elem` of LHS set
  - take union between (left union right) and `other` set
  - finally, `incl` elem from LHS set
- each subsequent call **union** is on a set smaller than the previous, so we can safely assume the recursion terminates 

**dynamic method dispatch model**
- OO languates implement this, meaning code invoked by a method call depends on the runtime type of the object that contains the method
- similarity between dynamic dispatch of methods **to higher-order functions** 


### Lecture 3.2: How Classes are Organized

**Packages**
- classes and objects are organized in **packages**
- a **package clause** at top of source file places subsequent classes and objects into the package
```
package progfun.examples

object Hello()
```
- this would place Hello in the package progfun.examples
- can subsequently refer to Hello by its **fully qualified name**
    - `progfun.examples.Hello`
    - to run Hello from cmd line: `scala progfun.examples.Hello`

**Import clause**
- `import week3.Rational` --> imports all objects from package.module
- `import week3._` --> imports all objects in package
- `import week3.{Rational, Hello}` --> imports two objects from week3


**Default Imports**
- members of package: 
  - **scala** --> scala.Int, scala.Boolean
  - **java.lang** --> java.lang.Object
  - **scala.Predef** --> scala.Predef.assert


**Traits**
- A class can only have one superclass --> **Single Inheritance** 
- many times, there are many classes that a type naturally conforms to and inherits from --> **traits**
  - like an abstract class
- objects can inherit from arbitrarily many traits
  - ` class Square extends Shape with Planar with Movable...`
- Traits can contain fields and concrete methods 
  - cannot contain value parameters


**Scala Class Hierarchy**
- scala.Any --> superclass of any other classes 
  - base type of all types
  - methods such as `==`, `toString`, universal methods
- scala.AnyVal
  - base type of all primitive types
- scala.AnyRef
  - base tyepe of all reference types
- scala.ScalaObject
- scala.Null
  - subclass of all AnyRef derived classes
  - the type of null is Null
  - incompatible with AnyVal derived classes

```
// acceptable
val x = null
val y: String = x

// not acceptable
val z: Int = null

```

- scala.Nothing
  - bottom of scala type hierarchy
  - subtype of every other type
  - no value of type nothing --> why useful?
    - to signal abnormal termination
    - as an element type of empty collections --> `Set[Nothing]`
  
**Exceptions**
- `throw Exc`
  - aborts evaluation with the exception `Exc`
```
object scratch { 

    def error(msg: String) = throw new Error(msg)
}
// returns Nothing --> abnormal termination
```

**what is type of following**
- if (true) 1 else false
  - evaluates as `AnyVal` 
- Why? Expression is ambiguous, technically can evaluate to Int or Boolean
- Both inherit from AnyVal --> **typechecker** picks AnyVal as type for expression

### Lecture 3.3: Polymorphism

**Types: How compiler Sees classes and functions**
- **Type parameterization:** classes and methods can have types as parameters
  
**Immutable Linked-List**
- fundamental functional data structure
- two building blocks: 
  - Nil: empty list
  - Cons: a cell containing 1) the first element of the list and 2) the reference/pointer to the remainder of the list

**Cons-Lists in Scala**
- A list is either
  - an empty list, `new Nil`
  - a list `new Cons(x, xs)` consisting of a head elem x and a tail list, xs


**Type parameters**
- too narrow to define only lists with Int elements
- Generalize the definition using a **type parameter**
  - Type parameters are **written in square brackets, [T]**

[code for Cons-List implementation]()

**Generic Functions**
- Functions can have **type parameters**
- here is a function that creates a list consisting of a single elem
```

def singleton[T](elem: T): Cons[T] = { new Cons[T](elem, new Nil[T]) }

// can then write:

singleton[Int](1)
singleton[Boolean](true)

```

**Type Inference**
- scala compiler can usually deduce the correct type parameters from **value arguments of func call**
- could write `singleton(true)`


**Types and Evaluations**
- notice that type parameters are not persisted somwhere in the data structure
- in fact, **type parametrs do not affect evaluation in scala**
- type parameters and arguments removed befor evaluation
  - ***Type Erasure***

**Polymorphism**
- func can be applied to args of many types 
- the type can have instances of many types

Two principle forms of polymorphism:
1) **subtyping**: instances of a subclass can be passed to a base class
    - List --> Cons, Nill
    - methods that accept class List can accept Nill or Con as well
    - associated with OO

2) **generics**: instances of a func or class are created by type parameterization
    - using type parameterization to create List of Ints, Booleans, etc.
    - Associated with FP

**Exercise** 
- write func `nth` that takes an integer n and a list and selects the nth element from the list
- elements numbered from 0
- if index outside range from 0 up to length of list minus 1, a IndexOutOfBOundsException should be thrown 

[code for nth val]()


**Assignment**
Object-oriented representations based on binary trees.
[Assignment Code]()
