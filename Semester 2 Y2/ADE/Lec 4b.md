## Little-oh
f(n) is o(g(n))
iff
for all c > 0, there exists n0 >= 1 such that
for all n >= n0, f(n) < c g(n)

is an upper bound, but is strict, ie, does not say f(n) grows as most as fast as g(n) times a fixed constant, says instead f(n) grows strictly slower than g(n).

This roughly denotes:
![[Pasted image 20250224003626.png]]

n0 is not allowed to depend on c. Inequality has  to be proved for all c, rather than there exists.

## Examples
f(n) = n
prove this functions little oh behavior is o(n)

n < c . n, for all c, for all n>n0

1 < c . 1, fails for any value of c 1 or below. 


## Rules
- May use multiplication rule:
	f1 is o(g1)
	f2 is o(g2) 
	implies f1*f2 is o(g1*g2)
- May use 'drop smaller terms'

## Mnemonics for Big-Oh family
### Big Oh
f(n) is O(g(n)) if f(n) is asymptotically 'less than or equal to' g(n). Upper bound on growth rate of a function. 'As most as fast as'

### Big Omega
f(n) is $\Omega(g(n))$ if f(n) is asymptotically 'greater than or equal to' g(n). Lower bound on growth rate of a function. 'At least as fast as'

### Big Theta
f(n) is $\Theta(g(n))$ if f(n) is asymptotically 'equal' to g(n).

### Little oh
f(n) is o(g(n)) if f(n) is asymptotically strictly less than or equal. to g(n).

## Further examples
f(n) = o(n) then, show its big omega behaviour
![[Pasted image 20250224113929.png]]



