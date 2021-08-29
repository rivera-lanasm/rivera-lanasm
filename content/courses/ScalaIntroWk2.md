---
title: "Scala Intro"
subtitle: "Week 2: HOF"
date: 2021-02-24T22:47:56-05:00
draft: false
---

The following series of notes and related resources were compiled while taking Martin Odersky's Coursera course, [Functional Programming Principles in Scala](https://www.coursera.org/learn/progfun1/home/welcome). 

I plan to use these as a reference for myself, to review concepts and examples from the course in the future. Also plan to further elaborate on external links I include here. 

- Week 1: 
- Week 3: 
- Week 4: 
- Week 5: 
- Week 6: 

## Week 2 Outline: Higher Order Functions
 
### Lecture 2.1: Higher-Order Functions:
FP languages treat functions as **first class** values
- function can be passed as a parameter and returned as a result
- **higher order functions** take other functions as parameters or return functions as results, as opposed to **first order functions**

There are many variations of the pattern, $\sum_{x=1}^n f(x)$, and we can generalize this pattern using higher order functions
- nest the f(x) from above, as a parameter, into a general sum function 
- Function below takes **sum of f(x), from a to b**
- Function takes as input: a function, and limits a and b
   ```
   def sum(f: Int => Int, a: Int, b:Int): Int = {
       if (a>b) 0 else f(a) + sum(f,a+1, b)}

   def cube(x:Int): Int = {x*x*x}

   def sumCubes(a: Int, b: Int):Int = {
     sum(cube, a, b)
   }
   ```
 
Note the use of **function type** in the above function, `f: A => B`
- Indicates that `f` takes in a value of type A and returns a value of type B


**Anonymous Functions**
- Getting around writing separate auxillary functions to pass as `f(x)` to a function like `sum`.
- Think about passing strings to functions, like print(). We do not need to define a str value prior to passing to print()
- because strings exist as **literals**. Analogously, **function literals** let us write a function without giving it a name
 
   ```
   # anonymous function syntax
   (x: Int) => x*x*x
   ```
- `(x:Int)`, lhs, is the parameter, and `x*x*x`,rhs, is the body
- `SumInts` and `SumCubes` using anonymous function, using def of `sum` above:
   ```
   def sumInts(a: Int, b:Int)= sum(x=>x, a, b)
   def sumCubes(a: Int, b:Int)= sum(x:Int =>x*x*x, a, b)
   ```
 
**Exercise: Write tail recursve version of sum**
 ```
def sum(f: Int => Int, a: Int, b: Int): Int = {
 
    def loop(a: Int, acc: Int): Int = {
        if (a>b) acc else loop(a+1, acc+f(a) )
        }
    loop(a, 0)
}
```
 
### Lecture 2.2: Currying:
 Special form for writing higher-order functions: **Currying**
- `sum` function above: first parameter represents the f(x)
- `a` and `b` get passed **unchanged** to sum()
 ```
 def sumInts(a: Int, b: Int) = {
   sum(x => x, a, b)
 }
 ```

**Functions Returning Functions**
```
-- new implementation of sum: Higher Order Function

def sum(f: Int => Int): (Int, Int) => Int = {
   def sumF(a: Int, b: Int): Int = {
       if (a>b) 0
       else f(a) + sum(f)(a + 1, b)
   }
   sumF
   }

-- directly apply new implementation
sum(x => x*x*x)(1,10)

-- or define new functions to apply later
def sumInts = sum(x => x)
def sumCubes = sum(x => x * x * x)
def sumFactorials = sum(fact)

```


**More concise definition/application: Using Multiple Parameter Lists**

- **We can avoid writing the intermediate function, sumCube** using **multiple parameter groups**
- The function `sum()` takes in a function and also retuns a funtion
- the returned function,`sumF`, applies the given function parameter, f, and sums the results

```
-- more concise definition of sum 

def sum(f: Int => Int)(a: Int, b: Int): Int = {
   if (a>b) 0 else f(a) + sum(f)(a+1,b)
}
```
- The recursive `sum` is called with **two sets of parameter lists** 
- the `sum` function takes in one argument and returns a **partial application** of itself with `f` fixed in the **closure scope**

**Currying**
- **Functions with multiple, n, parameter lists** are equivalent to a function with no parameter list, but whose body consists of n nested anonymous functions
- This style of definition and function application where every function is mapped to an expression consisting of anonymous functions.
- All curried functions return partial applications, but not all partial applications are the result of curried functions.

Note that functional types **associate to the right**
- Int => Int => Int == Int => (Int => Int)


**Some resources on 1) Currying, 2) Partial Function Application, and 3) Multiple Parameter Groups**
- [Currying](https://www.baeldung.com/scala/currying)
    - "Currying is the process of converting a function with multiple arguments into a **sequence of functions** that take one argument.Each function returns another function that consumes the following argument"
- [Partially Applied Functions/Currying](https://alvinalexander.com/scala/fp-book/partially-applied-functions-currying-in-scala/)
- [The Function Environment Problem](https://darrenjw.wordpress.com/2015/11/16/hofs-closures-partial-application-and-currying-to-solve-the-function-environment-problem-in-scala/)
    - common issue, like in the `sum` method, where there is a function which has some additional parameters which need to be fixed before the function can actually be used


**Exercise: Write a product function that calculates the product of the values of a function from the points on a given interval**

```
// Not tail recursive!
def product(f: Int => Int)(a: Int, b: Int): Int = {
   if (a>b) 1
   else f(a) * product(f)(a+1,b)
}
```

**Exercise: Write factorial in terms of product()**
```
def fact(n: Int):Int = {
  if (n==1) 1 
  else product(x: Int => x)(1,n)
}
```

**Exercise: Generalize sum() and product(): mapReduce()**
```
// here, combine is like the reducer
// f is like the map function 

def mapReduce(f: Int => Int, combine: (Int, Int) => Int, zero:Int)(a: Int, b: Int): Int = {
   if (a>b) zero
   else combine( f(a),mapReduce(f, combine, zero)(a+1,b))
}
 
// Redefine product()
def product(f: Int => Int)(a: Int, b: Int): Int = {
   mapReduce(f, (x,y) => x*y, 1)(a,b)
}
 
// Redefine sum()
def sum(f: Int => Int)(a: Int, b: Int): Int = {
  mapReduce(f, (x,y) => x+1, 0)(a,b)
}

```
- `combine` parameter defines how values are combined or reduced in the recursive call
- `zero` paramter defines what value to return in the degenerate case, when the interval is 0
 

### Lecture 2.3: Finding Fixed Points:
 
A number x is called a **Fixed point** of a function if f(x) = x
- for **some functions**, we can locate the fixed points by iteratively applying f to a given initial estimate, x
- x, f(x), f(f(x)),... until the values does not vary anymore, given some epsilon

**Iterative Fixed Point Estimate of Square Root Using "Damping"**
- Note the use of **currying** and **higher order functions**

```
val tolerance = 0.0001

def isCloseEnought(x: Double, y: Double) = {
  abs( (x-y)/x ) / x < tolerance
}

def fixedPoint(f: Double => Double)(firstGuess: Double) = {

  def iterate(guess: Double): Double = {
    val next = f(guess)
    if (isCloseEnough(guess, next)) next 
    else iterate(next)
  }

  iterate(firstGuess)
}

```

**Previously,** we saw that the expressive power of a language increases if we can pass **functions as arguments**
- also the case for **functions that return functions**

**Recall square root**
- sqrt(x) is the fixed point of the function, `f(y) = x/y == y`
  - `y => x / y`
- suggests we can calculate sqrt(x) by iteration towards fixed point
  ```
  def sqrt(x: Double) = {
    fixedPoint(y => x/y)(1.0)
  }
  ```
- **unfortunately**, this does not converge
- **one solution: Average Damping** --> `f(y) => (y + x/y)/2` 
  - note that the technique of **stabalizing by averaging** is general enough to be abstracted into its own function 
  ```
  def averageDamp(f: Double => Double)(x: Double): Double => Double =
  { (x + f(x))/2 }
  ```
  - This takes a function as an input, and returns a function as an output

**Notes on root finding methods (Fixed point iteration)**
- https://briangordon.github.io/2014/06/sqrts-and-fixed-points.html
- https://www.lvguowei.me/post/sicp-goodness-sqrt/
- https://medium.com/@JosephJnk/an-introduction-to-function-fixed-points-with-the-y-combinator-e7bd4d00fb62
- https://www.kimsereylam.com/racket/lisp/2019/02/22/fixed-point-and-newton-method.html

**Write square root using average damp and fixed point**
```
def sqrt(x: Double) = {

  // fixed point takes two args: 1) function 2) initial guess
  // average damp takes function and returns function

  fixedPoint(averageDamp(y => x /y))(1)

}

```


### Lecture 2.5: Functions and Data

Ways to use functions to compose and abstract data: introducing **objects and classes**

**Consider a class for rational numbers**
```
// simple class example

class Rational(x: Int, y: Int) {
  def numer = x
  def denom = y
}


// more complex class, implementing rational arithmetic

class Rational(x: Int, y: Int) {
    require(y != 0, "denominator must be non-zero")

    def this(x: Int) = this(x, 1)

    // automatically simplify fracation upon entry
    private def gcd(a: Int, b: Int): Int = {
        if (b==0) a else gcd(b, a%b)
    }
    private val g = gcd(x,y)

    def numer = x/g
    def denom = y/g

    // method for pretty printing
    override def toString(r: Rational) = {
      r.numer + "/" + r.denom
    }

    // method for adding rationals
    def add(that:Rational) = {
        new Rational(
            numer * that.denom + that.numer * denom, 
            denom * that.denom)
        }

    // alternative for adding, using **symbolic identfier**
    def + (that:Rational) = {
        new Rational(
            numer * that.denom + that.numer * denom, 
            denom * that.denom)
        }

    // method to return the negative of a rational
    def neg(r: Rational) = {
      new Rational(-1*r.numer, r.denom)
    }

    //method to subtract two rationals (add the neg)
    def sub(that:Rational) = {
      this.add(this.neg(that))
    }


    // metod to determine if one rational is less than another
    def less(that:Rational) = {
        numer * that.denom < that.numer * denom
    }

    // methiod for finding max between two rationals
    def max(that:Rational) = {
        if (this.less(that)) that else this
    }
}
```
This definition introduces **Two new entities**
- a new **Type** called Rational
- a new **constructor** called Rational, to create elements of this type 
- note that Scala keeps different names for types and values in different **namespaces**, no conflict between two definitions of **Rational**


**Notes on Objects**
- a **type** is a set of values
- elements of a class type are **objects**
- `val x = new Rational(1,2)` is an object
- **Members of an object**:
  - `x.numer` and `x.denom`
- **Methods**
  - **functions** that are packaged into classes



### Lecture 2.6: More Fun with Rationals

Previous example of `Rational` class did not have a method to simplify the results from the `add` method
- we could call a simplify method after any addition operation
- a better alternative, because it does not necessitate coupling the add method with a simplify whenever the first is called, is to simplify the representation of the class **when object is constructed** 
- Note that in the implementation above `gcd()` is defined as a **private** method, indicating that clients of class, Rational, will not be able to acess this method.  
  
- note that on the inside of a class, **this** represent the object on which the current method is executed. 
- members of a class can also be referenced with `this.` prefix.

**How to prevent users from instantiating irrational numbers, like 1/0**
- note the **Require** predefined function
- if not fulfilled, scala will throw `IllegalArgumentException`
- besides `require` there is also **assert**
  
```
    val x = sqrt(y)
    assert(x>=0)
```
- like `require`, a failing `assert` also throws a an exception, a `AssertionError`
- **difference in intent:** require is a precondition, and assert is a check on the code of the function itself. 


**Constructors**
- a class implicitly introduces a constructor called the **primary constructor**, which:
  - takes paramters of class 
  - executes all statements in class body 
  
Scala can include **multiple constructors** for a class
- note `def this(x: Int) = this(x, 1)` above
- This represents an alternative constructor, which is **utilitzed when an instance of Rational is constructed with only one argument, x**
- when **this** used as a function, indicates a new constructor for the class in addition to primary one. 
  - notice that the new constructor function **calls the primary constructor**

Note that if, in the Rational class, rationals are kept unsimplified internally, and only simplified when rationals are converted to strings. **Do clients observe the same behavior when interacting?**
- yes, for small sizes of denominators and numerators and small numbers of operations. 
- thus, better to simplify internal values as early as possible to alleviate strain on later computations. 


### Lecture 2.7: Evaluation and Operators

Previously defined the meaning of a function application using a **substitution** based computation model, now we extend this model to **classes and objects**
- how is an instantiation of the class `new C(e1,...,em)` evaluated?
- the expression args, `e1,...,em` are evaluated first

**Given, class, C, and method, f:** 
```
class C(x1,...,xm) {
    def f(y1,...,yn = b)
  }
```
**how is `new C(v1,...,vm).f(w1,...,wn)` evaluated?**
- first, the arguments of `f` are substituted by the arguments, `w1,...,wn`
- then, the arguments of the instantation of C, `x1,...,xm`, are substituted with `v1,...,vm` and evaluated
- finally, the reference, `this`, in the function call, `f`, is replaced with the newly instantiated object, `new C(v1,...,vn)`
- resulting in the evaluated version of `f(w1,...,wm)`, with any inner references to the class instance attributes also evaluated

note that evaluation happens in the order of: 
- **1) method parameters** 
- **2) class arguments** 
- **3) class reference in any class methods**


**Infix Notation**
- Scala supports infix notation
- Any method with a parameter can be used like an **infix operator**
- `r add s` same as `r.add(s)`

**Relaxed Identifiers**
- Scala supports both **alphanumeric** and **symbolic** identifiers ("+?%&" or "counter_++" for example)
- If we want to replcae the `neg` method with a `-` prefix, there is an issue. The prefix operator, `-`, is different from the infix operator, `-`, referring to subtraction. Must call it `unary minus`

**Precedence Rules**
- precedence of an operator is determined by its **first** character
- `a+b^?c?^d less a ==> b |c` --> `((a+b)^?(c?^d)) less ((a ==> b) | c)`


**Assignment 2**
This assignment works with a functional representation of sets based on the mathematical notion of characteristic functions. 
- [Link to week 2 code](https://github.com/rivera-lanasm/ScalaFP_Coursera/blob/master/week2/funsets/src/main/scala/funsets/FunSets.scala)


