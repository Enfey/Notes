**ADT** = Priority Queue(PQ)
**CDT** = Binary Heap


### Priority Queue ADT
Usage: Data structure which collects together elements and treats them as a queue, but with each element having a priority
	Higher priority elements are retrieved first - elements served based on priority, not just insertion order
Used in many queuing systems e.g., OS scheduling.
Used internally in many algorithms

The PQ stores multiple elements, each in the form of a '**Key-Value pair**'.
- **Key K**
	- Typically a number representing the priority, we take smaller numbers to be higher priority
	- Generally, key can be anything that has a total order, but often use integers for simplicity.
- **Value V**
	- This can be any associated value e.g., could be ID of a person
	- The value itself plays no role in the functioning of the heap, so typically ignore it.

#### PQ - Main operations
##### Add 
`void add (K k, V v)` 
	Adds the Key-Value pair (k,v) to the data structure
##### removeMin
`(K, V) removeMin()`
	Removes and returns a K,V pair in which the key is guaranteed to be a minimum within all the keys.
##### size
`int size()`
	Returns the number of KV pairs stored
##### peekMin
`(K,V) peekMin()`
	Returns a K,V pair in which the key is guaranteed to be the minimum within all keys, but leaves the entry in place.


#### PQ: Using lists
- Can implement PQ using linked lists
- If the list is unsorted: 
	- `add(k,v)` is **O(1)** - we can just put the entry at the tail
	- `removeMin()` is **O(n)** as must scan the list for the minimum
- If the list is sorted(smaller first)
	- `add(k,v)` is **O(n)** - must scan to find the correct position to insert it
	- `removeMin()` is **O(1)** as know the min is at the head
- However, in both cases the average could still be O(n), the worst case operations must be rare for this to average out to better than O(n) (amortised complexity), otherwise the cost will be dominated by these operations.
- Muust aim for a better implementation of a PQ such that all operations are better than O(n) ideally, O(log n)
- This can be achieved by implementing a PQ using a kind of binary tree called a **heap**

#### PQ: improve upon O(n)
- How would we organise the tree?
- We want a minumum element to be easy to access
	- Make reasonable sense to place this at the root
	- Then we want same idea for all subtrees, ie, the root should be the minimum key
- This leads to **heaps**
### PQ using heaps
- Numerous kinds of heaps, e.g., binary heaps, min heaps, max heaps.
- Here we use a binary min-heap, a numeric tree where **arity = 2**, and the root is the smallest key.
	- For every node: the key of the node must not be smaller than the key of the parent
	- That is for every node, the children of that node cannot have a smaller key.
	- For all paths from the root to any leaf, the sequence is non-decreasing
- Heaps also have some restrictions on the properties of the binary tree
- Standard restriction - tree must be **complete** - every level is full, except possibly the last, and in this case, all the nodes must be as far left as possible.
- ![[Pasted image 20250515125731.png]]
- ![[Pasted image 20250515125740.png]]
- ![[Pasted image 20250515125754.png]]
- ![[Pasted image 20250515125804.png]]
- Not leftmost, and see array based representation - would be a gap prior to allocating right child of the highlighted node.
- ![[Pasted image 20250515125851.png]]
- The array is **not sorted** - efficient representation, achieved simply using binary shifts and addition to calculate indices from parent, and provides constant access.

#### Heaps: Add example
- Suppose we must `add(1, v)`
- Last node is 7
- Must place is after 7, i.e., as a left child of 5.![[Pasted image 20250515130158.png]]
- Initially breaks the heap property of min-heaps
	- 1 is a child of 5 and should not be smaller.
- Immediate repair, swap them.
- ![[Pasted image 20250515130241.png]]
- ![[Pasted image 20250515130250.png]]
- ![[Pasted image 20250515130300.png]]
	Maybe go over this.

#### Heap: Remove min example
- Need to remove the root
	- But want to keep the shape of the binary tree and preserve its properties
- We make a copy of the KV of the root and return it at the end
- For now, copy the contents of the last node into the root node, and then remove the last node:
- ![[Pasted image 20250515130624.png]]
- ![[Pasted image 20250515130633.png]]
- Numeric property is now broken, need to move 5 downwards.
- Downheap always swaps with the smallest child.
- ![[Pasted image 20250515130725.png]]
- ![[Pasted image 20250515130738.png]]
- ![[Pasted image 20250515130806.png]]
- ![[Pasted image 20250515130924.png]]
- ![[Pasted image 20250515130930.png]]
- Because potential violatings are introduced only at the point of insertion or removal, only need to walk from that point back to the root, or from the root down, and so with at most one swap per level in a complete tree of height h, these operations are **O(log n).** 

#### Heapsort
?