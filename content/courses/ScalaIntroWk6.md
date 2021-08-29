---
title: "Scala Intro"
subtitle: "Week 6"
date: 2021-03-03T22:47:53-05:00
draft: false
---

#### ================================
**Lecture 6.1: Other Collections**
#### ================================

Other Sequences
- another Seq implementation, **Vector**
- has more evenly balanced access patterns than List
  - recall that lists are **linear**, access to the first elem is faster than access to the middle or end of a list

**Vector**
- essentially, a very shallow tree
- A vector of up to 32 elements is just an array, where the elements are stored in sequence.
  - as the array grows larger, the representation changes, so that each element of the original array points, instead of to a value, to a another array with 32 elements
    - array of level 0: 32 elements
    - array of level 1: 32*32 elements
    - array of level 2: 32* 32 * 32 elements

**retrieval/access performance**
- log(32)n complexity 
- very good random access profile 
- good for **bulk operations**
  - those that traverse a sequence
  - **map, fold**, etc
- **locality** is also better for Vectors than lists
  - more certainty about **cache lines**

**Choice between List and Vector**
- depends on access pattern
- List good for operations with recursive structure
- Vector good for bulk operations like map, filter, or fold 

**Operations on Vectors**
- created analogously to lists
  - `val nums = Vector(1, 2, 3)`
- support same operations as lists, except for `::`, prepend 
- instead of `x :: xs`,
  - `x +: xs` => creates a new vector w leading elem x
  - `xs :+ x` => create a new vector w trailing elem x
- how append elem to vector works: immutable structure
  - copy last array create new array with additional element, in addition to existing array contents
  - add pointers to all children of array 
  - copy parent arrays and point these to new array instead of old one

**Sequence**
- common base class of List and VEctor is Seq, the class of all sequences
- Seq is a subclass of Iterable 
- Set, Seq, Map are subclasses of Iterable 
- Arrays and Strings are Seq like structures, support same operations, but these classes come from Java
- Range: another kind of Sequence
  - to, until and by are its operators
  - `val r: Range = 1 until 5`
  - `val s: RAnge = 1 to 5`
  - `1 to 10 by 3`
  - generator? 

**Seq Operations**
- xs exists p : Boolean
- xs forall p : Boolean
- xs zip ys : List( (xi, yi)) 
- xs.unzip: (List(x1, x2, x3), List(y1, y2, y3))
- xs.flatMap f : 1) appy f to xs, where f yields some collection, 2) concatenate results into a single collection 
- xs.sum, xs.product, xs.max, xs.min 

- Example; All combinations, scalar product, isPrime see test6.scala


#### ================================
**Lecture 6.2: COmbinatorial Search and For-Expressions**
#### ================================

HOw to handle nesterd sequences in combinatorial search problems, **for expressions**, which replace loops in imperative languages 

example: given pos integer n, find all pairs of positive integers, i and j, s.t. i + j are prime, j < i 
- in imperative style, we could use a nested loop and a predicate to test for inclusion of given i and j 
- a functional way: 
  - generate the sequence of all pairs of integers (i, j)
  - filter for pairs which meet predicate 
```
1 until n map (i => 1 until n map( j => (i,j)))

```
- why vector of vectors? IndexedSeq
  - converts ranges to sequence of pairs, so ranges must be coerced to another type
  - IndexedSeq is implemented with Vector

- Combining sub-sequences into a single sequence 
  - can use a foldRight with `++` or `flatten`, equivalents
```
val seq = (1 until n) map (i => (1 until i) map (j => (i,j)))

// method 1, fold
(seq foldRight Seq[Any]())(_ ++ _)

// method 2, flatten 
seq.flatten

```
- recall flatMap
  - rule: `xs flatMap f = (xs map f).flatten
  - `val seq = (1 until n) flatMap (i => (1 until i) map (j => (i,j)))`

- apply predicate 
```
val seq = (1 until n) flatMap (i => (1 until i) map (j => (i,j)))

// isPrime is custom func 
seq filter (pair => isPrime(pair._1 + pair._2))

```

The above is complex, can we simplify? For-Exppression notation 
```
// example
for (p <- persons if p.age > 20) yield p.name
// <- -- 'taken from'

```
- similar to for-loop, but
  - no side effect
  - produces new result with yield 

- For expressions --> `for (s) yield e` 
  - `s` is a sequence of generators and filters
  - value `e` returned by an iteration
- **generator** is of the form p <- e where p is a pattern and e is an expression whose value is a collection 
- can be several generators, wherein last generators vary faster than first ones 

```
// re-write above example using for-expr

for {
  i <- 1 until n
  j <- 1 until i
  if isPrime(i + j)
  } yield (i, j)

```

Re-write scalar-product
```


```

#### ================================
**Lecture 6.3: N-Queens Problem**
#### ================================

- Recall the Iterabel class hierarchy from before, here exploring `Set`
- most operations on sequences also in sets 
- Differences:
  - sets are unordered
  - sets do not have dupl. elems
  - fundamental operation on sets is `contains`
    - whereas for `seq` is head, tail, or `indexing` for vector

(N-Queens Problem)[https://en.wikipedia.org/wiki/Eight_queens_puzzle]
- place 8 queens on a chess board (8x8) s.t. no queen is threatened by another 
- generalize to nxn board

**Recursive Solution; placing k queens on an n x x board**
- suppose we have already placed k-1 queens
- each solution represented by a list of len k-1, containing numbers of columns corresponding to each queen in rows 0 to n-1
  - col number of queen in row k-1 comes first in list, followed by col number of queen in row k-2, etc. 
  ```
  https://en.wikipedia.org/wiki/Eight_queens_puzzle
  // 8 queens, 8 by 8 board 
  // index by 0, start row and col enumeration from top-right
  
  // start with col indexes of first k-1 (7) queens in rows 0-(k-1)
  [4, 1, 7, 0, 6, 3, 5]

  // complete to full solution by adding col placemenet of the kth (8th) queen in nth (8th) row
  [2, 4, 1, 7, 0, 6, 3, 5]

  ```
- Illustrates method of using a tree to find the solution for k-1 queens, k-2, etc. as a way to build solution set for k queens
- the solution set for k queens **contains** solution set for k-1, k-2, etc. 


#### ================================
**Lecture 6.4: Maps**
#### ================================

**Associative Maps**
- both iterables, as well as functions 
- defined with two type parameters, `Map[Key, Value]`
- iterables:
  - Class Map[Key, Value] extends the collection type, Itreable[(Key, Value)]
  - supports collections operations of key/value pairs --> (key, value), like a python dict
- functions
  - Class Map[Key, Value] also extends the function type Key => Value
  - maps can be applied to key arguments

**Option Type**
- can use map to query using func invocation or with a `get` method
  - if no corresponding val in map found, returns Option type value equal to None, else returns an OPtion(Type) type with value equal to Some(Type)
- Option type defined as a `trait`
  - takes type parameter
- contains case class `Some`, which extends OPtion[A] and object `None`, which extends Option[Nothing]
  
**Decomposing Option**
- since Option defined using **case-class**, can **take advantage of pattern matching**
```
val romanNumerals = Map("I" -> 1, "V" -> 5, "X"->10)

def showNumeral(symbol: String) = romanNumerals.get(symbol) match {
    case Some(value) => value 
    case None => "no corresponding numeral"
    }

```

**Sorted and Groupby**
- orderBy
  - `fruit sortWith (_.length < _.length)` --> length of fruits on the left is less than those on the right
- groupBy
  - partitions collection into a **map of collections** according to a **discriminator function, f**
  - `fruit groupBy (_.head)`
  
**polynomial**
- a map from exponents to coefficients 
- can express polynomials as maps
- test6_3

**Default Values**
- so far, maps have been treated like **partial functions**
- `withDEfaultVAlue` turns a map into a total function 
  - `val cap1 = capitalOfCounty withDefaultValue "<unknown>"`

**Repeated Function Parameters**
- auxillary constructor,
  - `def this(bindings: (Int, Double)*) = this(bindings.toMap) // toMap converts sequence to map`

**Fold left vs. concatenation**
- FL more efficient



#### ================================
**Lecture 6.5: Apply Immutable Collections --> Telphone Numbers encode as Sentences**
#### ================================

design method, `translate`, such that a given phone number, such as 7225247386, can be translated into corresponding sentence, pulled from a dictionary of words, such as "scala is fun"


#### ================================
**Lecture 6.6: Conclusion; Further Questions**
#### ================================

- FP Program design principles
- FP and mutable states: what does it mean to have mutable states in FP
- Parallel and Distributed systems: explit immutability for parallel execution and distributed collections


#### ================================
**Excercise; Anagrams**
#### ================================

Your ultimate goal is to implement a method `sentenceAnagrams`, which, given a list of words representing a sentence, finds all the anagrams of that sentence. 

Note that we used the term meaningful in defining what anagrams are. You will be given a dictionary, i.e. a list of words indicating words that have a meaning.

Here is the general idea. 
- We will transform the characters of the sentence into a list saying how often each character appears. We will call this list the occurrence list. 
  
- To find anagrams of a word we will find all the words from the dictionary which have the same occurrence list. 
  - group by 

- Finding an anagram of a sentence is slightly more difficult. We will transform the sentence into its occurrence list, then try to extract any subset of characters from it to see if we can form any meaningful words. From the remaining characters we will solve the problem recursively and then combine all the meaningful words we have found with the recursive solution.

