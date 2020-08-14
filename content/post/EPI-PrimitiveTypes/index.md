---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "EPI (Python) PrimitiveTypes"
subtitle: "Primitive Types"
summary: ""
authors: []
tags: []
categories: []
date: 2020-07-15T09:52:15-04:00
lastmod: 2020-07-15T09:52:15-04:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

### Elements of Programming Interviews (Python) by Adnan Aziz, Tsung-Hsien Lee, and Amit Prakash
This series of posts explores the problems, concepts and my solutions <br>
I'll be posting the descriptions of the solutions as well as the results from the test suite the book authors provide <br>
also will be posting additional problems, wherever relevant, pulled from other places.

## Part 1: Primitive Types

This section primarily focuses on bitwise operations. 
The key concepts addressed are:
  1) Using different bitwise operators, especially XOR
  [more on XOR](https://hackernoon.com/xor-the-magical-bit-wise-operator-24d3012ed821)
  2) Using masks
  3) How to clear the lowermost set bit
  4) Retrieve index of lowermost set bit
  5) Using a cache to accelerate operations
  6) Taking advantage of commutative and associative propoerty of certain bitwise operations to perform operations in parallel

[Other Bitwise Operation Problems](https://medium.com/@codingfreak/bit-manipulation-interview-questions-and-practice-problems-27c0e71412e7)

##  Parity 
 ```
def parity(val: int) -> int:

    par = 0
    while val:
        #? wipe out all bits, except the 1's bit --> flip count
        par ^= val & 1
        val >>= 1
    return par

```
Time complexity --> O(n), n is size of binary word 
procedure must conduct iterations bit by bit

improvement: erasing the lowest set bit in a word upon each iteration 
x&(x-1) --> returns x with it's lowest set positive bit flipped

flipping the par value only as long as some positive bits remain
0 --> even
1 --> odd

New time complexity --> O(k+1), k --> number of positive bits

note, x&~(x-1)

```
def parity(val: int) -> int:
    par = 0
    while val:
        par ^= 1
        val &= (val-1)
    return par

```
computing parity for very large number of bin words
1) processing mult bits at once
2) caching results in an array based lookup table 

caching:
parity of every 64 bit integer --> 2^64 bits of storage
--> compute parity of collection of bits, then recurse

cache --> <0,1,1,0> --> parities of <(0,0), (01), (1,0), (1,1)>

```
def parity_2(val: int) -> int:

    lookup = {v:parity_1(v) for v in range(0xFFFF+1) }
    mask_size = 16
    bit_mask = 0xFFFF

    mask0 = 3*mask_size
    mask1 = 2*mask_size

    g1 = lookup[val>>(mask0)]
    g2 = lookup[(val>>(mask1)) & bit_mask]
    g3 = lookup[val>>(mask_size) & bit_mask]
    g4 = lookup[val & bit_mask]
    return g1^g2^g3^g4
```


"""
Associativity
"""


## === === === === === === === === === === 
## Swapping Bits 



