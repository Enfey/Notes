# Sorting algorithms
## Bubble sort
![[Pasted image 20250311065322.png]]
![[Pasted image 20250312012034.png]]
Performs repeated inner scans, decrementing i each time, thus checks 1 less item each time as j < i
![[Pasted image 20250312012117.png]]

- Add a swapped boolean, to check on initial inner pass, if swapped is not true at any point, return false. Optimises best-case.
### Complexity
- Best Case - O(n^2), O(n) if using swapped
- Worst Case - O(n^2), using n-1 + n-2 ... , results in arithmetic series and is n(n-1)/2, nxn = n^2
- Average case - O(n^2)
### Stability
A sorting algorithm is said to be stable if two objects with equal keys retain their relative order before and after the sort. 

## Selection sort
- Similar to bubble sort
- On each scan, instead of moving the greatest element so far by performing excess swaps, just remember index and move it at end of scan.
- ![[Pasted image 20250312012148.png]]
- for i to array length, i--
	- for j to less than or equal to i, j++ (same as bubble sort)
		- save largest value encountered.
	- if (i != largestVal) //i = end of range 
		then swap directly to i.
- ![[Pasted image 20250312012625.png]]
- ![[Pasted image 20250312012632.png]]
- ![[Pasted image 20250312012652.png]]
- ![[Pasted image 20250312012718.png]]
- etc, until i = 0
#### Complexity
- O(n^2) given n(n), plus neglible primitive operations, which could be counted but due to asymptotic nature would say nothing useful about performance
- Despite this, achieves much better **real world** performance due to significantly less number of swaps required. 
- But same number of iterations, comparisons in the worst case.

## Insertion Sort
- Novel idea, **keep front of list sorted**, as move through back elements insert them into correct place in the front. 
- ![[Pasted image 20250312013640.png]]
- Assume first element is sorted
- Start from second element j=1 in outer loop
- temp stores element trying to place in correct position. 
-![[Pasted image 20250316170849.png]]


for (int j = 1; j < n; j++) // start from one
	temp = arr[j]
	i = j 
	while (i > 0 && arr[i-1] > temp){ //keep decrement in sorted area, to find place for arr[j] in there.
		arr[i] = arr[i-1]
		i--
	}
	arr[i] = temp
	