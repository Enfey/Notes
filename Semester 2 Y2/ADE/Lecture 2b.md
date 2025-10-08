f(n) = | n if n is even
      | 1 if n is odd
n $\le$ c . n, c = 1, n0 = 1
works for both, definition requires one c and one n0. 
Big O behaviour is O(n). Not O(1) as would need a c that grows at same rate as n, wouldn't be constant..

Analysing ratios:
For above function, to find limit of f(n)/n, as n tends to infinity:
	limit is 1 if n is even, n/n = 1
	limit is 0 if n odd, 1/n = 0/approaches 0 as n grows larger.
	Limit doesn't converge on single value, oscillates between two behaviours. Big-Oh, ignores the constant and has well-defined growth behaviour. 
Where the ratio is pulled from g(n), = 1 from O(1).
Can still come to a conclusion if ratio is omitted or does not converge on single value.

f2(n) = n if n is even
    4 if n is odd
n $\le$ c . n, 1, n0 =4, is O(n)

## Big-Oh usage
Consider 2 functions
f1(n) = n  if n is even
	= 1 if n is odd

f3(n) = 1 if n is even
	= n^2 if n is odd.

Intuitively, f1 has much better scaling behaviour. Cannot get f1 is better from simple comparison of values, because f3 is better for even values.
But f1 is O(n), proved earlier, and f3 is O(n^2), which demonstrates that in the worst case f1 performs better. 

3n - 6 $\le$ 1 . 4n + 5, n0, 1
7n - 2 $\le$ 1 . n, n0 = 7
3n^3 + 20n^2 + 5 $\le$ 28 . n^3, n0, 1


![[Pasted image 20250221065903.png]]
![[Pasted image 20250221070042.png]]
By win, we mean lose algorithmically. 

Big Oh is a binary relation, f is O(g) can be regarded as a binary relation between two functions f and g. 

### Properties of Big-Oh
### Reflexivity
Every function is Big-Oh of itself, that is f(n) is O( f(n) ) for c = 1, for all n.

### Symmetric
Big Oh is not symmetric.
If f(n) = O(g(n)), it does not necessarily imply g(n) = O(f(n)).
	Example: f(n) = n, = O(n^2), but n^2 $\not =$ O(n)


### Transitive
Given f = O(g) (x) and g= O(h) (y), can we show f = O(h) (z)
E.g.,
1 is O(n) and n is O(n^2)
forces 
1 is O(n^2)

exists c1, c2, n1, n2 such that:
	f(n) $\le$c1 g(n), $\forall n \ge n_1$
	g(n) $\le$ c2 h(n), $\forall n \ge n_2$

c1 = 1, n1 = 1
c2 = 1, n2 = 1
But logically,
then:
f(n) $\le$ (c1, c2)h(n), $\forall n \ge n_3$
c = c1 * c2 ?

## Big-Oh as a set
Big Oh as a binary relation is reflexive and transitive, but not symmetric. It behaves like ∈, not like =. So helps to think of 'O(n)' as a set of functions, with each function f in the set, f ∈ O(n), satisfying 'f is O(n)'. E.g., f(n) = 1, f(n) = 3n, belongs to the set O(n).
	So then, O(1) ⊆ O(n), etc, giving a hierarchcal classification, rather than one based on equality. 
Using equality would give rise to statements like the following:
	1=O(1), 1 = O(n), therefore O(1) = O(n), which is incorrect
	Or 1 = O(1), O(1) = 1

