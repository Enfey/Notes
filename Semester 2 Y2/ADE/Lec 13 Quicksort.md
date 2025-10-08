Unlike mergesort where dividing is simple but merging is complex, quicksort makes dividing more complex so merging becomes trivial.
After dividing and sorting subarrays, merging is simply forgetting the boundary, so to speak, as array is sorted in place.

Divide array into 2 subarrays, L and R, and all elements in L must be $\le$ all elements in R. Uses a pivot value to determine the division.


1. Choose a pivot element from the array
2. Partition the array into
	- Elements greater than the pviot
	- Elements less than the pivot
3. Recursively apply the same process to the sub-arrays
4. Combine the sorted sub-arrays


### Pivot selection
Critical for performance
1. First/last element: simple but can be worst case on sorted input
2. Random element: helps avoid worst case on average
3. Median of three: Choose median of first, middle, and last elements
4. Equal partitioning pivot(median): Select pivot to be the median, end up with equal sized partitions afterwards
	1. Easy to do with auxiliary workspaces ie arrays, 
	2. ![[Pasted image 20250512002446.png]]
	3. But want to avoid this 

### Partition array
- Rearrange array so that elements less than the pivot go to the left, and elements greater than the pivot go to the right.
- How to verify a partition is correct? Just compute max(L) and min(R) and check max(L) $\le$ min(R), avoiding pairwise computations $x \in L, y \in R, x \le y$ 
- Prefer in place partitioning, which doesn't require copying, like above with equal partitioning pivot example
- In-place partitioning:
	- Require that split $A$ into $L$ and $R$ with **pivot = 11**
	- elements in $L$ are all < pivot
	- elements in $R$ are >= pivot
	- Then can work from both ends inwards looking for swaps whenever the entries are 'in the wrong order'
	- Pairwise swaps, using 2 indices kL and kR, doing kL++ and kR--
	- ![[Pasted image 20250512002916.png]]
	- ![[Pasted image 20250512002922.png]]
	- ![[Pasted image 20250512002950.png]]
	- Once in right place, decrement kR, and increment kL
	- When the indices meet, then know we have performed partition in place
		- Only need O(1) extra space, and only traverse array once, so O(n) for partioning.
### Recursively apply process
- Then apply quicksort to the subArrays, until subarray length is 1 and then concatenates the array back together via walking back up call stack, leaving fully sorted array?

![[Pasted image 20250512004143.png]]
See mergesort depiction tree to understand this!
![[Pasted image 20250512004200.png]]
![[Pasted image 20250512004307.png]]
![[Pasted image 20250512010230.png]]
![[Pasted image 20250512010327.png]]
![[Pasted image 20250512010339.png]]
