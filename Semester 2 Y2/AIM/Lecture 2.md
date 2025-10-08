## Introduction to more COPs
### Bin Packing Problem
Goal: Pack objects of different sizes into finite number of bins in most efficient way, ensuring total volume of objects in each bin does not exceed its fixed-capacity. Formally: 
	Given a set of items $I$, $|I| = n$, containing items of various sizes $s_x$, and an unlimited number of bins $a_x$, each with fixed capacity V, where $0 \le x \le n$. 
	What is the minimum number of bins required to pack all items without exceeding capacity of any of the bins.
### Travelling salesman problem
Formally: 
	Given set of cities $C$ of size $N$, and distances between each pair of cities $d(i, j), \forall i \in C j \in C$
	What is the shortest possible route that visits each city exactly once and returns to the origin city? 
### 0/1 Knapsack problem
Formally: 
	Given a knapsack with a capacity C and a set of items $I$, $n = |I|$, where each item has a weight $w_x$ and a value $v_x$, $0 \le x \le n$
	Which items should be packed into knapsack to **maximise** the total value of items in the knapsack without exceeding the capacity?
### Boolean satisfiability problem (SAT)
First problem to be proven to be NP-complete.
Formally: 
	Given formula $\varphi$ of $n$ boolean variables $x_i$, where $0 \le i \le n$ in conjunctive normal form (CNF). 
	Is there an assignment of true/false values to each boolean variables such that $\varphi$ is satisfied (true)? 
This is a problem which we call a decision problem, as per the objective of this problem. 
#### Decision problems vs COPs
##### Decision problems
Seek existence of satisfaction to a problem, the output is a binary decision. Depending on the problem, can be easier to solve than a COP. 

##### COPs
Seek best solution to a problem
Output is an objective value or a complete solution. More complex usually as looking for optimal solution among a finite set of distinct possibilities. 

### Maximum satisfiability problem
The combinatorial optimisation problem variant of the SAT.
Formally:
	Given a formula $\varphi$ of $n$ boolean variables $x_i$, where $0 \le i \le n$, in conjunctive normal form.
	What is the greatest number of clauses that can be satisfied in $\varphi$?

![[Pasted image 20250217012443.png]]


## Main components of heuristic search methods
### Representations
> A representation is a way to encode a solution to a given problem.
> Specific to each problem. 

How we structure and encode search space, solutions and states in heuristic search methods.

#### Characteristics of representations
- **Completeness** - a solution encoding is complete if it is able to represent all possible solutions to the problem
- **Connexity** -  a solution encoding is connex if for all candidate solutions there exists a search path between itself and all other possible solutions in the search space.
	- Ie not possible to get to another region of a search space after starting in one neighbourhood. 
- **Efficiency** - a solution encoding is efficient if it is fast and easy to manipulate it. 

#### Binary (bitstring) representations
Typically used when each variable is a binary/boolean decision.
Each bit in the representation represents each of the $n$ variables being either True or False. Search space is $2^N$
Most common representation and can be generalised for most problems.


##### Example : 0/1 knapsack problem
![[Pasted image 20250217013902.png]]
Can use binary representation to encode the following solutions:
- 10011 : V = 8, W = 15
- 11110 : V = 15, W = 11

##### Example: MAX-SAT problem 
![[Pasted image 20250217021709.png]]

#### Permutation representation
Typically used when searching for a sequence of events, to preserve the order of elements. Each entity in the permutation has an index which represents the order, with each entity/element appearing exactly once. 
N! search space

##### Example: TSP
![[Pasted image 20250217014237.png]]
![[Pasted image 20250217014241.png]]
![[Pasted image 20250217014253.png]]
or c0->c1 -> c2 -> c3 -> c4 etc


#### Integer encoding
Use integers instead of binary or continuous values. Each integer could represent a type of composite material used in an N-layer composite structure. Search space is $M^N$ where N is length of representation (number of layers) and M is the number of choices. 

Useful for resource allocation problems

##### Example: Structure
Given a set of composite materials $C$, $|C| = M = 5$ and a structure containing 8 layers.

Can use integer encoding to represent the following solutions:
<11345532>
<52312315>


#### Value encoding
Encodes solutions using real (continuous) numbers. Generally used for continuous optimisation. Can be used for others such as DNA sequencing and planning. Search space dependent.

E.g., fractional knapsack problem, where we can take portions of a particular item. 


#### Non-linear encodings 
Non-linear representation is where relationships between variables are non-linear e.g., doing something with a increases something to do with b. The potential components of a solution interact with each other, and the search space is not linear ie solutions do not follow straightforward pattern. E.g., tree encoding. Used in genetic programming. 




### Neighbourhoods
A neighbourhood of a solution is a set of solutions that can be reached from a solution by applying an (operator/move operator/heuristic)

$N$ defined as ±$\delta$, means small interval around a point, given as:
$$ N(x_0) = {x | x \in [x_0 - \delta, x_0, + \delta]} $$
This is the neighbourhood of $x_0$
![[Pasted image 20250216214506.png]]

#### Example neighbourhood for binary represented solutions
- A bit flip operator flips a bit in a given solution
- The hamming distance between two bit strings of equal length is the number of positions  at which the respective bits/symbols differ.
	- E.g., HD(00110, 00111) = 1
- For a binary string of length $n$ its neighbourhood size is $n$, given $n$ possible single bit flips and the hamming distance is $1$
	- E.g., $n$ = 5 and solution 00110, neighbourhood consists of 5 solutions
	- - **10110** (flipping 1st bit)
	- **01110** (flipping 2nd bit)
	- **00010** (flipping 3rd bit)
	- **00100** (flipping 4th bit)
	- **00111** (flipping 5th bit)
	- When considering hamming distance of size 1 this is. 
#### Example Neighbourhood for integer and value represented solutions 
A random (perturbation/move/assignment) operator replaces a discrete value with another from a given alphabet $\Sigma$ 
	Example: given $\Sigma$ = {0, 1, 2, 3} and X = 0023 -> 2023
For a solution of size $n$ and an alphabet of size $k$ the neighbourhood size is (k-1)n. 
	Example., above have $n$ = 4 and $k$ = 4 hence 3 x 4 = 12


#### Example Neighbourhoods for Permutation Represented Solutions
Many operators to define different neighbourhoods:
- Adjacent (pairwise interchance/swap)
- Insertion operator
- Exchange operator
- Inversion operator

##### Adjacent pairwise interchange
Swaps two adjacent entries in the permutation:
	 Example: 51432 -> 15432
For a permutation of size n, the neighbourhood size is n - 1.
Hamming distance is always 2. 

##### Insertion
Insertion operator removes an entry from the permutation and inserts it into a different position
	Example:![[Pasted image 20250217023438.png]]
For permutation of size $n$, the neighbourhood size is ($n - 1)^2$
Hamming distance is dependent on the number of places an item is shifted, $2 \le HD \le n$
Subsumes adjacent swap.


##### Exchange
Exchange operator swaps two arbitrarily, but different, selected entries
	 Example: ![[Pasted image 20250217023456.png]]
For a permutation of size $n$, the neighbourhood size is $n(n - 1) / 2$
Hamming distance is always 2.
Subsumes adjacent swap.


##### Inversion 
Inversion operator reverses the entries between 2 arbitrarily selected start and end points
	 Example: ![[Pasted image 20250217023621.png]]
- For a permutation of size $n$, the neighbourhood size is $n(n - 1) / 2$
- Hamming distance is dependent on the selected start and end points $2 \le HD \le n$
- Subsumes adjacent swap.

### Evaluation/Objective functions
An **evaluation function** indicates how good a given solution is, distinguishing it between one that is objectively better or worse.

(note, may be referred to as objective/cost/fitness/penalty function)

Extremely important for the search process by providing feedback on the quality of the solution. 
- Help select best candidate from a neighbourhood of solutions. 
- Can be used to guide the search to promising regions.

Different types exist: 
- Separable vs non-separable
	- Can be broken down into independent sub functions vs cannot be broken into independent sub-functions.
	- Makes harder to optimise if the latter. 
- Unimodal vs multi-modal (number of global optima.)
- Single-objective and multi-objective
	- Optimising single criterion vs multiple criteria, potentially conflicting.
Could be computationally expensive themselves.
- Exact models(precise evaluations) vs surrogate models(approximations to reduce computational time) 

##### Evaluation function for MAX-SAT
Given the total number of clauses $n$ and the total number of satisfied clauses $C_{SAT}$, can design many evaluation functions:
- $f1 = C_{SAT}$
- $f2 = n - C_{SAT}$
- ...
These can be designed for maximisation or minimisation.
Example: 
![[Pasted image 20250217024838.png]]


##### Example for TSP
Given a permutation of {0,..., $n - 1$} denoted $\pi$ and a cost matrix C of dimension $N$ x $N$ where $c_{i,j}$ denotes a known distance between cities $i$ and $j$, and $\pi(i)$ denotes the city at the i-th locationr
Evaluation function for minimising the total path length: $$ f(\pi, C) = \sum_{i=0}^{N-1} c_{\pi(i),\pi((i+1)\bmod N)}$$
Add up distances between each consecutive city visiting in tour, including return to starting city via Mod N. 

Distance calculation between cities
$$c_{i,j} = \sqrt{(x_i - x_j)^2 + (y_i - y_j)^2}$$
(pythagorean theorem)
Overhead of squaring and square root. 


#### Delta evaluation
Delta evaluation is a technique to improve computation efficiency by only computing the change in the objective function, rather than the entire function from scratch. 

(FROM LEC)
...calculates the differences between the current search position s, and a neighbouring solution, s' on the evaluation function value.

Crucial for an efficient implementation of heuristics/metaheuristics/hyper-heuristics. 


#### Example
CBA



### Hill Climbing algorithms
A hill climbing algorithm is a **local search** algorithm which constantly moves in the direction of (decreasing/increasing) objective value for a given (minimisation/maximisation) problem to find the (lowest/highest point) in the search landscape.

1. Pick an initial starting point in the search space
2. Repeat
	1.  Consider the neighbour(s) of the current solution as new points
	2. Compare the new points using an evaluation function against the current solution, choosing the best solution among them
	3.  Move to the new point.
3. Until a termination criteria satisfied e.g., num of iterations
4. Return the current point as best solution found so far

Initial starting point can be chosen:
- Randomly (stochastic) (perturbative)
- Using a constructive heuristic
Often differ based on how new point(s) is/are chosen

#### Some hill climbing heuristics
- Stochastic hill climbing
	Randomly sample the neighbourhood of solutions
		Random mutation/random selection hill climbing
		![[Pasted image 20250217032220.png]]
- Simple hill climbing 
	Have strategies for systematically sampling neighbours of a solution
		First improvement/next descent/ascent vs Best improvement/steepest descent/ascent.
		![[Pasted image 20250217032245.png]]
		![[Pasted image 20250217032258.png]]

- Random restart
	Randomly changes the starting solution at times. 


#### When to stop
If the target objective value is known, we can stop if this is achieved


#### Strengths of hill climbing methods
Very easy to implement, requiring only:
- Solution representation
- Evaluation function
- A way to define the neighbourhood of a solution]

#### Weaknesses of hill climbing methods
- Local optimum: get stuck as all neighbouring solutions have a worse (or same) quality, so no guarantee will find global optimum
- Plateau/neutral space/shoulder: all neigbouring states have the same quality
- Ridge/valley: a combination of local optimum and plateau; the seach explores solutions along the valley but is unable to escape
- No information about how much a local optimum deviates from global optimum
- No upper bound on computation time
- Success/failure determined based on starting point. 