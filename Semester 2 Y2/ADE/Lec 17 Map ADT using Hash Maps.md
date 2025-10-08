### Hash Maps
- CDT useful for implementing the Map ADT
- Idea: convert each key into an index into a large array
	- Array is called the **hash table**
	- The conversion of key to index is called a **hash function**
- Look up of keys and insertion and deletion in a hashmap usually runs in $O(1)$
	- This is predicated on the design of the table and hash function - needs to be done carefully if we want to ensure $O(1)$ 
- Very useful as auxilary data structure.

#### Hash Function
- A hash table will typically allow the key to be an arbitrary type, and does not restrict it to be an integer
- Even if key is an integer, may not be in a suitable range to be an index
	- e.g., could be negative, or too large
- We need a hash function $h$ to convert Key $k$ into a suitable index $h(k)$ into the array.
	- `unsigned int h(Key k){...}`
##### Convert and compress
- Hash function typically has two stages
	1. Conversion to a large unsigned int
	2. Compression of the range of integers to allow them to be used as an index in the hash table.
##### Hash Function: Collisions
- It is standard with hash functions that there can be multiple keys that map to the same index.
- If two keys have
	$h(k1) = h(k2)$ 
	this is called a collision. 
- Must decide how to handle the collision
- This collision handling makes the hash map less efficient
- Hence, want hash functions that reduce the chance of collisions. 
	- If can reduce the collisions to zero, this is called a perfect hash - but it is rare, and then were using the resulting key as if it were guaranteed to be unique. 

#### Hash Function: Reducing collisions
- Want to reduce chance of 2 keys mapping to same index
	- Want to spread the value of h(k), i.e., the hash function results should be spread uniformly
	- We want no discernable patterns
	- Otherwise, a similar pattern in the distribution of the keys could cause an 'alignment' in resulting indexes, increasing collisions
		- Example *if isEven(k) implies isEven(h(k))*
		- If all keys are even, then only half the table would be used, and collisions would increase drastically.

#### Hash Function: Checksums
- No properties of the $k$ should be discernable given $h(k)$, i.e., one-way operation that totally obscures $k$, whilst also being deterministic
- Slightly cryptographic, as hiding information about the $k$ 
- There are cryptographic hash functions that do this very well - they can be used as '**Checksums**'
- Checksums are a small sized value/piece of data derived from a larger block of data e.g., a message or packet, serving as a quick fingerprint. A cryptographic checksum is a hash function which takes this larger block of data, and produces the checksum, which can then be reproduced, given the exact same message or packet.
- The catch is that these are too slow to compute generally, when hashing a large number of keys, we need the hash function to be fast to compute.

#### Hash Function: Birthday Paradox
- Even when h(k) values are (psuedo)-randomly distributed, collisions are more likely than might be expected
- k=person, h(k) = birthday(day and month)
- Birthday paradox: If have a random group of people, how many do we need to have a 50% chance that two or more of them have the same birthday?
	- Birthdays are uniformly and randomly distributed.
- Actual answer: only 24 people are needed.
- Collisions are thus more likely than might be intuitive.
![[Pasted image 20250515162951.png]]

#### Hash Function: Compress
Given a hash table of size $N$ we need to compress the integer key $k$ to be in the range $0,..., N-1$.
Obvious choice is $h(k) = k \ mod \ N$ 
Tends to lead to too many collisions, and is too dependent on the size.
Generally, use **'Multiply, Add, and Divide' (MAD)**
$h(k) = (a \ k + b)\ mod \ N$ 
Where $a$ and $b$ are carefully chosen constants.

Suppose we had $a=6$ and $N$ = 16 and $b$ is odd
Then all keys would map to odd entries of the hash table and would only use half the table, as 6 and 16 have a factor, 2, in common. 

h(3) = (6 $\cdot$ 3 + $odd$ ) mod $16$ 
![[Pasted image 20250515163721.png]]

![[Pasted image 20250515163825.png]]

Hence we need that $a$ and $N$ are co-prime - no factors in common. 
If $N$ is a power of 2, then $a$ should not be even.

#### Collisions: Separate Chaining
- Simplest way to handle collisions
- At each index, when it is used(or when the collision itself occurs) create a pointer to a separate list-based map that handles all the keys hashing to that index.
- All the insert/delete operations are simply delegated to the list-based Map stored at $h(k)$ 
- Number of keys should be much smaller, then might be practical to use a list-based Map. 
- ![[Pasted image 20250515164158.png]]
#### Collisions: Open addressing
- Separate chaining has the disadvantage of requiring extra memory for the list-based maps
- Instead, try to find ways to use existing spaces in the existing hash table wherever possible.
- This is only possible if the key $k$ has the possibility to be placed at an index other than $h(k)$ 
- Such systems are called **open addressing**

##### Collisions: Open Addressing: Linear probing
Given a key $k$, if the entry for $h(k)$ is already occupied, then we move to the right until find an empty position. 
The array is treated in a circular fashion, once we get to the end, then we start again at the beginning.
That is, we try/probe:
	h(k)
	(h(k)+1) mod N
	(h(k)+2) mod N
	(h(k)+3) mod N
	...
	$(h(k)+i)$ mod $N$ 
	Mod N takes you back to the beginning when reach size?
		If $h(k) + i \le N$ then $(h(k)+i) \ mod \ N$ = $h(k) + i$
		![[Pasted image 20250515164906.png]]
We do this until we find an empty cell, and place the entry there OR we have tried all cells, and the hash table is full, in which case we need to expand the hash table using this collision handling mechanism.
Always store single KV pair in this collision handling scheme, as need the key to support the map ADT operations. 

###### Example: 
- $N$ = 8, $h(k)$ = $k \ mod \ N$ 
- Insert keys 14, 15, 8, 22 in this order.
	- 14, 15, and 8 have no collisions
- But 22 mod 8 = 6, collides with 14 mod 8 = 6, same index
	- Probe index = (6+1) mod 8 = 7, fails
	- Probe index = (6+2) mode 8 = 0, fails, circular probe
	- Probe index = (6+3) mod 8 = 1, succeeds.
- Generally results in short walks, where the hash table is fairly empty.![[Pasted image 20250515165302.png]]
###### Example: Get
- When lookup, need to follow same procedure:
	 Walk **right** until either:
	1. Find the key
	2. Find an unoccupied space, and then can stop, if the key were present, then the key would have been placed there
- E.g., previous example, had![[Pasted image 20250515165627.png]]
- Suppose wanted to do get(22)
	- h(22) = 6, so start probing from index = 6
	- In this case, find key = 22
- ![[Pasted image 20250515165740.png]]
- 
###### Example: Deletion
- Previously, had ![[Pasted image 20250515165758.png]]
- ![[Pasted image 20250515165842.png]]
- Would walk right, see the empty space, and early return without finding the index 22 maps to.
- How do we then safely remove elements
- One answer:
	- Find $x$ using get and set the entry back to blank
	- Fix the sequence on its right hand side
		- Fix: Move such entries, by removing them and then reinserting them all. 
	- Shit solution basically.
- ![[Pasted image 20250515170158.png]]
###### Example: Big Jumps instead of walk right
- Instead of moving one cell to the right circularly, could do a jump of $d$ cells
- That is we try:
	$h(k)$
	$(h(k)+d) mod N$
	$(h(k)+2d) \ mod \ N$
	$(h(k)+3d) \ mod \ N$
	...
We require that d and N are co-prime, have no factors in common. 
![[Pasted image 20250515171823.png]]

##### Collisions: Open Addressing: Double hashing
Extension of linear probing, but in which the size of the jump is allowed to depend on the key. In particular, depends on a secondary hash function $d(k)$.

That is, we try: 
	h(k)
	$(h(k)+1 \times d(k)) \ mod \ N$
	$(h(k)+2 \times d(k)) \ mod \ N$
	$(h(k)+1 \times d(k)) \mod \ N$

Obviously need $d(k)$ $\gt$ 0, otherwise just probe the same cell again. 
This is in an effort to reduce clustering of KV pairs compared to plain linear probing.
So have 1 hash function for starting slot, and secondary hash for step size
For maximal coverage, d(k) and N are co-prime, so hit every slot, rather than cycling through a subset.
Wrap back around still due to the modulo. 

###### Double Hashing: Example
- Take h(k) = k mod 8 again
- Take d(k) = 1 + (4 * k) mod 7 - always in range 1...7
- Again, insert keys 14,15,8,22.
- Firstly, compute the $h$  and $d$ values
- ![[Pasted image 20250515172549.png]]
- The 14, 15, and 8 do not collide so again get:
- ![[Pasted image 20250515172608.png]]
- The 22 collides with 14, as both have $h(k) = 6$, so just probe (h(22)+1xd(22)) mod 8, which is 3, which is free.
- Through carefuly design, less likely to have long walks of probing due to reduction in clustering via right-walks.
###### Performance: Best case
- Suppose have a table with size $N$ and with $m$ entries
- Best case: O(1)
	- If have no collisions, simply insert index, lookup and remove is simply a matter of looking at index h(k), which takes a constant amount of time to compute.
	- Essentially array access + the cost of computing the hash function
###### Performance: Worst case
- Suppose have a table with size $N$ and $m$ entries
- Worst case: O(n)
	- As it might happen that all keys have the same hash value
	- Separate chaining: has a list of length $n$ with operations averaging to O(n)
	- Linear probing: we get a sequence on $m$ entries and have to walk to them.
	- Double hashing: here would need all keys to happen to have same h(k) and d(k) and so less likely????
![[Pasted image 20250515173310.png]]
![[Pasted image 20250515173324.png]]
![[Pasted image 20250515173334.png]]
![[Pasted image 20250515173409.png]]

