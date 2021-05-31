---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "538 Robopizza"
subtitle: ""
summary: ""
authors: []
tags: []
categories: []
date: 2020-05-20T12:42:10-04:00
lastmod: 2020-05-20T12:42:10-04:00
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
## Riddler #1; What if Robots Cut Your Pizza?

This Riddler puzzle is about cutting a circle at random points and understanding how many slices are likely to result. 

"At RoboPizza™, pies are cut by robots. When making each cut, a robot will **randomly (and independently) pick two points** on a pizza’s circumference, and then cut along the chord connecting them. If you order a pizza and specify that you want the robot to make exactly three cuts, **what is the expected number of pieces** your pie will have?" (my highlighting)

I am borrowing the theory behind the solution from Northeastern professor of mechanical and industrial engineering, Laurent Lessard. Drawing from his post about [this same problem](https://laurentlessard.com/bookproofs/what-if-robots-cut-your-pizza/)

#### =============================


## Reframing the Problem 

Lessard reframes the question using Euler's Formula for Simple Planar Graphs. A graph G is planar if it can be drawn in the plane in such a way that no two edges meet each other except at a vertex to which they are both incident, i.e. such that no two edges cross in the plane. 

Euler's formula relates the number of vertices, edges and faces (including the "infinite" outside face) of a planar graph. If n, m, and f denome the number of vertices, edges, and faces respectively of a connected planar graph, then:

$ n - m + f = 2 $

Given some slices across the pizza resulting in a circle with `n` chords, `p` intersections and `s` regions (or slices), Lassard applies Euler's formula as follows:
- $V = 2n + p$
- $E = 3n + 2p + 1$
- $S = n + p + 1$ 
- $F = S + 1 (inf. face)$
$n$ is given by the problem, so we're only solving for p, the number of intersections given $n$ chords.

**References:**
[Graph Planarity](http://www.personal.kent.edu/~rmuhamma/GraphTheory/MyGraphTheory/planarity.html)

#### ================================

## Estimating the number of intersections given n "randomly" drawn chords

Lessard introduces some notation I found quite useful to think about the problem, so will reiterate here. Given n chords drawn, the value $X_{i,j}$ is equal to 1 if chords $i$ and $j$ intersect, and 0 otherwise. $C$ is the set of all pairs $(i,j)$. 

- The values $X_{i,j}$ are not mutually independent. This can be shown by imagining a circle with three chords, $a, b, c$, two ($a, b$) of which are parallel. If $X_{a,c} = 1$, it must be the case that $X_{i,j}$. However, lack of independence does not affect linearity of expectations. 
- By symmetry, each $E[X_{i,j}]$ is equal and equivalent to 1/3.


**Pizza cutting process in two stages:** 
- First, assign $2n$ points along the circumference of the circle.
- Then, assign labels and draw chords between points. In determining the number of intersecting points, **the key randomizing event is the ordering of labels (and by extension the assignment of pairings), not their position along the circumference of the circle.**

By enumerating the different possible ways of pairing points on a circle given an number of chords to be drawn, one can deduce the probability of acheiving between 0 and n choose 2 inctersections intersection. 

This counting exercise becomes increasingly difficult as the number of chords involved increases, so Laurent introduces some general solutions. 


##### =======================================

### Aside: Bertrand's paradox: Assigning uniform/IID properties to simulated events

In analyzing the specific case from the problem, that of estimating the expected number of slices (regions) resulting from three slices (chords) across the pizza (circle), Larant points out the following probabalistic insights:
- The likelihood that two chords will begin or end at the exact same point along the circumference of the pizza is 0, allowing for arbitarily small pizza slices. 
- Any point of intersection will involve exactly two chords. The probability of the case involving an intersection of three or more chords is equivalent to the probability of one end of the 3rd (and subsequent) occuring at an arbitrary points along the circle's circumference, which is 0.

A key insight from Laurent above is the the approach to introducing randomness. Assigning the order of labels among the points along the circle works because the way the chords are chosen **decouples how endpoints are chosen from how labels are assigned**. In the later section on simulation based methods, this is essential for safely estimating the probability distribution of the number of intersections. 

The question of properly specifying the process of randomly generating chords is related to **Bertrand's Paradox**.


### Bertrand's Paradox: Probability that 2 chords intersect


https://www.cut-the-knot.org/bertrand.shtml

https://www.cut-the-knot.org/bertrand2.shtml




###### ===================================

### Analytic Solution 

Laurent draws a general solution for the expected number of intersections in a circle given n chords from a 1975 paper, [“The Distribution of Crossings of Chords Joining Pairs of 2n Points on a Circle”](https://www.ams.org/journals/mcom/1975-29-129/S0025-5718-1975-0366686-9/S0025-5718-1975-0366686-9.pdf), by John Riordan, who specialized in combinatorial analytics. 

As an aside, **Analytic Combinatorics** aims at predicting precisely the asymptotic properties of structured combinatorial configurations, through an approach that bases itself extensively on analytic methods. Generating functions are the central objects of the theory
[reference](https://lipn.univ-paris13.fr/~nicodeme/nablus14/nafiles/gentle.pdf)

The key takeaway, as Laurent points out, is the following generating function, Tn(x). 



Riordan derives a generating function for this polynomial:

This generating function yields a polynomial with coefficients describing "the number of ways of choosing n pairs of points among 2n  general points **such that there are k intersections**"


### Ballot Problem and Catalan Numbers

Laurant highlights that Riordan makes use of a ballot number, anm, in this generating function. Laurent offers the geometric  interpretation is that amn "is the number of “North-East” lattice paths from (0,0) to (n,m) that do not cross the diagonal points (i,i)".

Pointing back to Riordan's generating function, Laurent points out that for each Tn(x), extracting the first coefficient, representing the number of ways of choosing n pairs among 2n general points such that there are 0 intersections, yields the **Catalan numbers**, which appear in a variety of combinatorial problems. 

The ballot number here is central to determining the value of coefficients, so I thought it was worth devling more into exactly how to interpret it. I draw the following from [a paper by Peter Hilton and Jean Pedersen](http://www.math.uakron.edu/~cossey/636papers/hilton%20and%20pedersen.pdf)

Starting with the binomial coefficient, (n r), the authors stress one particular interpretation: that of paths on a coordinate plane. A path is a sequence of points on a coordinate plane, where each point with positive integer coordinates.

the authors show that the number of paths from (0,0) to (a,b) is equivalent to the binomial coefficient, (a+b, a). This simply means that given a coordinate (a,b), where a and b are positive integers, the path to (a,b) can be thought of set of steps, either along the x or y axis. The total number of paths is equivalent to the number of ways to assign the steps along the x-axis to the set of total steps, so can be phrased as the value (a+b a)


[paper](https://sms.math.nus.edu.sg/smsmedley/Vol-25-2/Catalan%20numbers%20(Wong%20Kar%20Lyle).pdf)
The ballot problem is 

The connection drawn to Catalan numbers here is the observation in 1887 by French mathematician Desire Andre called the "Reflection Principle", which 


Connecting to chord intersection problem to Catalan numbers, a related problem. 
[post](https://math.stackexchange.com/questions/284512/proof-of-catalan-numbers-on-a-circle)


Riordan genearlizes the ballot sequence solution, drawing on work done by Touchard, 




##### =========================================== 




```python
import pandas as pd
import numpy as np
import seaborn as sns

order_four_dist = pd.read_csv("distribution_order_4.csv", header=None).reset_index()
order_four_dist.columns = ['intersections', 'likelihood']
# incorporate into scala
order_four_dist['regions'] = order_four_dist['intersections'] + 4 + 1
```


```python
sns.barplot(x = "regions", y="likelihood", data=order_four_dist, color="steelblue")
```



![png](/img/538_robopizza_pic0.png)


##### ============================================

### Simulation based/Empirical Solutions 

1) random parings; test for point in region


2) random pairings; test for intersection 

https://stackoverflow.com/questions/6270785/how-to-determine-whether-a-point-x-y-is-contained-within-an-arc-section-of-a-c




```python

```

##### ============================
### Limit of Distribution

[paper](http://algo.inria.fr/flajolet/Publications/FlNo00.pdf)


https://github.com/dingran/latex-ipynb/blob/master/latex-cheatsheet.ipynb

https://www.cs.upc.edu/~conrado/docencia/udelar-course-2011.pdf


```python

```
