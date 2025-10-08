> There exist various paradigms for finding solutions for a given problem instance/domain. 

They include:
- Brute Force
- Divide and Conquer
- Heuristics
- Dynamic Programming

## Brute Force
Try **all** possible candidate solutions, and check which one(s) satisfy the problem.
Guaranteed to find the optimal solution, exhaustive method.
For example, could do sorting by generating all possible permutations, and checking which one preserves order
	Would be inefficient for this purpose, as O(n!)
		![[Pasted image 20250516132514.png]]
Useful in some small cases, but does not scale.
## Divide and Conquer
Divide problem into smaller subproblems, solve each recursively, then combine the results. Efficient for problems with natural recursive structure, and reduces complexity significantly.

## Heuristics
> A heuristic is a rule of thumb method derived from human intuition. 

Have an idea about how we might approach some problem, and just try it. 

Heuristics are:
- **Problem dependent** search methods which seek good solutions in reasonable computational budget
- **Unable to guarantee optimality** - can only give good answers, dependent on computational budget.
Suitable for ill formatted problems, and large solution spaces.

Heuristics may be present in **exact** methods, such as choosing a random pivot in quicksort to retain its superior complexity, rather than always choosing the most optimal one. 

Heuristics may be present in inexact methods/form the inexact method e.g., metaheuristics, genetic algorithms...

#### Greedy Algorithm
- A common problem-solving heuristic
- Takes the decision that is locally optimal at each step, without considering the overall structure of the search space, i.e., does not consider how an immediate move will impact future steps.
- Can sometimes give optimal answers depending on the complexity problem domain/instance.
- But usually cannot guarantee optimal answers, but the solutions will often be good enough, despite likely getting stuck in local optima. 


### Change-giving
> ***Problem: Given a collection of coins, (a multi-set, allowing repeated elements), and a desired target, find the minimum number of coins from the given multiset that sum to the desired total, or show that it is not possible



### Formalised domain
- Given set $S$ of coins and their values $V[ \ ]$
	- $S = \{v[1,], v[2],...,v[n]\}$
		- Where coins can be repeated, ie any $V[n]$ may appear more than once
- And a target $T$ the total value to be returned
- Find the set, a subset of $S$, with the minimum number of coins but whose total value/sum is **exactly** $T$. 
##### Example instances:
![[Pasted image 20250516135203.png]]


#### Solutions
Obviously, could solve by enumerating all possible subsets of $S$, selecting those that sum to $T$, via brute force.
With n coins, there are 2^n  subsets, as there are 2 possibilities for each coin (either include it or exclude it.)
	![[Pasted image 20250516135401.png]]
As such, a brute force would yield exponential complexity in the worst case.


##### Change-Giving Greedy Algorithm
- Greedy strategy
- Pick the largest coin which is still available and does not cause to exceed $T$.
- ![[Pasted image 20250516135516.png]]
- Sometimes will fail, as does not consider how decision will affect future.![[Pasted image 20250516135545.png]]
- ![[Pasted image 20250516135559.png]]
- Does not guarantee optimality, or even a solution, and so is not really suitable.

### Dynamic programming
Solve problems by breaking them into overlapping subproblems, storing results(memoisation) and reusing them to avoid recomputation. 

If a problem has an optimal substructure i.e., the problem can be broken down into smaller subproblems, then DP can be used. 

The optimal solution here includes optimal solutions to the subproblems, known as the decomposition property. 

Unlike divide and conquer, DP deals with problems where the same subproblems appear multiple times. Instead of solving them repeatedly, we solve each once, and store the result.

The strategy is to solve the smallest subproblems first, and then build up toward full solution, by combining the small subproblem solutions.

#### DP for Change Giving/Subset Sum
- First, concern self with subset sum, rather than worrying about number of coins in given subset.
- This problem is known as the **Subset-Sum**
	> Given a multi-set $S$ of positive integers, $x[i]$, and a target $K$, is there a subset of $S$ that sums to **exactly** $K$?
	
- Algorithm: Consider the numbers one at a time, keeping track of which subset sums are possible so far.

**TRACKING**
- Have a multiset of positive integers $\{ x[0], x[1],...x[n-1]\}$
	- x just stores all the integers
- A target sum $K$
- Want to know whether some subset of these numbers add exactly to $K$ 

**Key idea**
Maintain boolean array $Y[0..K]$
Where $Y[m]$ = True if some subset of the considered items sums to m
Before looking at any $x[i$] the only achievable sum is 0, by taking no items.
$Y[0]$ = true, $Y[1...K]$ = false


Suppose already processed $\{x[0]...x[i-1]\}$, so $Y[m]$ correctly reflects which sums $m$ are possible using only those first $i-1$ items. Now add in item $x[i]$. Any sum m that was previously achievable can now also give us sum $m+x[i]$ 
So need to set $Y[m+x[i]]$ = true for all m where $Y[m]$ was true.


**Solution**
```
input x0,...x[n-1] and K
initialise all Y[m] = false for m = 1..K
Y[0] = true 
for (i = 0; i < n; i++){
	for(m = K-x[i]; m >= 0; m--)
		if (Y[m]==true)
			if(m+x[i]==K)
				return true; //early exit, can reach K
		Y[m + x[i]] = true //still part of above if.
}
```
Example:
x = [3,1,4], K=5
Y = [T, F, F, F, F, F]

1. i = 0, x[0] = 3
	m = 5-3, scan down to 0
	- m=2,1
		- False->skip
	- m=0
		- True, Y[0+3] =True, set Y[3] = true.
2. i = 1, x[1] = 1
	m = 5-1 = 4, scan down to 0
	- m = 4, 
![[Pasted image 20250516143025.png]]


**Complexity**
- Outer loop has to consider all the coins
- Inner loop scans up to K entries in the worst case, from K-x[i] down to 0. 
- Therefore, overall cost is
- ![[Pasted image 20250516143319.png]]
- But technically hides an exponential, as we refer to input size when measuring an algorithms complexity.  Above we are referring to its numeric value.
- The bit size of the input, to write down the number $K$, we would need around $[log_2(K)] + 1$ bits
- So the length of the input that represents K is B = O(log K)
- Means that K = 2^O(B) = 2^B
- ![[Pasted image 20250516143628.png]]

#### Min-coins version
- Previous problem simply asked if there was a subset that summed to the required total $K$, but want to now minimise the number of items in this subset.
- Hence we need to not just track if a given target is possible, but also, the minimum number of coins that are needed.

#### DP For Min-Change giving
Inspect coins one at a time, keeping track of best answers obtained with coin so far.
**Main Data structure** 
	Integer array $Y$, for $[0..K]$
	- $Y[m] = -1$ if not found any sum for m yet
	- $Y[m] = c >= 0$ means that have found the sum for m, and can achieve it with $c$ coins
Aim, when algorithm finished Y[K] will be the minimum number of coins, by piecewise constructing the sums for subsets of K.

The underlying idea, suppose have found all best answers to coins x[0],...x[i-1] and then want to add one more coin x[i]. If some set summed to m, then with the inclusion of x[i], we can now also find a subset that sums to m+x[i]. If a set of coins had already been found also, we can now take the one that gives the minimum. 

```
Input: x[0],...,x[n-1] and K
Initialise: Y[0] = 0 // can give 0 with 0 coins and 
Y[1..K] = -1

for (int i = 0; i < n; i++)
	for (int m = K-x[i]; m >= 0; m--)
		if (Y[m]>= 0)
			int newSum = m + x[i]
			int newCount = y[m] + 1 //one more coin

			if(Y[newSum] == -1)
				Y[newSum] = newCount // first time seen
			else
				//Take cheaper of old vs new way
				Y[newSum] = min(Y[newSum], newCount)
```
![[Pasted image 20250516160223.png]]
![[Pasted image 20250516160239.png]]
![[Pasted image 20250516160321.png]]
![[Pasted image 20250516160326.png]]
![[Pasted image 20250516160331.png]]
![[Pasted image 20250516160337.png]]
