### Map ADT
- A map stores a collection of key-value pairs
- The primary difference between PQ ADT is that the Map allows to search for entries by their key
	- In contrast, PQ only supports removal of the minimum/highest priority key
- A map also allows removal of entries by their key
- Also allows insertion of new entries when provided with a KV pair
- Multiple entries in a Map, typically, are not permitted
	- Inserting an entry when inserting an existing key will overwrite the old-key value pair
	- Multiple copies of a key are allowed within a 'multi-map'
	- Multiple copies of a key are permitted within a PQ.
#### Usage
- Maps are very useful and common in many tasks, e.g., storing things by a unique key, e.g., storing employee data by employe ID
- May also be known as dictionary or associative arrays
	- As they associate some value with a key, and the key need not be an integer
- ![[Pasted image 20250515140237.png]]
#### Main Operations
##### Get
`V get(K k)`
- Gets the value associated with the key $k$, or null if $k$ $\notin$ $Map$ 
- Does not change the contents of the map, simply retrieves.
##### Add
`(K,V) add (K k, V v)`
- Add the KV pair to the map/storage
- If the key $k$ is already in the map, then it will be overwritten to prevent multiple keys
	- The existing KV may be returned
##### Remove
`V remove (K k)`
- Gets the value associated with the key $k$, or null, if $k \notin map$ 
- Also removes the entry $k$ refers to.
##### Size
`int size()`
- Returns the number of KV pairs stored
##### GetMin
`(K, V) GetMin()` 
-  Returns the minimum value Key $k$ in the current map, assuming the keys have a total order.


### List-Based map
- Straightforward implementation of a Map ADT, but not efficient to implement a map using a list - either singly  or doubly linked.
- Also store separate size counter n so that size() is O(1) and maintain it when calling add and remove.
- If the list is unsorted (using integers for keys for simplicity):
	- `Add(int k)` is $O(1)$ - can just place entry at the head/start.
	- `remove(int k)` is $O(n)$ as must scan the entire list for the key.
- If the list is sorted (smaller first):
	- `Add(k)` is $O(n)$ must scan list to find correct position to insert it.
- Overall, the operations are $O(n)$ due to the need to traverse the list
- As such, this representation is only suitable for maps of small size.
- 2 alternatives: Hash Maps, Binary Search trees
#### (K, V) get(K k) for lists
1. Walk along list looking for key $k$ 
2. If found, return associated value
3. Else return null
Worst case: $O(n)$
#### void||(V v) add(K k, V v) for lists
1.  Walk along list looking for key $k$ 
2. If find it, return the currently associated value, and overwrite with the new value v.
3. Else insert KV as a node at the end of the list (tail.)
Cannot simply place new entry at start of the list, would lead to duplicate, and would never return the other key, so need to check that $k$ not already in use.
Worst case: $O(n)$ 

#### (K, V) remove(k) for lists
1. Walk along list looking for $k$ 
2. If found, return the currently associated value and remove the node
3. Else return null
Worst case: $O(n)$


