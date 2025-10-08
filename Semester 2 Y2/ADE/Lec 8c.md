# Trees - properties
## Height vs Number of nodes
- Height is very important property of trees, algorithms operating on trees often work down tree going level-by-level, root->leaf
	- Their runtime depends on the number of levels, thus their runtimes often O(height)
- Usually only know the number of nodes, n, and so need to relate the height $h$ with $n$ 
- Want to put limits on the relationship between **height** and **num of nodes $n$ **
	- E.g., for given $n$ what is range the $h$  can have? 
### Maximum $h$ for a given $n$
- For a given $n$, the max $h$ is clearly when all nodes have at most one child, get a long chain, essentially equivalent to a list of sorts. 
- Then the number of edges to traverse to reach the lowest level will be n-1
- So maximum $h$ is h_max(n) = n-1
- hence h(n) $\in$ O(n)

### Minimum $h$ for a given $n$
- In worst case scenario, can have extremely unbalanced tree, one node as root and all other nodes as children of that root, a single parent with n-1 children
- Results in tree height of 1, but this is a special and unhelpful case.
- More meaningful case is a fixed arity tree where nodes branch out systematically. 
- Useful to reverse question to determine $min_h(n)$, ask
	- *What is the maximum n for a given h?*
	- So take perfect binary tree
		![[Pasted image 20250316175309.png]]
		- Strongly suggests:
		- Number of nodes e.g., size at given $L$ - is $2^L$ 
		- Total number of nodes up to a given level($h$): $2^{(h+1)} - 1$
			- L and H interchangeable
			- ![[Pasted image 20250316175821.png]]
			- 
			- ![[Pasted image 20250316175826.png]]
		- 
DO THIS AT ANOTHER POINT
![[Pasted image 20250515132056.png]]
![[Pasted image 20250515132107.png]]








![[Pasted image 20250316173628.png]]
