## Classification of functions
In CS, need way to group together functions by their scaling behaviour as n increases, the classification should:
- Remove unnecessary details
- Be relatively quick and easy
- Handle 'weird' functions that can happen for runtimes
- Be mathematically well defined
Bes achieved by 'big-Oh notation and family'
- O(Big-Oh)
- Ω(Big-Omega)
- Θ(Big-Theta)
- ο(little-Oh)
- ω(little-omega)
## Big-Oh notation
Motivation: 
	Suppose we have 2 functions:
	1. 4n + 5
	2. 3n - 6
	 Want to express that the behaviour is driven by the fact both are linear in n, and suppress the details of the exact function. 
### Big-Oh
Big-Oh is a way to describe an upper bound on the growth rate of a function. Concretely, say that a function f(n) = O( g(n) ) ("f is Big-Oh of g") iff there exist constants c ≥ 0 and $n \ge n_0$ such that the following holds.

$$f(n) \le c \ . \ g(n)$$
For a sufficiently large n, the function f(n) does not grow faster than a constant multiple of g(n) which always upper bounds f(n).
Threshold = n0, point from which the inequality holds for all n. 
![[Pasted image 20250221051547.png]]

f(n) = 1

1 ≤ 1 . 1 
2 ≤ 1 . 2

f(n) = 1

1 ≤ n . 1 n0 = 1

f(n) = k is O(n) for any k

k ≤ c . n, c = k, n0 = 1

f(n) = k n, is O(n)
k+n ≤ c n, c = 2, n0 = k

f(n) = n is not O(1)

n ≤ 1 . c, c = n(not allowed, infinity not allowed for constant so fails.)


IF MAKE g(n) grow super fast, O(g(n)) will always be great enough, e.g., for any constant function, always right to say they are O(2^n). But want to know tight bound to describe worst-case scenario. 

f(n) = n 

n $\le$ c . n^2,  c = 1, n0 = 1

2n < 1 x 4n

