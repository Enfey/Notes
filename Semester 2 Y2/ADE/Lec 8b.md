## Traversals and array based representations
### Traversals
- Given data structure, common to traverse all stored data elements in some systematic and meaningful manner, processing each element precisely once exploiting its structure in some manner.
- For arrays and linked lists - forward scan via size calculation with indices or stored pointers.
- Much more natural options for trees.
- In traversing a tree, follow edges between nodes
	- Distinguish:
		- Via - passing through node not processing contents
		- Process - handle contents.
#### Preorder
- Parent first then children
- ![[Pasted image 20250312040737.png]]
- ![[Pasted image 20250312040950.png]]
- Leftmost nodes visited first.
- Root -> left->right
#### Postorder
- Children first, then Parent afterwards
- ![[Pasted image 20250312041230.png]]
- ![[Pasted image 20250312041242.png]]
- Left->Right->Root

#### Inorder
![[Pasted image 20250312041342.png]]
![[Pasted image 20250312041348.png]]
![[Pasted image 20250312041353.png]]
- When waiting for return from described function above, process (n) straight after doing left, which is why B is 2, as processes left child, then itself, and then its right node, which needs to evaluate its left child before evaluating itself, then visit root as all sub-tree evaluated, then inorder(g), process left child, process g, finished.
![[Pasted image 20250312041400.png]]

### Representations
- Trees are linked structures, natural given how drawn.
- Are there alternatives?
- Linked lists, easier to do insertions/deletions, most nodes do not move in memory, constant depending on insertion/deletion location, but poor memory locality which cache cannot exploit, and higher memory footprint
- Array, may seem confusing at first, but lower memory footprint, contiguous so permit exploitation of memory locality, but insertion/deletion always O(n), and is complex in terms of real-life implementation. 
#### Tree representation: array
- **ADT** - behaves like a tree at user level, needed so to match tree operations
- **CDT** - implemented using an array, good memory locality, less memory footprint (pointer storage)
- ![[Pasted image 20250316171818.png]]
- Have to associate every node with some unique index in the array, the '?'
- Might be reasonable to have shallower nodes earlier in the array, also need a clear mapping between indices of parents and children.
- We avoid the use of index 0 in order to calculate children indices from parents
- ![[Pasted image 20250316172215.png]]
- Easy to calcuate indices, goes left -> right from given parent node. integer multiplication and division by 2 (at least for binary tree) to calculate left child node, and then +1 to calculate right child. 
- This is called the standard map:
	Call the index of a node n its **rank** $r(n)$ 
		Then: 
		- For the root r(root) = 1
		- For a given parent node p at r(p):
		- Place left child at 2*r(n)
		- Place right child at 2r(n)+1
- ![[Pasted image 20250316172508.png]]
- ![[Pasted image 20250316172520.png]]
- Using left shift to calculate parent, right shift to get back to parent from given child node.