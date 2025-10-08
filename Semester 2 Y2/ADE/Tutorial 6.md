Min heap - data structure where the key of each node is less than or equal to the keys of its children. 
![[Pasted image 20250517141503.png]]
1. Neither (3,2, bad ordering)
2. Min-heap
3. BST

![[Pasted image 20250517141615.png]]
1. swap 5 and 2
![[Pasted image 20250517141749.png]]
![[Pasted image 20250517141758.png]]
[1]=5
[2]=3
[3]=7
[4]=2
[5]=4

Easy to calcuate indices, goes left -> right from given parent node. integer multiplication and division by 2 (at least for binary tree) to calculate left child node, and then +1 to calculate right child. 
- This is called the standard map:
	Call the index of a node n its **rank** $r(n)$ 
		Then: 
		- For the root r(root) = 1
		- For a given parent node p at r(p):
		- Place left child at 2*r(n)
		- Place right child at 2r(n)+1
![[Pasted image 20250517142019.png]]

2, lc = 3, rc = 5, = heap,

4, lc = 3, rc = 5, BST, root does not contain the smallest element either, and tree would be incomplete(heaps need to be complete.)


![[Pasted image 20250517142456.png]]
ALWAYS ADD AT LEFTMOST AVAILABLE SPACE TO MAINTAIN COMPLETE PROPERTY
![[Pasted image 20250517142713.png]]
Breaks min property, so swap upheap
![[Pasted image 20250517142744.png]]
![[Pasted image 20250517142757.png]]
![[Pasted image 20250517142825.png]]
![[Pasted image 20250517142831.png]]
![[Pasted image 20250517142837.png]]

insertItem(10), add at 9 as left child, works because 9 < 10


![[Pasted image 20250517143054.png]]
removeMin(), takes root always (unless immediate children of root are equal to or whever)

Swap 10 with 1, to preserve completeness property. Now need to swap 10 downheap. 
![[Pasted image 20250517143217.png]]
ALWAYS SWAP WITH SMALLEST CHILD WHEN GOING DOWNHEAP TO PRESERVE NUMERIC PROPERTY.
![[Pasted image 20250517143253.png]]
If removeMin() here, swap 10 with 2. 10 is at root, swap with smallest, so swap 10 with 3, and then swap again with smallest, which is 6, so swap 10 with 6, finished.


![[Pasted image 20250517144536.png]]
Place 4 on far left, swap 4 with 1, swap 4 with 2, finished (IF THIS WAS A HEAP, WHICH IT ISN'T)
![[Pasted image 20250517144738.png]]
So goes to right of 3. 

![[Pasted image 20250517144839.png]]
**REMOVE(2)**
Find in-order successor of 2
inorder(5) gives inorder(2) gives inorder(1) gives 1
inorder(2) processes 2, then gives inorder(3) gives 3(no left child to process), gives inorder(4) gives 4
inorder(5) gives 5 etc etc.

Successor of 2 is 3, so swap 2 with 3, and then delete the original 3 giving
![[Pasted image 20250517145852.png]]
REMOVE 7, has 2 children, in-order successor is 8.
Swap 7 with 8, delete original 8, gives:

![[Pasted image 20250517150116.png]]

Then remove 5, copy the 6 to the 5, and then delete the original 6, yielding. 

![[Pasted image 20250517150302.png]]
![[Pasted image 20250517150511.png]]
![[Pasted image 20250517150517.png]]
![[Pasted image 20250517150535.png]]
T1 is a heap(the bottom image).
![[Pasted image 20250517150613.png]]
AS T1 is heap, perform removeMin()
![[Pasted image 20250517152158.png]]
Then perform insert(1)
![[Pasted image 20250517152302.png]]
![[Pasted image 20250517152307.png]]
Result, the heap is not identical




REMOVE vs REMOVEMIN, INSERT(BST) vs INSERT(MINHEAP)
REMOVE MIN, REPLACE WITH LEFTMOST LEAF NODE, THEN WORK THAT INTO PLACE, REMOVE, COPY, DELETE ORIGINAL


![[Pasted image 20250517152643.png]]
![[Pasted image 20250517153524.png]]
Then just perform remove min 7 times, and then build array back up.

![[Pasted image 20250517153758.png]]
![[Pasted image 20250517153811.png]]


![[Pasted image 20250517153805.png]]
![[Pasted image 20250517153851.png]]
![[Pasted image 20250517154025.png]]


Remove for BST - If has 0 children, just delete, if node to delete has 1 child, delete the node, and move the child up. If has 2 children, find in-order successor, copy it to where node to be deleted is, and then delete original.
RemoveMin for min-heap - locate minimum, replace with leftmost leaf node, and simply work into place, always swapping with smallest child. 
Insert for heap - insert at leftmost available leafnode, and swap upheap into place.
Insert for BST - start from top, and keep comparing, go left if node to add is less than current node, and go right if node to add is greater than current node.



### Hash Maps
![[Pasted image 20250517155750.png]]
![[Pasted image 20250517155957.png]]
0, 16, 2, 32, 4, 64
Via linear probing, as scan right, from the collision, until find empty position, hence 16 goes into 1. With separate chaining, would have had list-based map placed at [0] for all collisions to be accessed with O(n). 

![[Pasted image 20250517160925.png]]
![[Pasted image 20250517160953.png]]
With linear probing, a call to get walks right until empty space is found (or all cells inspected), as the assumption is that the key would be placed there. Need to use lazy deletion, i.e., mark as deleted, and skip it in get, otherwise would return early without locating the key due to the gap in the array. 

![[Pasted image 20250517161101.png]]

[ -, -, -, -, -, 4, 5 ]
(11+1) % 7 = 5, collision with 4 at [5], scanning right, [6] is taken up by 5, wraps back around due to mod N, (h(k) mod N and places at 0
![[Pasted image 20250517161450.png]]
