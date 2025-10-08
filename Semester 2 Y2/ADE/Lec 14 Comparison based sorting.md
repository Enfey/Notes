- Seen mergesort runs in O(n log n)
- But can we do better?
- Asking whether **any** sorting algorithm might do better
	- Hence listing more algorithms not answering question
- Must take a high-level view of what sorting algorithms need to do, placing lower bound on big oh behaviour.

## Comparison based sorting
- If a sorting algorithm only obtains information about the elements by doing pairwise comparisons between the elements, then it is said to be **comparison based.** 
- Makes decisions based solely on comparisons like if (A[i] <= A[j]). They cannot peek at the actual values to make sorting decisions. 
- All algorithms seen thus far are comparison based
- Not all sorts are comparison based

### Lower bound for comparison sort
- Use an information theoretic argument.
- Model comparison based sorting as making comparisons between elements,  and obtain a binary decision tree
- At each node, a comparison between two elements is made
- Each leaf represents a final ordering
- The path from root to leaf shows the sequence of comparisons made.
- If collect enough information, can deduce the correct order of the items in the array.

#### Information collecting
For $n$ numbers, there are $n!$ possible orderings.
For 3 elements, there are 3! = 6 possible orderings.
We do halving until we reduce n! down to a unique ordering.
	We will need at least Omega(log_2 n!) comparisons, which gives us the minimum height of our binary tree.
	Which is Omega (n log n)
		![[Pasted image 20250512022029.png]]
![[Pasted image 20250512021611.png]]
![[Pasted image 20250512021838.png]]

![[Pasted image 20250512022209.png]]
Even a completely naive algorithm which tries all algorithms would have an upper bound of O(n^2):
This is due to ![[Pasted image 20250512023233.png]]
![[Pasted image 20250512023538.png]]
where 1 is first term, n-1 is last term, nxn = O(n^2) comparisons, cannot perform worse than this unless purposefully discards information.

