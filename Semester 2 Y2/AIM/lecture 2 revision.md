# Combinatorial optimisation problems
## Bin Packing problem
> **Given a set of items $I$, $|I| = n$, containing items of various sizes $s_x$, and an unlimited number of bins $a_x$, each with a fixed capacity $C$, where $0 \le x \le n$** 
> 
   **What is the minimum number of bins required to pack all items, without exceeding capacity of any of the bins.**
 X acts as index.
## Travelling salesman problem
> **Given a set of cities $C$ of size $N$ and distances between each pair of cities $d(i, j) \forall i, j \in C$** 
   **What is the shortest possible route that visits each city exactly once and returns to the origin city?** 
## 0/1 Knapsack problem
> **Given a knapsack with a capacity $C$ and a set of items $I$, $n = |I|$ where each item has a weight $w_x$ and a value $v_x$, $0 \le x \le n$** 
> 
   **Which items should be packed into the knapsack to maximise the total value of the items in the knapsack without exceeding the capacity.**
## Boolean satisfiability problem
> **Given formula $\varphi$ of $n$ boolean variables $x_i$, where $0 \le i \le n$ in conjunctive normal form (CNF).** 
> 
   **Is there an assignment of true/false values such that  $\varphi$ is satisfied (true)?**
## Maximum satisfiability problem
Combinatorial optimisation problem variant of SAT.
> **Given a formula of  $\varphi$ of $n$ boolean variables $x_i$, where $0 \le i \le n$, in conjunctive normal form.**
> 
   **What is the greatest number of clauses that can be satisfied in $\varphi$?**

# Decision problem vs COPs
## Decision problem
Seek existence, satisfaction to problem. Output is binary decision. Depending on problem, easier to solve.

## COPs
Seek best solution to a problem. Output is objective value or complete solution. More complex, usually looking for optimal solution among finite set of distinct possibilities. 


# Main components of heuristic search methods
## Representations
> Way to encode solution to given problem. Specific to each problem.

How we structure and ecncode search space, solutions and states in heuristic search methds.

### Characteristics of representations
- **Completeness** - solution encoding is complete if it is able to represent all possible solutions to a problem
- **Connexity** - a solution encoding is **connex** if for all candidate solutions, there exists a search path between itself and all other possible solutions in the search space. 
- **Efficiency** - solution encoding is efficient if it is fast and easy to manipulate.
### Binary/Bitstring representation
- Used when each variable is binary/boolean decision. 
- Each bit in representation, represents each of $n$ vairbales being either True or False. Search space is $2^n$
- Most common representation, can be generalised for most problems. 
#### Example: 0/1 Knapsack problem
![[Pasted image 20250217013902.png]]
Can use binary representation to encode the following solutions:
- 10011 : V = 8, W = 15
- 11110 : V = 15, W = 11
ALSO
![[Pasted image 20250217021709.png]]


### Permutation representation
- Typically used when searching for sequence of events, to preserve order of elements. 
- Each entity in permutation, has an index when represents the order, each element/entity appearing exactly once.  
- N! search space.
#### Example: TSP
![[Pasted image 20250217014237.png]]

![[Pasted image 20250217014241.png]]

![[Pasted image 20250217014253.png]]

### Integer encoding
Use integers instead of binary or continuous values. Each integer could represent a type of composite material used in an $N-Layer$ composite structure. Search space is $M^N$ where $N$ is length of representation (number of layers) and $M$ is the number of choices.

**Useful for resource allocation problems**

##### Example: Structure
Given a set of composite materials $C$, $|C| = M = 5$ and a structure containing 8 layers.

Can use integer encoding to represent the following solutions:
<11345532>
<52312315>

### Value encoding
Encodes solutions using real, continuous numbers. Generally used, continuous optimisation. Can be used for others such as DNA sequencing and planning.

E.g., fractional knapsack problem, where can take portions of a particular item. 
### Non-linear encoding
Non-linear representation, where relationships between variables are non-linear. Potential components of a solution interact with each other, and search space is not linear i.e., do not follow straightforward pattern. E.g., tree encoding, genetic programming.

## Neighbourhoods and neighbourhood operators
> A neighbourhood of a solution is a **set** of solutions that can be reached by applying a corresponding neighbourhood operator/heuristic.

$N$ is defined as ±$\delta$, means small interval around a point, given as:
$$ N(x_0) = {x | x \in [x_0 - \delta, x_0, + \delta]} $$
This is the neighbourhood of $x_0$
![[Pasted image 20250216214506.png]]

### Example neighbourhood for binary represented solutions
- A bit flip operator, flips a bit, given solution
- Hamming distance between two bit strings of equal length is the number of positions at which the respective bits differ. 
	- HD(00110, 00111) = 1
- For a binary string of length $n$ its neighbourhood size is $n$ given $n$ possible single bit flips and the hamming distance is $1$. 
	- E.g., $n$ = 5 and solution 00110, neighbourhood consists of 5 solutions
	- - **10110** (flipping 1st bit)
	- **01110** (flipping 2nd bit)
	- **00010** (flipping 3rd bit)
	- **00100** (flipping 4th bit)
	- **00111** (flipping 5th bit)
	- When considering hamming distance of size 1 this is. 
### Example neighbourhood for integer and value represented solutions
A random (perturbation/move/assignment) operator replaces a discrete value with another from a given alphabet $\Sigma$ 
	given $\Sigma = \{0,1,2,3\}$ and X = 0023 -> 2023
For a solution of size $n$ and an alphabet of size $k$  the neighbourhood size is $(k-1)n$ 
	Above example, n = 4, k-1 = 3, 3x4 = 12
### Example neighbourhoods for permutation represented solutions
Many operators to define different neighbourhoods

#### Adjacent pairwise interchange
Swaps two adjacent entries in permutation
For a permutation of size $n$, the neighbourhood size is $n-1$
Hamming distance is always 2.
#### Insertion
Insertion operator removes an entry from the permutation and reinsertis it into different position.
	![[Pasted image 20250217023438.png]]
For permutation of size $n$, the neighbourhood size is $(n-1)^2$ 
Hamming distance is dependent on number of places an item is shifted.
$2 \le HD \le n$ 
Subsumes adjacent swap.
#### Exchange
Swaps two arbitrarily, but different, selected entries.
![[Pasted image 20250217023456.png]]
For a permutation of size $n$, neighbourhood size is $n(n-1)/2$ 
HD = 2
Subsumes adjacent swap.

#### Inversion
Reverses the entries between two arbitrarily selected start and end points
For a permutation of size n, neighbourhood size is $n(n-1)/2$
$2 \le HD \le n$ , dependent on start and end points
Subsumes adjacent swap

## Objective Function
Indicates how good a given solution is, distinguishing it between one that is objectively better or worse.

Extremely important, search process, provide feedback on solution quality.
Help select best candidate from a neighbourhood of solutions. 
Can be used to guide the search to promising regions.

Different types exist.
### Separable vs Non Separable
- **Can be broken down into independent sub-functions vs cannot be broken down into independent sub functions**
- **Makes harder to optimise if the latter.**
### Uni modal vs multi modal
- **Number of global optima**
### Single objective vs multi objective
- **Optimising single criterion vs multiple criteria, which may be conflicting.**
### Exact models vs surrogate models
- **Precise evaluation vs surrogate model to reduce computation time, obj function could be expensive to compute**

### Evaluation function for MAX-SAT
Given total number of clauses $n$ and total number of satisfied clauses $C_{SAT}$ can design many evaluation functions.
- $f1 = C_{SAT}$ 
- $f2 =$ $n - C_{SAT}$ 
Can be designied for minimisation or maximisation,
![[Pasted image 20250217024838.png]]

### Evaluation function for TSP
Given a permutation of $\{0,...n-1\}$ denoted $\pi$ and a cost matrix $C$  of dimension $N \times N$ where $c_{ij}$ denotes known distance between cities $i$ and $j$ and $\pi(i)$ denotes city at $i-th$ location.
Evaluation function for minimising the total path length:

$$ f(\pi, C) = \sum_{i=0}^{N-1} c_{\pi(i),\pi((i+1)\bmod N)}$$
Add up distances between each consecutive city visiting in tour of permutation, including return to starting city via Mod N. 

Distance calculation between cities
$$c_{i,j} = \sqrt{(x_i - x_j)^2 + (y_i - y_j)^2}$$
Overhead of squaring and square root. 
### Delta evaluation
Technique, improve computation efficiency by only computing the change in the objective function/solution, rather than the entire function from scratch. 

Crucial for efficient heuristic/metaheuristics/hyper-heuristics.
![[Pasted image 20250527141308.png]]


## Hill Climbing algorithms
A hill climbing algorithm is a **local search** algorithm which constantly moves in direction of (decreasing/increasing) objective value for a given (minimising/maximisation) problem to find the (lowest/highest) point in the search space.

1. Pick initial starting point in search space.
2. Repeat
	i. Consider neighbour(s) of current solution as new points
	ii. Compare new points using an evaluation function against current solution, choosing the best solution among them
	iii. Move to new point
3. Until termination criteria satisfied e.g., num of iterations
Starting point may be chosen randomly (stochastic) (perturbative). Or using a constructive heuristic.
Often differ on this basis.

### Example hill climbing heuristics
#### Stochastic hill climbing
Randomly sample the neighbourhood of solutions. DBHC.
![[Pasted image 20250217032220.png]]
0 .. N
#### Simple hill climbing 
Has strategy for systematically sampling neighbours of a solution, rather than being stochastic.
![[Pasted image 20250217032245.png]]
Often break here. Could be 0 .. N

![[Pasted image 20250217032258.png]]
Take only best improvement. Modifies 0..1


### Strengths of hill climbing
Very easy to implement, only need:
- Solution representtion
- Eval function
- A way to define neighbourhood of solution
### Weaknesses of hill climbing
- Local optimum: get stuck as all neighbouring solutions have a worse or same quality, unable to explore to escape from plateau.
- Ridge/valley: combination of local optima and plateua, search explores along here but cannot escape
- No info about how much a local optimum deviates from global optimum.
- Success/failure determined based on starting point. 