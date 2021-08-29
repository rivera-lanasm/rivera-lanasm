---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "538 Riddler: Robopizza"
subtitle: "(some parts still in progress)"
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

This [Riddler puzzle](http://fivethirtyeight.com/features/what-if-robots-cut-your-pizza/) is about cutting a circle at random points and understanding how many slices are likely to result. 

"At RoboPizza™, pies are cut by robots. When making each cut, a robot will **randomly (and independently) pick two points onf a pizza’s circumference, and then cut along the chord connecting them**. If you order a pizza and specify that you want the robot to make exactly three cuts, **what is the expected number of pieces** your pie will have?" (my highlighting)

I am borrowing the theory behind the analytic solution from a similar post by Northeastern professor of mechanical and industrial engineering, Laurent Lessard. Link here to his post about [this same question](https://laurentlessard.com/bookproofs/what-if-robots-cut-your-pizza/)


### Reframing the Problem 

Lessard's key insight for my approach was to reframe the question using Euler's Formula for Simple Planar Graphs. A graph G is planar if it can be drawn in the plane in such a way that no two edges meet each other except at a vertex to which they are both incident, i.e. such that no two edges cross in the plane. 

Euler's formula relates the number of vertices, edges and faces (including the "infinite" outside face) of a planar graph. If V, E, and F denote the number of vertices, edges, and faces respectively of a connected planar graph, then:

$ V - E + F = 2 $

Lassard applies Euler's formula as follows to express the number of vertices, edges and faces of the resulting planar graph in terms of the number of cuts across the pizza, `n`, `p` intersections and `s` regions (or pizza slices):
- $V = 2n + p$
  - Number of vertices equal to 2 times number of cuts across pizza (the vertices along the circumpherence) plus the number of intersections among unique pairs of cuts (p)
- $E = 3n + 2p $
  - Each internal intersection adds an edge to the graph, as well as the arcs between cuts along the circumference.
- $F = s + 1 (inf. face)$
  - The number of faces is equal to the number of distinct regions in the circle plus the outside (inf.) face
Substituting this all into Euler's formula:
  - $ V - E + F = 2 $
  - $ (2n + p) - (3n + 2p) + (s + 1) = 2 $
  - $ -n - p + s + 1 = 2 $
  - $ s = p + (n + 1) $  

$n$ is given by the problem, so we're only solving for $p$, the number of intersections given $n$ chords (or cuts across the pizza)

**References:**
[Graph Planarity](http://www.personal.kent.edu/~rmuhamma/GraphTheory/MyGraphTheory/planarity.html)


## Estimating the Expected Intersections given N Randomly Drawn Chords

Given $n$ chords drawn, the random variable or event $X_{i,j}$ is equal to 1 if chords $i$ and $j$ intersect, and 0 otherwise. $C$ is the set of all pairs $(i,j)$. 

The outcomes of values c are not mutually independent. This can be shown by imagining a circle with three chords, $a, b, c$, two, $a$ and $b$, are parallel. If $X_{a,c} = 1$, it must be the case that $X_{b,c}$. 
- However, importantly, lack of independence does not affect linearity of expectations

To find the likelihood of a given event, $X_{i,j}$, we can draw on the wording of the problem statement: 
> "randomly (and independently) pick two points on a pizza’s circumference, and then cut along the chord connecting them".
This statement is important because it specifies the method for randomly selecting chords. Drawing out the distinct ways chords can be drawn in this way, you can show that the likelihood of intersection is 1/3. 

Had the method here been specified differently, the outcome likelihood of intersection, and thus the outcome expected number of slices, would also be different. 
  - I'll elaborate on this in the last section on **Bertrand's Paradox**


**References:**
[Linearity of Expectations](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-042j-mathematics-for-computer-science-fall-2005/readings/ln14.pdf)


### General Empirical Solution: 

**Pizza cutting process in two stages:** 
- First, assign $2n$ points along the circumference of the circle.
- Then, assign labels and draw chords between points. In determining the number of intersecting points, **the key randomizing event is the ordering of labels (and by extension the assignment of pairings), not their position along the circumference of the circle.**

By enumerating the different possible ways of pairing points on a circle given an number of chords to be drawn, one can deduce the probability of acheiving between 0 and n choose 2 inctersections intersection. 

This counting exercise becomes increasingly difficult as the number of chords involved increases, so I introduce here a simulation based method for estimating the number of resulting pizza slices:

- First, 

You can see the code for this [here]()

### General Analytic Solution 

Laurent draws a general solution for the expected number of intersections in a circle given n chords from a 1975 paper, [“The Distribution of Crossings of Chords Joining Pairs of 2n Points on a Circle”](https://www.ams.org/journals/mcom/1975-29-129/S0025-5718-1975-0366686-9/S0025-5718-1975-0366686-9.pdf), by John Riordan, who specialized in combinatorial analytics. 

As an aside, the field this solution draws from, **Analytic Combinatorics**, aims to understand the asymptotic properties of structured combinatorial configurations, through an approach that bases itself extensively on analytic methods. Generating functions are the central objects of the theory
[reference](https://lipn.univ-paris13.fr/~nicodeme/nablus14/nafiles/gentle.pdf)

This generating function ahieved by the paper by Riordan yields a polynomial with coefficients describing "the number of ways of choosing n pairs of points among 2n  general points **such that there are k intersections**". 

I implemented a set of Scala methods using this generating function to reach the analytic solutions for this question, as well as to compare against the empirical solutions I described above. 

You can see the code for this [here]()


**References:**
[Ballot Numbers](http://www.math.uakron.edu/~cossey/636papers/hilton%20and%20pedersen.pdf)
[Cataland Numbers](https://sms.math.nus.edu.sg/smsmedley/Vol-25-2/Catalan%20numbers%20(Wong%20Kar%20Lyle).pdf)
[Catalan Numbers and Chord Intersection](https://math.stackexchange.com/questions/284512/proof-of-catalan-numbers-on-a-circle)


### Results: Empirical vs. Analytic Solution

see Scala code for implementing the empirical and analytic solutions [here](https://github.com/rivera-lanasm/RiddlerSolutions/tree/main/solutions/src/main/scala/robopizza)

**Resulting Distributions of Chord Intersections from 3 Slices**
![png](/img/robopizza_empirical_3slice.png)

**Resulting Distributions of Chord Intersections from 6 Slices**


**Resulting Distributions of Chord Intersections from 20 Slices**


### Aside: Bertrand's paradox: Assigning uniform/IID properties to simulated events

In analyzing the specific case from the problem, that of estimating the expected number of slices (regions) resulting from three slices (chords) across the pizza (circle), Larant points out the following probabalistic insights:
- The likelihood that two chords will begin or end at the exact same point along the circumference of the pizza is 0, allowing for arbitarily small pizza slices. 
- Any point of intersection will involve exactly two chords. The probability of the case involving an intersection of three or more chords is equivalent to the probability of one end of the 3rd (and subsequent) occuring at an arbitrary points along the circle's circumference, which is 0.

A key insight from Laurent above is the the approach to introducing randomness. Assigning the order of labels among the points along the circle works because the way the chords are chosen **decouples how endpoints are chosen from how labels are assigned**. In the later section on simulation based methods, this is essential for safely estimating the probability distribution of the number of intersections. 

The question of properly specifying the process of randomly generating chords is related to **Bertrand's Paradox**.

