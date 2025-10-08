## Linked lists
### Singly linked list
#### Node
==Each node== in singly linked list ==contains some data==(either primitive, or pointer to some data object) and a ==pointer to the next node== in the sequence.

Node { 
	Node* next; 
	void* k; 
}

#### List
Often contains ==head== and ==tail pointer,== and the ==length/num of elements.==

List {
	Node* head;
	Node* tail;
	int n;
}

==Need to determine== via this structure, ==which traditional operations==(==insertion, deletion, deletion(given position or value))== are i==mplementable efficiently.==

#### Operations and their efficiency
##### ==Insert at head==
1. ==Create new node N==
2. Set N Node* next = previous head
3. Set list Node* head to new head
O(1), does not need access n elements to do this.
##### Remove at head
1. If list empty, nothing to delete
2. Otherwise, set list Node* head to current head.next
3. Delete old head node, or let gc as not referenced
O(1), does not need access n elements to do this
##### Insert at tail - BACK OF LIST
![[Pasted image 20250518213500.png]]

1. ==Create new Node N== 
2. Set ==tail.next== = ==N==
3. Set list Node* tail to point to N.
O(1).
O(n) if no tail pointer, walk list until .next = null, then do steps 1-2.
##### Remove at tail
1. ==If list empty== or ==contains 1 element,== set ==head to null==
2. Otherwise, ==traverse list== to find ==second to last node==
3. Set ==second-last node next pointer to null==
4. Set ==list tail = second to last node.== 
O(n) as need to traverse list, inefficient, can be made more efficient with Node* prev; in each node, such that we can access previous elements and remove at tail more easily. 


### Doubly linked list

#### Node
==Each node in doubly linked list has:==
- ==Pointer to next node==
- ==Pointer to previous node==
- Some data (either primitive, or pointer to some data object)

Node {
	Node* prev;
	Node * next;
	Type data;
}

### List
The overarching list structure contains:
- Pointer to head of list
- Pointer to tail of list
- int, specifying length

List {
	Node* head;
	Node* tail;
	int n;
}

Removal at tail now O(1), go to tail pointer, access prev node, make prev nodes next pointer null, set list Node* tail = new tail. Old tail deleted or gc.

More storage needed for linked list, increased cache usage.
More maintenance. 
So not suitable for resource constrained environments.
