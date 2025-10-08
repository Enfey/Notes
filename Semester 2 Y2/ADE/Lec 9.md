# Merge sort - Algorithm and cost analysis
## General algorithm strategy - Divide and Conquer
- To solve a problem, a strategy that may work is:
1. Divide the problem into a number of smaller piece
2. Solve each small piece
3. Construct the solution(conquer) by building up piecewise from the solutions of each small piece
Obviously a recursive tactic, self-invocation until 'pieces' of problem so small that base case is reached and construct solution. 

### Merge sort
- Merge sort leverages this strategy.
- To sort an array A: 
	- Divide the array into two smaller array A1 A2
	- Sort each smaller array A1 and A2
	- Conquer original problem by combining the sorted smaller arrays. 
#### Psuedocode
![[Pasted image 20250316182300.png]]
![[Pasted image 20250316182309.png]]
![[Pasted image 20250316182906.png]]
![[Pasted image 20250316183053.png]]


#### Depiction trace of a mergesort run
- Mergesort is binary recursion, ie 2 recursive calls, and as such the stack trace will become a binary tree. 
- ![[Pasted image 20250316190444.png]]
- What is the height of this **Trace tree**?
	suppose $l$ = $2^k$ , ie length of array is power of 2.
	Then how many times do we need to split arrays to reach singleton arrays?
	Answer: k
	E.g., $l = 4 \ and \  2^2, \ k=2, \ height \ = 2$ 
	So height = $log_2(l)$
- What is the height for general l?
	- Suppose $l \neq 2^k$
	- E.g., at $l = 3$, height is still 2.
		![[Pasted image 20250316190955.png]]
	- So general $l$  behaves like a roundup to next power of 2.
	- So height $h$ = $ceiling(log_2(n))$ 
	- Thus height is O(log n)
##### Depiction trace: including the merge
- ![[Pasted image 20250316191506.png]]
- Place marker to show partitioning of array, shows some work done, is cheap, but is O(n), given that constant -1 is neglible
- ![[Pasted image 20250316212026.png]]
- ![[Pasted image 20250316212153.png]]
- ![[Pasted image 20250316212201.png]]
- Work per level is O(n)
- Number of levels is O(log n)
- Total work is O (n log n)
- ![[Pasted image 20250316212304.png]]
- ![[Pasted image 20250316212459.png]]
- ![[Pasted image 20250316212531.png]]
- 