> **BIG OMEGA** Given 2 positive functions f(n) and g(n), f(n) is O(g(n)) iff $\exists$ positive constants $n_0$, $c$ **s.t**. $f(n) \ge c . g(n)$, $\forall{n} \ge n_0$


> **BIG THETA**: Given positive functions f(n) and g(n), f(n) is O(g(n)) iff $\exists$ positive constants $c', c'',$ and $n_0$ **s.t.**
> 	$f(n) \le c . g(n), f(n) \ge c . g(n), \forall{n} \ge n_0$ 
> Grows exactly as fast as.

> **Little-Oh**: Given positive functions f(n) and g(n) f(n) is O(g(n)) iff $\forall$ c > 0, $\exists$ n0 s.t. 
> 	f(n) < c . g(n), $\forall{n} \ge n_0$ 
> 	$c \in \mathcal{R}^+$ 



## Prove that 2n+1 is Ω(3n) and hence 2n+1 is Θ(3n)
2n+1 >= c . 3n
2n + 1 >= 1/3 . 3n
2n + 1 >= n
trivially pick n0 = 1
c = 1/3

Can trivially satisfy big oh using c'' = 1
Therefore big theta holds for 3n.


## Prove that 5 is Ω(1) and hence 5 is Θ(1)

5 >= c . 1
pick c = 1, n0 irrelevant, trivially true

5 <= c . 1
c = 5, n0 irrelevant

5 is Θ(1) with c' = 1, c'' = 5, and n0 = 1

## Prove that 4 is Ω(2) and hence 4 is Θ(2)

4 >= c . 2
c' = 1, n0 = 1

4 <= c . 2
c'' = 2, n0 = 1

## Prove that 2n+1 is  Ω(n) and hence 2n+1 is Θ(n)
2n+1 >= c . n
c = 1, n0 = 1

2n+1 <= c . n
1 <= (c-2)n
1 <= n, c = 3
c = 3, n0 = 1

hence, works for c' = 1, c'' = 3, n0 = 1.


## Prove that n^2 is Ω(2n^2)

n^2 >= c . 2n^2
n^2 >= 0.5 . 2n^2
n^2 >= n^2
c = 0.5, n0 = 1

## Prove that n^2 - 3 is Ω(n^2)
n^2 - 3 >= c . n^2
n^2 >= c . n^2 + 3
n^2 - c . n^2 >= 3, pick any c not 1, 0.5 for ease.
JUST PICK n0 = 3
n^2 - 0.5 . n^2 >= 3
0.5 n^2 

## Prove that n^2 - 5n is Ω(n^2) and hence is Θ(n^2)
n^2 - 5n >= c . n^2 
n^2 >= c . n^2 + 5n
n^2 - c . n^2 >= 5n 
n^2 - 0.5 . n^2 >= 5n, c = 0.5
0.5 . n^2 >= 5n
n^2 >= 10n
n >= 10, divide by n

n0 = 10, c = 0.5

n^2 - 5 <= c . n^2 
c = 1



## Prove that n^2 + 1 is Ω(n^2)
n^2 + 1 >= c . n^2 
n^2 >= c . n^2 - 1
1 >= 1 . 1 - 1, c = 1, n0 = 1


## Prove that 2n^2 + 1n - 1 is Ω(n^2)
2n^2 + 1n - 1 >= c . n^2
n^2+1n-1 >= 0
n0 = 1, c = 1

## Prove that 2n^2 + 1n - 1 is O(n^2)
2n^2 + 1n - 1 <= c . n^2
1n - 1 <= (c-2)n^2
1-1 <= (c-2)n
0 <= n
n0  = 1, c =3


## Prove that n log n + 3n is O(n log n)
n log n + 3n <= c . n log n
3n <= (c-1) n log n
3 <= (c-1) log n
c = 2, n = 8

## Prove that n log n + 3n is Ω(n log n)
n log n + 3n >= c . log n
n log n + 3n >= 1 . log n
3n >= log n
n = 8, c = 1


## Prove that log^2 n + 10 log n is O(log^2 n)
log^2 n + 10 log n <= c . log^2 n
10 log n <= (c-1)log^2 n
10 <= (c-1)log n //divided
10 <= log n
c = 2, n0 = 1024


## Prove that  log^2 n + 10 log n is Ω(log^2 n)
log^2 n + 10 log n >= c . log^2 n
pick c = 1
log^2 n + 10 log n >= 1 . log^2 n
10 log n >= 0
n = 1024


## Prove that 5 is o(1)

5 < c . 1
5 is not o(1), c has nothing to depend on which could raise it above 5 for all c > 0

## Prove or disprove that 5 is o(n)
5 < c . n

lim n -> infinity, 5/n = 0, thus proven

OR

5/c < n

Thus n0 = 5/c + 1, trivially chosen


## Prove or disprove that n is o(n^2)
n <  c . n^2 

1 < c . n
1/c < n
n0 = 1/c+1

## Prove or disprove that 1 is o(log n)
1 < c . log n
1/c < log n
To isolate n on rhs, can do 2^x on both sides
2^1/c < 2^log n // cancels out the log
2^1/c < n
n0 = 2^1/c + 1

## Prove or disprove that log n is o(1)
log n < c . 1
log n/c < 1
2^log n < 2
n/c < 2
Will not hold for any values of c, no matter what n0 picked

## 







# RULES FOR DERIVING BIG OH

## Drop smaller terms
Prove that n^3 + 2n^2 is O(n^3) using drop smaller terms

n^3 + 2n^2
n^3(1+2n^2/n^3)
n^3(1+2/n)
2^n -> 0 as n->infinity, so drop it
n^3(1) = n^3

Prove that n^3 + 2n^2 log(n) is O(n^3)
n^3(1+2n^2 log(n) / n^3)
n^3(1+2log(n)/n)
n grows faster than logn, regardless of the 2, so discard
leaving n^3(1)


## BIG THETA PROOFS WITH MINUSES
### Prove that n^2 - 5n is Ω(n^2) 
n^2 - 5n >= c . n^2
n - 5 >= c . n
n >= c . n + 5
n - c . n >= 5
n - 0.5 . n
n/0.5 >= 5
n = 10, c = 0.5

## Prove that n^3 - n^2 - 5n - 4 is Ω(n^3)
n^3 - n^2 - 5n - 4 >= c . n^3
n^2 - n - 5 - 4 >= c . n^2
n^2 - n >= c . n^2 + 9
n - 1 >= c . n 
n >= c . n +1
n - c . n >= 1
n - 0.5 n >= 1
n/0.5 >= 1








