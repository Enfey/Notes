## Relatives of Big-Oh
- big-Oh: O
- big-Omega : Ω
- big-Theta: Θ
- little-oh: o
- little-omega: ω

### Big-Omega def
For two positive functions f(n) and g(n), of single variable n then:
f(n) is Ω( g(n) )
iff
exists constants c > 0 and n0 >= 1 such that 
for all n $\ge$ n0 
f(n) $\ge$ c . g(n)

Need c greater than 0 this time, and greater than or equal rather than less than or equal. 

E.g.,
f(n) = n is Ω(n)

n $\ge$ 1 . n, n0 = 1

E.g., 
f(n) = n^2 is Ω(n)
n^2 $\ge$ 1 . n, n0 = 1

E.g.,
f(n) = n^3 - n is Ω(n^3)
n^3 - n $\ge$ c . n^3 
n^3 (1 - n/n^3) $\ge$ c .n^3
n^3 (1)
n^3 $\ge$ c . n^3 (dropped)
given that the big omega of n^3 is n^3 trivially and the multiplication rule, this is true. 





![[Pasted image 20250222042730.png]]
THIS IS THE OPPOSITE OF BIG OH
f(n) is Ω( g(n) ) states that f(n) grows at least as fast as g(n) at large n, may grow faster. Puts lower bound, describes best case by giving guarantee on at least certain amount of time. 

Big Omega is reflexive, not symmetric, and transitive. Reflexive is trivial, symmetric simple, recall transitive definition. 

#### Linking big-Oh and Big-Omega
Suppose say f is O(g)
What can we then say about big Omega
𝑓 (𝑛) ≤ 𝑐 𝑔 (𝑛) ∀ 𝑛 ≥ 𝑛0
Can assume c > 0
𝑔 (𝑛) ≥ 1/c .  f(n) ??
f ∈ O(g) -> g ∈ Ω(f)
𝑓 ∈ Ω 𝑔 → 𝑔 ∈ O(f)

E.g.,
f(n) = 3n + 2
g(n) = n

3n + 2 ≤ 4 . n, n0=2
n ≥  1/4 . 3n+2, n0=2

If f(n) is at most some multiple of g(n), then g(n) must be at least some fraction of f(n).






Can apply multiplication rule and still drop smaller terms e.g., n^3-n is Ω(n^3)
n^3 - n $\ge$ 0.5 . n^3, n0 = 2

Do not take the smallest term, esp on assessments, used to denote  ' best case for algo X is Ω(g(n)) and so will scale well/poorly '
Prove for the largest term.

One also may say if the worst case behaviour of an algorithm is only partially known, then we can specify the best-case and derive the worst case from that. 

### Big-Theta
For two positive functions f(n) and g(n) of a single variable n, then:
f(n) is Θ( g(n) )
iff
exist constants c1, c2 > 0 and n0 >= 1 such that
f(n) ≤ c1 g(n)
f(n) ≥ c2 g(n)
Defines a tight bound for a function, 'grows exactly as fast as' (though ignoring constants, does not imply f(n) = g(n))

f(n) = n if n is even 
= 2n if odd

n ≥ c . n, n0 = 1, c = 1
2n ≥ c . n, n0 = 1, c = 1
, trivially O(n), Ω(n)
Therefore its big-Theta behaviour is Θ(n)

Big theta is reflexive and transitive.

f(n) = n
OMEGA
n^2 + n ≥ c . n^2, 
take from both sides
n ≥ c . 0, n0 = 1, c = 1

f(n) = n^2 + n if even
n if odd

BIG OH
n ≤ c . n^2 
1 ≤ c . n, n0 = 2, c = 2.
n^2 + n ≤ c . n^2, n0 = 2, c0 = 2
works!

BIG OMEGA
n^2 + n ≥ c . n^2
1 + n ≥ c, for arbitrarily large n, inequality does not suffice, cannot set constant to infinity.

Therefore does not have a useful big theta.










![[Pasted image 20250224002657.png]]











