1# Recurrence relation
- A recurrence relation is a recurisvely defined function that expresses the cost of solving a problem of size n, and often interested in the big-Oh family properties of such a relation. 
- Suppose the runtime of a program is T(n), then a recurrence relation will express T(n) in terms of its values at smaller values of $n$ 

## Example - Mergesort
- Suppose the runtime of mergesort on array of integers of size $n$ is $T(n)$. Then:
	$T(n) = 2T(n/2) + b + a \cdot n$ 
	2T(n/2) due to having to sort the two sub-arrays each of size n/2
	b is the cost of doing the split
	a is the cost of doing the merge, proportional to the input size. 
- Gave the recursive case, also need base case. 
	$T(1) = 1$
	As just need to check if array length = 1.
- How would we solve this? 
	- T(2)
		- T(2) = 2 T(2/2) + b + a . n
		- T(2) = 2 T(1) + b + a . n
		- T(2) = 2 + b + a . n
	- T(4)
		- T(4) = 2 T(4/2) = 2T(2) = 4
	- T(8)
		- T(8) = 2 T(8/2) = 2T(4) = 8
- Seems a good guess that $T(2^k) = 2^k$
- To generalise this assumption, must use induction. 
- Claim: $\forall k, T(2^k) = 2^k$ 
	- Base case: 
		- T(2^0) = T(1)
		- T(1) = 1
		- 2^0 = 1
	- Inductive case: Suppose true at k, (IH), need to shw true at k+1
		- 2 T(2^k+1/2) = 
		- 2 T(2^k) =
		- 2 2^k
		- 2^k+1
- How we would solve this?
	- T(3)
		- 3 T(3/3)+2 = 3 + 2
		- 3 T(9/3)+2 = 3(3+2) + 2 = 17
		- 3 T(27/3) + 2 = 3(9+2) + 2 = 35
		- T(3^k) = 2 * 3^k -1
	- Base case, k = 0
		- T(3^0) = T(1) = 1
		- 2 * 1 -1 = 1, as specified
	- Inductive case, k+1
		- T(3^k+1) = 3 T(3^k+1/3) + 2
		- T(3^k+1) = 3 T(3^k) + 2
		- 3 T(2 * 3^k -1) + 2 -- using induction hypothesis
![[Pasted image 20250316220604.png]]

### PRACTICE Q'S
### Resolving exact solutions from recurrence relations
1. T(n) = T(n-1) +1 and T(1) =1
	T(2) = T(2-1) + 1 = T(1) +1 = 1 +1
	T(3) = T(3-1) = T(2)+1 = 1+1+1
	T(4) = T(4-1)+1 = T(3)+1 = 1+1+1 + 1
	T(n) = n
2. T(n) = 2 T(n-1) and T(1) =1
	2(2-1) = 2(1) = 2 x 1
	2(3-1) = 2(2) = 2 x 2 x 1 --just 2x whatever T(2) was
	2(4-1) = 2(3) = 2 x 2 x 2 x 1
	T(n) = 2^n-1 
3. T(n) = 2 T(n/2) and T(1) =1
	2 T(2/2) = 2 T(1) = 2
	2 T(4/2) = 2 T(2) = 4 -- 2x whatever T(2/2) was
	2 T(8/4) = 2T(4) = 8
	T(2^k) = 2^k
	Of the form 2^k, as dividing by the constant 2.
4. T(n) = 3 T(n-1) and T(1) = 1
	3T(2-1) = 3T(1) = 3
	3T(3-1) = 3T(2) = 9
	3T(4-1) = 3T(3) = 27
	T(n) = 3^n-1
5. T(n) = 3T(n/3) and T(1) =1
	3T(3/3) = 3T(1) = 3
	3T(9/3) = 3T(3) = 9
	3T(27/3) = 3T(9) = 27
	T(3^k) = 3^k
6. T(n) = 2T(n/4) and T(1) =1
	2T(4/4) = 2T(1) = 2
	2T(16/4) = 2T(4) = 4
	2T(64/4) = 2T(16) = 8
	T(4^k) = 2^k
	![[Pasted image 20250316224212.png]]
	
	

### Inductive proofs on recurrence relations
Given: T(n) = T(n-1) + 1 and T(1) = 1
Prove: T(n) = n
n=general case
k = arbitrary integer which we assume works in induction hypothesis
Base case: T(1) = 1
Induction step: assume T(k) = k, prove T(k+1) = k+1
	T(k+1) = T(k+1-1)+1 = T(k)+1 = k+1

2. Given: T(n) = 2T(n-1) and T(1) = 1
	Prove: T(n) = 2^n-1
	Base case
		T(1) = 2^1-1 = 2^0 = 1, so holds. 
	Induction step: assume T(k) = 2^k-1, prove T(k+1) = 2^k
		T(k+1) = 2T(k+1-1) =
		2T(k) = 2 2^k-1 = 2^k
3. Given T(n) = 2 . T(n/2) and T(1) =1
	Prove: T(2^k) = 2^k
	Base case
		T(2^0) = T(1) = 1
	Induction step: assume T(2^k) = 2^k, prove T(2^k+1) = 2^k+1
		T(2^k+1) = 2 T(2^k+1/2) 
		2 T(2^k+1/2)  = 2 T(2^k)
		2 2^k -- via ih
		2^k+1
4. Given T(n) = 3 . T(n-1) and T(1) =1 
	Prove T(n) = 3^n-1
	Base case
		T(1) = 3^1-1 = 3^0 = 1 so holds
	Induction step: assume T(k) = 3^k-1, prove T(k+1) = 3^k
		T(k+1) = 3 . T(k+1-1)
		T(k+1) = 3 . T(k)
		3 . T(k) = 3 . 3^K-1
		= 3^K
5. Given T(n) = 3 . T(n/3) and T(1) = 1
	Prove: T(3^k) = 3^k
	Base case
	T(3^0) = T(1) = 1 = 3^0, so holds
	Inductive case: assume T(3^k) = 3^k, prove T(3^k+1) = 3^k+1
	T(3^k)+1 = 3 . T(3^k+1/3)
	3 . T(3^k+1/3) = 3.T(3^k)
	3 3^k --via ih
	= 3^k+1
	Proved
6. Given T(n) = 2 . T(n/4) and T(1) = 1
	Prove: T(4^k) = 2^k
	Base case:
	T(4^0) = T(1) = 1 = 2^0, so holds
	Induction step: assume T(4^k) = 2^k, prove T(4^k+1) = 2^k+1
	T(4^k+1) = 2 . T(4^k+1/4)
	2 . T(4^k+1/4) = 2 . T(4^k)
	2 . T(4^k) = 2 . 2^k -- via ih
	2 . 2^k = 2^k+1
	Proved.

## For each of following, find exact solution, and prove by induction
1. T(n) = 4 . T(n/4)
	SOLUTION:
	T(4) = 4 . T(4/4) = 4 T(1) = 4
	T(16) = 4 . T(16/4) = 4 T(4) = 16
	T(4^k) = 4^k

	INDUCTION:
	Given: T(n) = 4 . T(n/4) and T(1) = 1
	Prove T(4^k) = 4^k
	Base case: 
	T(4^0) = T(1) = 1 = 4^0, proved
	Inductive step: assume T(4^k) = 4^k, prove that T(4^k+1) = 4^k+1
	T(4^k+1) = 4 . T(4^k+1/4)
	4 . T(4^k+1/4) = 4 . T(4^k)
	4 . T(4^k) = 4 . 4^k -- via ih
	= 4^k+1
2. T(n) = 4 . T(n/2)
	SOLUTION
	T(2) = 4 . T(2/2) = 4T(1) = 4
	T(4) = 4 . T(4/2) = 4T(2) = 16
	T(2^k) = 4^k

	INDUCTION
	Given T(n) = 4 . T(n/2) and T(1) = 1
	Prove that: T(2^k) = 4^k
	Base case
		T(2^0) = T(1) = 1 = 4^0, proved
	Inductive step: assume T(2^k) = 4^k, prove that T(2^k+1) = 4^k+1
		T(2^k+1) = 4T(2^k+1/2)
		4T(2^k+1/2) = 4T(2^k)
		4T(2^k) = 4 . 4^k 
		4 . 4^k = 4^k+1
		Proved.
3. T(n) = T(n-1) + n
	SOLUTION
	T(2) = T(2-1) + 2 = T(1) + 2 = 3
	T(3) = T(3-1) + 3 = T(2) + 3 = 6
	T(4) = T(4-1) + 4 = T(3) + 4 = 10
	![[Pasted image 20250317142615.png]]
	Given T(n) = T(n-1) + n and T(1) = 1
	Prove that: T(n) = n(n+1) / 2
	T(1) = (1 . (1+1))  / 2 = 1, proved.
	Assume T(k) = n(n+1) / 2, prove that T(k+1) = n+1(n+1+1) / 2
	T(k+1) = T(k+1-1) + k+1
	T(k+1-1) + k+1 = T(k)+ k+1
	T(k)+ k+1 = k(k+1) / 2 + k + 1
	=  k(k+1) + 2k + 2 / 2 -- MULTIPLIED BY 2
	= k^2 + k + 2k + 2 / 2
	= k^2 + 3k + 2 / 2 
	???

4. T(n) = 3 T(n/3) + 2
	Prove that T(3^k) = 2 * 3^k - 1
	Base case:
	T(3^0) = T(1) = 1 = 2 * 3^0 - 1 = 2 * 1 - 1 = 1, proved
	Induction step: assume T(3^k) = 2* 3^k - 1, prove T(3^k+1) = 2 * 3^k+1 - 1
	T(3^k+1) = 3T(3^k+1/2) +2
	3T(3^k+1/2) +2 = 3T(3^k) + 2
	3T(3^k) + 2 = 3T(2 * 3^k - 1) + 2
	3T(2 * 3^k - 1) + 2 = 
	