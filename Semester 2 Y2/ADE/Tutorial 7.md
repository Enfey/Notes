## Rotations with BSTs
Important BST algorithms are O(height)
Imbalanced can lead height to O(n), essentially an ordered list
Aim to rebuild part of the tree, ie recently changed node, often part of other operations like insert and remove, to maintain the shape of the tree to keep log n complexity, as a full re-ordering would also be O(n)

![[Pasted image 20250517165257.png]]
![[Pasted image 20250517165332.png]]
Insert(1)
![[Pasted image 20250517165727.png]]
Swap 2-3


Push up, then push down in opposite direction you pushed up
![[Pasted image 20250517170128.png]]
Whilst preserving the subtree structure.
Push 8 up, push 6 down left, 9 was on right so keep it there, and 5 and 7 were on left, do a comparison at 6 to determine where to place them.
KEEP THE RELATIONSHIPS THE SAME


PUSH THE NODE UP, THEN PUSH THE NODE ROTATING WITH DOWN IN OPPOSITE DIRECTION, AND THEN REBUILD.

![[Pasted image 20250517170209.png]]
![[Pasted image 20250517171224.png]]
For one rotation
![[Pasted image 20250517171425.png]]
For 2


## Change-Giving
### Subset Sum
First concern ourself with the following
	Given a multiset $S$ of positive integers $x[i]$, and a target $K$, is there a subset of $S$  that sums to exactly $K$
**Track**:
- Array $[x]$ of integers
- A target sum $K$ 
- Boolean array $Y[1..K]$, where $Y[m]$ = True if some subset of the considered items sums to $m$ 
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

If know can already reach $Y[m]$ and are now considering new integer, $Y[m]+x[i]$ is now also reachable, so set to true, and if the sum is achievable, return.

Example:
x = [3,1,4], K=5
Y = [T, F, F, F, F, F]

1. consider 3
	m = 5 - 3 = 2
	Y[2] = false, so skip
		m = 2-1, = 1
		Y[1] = false, so skip
			m = 1-1 = 0
				Y[0] = true
					0+3 $\neq$ 5, so no early return
				Y[0+3] is achievable, set to true.
2. consider 1
	m = 5-1 = 4
	Y[4] = false, so skip
		m = 4-1 = 3
			Y[3] = true
				Y[3+3] $\neq$ 5, so no early return
			Y[3+3] = true
				m = 3 - 1 = 2
					Y[2] = false, so skip
						m = 2-1 = 1
							Y[1] = false, so skip


```
initialise all Y[m] = 1..K to false
Y[0] = true
x0...x[n] and K

for (int i=0; i < n; i++)
	for (int m = K - x[i]; m >= 0; m--)
		if (Y[m]==true)
			if(m+x[i]==K)
				return;
		Y[m+x[i]] = true
```

REMEMBER THIS


### DP for min-coins-change-giving
Given a multiset of coins $S$ and a target value $K$, find the subset of coins which sums to $K$ with the smallest cardinality.

```
Input: x[0],...,x[n-1] and K
Initialise: Y[0] = 0 // can give 0 with 0 coins and 
Y[1..K] = -1

for (int i = 0; i < n; i++)
	for (int m = K-x[i]; m >= 0; m--)
		if (Y[m]>= 0)
			int newCount = y[m]+1
			if(Y[m+x[i]] == -1)
				Y[m+x[i]] = newCount // first time seen
			else
				//Take cheaper of old vs new way
				Y[m+x[i]] = min(Y[m+x[i]], newCount)
```

![[Pasted image 20250517180642.png]]
![[Pasted image 20250517180709.png]]
![[Pasted image 20250517180834.png]]
![[Pasted image 20250517180912.png]]
1, 1, 3, 5 9 = x
10 = k


**1st outer loop**
x[0] = 1

10 - 1 = m
Y[m] == -1 until get to 0
Y[0] == 0
	Y[0+1] == -1
		Y[0+1] = y[m]+1 = 1

2nd outer loop
x[0] = 1
10 - 1 = m
Y[m] == -1 until get to 1
Y[1] == 1
	Y[1+1] = -1
		Y[1+1] = y[m]+1 = 2
Y[0] == 1
	Y[0+1] = 1
	take min of new count y[m]+1 = 2, or old count, which is 1

SERIOUSLY JUST KEEP GOING OVER THIS

