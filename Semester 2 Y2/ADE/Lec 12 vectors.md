### Abstract data type
An ADT is a logical description of how data is organised and what operations can be performed on it, without specifying concretely how it will be implemented.
Implementation independent, and defines its behaviour from the point of view of a user. 
Stack with operations: push(), pop() and peek() form an ADT

### Concrete data type
A CDT is the *actual implementation* of an ADT in a specific programming language, focused on the implementation, and how data is stored and the operations are implemented.


## Vectors
A vector is a dynamic array - a data structure that stores elements of the same type in contiguous memory but can grow or shrink in size during runtime. We hide this implementation in an ADT.

#### Methods of Vector ADT
- **Get** - returns the element at the specified position in the Vector
- **Set** - sets the element at the specified position in the Vector
- **Size** - returns the number of elements in vector.
- **setSize** - sets the size of this vector, if new size greater than current size, appends null items to the end of the vector. If new size less than, discards the elements at the end
- **Delete** - deletes the element at the specified position in the Vector
- **Add** - adds element after the 'nominal end' ($0,...,size()-1$) of the Vector, acts like a push, underlying implementation increases in size to cope rather than throwing exception. 


### Vector CDT
- Assume implemented as a simple array for purpose of course
	- For simplicity, assume Vector of int, stored elements are simply integers
```java
class Vector {
	int[] A;
	int size;
	int get(int k) { return A[k]; }
	void set(int k, int e) { A[k]=e; }
}
```
Distinguish between capacity and size
**Size**: Total number of elements stored in $A$ 
**Capacity**: The length of the array $A$ 
These do not need to be equal, some indices may be unused.

**Add** easy case: if size < capacity, already have space to store new element, just place at correct position and increment size.
```java
add (Element e){
	if (size < capacity)
	A[size] = e //adding at end of current elements remember
	size++
}
```
This is simply O(1), as is typical of array access due to calculating offset with regard to start of the array, $elementAddress = baseAddress + i \times typeSize$


**Add** hard case: if size=capacity, the array is full, we then have to create a new array with a larger capacity. 

```java
resize(int newCap){
	newA = new int[newCap]
	for each k in 0...size-1
		newA[k] = A[k]
	A = newA //old A will be garbage collected, ref out of      scope
}
```
Cost is $O(size)$ due to copying.
![[Pasted image 20250511213724.png]]

**Add costs**:
- Best case add is O(1)
	- if (size < capacity), cost $O(1)$
- Any resize is O(size) as need to copy all of array
	- if (size = capacity), cost $O(size)$ for resize and cost $O(1)$ for the add

Picking new capacity
- Can pick any $newCap > capacity$
- Will consider two policies
- **Incremental** - $newCap = capacity + d$ where $d >= 1$ is some constant
- **Doubling** - $newCap = capacity \times 2$ 
Need to do some analysis of how well they perform.

Analysis of add cost
- If simply take worst case, cost of add is O(n) due to a potential resize
	- n = size
- This would be independent of the resize policy?
- And generally, do not always resize, may do many add operations, but few resizes.
- So how can we account for many add operations, but few resizes?
- In practice, often do long sequence of add operations
	- E.g., if reading data from a file, without knowing how many elements the array will need at compile time
- So we can:
	- 1. Consider the total cost of a long sequence of add operations
	- 2. Compute the average cost per add in that sequence
- For simmplicity, consider the cost of starting with empty vector, and then doing $n$ **add** operations.
- Then, compute:
	- $T(n)$ = total cost of the entire sequence of $n$ add operations
	- the mean cost per operation over the complete sequence will then be $T(n)/n$
- This is called the **amortised cost** of the operation, and it looks at the average cost per operation over a sequence of operations, even if some operations are expensive.


Incremental strategy amortised analysis:
take $d$ = 3, starting from empty, and capacity = 3 and count the costs of a sequence of $add(0)$.
![[Pasted image 20250511215034.png]]
Every add costs the usual '1':
- Contributes $n \times 1$ to  $T(N)$
If $n = size$ so far
- Every $d$ operations we incur a resize cost of $n$
- Sizes at a resize will be a sequence
	- d, 2d, 3d.
- Giving overall cost of resizes:
	- d+2d+3d+ ... + N = d(1 + 2 + 3 + ... + N/d) (factored)
	- Number of resizes needed is just N/d:
		- ![[Pasted image 20250511220301.png]]
		- e.g., 6/3 = 2, 2 resizes needed by size 6, all the way up to n.
	- The arithmetic sum is O(n^2)
	- ![[Pasted image 20250511221654.png]]
	- Hence, T(N) is O(n^2)
	- Hence, amortised cost (T(N^2)/n) is O(N) or O(size)

Doubling strategy amortised analysis:
Starting from empty, and capacity = 2, count the costs of a sequence of add(0)
1. [ _ _ ] size = 0, cap = 2, no resize: cost = 1
2. [0, _ ]size = 1, cap = 2, no resize: cost = 1
3. [0, 0 ]size = 2, cap = 2, resize: cost = 2+1
4. [0, 0, 0, _ ]size = 3, cap = 4, no resize: cost = 1
5. [0, 0, 0, 0 ]size = 4, cap = 4, resize: cost = 4+1
- We get a sequence of costs
	1 (add),
	1 (add),
	2 (resize) + 1 (add) = 3,
	1 (add),
	4 (resize) + 1 (add) = 5,
![[Pasted image 20250511223928.png]]
![[Pasted image 20250511224031.png]]
The 1,s after the resize indicate the operations to follow which will be used to spread the cost over.
![[Pasted image 20250511223915.png]]

The resize costs themselves are a geometric series

1+2+4+8+16+...+2^k
![[Pasted image 20250511234842.png]]
![[Pasted image 20250511224853.png]]
![[Pasted image 20250511234851.png]]
![[Pasted image 20250511235210.png]]
???
Amortised cost of add
- Incremental strategy: capacity += const
	- Worst case: O(n) per add
	- T(N) is O(n^2)
	- Amortised cost per add is O(n)
		- This is bad, as many add are just O(1)
- Doubling strategy: capacity *= 2
	- Worst case: O(n) per add
	- T(N) is O(n)
	- Amortised cost per add is O(1)
	- Amortises to a constant
- 

In general, some operation might take worst case time, T_W
If do a sequence N operations the call the actual cost T(N)
T * N is clearly O( N * T_W)
But in some cases the actual cost can be strictly smaller.



