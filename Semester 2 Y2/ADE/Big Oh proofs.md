

Given positive functions f(n) and g(n) we say that f(n) $\in$ O(g(n)) iff $\exists$ c, n0 $\gt$ 0 such that f(n) $\le$ c . g(n), $\forall{n} \ge n_0$ 
## Prove that 5 is O(1)

5 <= c .1
pick c = 5, n0 = 1


## Prove that 2n+1 is O(3n)
2n+1 <= c . 3n
2n/n+1/n <= c . 3n/n
2 <= c . 3
c = 1, n0 = 1


## Prove that 4 is O(2)

4 <= c . 2
c = 2, n0 = 1

## Prove that 2n + 1 is O(n)
2n+1 <= c . n
1 < = (c - 2)n
pick 3
1<= (3 - 2)n
holds for n0 = 1

## Prove that n^2 is O(2n^2)
n^2 <= c . 2n^2
n^2/n^2 <= c . 2n^2/n^2
1 <= c . 2
c = 1, n0 = 1

## Prove that n^2 - 3 is O(n^2)
n^2 - 3 <= c . n^2
n^2 <= c . n^2 + 3
0 <= c . 0 + 3

c = 1, n0 = 1



## Prove that n^2 - 5n is O(n^2)
n^2 - 5n <= c . n^2
n^2/n^2 - 5n /n^2 <= c . n^2/n^2
1 - 5n/n^2 <- c . 1
trivially true for c = 1, n = 1

## Prove that n^2 + 1 is O(n^2)
n^2 + 1 <= c . n^2 
n^2/n^2 + 1/n^2 <= c . n^2/n^2
1+1/n^2 <= c . 1
1/n^2 <= (c . 1) - 1
1/n^2 <= (2 . 1) - 1
pick n0 = 1, c = 2



## Prove that 1 is O(n)
1 <= c . n
trivial, pick c = 1, n0 = 1

## Prove/Disprove that n is O(1)
n <= c . 1
Would need c to depend on then, but require both to be constants, so we just pick n = c + 1 to invalidate the inequality.


## Prove or disprove that n^2 is O(n)
n^2 <= c . n
n^2/n <= c . n/n
n <= c . 1
Would need c to depend on n^2, but must be a constant, so just pick n = c +1



## 3n^3 + 10000n
3n^3 (1 + 10000n/3n^3 ) (or just factor out n^3)
As n grows -> infinity, grows closer to 0. Via drop smaller terms, becomes O(1)

3n^3 is n^3 via constant factor of 3, for any n0 >= 1

O(n^3 * 1) = O(n^3)

## n log(n) + 2n
n log(n) + 2n <= c . n log(n)
2n <= (c-1)n log(n)
2n <= n log(n), c = 2
2 <= log(n), divide by n
n0 = 4, c = 2

## 2^n + n
2^n + n <= c . 2^n
n <= (c-1)2^n, pick c = 2
n <= 2^n, pick n0 = 1, done














