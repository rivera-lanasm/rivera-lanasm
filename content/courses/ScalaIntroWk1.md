---
title: "Scala Intro"
subtitle: "Week 1"
date: 2021-01-31T22:59:35-05:00
draft: false
---

The following series of notes and related resources were compiled while taking Martin Odersky's Coursera course, [Functional Programming Principles in Scala](https://www.coursera.org/learn/progfun1/home/welcome). 

I plan to use these as a reference for myself, to review concepts and examples from the course in the future. Also plan to further elaborate on external links I include here. 

- Week 1: 
- Week 2: 
- Week 3: 
- Week 4: 
- Week 5: 
- Week 6: 

## Week 1 Outline: Getting Started + Functions & Evaluation

### Why Scala?
**Concurrency and Parallelism**, see [Working Hard to Keep It Simple, 2011](https://www.youtube.com/watch?v=3jg1AheF4n0)
- **Parallel** programming executes programs faster on parallel hardware
   - Programs could just as simply run sequentially, but take advantage of specific hardware for speed
   - **Concurrent** programming manages concurrent execution threads explicitly
   - Why are both difficult?
       - **Non-determinism** caused by concurrent threads accessing **shared mutable state**
       - ```
            var x = 0
            async {x = x+1}
            async {x = x*2}
            //can yield 0, 1, or 2
            ```
       - to get deterministic processing, we must avoid the mutable state
       - Avoiding the mutable state means programming functionally
 
**Imperative vs. Functional Programming**
- Imperative programming focuses on the time dimension, executing commands sequentially with some deterministic control flow
- Functional programming focuses on 'space', seeking to complete tasks as a graph of dependent subtasks
 
**Scala: Combines Object-Oriented and Functional Programming**
- Parallelism:
   - Collections; parallel and distributed collections
- Concurrency:
   - Actors, Software transactional memory, Futures --> **Akka**
 

### SBT: Simple Build Tool
**Some resources**
- [guide to using SBT](https://github.com/shekhargulati/52-technologies-in-2016/blob/master/02-sbt/README.md)
- [sbt for Scala development](https://www.youtube.com/watch?v=PDhOv4NMK-Y)
 
**SBT Project Directory:**
- **Base or project's root directory:**
   - the “base or project's root directory” is the directory containing the project.
   - `build.sbt` is declared in the top-level directory that is, the base directory.
- **project directory**
   - another build inside your build, which knows how to build your build.
   - To distinguish the builds, we sometimes use the term proper build to refer to your build, and meta-build to refer to the build in project
 
### Source Files, Classfiles and the JVM
- [How Java Works](https://www.cs.cmu.edu/~jcarroll/15-100-s05/supps/basics/history.html)
- [Scala and JVM](https://www.toptal.com/scala/scala-bytecode-and-the-jvm#decompiling-class-files-with-javap)
  - Scala source code is stored in text files with the extension .scala
  - The scala compiler compiles .scala source files to .class files
  - Classfiles are binary files containing machine code for the Java Virtual Machine and are stored to `classpath`
 
**Scala Language Notes and Resources**
- [avoid using return in functions](https://tpolecat.github.io/2014/05/09/return.html)
- [Scalastyle](http://www.scalastyle.org/)
- [Scala Cheat Sheet](https://github.com/lampepfl/progfun-wiki/blob/gh-pages/CheatSheet.md)
- [Twitter Scala School](http://twitter.github.io/scala_school/)
- [SO Questions](https://stackoverflow.com/tags/scala/info)
- Scala exercises 47 degrees 
- [JUnit and Scala](https://medium.com/@alonso.delarte/testing-scala-with-junit-a79bc2d1bb4c)
- [Scala Basics](https://www.baeldung.com/scala/category/scala-basics)

### Lecture 1.1: Programming Paradigms:
 
A paradigm describes distinct concepts or thought patterns
- imperative
- functional
- logical
- OO: orthogonal to previous 3, in that it can be combined with these paradigms
 
**Imperative:**
- modifying mutable variables
- using assigments
- using control structures (if-then-else, loops, break, return)
- Imperative programs understood as instruction sequences for a **Von Neumann** architecture
  - [Von Neumann Architecture](https://www.youtube.com/watch?v=H0xGKKpKaRE) 
  - CPU (Datapath and Control FSM), Main Memory, and I/O 
  - mutable variables, variable de-references and assignments, and control sturctures all can be mapped to aspects of the Von Neumann computer
 

**Correspondance Between Imperative Prog. and VN Architecture**
- Pure imperative programming is limited by the **Von Neumann bottleneck**, coined by Backus, whereby one tends to conceptualize data strutures "word-by-word"
- There is a strong correspondance between mutable variables and memory cells, variable dereferences and load instructions, variable assignments and store instructions, control structures and jumps.

Need **techniques** for defining high level abstractions and **theories** for these higher level abstractions in order to reason about them
- Theory consists of:
   - one or more data tyes
   - operations on these types
   - laws that describe relationship between values and operations
- Theories do not account for mutation
   - theory of polynomials defines the sum of polynomials for any degree, but does NOT define an mutation operator to change a coeffiecient while keeping the polnomial the same
- Consequences for programming:
   - if we want to implement high level concepts following mathematical theories, no place for mutation
   - Therefore, focus on defining theories for operators expressed as functions, avoid mutations, define ways to abstract and compose functions


- John Backus' 1977 Turning Award lecture, [Can Programming be LIberated from the von Neumann Style? A Functional Style and its Algebra of Programs](https://medium.com/luteceo-software-chemistry/can-programming-be-liberated-from-the-von-neumann-style-932ba107402b)
- A bigger topic than what I can address here
  - [Does FP actually address VN Bottleneck?](https://stackoverflow.com/questions/48674498/does-functional-programming-reduce-the-von-neumann-bottleneck)

**Functional Programming:**
- **restricted sense:** FP means programming without mutable vars, assignments, loops, and other imperative control structues
- **wider sense (Scala):** FP means focuseing on the functions --> for example, functions can be values that are prduced, consumed, and compsued (**first-class citizens**)
 
**Why FP Matters, John Hughes**
- [Paper](https://www.cs.kent.ac.uk/people/staff/dat/miranda/whyfp90.pdf)
- [Keynote Lambda Days 2017, ](https://www.youtube.com/watch?v=1qBHf8DrWR8&t=61s)
- [Lambda Calculus](https://arxiv.org/pdf/1503.09060.pdf)


### Lecture 1.2: Elements of Programming
 
**Operator Evaluation Pattern:**
- take leftmost operator, then evaluate its operands (l before r), and finally apply the operator to the operands
  - `(1+2) + (6+7) --> (3) + (13) --> 16`
- a name is evaluated by replacing it with the right hand side of its definition
- the eval process stops once it results in a **value**, rather than another function evaluation
   - for the moment, consider a value to be a number
- **Substitution Model** or **Call by Value**   
```
// call by value (CBV) implementation --> NAMES after ARGS
sumOfSquares(3,2+2)
sumOfSquares(3,4)
square(3) + square(4)
9 + square(4)
9 + 16
25
```
- this **substituion model** is formalized in the lambda calculus
  - Note that this model cannot express operators with **side effects**
 
**Termination**
- not every expression reduces to a value (in an infinite number of steps)

Note an alternative evaluation strategy for SumofSquares: 
**Call by Name, CBN**
- CBN is another implementation of substitution model
```
// CBN --> NAMES before ARGS
sumOfSquares(3,2+2)
square(3) + square(2+2)
3*3 + square(2+2)
9 + square(2+2)
9 + (2+2) * (2+2)
9 + (2+2) * 4
9 + 4 * 4
25
```
**Both strategies reduce to the same final value if:**
  - reduced expressions consist of pure functions
  - both evals terminate
- CBV has advantage that it evaluates every function arg only Once
- CBN has advantage that a function arg is not evluated if the corresponding param is unused in the eval of the function body
 
**Examples**
```
def test(x:Int, y:Int) = x * x

=================
CBV
test(3+4,8)
test(7,8)
7*7
49
 
CBN
test(3+4,8)
(3+4)*(3+4)
7*(3+4)
7*7
49
=================
CBV
test(3+4,2*4)
test(7,2*4)
test(7,8)
7*7
49
 
CBN
test(3+4,2*4)
(3+4)*(3+4)
7*(3+4)
7*7
49
```
 
### Lecture 1.3: Eval strategies and Termination:
 
**CBV, CBN, and termination**
- what if termination is not guaranteed?
 
**Theorem:** If CBV eval of an expr, e, terminates, **then CBN eval of e terminates as well**
- **the opposite is not necessarily true**
 
**Example:** 
- `def first(x: Int, y: Int) = x`
- `def loop() = loop --> an inf loop`
- if `first(2,loop)` is called **using CBN**, the function will evaluate, as the second parameter, y, is not invoked in the function definition and thus never evaluated by CBN.
- CBV, however, evaluates expressions as soon as they are encountered rather than waiting to see if they will be invoked in function definition. 
- CBV attempt to evaluate value passed to `y`, and enters inf loop


**Scala's eval strategy:**
- Scala normally uses **CBV**
   - if the type of a function parameter stars with =>, it uses CBN
   - `def constOne(x: Int, y: => Int) = 1
- In practice, CBV is often exponentially faster
   - Also plays nicer with imperative effects and side effects
 
 **Evaluation Strategy Resources**
 - [Baeldung](https://www.baeldung.com/scala/parameters-by-value-by-name)
 - 

### Lecture 1.4: Conditionals and Value Definitions:

**Conditional Expression**
- `if-else` is an expression, not a statement
- `def abs(x: Int) = if (x>=0) x else -x`
 
**Value Definition**
- we saw that **function parameters** can be passed by value or by name
- the `def` form is "by-name", its rhs is evaluated on each use (invocation)
- `val` is "by value"
- `val y = square(x)`, the rhs of a val def is evaluated **At the point of the definition itself**
 - val y refers to 4, not square(2)
- Note that `def loop: Boolean = loop` evaluates, but `val x = loop` **does not --> inf loop**
 
### Lecture 1.5/1.6: Blocks and Lexical Scope 
**Newton's Method**
- achieve estimate of square root by successive approximations
 
**Fixed Point Iterative Implementation Newton's Method**
```
 
def isGoodEnough(guess: Double, x: Double): Boolean = {
   //abs(guess*guess - x) < .001 // the absolute error
   // relative error accounts for very large and very small values
   abs(guess*guess - x)/x < .001
  
}
 
def improve(guess: Double, x: Double): Double = {
   (guess + x/guess)/2
}
 
def sqrtIter(guess:Double, x: Double): Double = {
   // note that only recursive functions require an explicit return type in Scala
   if (isGoodEnough(guess, x)) x)
   else sqrtIter(improve(guess,x),x)
}
 
def sqrt(x: Double): Double = {
   // can nest prevous functions here to avoid name-space pollution
   sqrtIter(1.0, x)
}
```

**Avoid name-space pollution**
- nested functions; nest `improve`, `isGoodEnough` functions inside the `sqrtIter() def`
- Note that values defined within the scope of the function is visible to any nested functions
- Note the redudant assignments of the variable, x, in the function definitions above
 
**Blocks**
- a block is delimited by `{}`
- contains a seq of defs or expressions
- the last element of a block is an expression that defines its value
- Blocks are themselves expressions
- Note that values defined in the block are only visible within the block, and that values in the name space are visible in the block **unless** they are shadowed (overwrite)
 

### Lecture 1.7: Tail Recursion
  
[JVM Stacks and Stack Frames](http://alvinalexander.com/scala/fp-book/recursion-jvm-stacks-stack-frames/)
- **Stack definition:** Each JVM thread has a private Java virtual machine stack, created at the same time as the thread. A JVM stack stores frames, also called “stack frames”...it holds local variables and partial results, and plays a part in method invocation and return
- When a new thread is launched, the JVM creates a new stack for the thread. A Java stack stores a thread’s state in discrete frames. The JVM only performs two operations directly on Java stacks: it pushes and pops frames.
- When a thread invokes a Java method, the JVM creates and pushes a new frame onto the thread’s stack. This new frame then becomes the current frame. As the method executes, it uses the frame to store parameters, local variables, intermediate computations, and other data.
- **Stack Frame:** consists of local variables, operand stack, and frame data (constant pool ref)
- As the method is executed, the code can only access the values in the current stack frame, which you can visualize as being the top-most stack frame.
 
[Tail Recursion](https://alvinalexander.com/scala/fp-book/tail-recursive-algorithms/)
- A tail-recursive function is just a function whose very last action is a call to itself (or another function)
- "When you write your recursive function in this way, the Scala compiler can optimize the resulting JVM bytecode so that the function requires only **one stack frame** — as opposed to one stack frame for each level of recursion!"
- Reuses stack frame, avoids stack overflow in functions susceptible to deep recursive chains
   - equivalent to a **loop** in an imperative program
   - remember to avoid premature optimization
  
**Pattern to make recursive function tail-recursive:**
- nest original function in a new function
- insert "accumulator" object into super function
- modify the nested function to utilize accumulator
  - A way to prove that sum isn’t tail-recursive is to tag the function with a Scala annotation named @tailrec. This annotation won’t compile unless the function is tail-recursive.
 
**Consider Factorial**
```
def factorial_nonTR(n: Int) : Int = {
   if (n == 0) 1 else n*factorial_nonTR(n-1)
}
 
// Tail Recursive
def factorial(n: Int) : Int = {
 
   def factorial_Acc(n:Int, acc:Int): Int = {
       if (n==0) acc*1 else factorial_Acc(n-1, n*acc)
   }
 
   factorial_Acc(n, 1)
}
```
 
**Exercises: Recursion**
- [Link to week 1 code](https://github.com/rivera-lanasm/ScalaFP_Coursera/blob/master/week1/recfun/src/main/scala/recfun/RecFun.scala)




