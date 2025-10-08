![[Pasted image 20250515175435.png]]
## Motivations
- Suppose have an array A(int) and want so search for a particular int x
- If the array is unsorted, no choice but to scan elements, hence O(n)
- If it is sorted, then can do a lot better.
### Binary search for sorted array
Search for index of x in a sorted (sub) array

```
int search(int x, int[] A, int L, int R)
	if (R < L) { return -1 }//fail
	int M = (L+R)/2 //middle index
	if (x < A[M]) {return search(x, A, L, M-1)}
	if (x = A[M]) { return M }
	if (x > A[M]) { return search (x, A, M+1, R)}

int search(int x, int[] A)
	return search(x, A, 0, A.size())
```
Recursive divide and conquer solution, halving problem domain. 
![[Pasted image 20250515180058.png]]
![[Pasted image 20250515180123.png]]
Binary search for an element within a sorted array is fast
- The array to be searched is halved at each iteration
- Hence $O(log n)$ 
It only works because of the step of knowing whether to discard the left or right
But arrays suffer from being slow, fundamentally.
	E.g., O(n) to make insertion, as need to shift O(n) elements to make room for the insertion. 

A **binary search tree** attempts to:
- Fix the inefficiency of insertion
- Keep the O(log n) properties of binary search
	- Always know which node to chose for binary decision when looking for an element $x$ 
## Binary Search Tree
A binary search tree:
- Is a binary tree, storing key-value entries in the nodes
- Satisfies 'search tree peroperties'
	- For every node $M$ and
	- For every node L in the LEFT SUBTREE OF N and
	- For every node R in the RIGHT SUBTREE of N
	- We must have:
		- $key(L) \le key(M) \le key(R)$
	- Or since we exclude duplicate keys, must have
		- $key(L) \lt key(M) \lt key(R)$
- ![[Pasted image 20250515180738.png]]
### BST examples
![[Pasted image 20250515181007.png]]
![[Pasted image 20250515181014.png]]
*immediate children*
![[Pasted image 20250515181023.png]]
![[Pasted image 20250515181031.png]]
![[Pasted image 20250515181035.png]]

### BST Property: In-Order traversal
Previously, considered in-order traversal
- Visit left sub-tree
- Visit node
- Visit right sub-tree
- L->M->R
- If no immediate left child, then visit it.
- ![[Pasted image 20250515181339.png]]
- As such a fundamental property of a BST is that an in-order traversal of a BST visits the keys in sorted(increasing) order.

### BST Property: Get the minimum?
 - How do we find the minimum?
 - If there is a left child, then it must have a smaller key than node.
 - Hence, repeatedlly take left child until there is no longer a left child
 - Cost = O(height)
 - Note: To get maximum, get always take the right.

### BST - Searching for a value - $get(k)$
- Suppose we needed to get(5) with root node key = 4
- Have to start with root
- What can we deduce
	- Clearly need to try the right subtree
	- We might find key = 4 by moving to left subtree of root?? NO
- If BST definition only had left child must be smaller, then could not exclude the left subtree, and as such the subtree definition is vital. ![[Pasted image 20250515182115.png]]
- So go to right child, then find another node, and check if 5 < node, in this case it is, so then try left child.
- And repeats, and then find 5 and succeed.
 ![[Pasted image 20250515182329.png]]
- Follows downward path from the root, to a successful node, or to a failed leaf node.
- Worst-case length of the path, and so worst-case of $get(k$) is $O(h)$, where $h$ is the height of the tree.

### Search in a BST psuedocode
```
int search (int x, BST T)
	if (T == null) return null 
	int M = T.root.key
	if (x = M) return M
	if (x < M) return search(x, T.Left)
	if (x > M) return search(x, T.Right)

--START USING THE BELOW
search(x, T.root)
```
Can also be implemented iteratively, for best efficiency.


### BST: Inserting a new value - $add(k)$ 
- When inserting a new value, add($k$), must do it in a way that get($k$) would succeed, i.e., doesnt violate the subtree property
- So:
	- If key added is less than current node, go to left child
	- If key added is greater, then go to right child.
	- Repeat until an empty spot is found, and then new node is inserted as a child, L or R depending on parent.
	- If find an existing node, then overwrite it with the new v value. 
- ![[Pasted image 20250515200727.png]]


### BST deletion : deletion
- Seen that $get(k)$ and $add(k, v)$ were straightforward
- Deletion is more complex
- Start by finding node location need to delete via $get(k)$ process
- Then have 3 cases:
	- The $k-node$ has no children
	- The $k-node$ has 1 child
	- The $k-node$ has 2 children

#### BST deletion : $delete(k)$ : zero children
- Case: the k-node has no children
- Trivial, just delete the node, and set the corresponding L or R child pointer for the parent node to null.
- ![[Pasted image 20250515201002.png]]

#### #### BST deletion : $delete(k)$ : one child
- Simply remove the $k-node$ and replace it with its child, moving the tree upwards.
- ![[Pasted image 20250515201217.png]]
- Same works for if 8 only had a left child.

#### BST deletion : $delete(k)$ : two children
- An in-order traversal, yields sorted list according to $k$'s
	- ![[Pasted image 20250515201552.png]]
- After deleting $k$, in-order traversal will have a successor where $k$ was
	- ![[Pasted image 20250515201600.png]]
- Suggests useful to think about involving node 6
- Could move 6 to be after 4 in the in-order traversal, i.e., by replacing 5
- And then delete the other, original 6?
- ![[Pasted image 20250515201826.png]]
- ![[Pasted image 20250515201839.png]]
- ![[Pasted image 20250515201927.png]]
- Only works because 6 has 1 child, if it had two, would have not made real progress, and would have to be called recursively ie get its in order successor, do the overwriting, check if has 2 children, if 2 child, delete again, if 1, then handle as mentioned by replacing node with its singular child.

### BST - Performance
![[Pasted image 20250515202549.png]]
Even deletion, as in-order successor can be defined as taking min-value from right subtree, which is halving. Height can be O(n), if the tree is not balanced, i.e., the height = size/num of elements.
![[Pasted image 20250515202711.png]]
This degenerates cost of operations, as the search space is not halved at each node, while still being a binary search tree.


### BST - Rebalance
- Can easily rebalance a tree
- Use in-order traversal $O(n)$ to extract a sorted list of keys
- Insert the median, and then recurse on the entries left of the median to insert in the left subtree, and same for the right, reducing the heigh to $O(log n)$
	- E.g., grab median from entries to left and right, and then recurse while inserting.
	- ![[Pasted image 20250515203018.png]]
### BST - Self balancing
- Goal to constantly restructure the trees
- Keep the trees height balanced so that the height is logairthmic in the size
	- $h = O(log n)$
- Then performance is always logarithmic as the operations are predicated on the height.
#### Issues in self balancing
- Suppose imbalanced search tree, there are always corresponding balanced search trees
- Saw that could make trees balanced using total rebuild, but that would require O(n), and inefficient compared to O(log n)
- Rebalancing itself needs to be O(log n)
- Suggests rebalancing needs to just look at the path to some recently changed node, not just the entire tree???
- 