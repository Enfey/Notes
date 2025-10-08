A **BST** is a binary tree storing KV entries at its nodes, and satisfying the ***'search tree property'***
	For any node then, nodes in the **left subtree** have strictly smaller keys, and nodes in the **right subtree** have strictly larger keys
	Such that an in-order traversal visits the keys in an increasing order.


## The BST Imbalance problem
![[Pasted image 20250516160726.png]]
- Both trees are valid BSTs with the same content
- Both satisfy the search tree property, only differing in height
- This matters are key BST algorithms are $O(height)$, and a rightmost or leftmost tree yields $height = n$, rather than height = $log_2 (n)$ as the tree is unbalanced.
- A tree is said to be ***balanced*** if the heights of the left and right subtrees of any node are close to equal, yielding a height of $log_2(n)$
- There are ALWAYS corresponding balanced search trees for an imbalanced search trees
- Could make trees balanced using a total rebuild, as seen in lec 18, but this requires visiting each node, and as such is very inefficient compared to O(log n)
- Rebalancing itself needs to be O(log n)/O(height)
- Suggests re-balancing itself needs to look at the path to some recently changed node, not the entire tree. 
- AIM: Do small local rebuilds during insert/delete operations, to maintain the overall balanced structure and retain the O(log n) cost.

### Example of a BST T
![[Pasted image 20250516161202.png]]
- Where subtrees $T1, T2, T3$ are BSTs themselves
- Height $h(T)$ = max (1+h(T1), 2+h(T2), 2+(T3)), cause of adding a or b
- In order traversal: $In(T) = In(T1), a, In(T2), b, In(T3)$
	- ![[Pasted image 20250516161520.png]]
- Can we use the same nodes and subtrees to build a different BST, with the same In-order traversal? 

### Example of a rotation on T
![[Pasted image 20250516161757.png]]
Yield same in-order traversal
- Node a moved down by one
- Node b moved up one
- Subtree T1 moved up by one
- Subtree T3 moved up by one
Called a single rotation, as the edge a-b rotated
Root changed from a to b.
A single rotation has constant cost????, even if this tree were a sub-tree of a BST. The parent of a in this case would be moved to be the parent of b.
![[Pasted image 20250516162131.png]]
![[Pasted image 20250516162235.png]]
![[Pasted image 20250516162255.png]]
In small examples, we can do this by inspection, but in general, need to be able to enforce rotations algorithmically.


SOME EXAMPLE QUESTIONS OF THIS

### Solution: Self-Balancing
- Constantly perform local restructuring of the trees so that the trees height is logarithmic in the size. Only do rotations along the paths encountered during the BST operations. 