## Prove that 5 is O(1)
f(n) = 5
5 ≤ c . 1, c = 5, n0 = 1

## Prove that 2n+1 is O(3n)
f(n) = 2n+ 1
2n + 1 ≤ c . 3n, c = 1, n0 = 1

or

1 ≤ c . n, trivially pick from there

## Prove that 4 is O(2)
f(n) = 4
4 ≤ c . 2, c = 2, n0 = 1


## Prove that 2n+ 1 is O(n)
f(n) = 2n+1
2n+1 ≤ c . n, c = 3, n0 = 1

or
1 ≤ (c-2)n, choose 3
1 ≤ (3-2)n, n0 = 1


## Prove that n^2 is O(2n^2)
f(n) = n^2
n^2 ≤ c . 2n^2, c=1, n0 =1

or

 1 ≤ c . 2 etc
divide by same power, cancels out. 

## Prove that n^2 - 3 is O(n^2)
f(n) = n^2 -3
n^2 - 3 ≤ n^2

prove individually and then multiply is one way

## Prove that n^2 -5n is O(n^2)
f(n) = n^2 - 5n

n^2 - 5n ≤ c . n^2, subtract from both sides
-5n ≤ c . 0, for any n0, c is irrelevant, just pick 1.

## Prove that n^2 + 1 is O(n^2)
f(n) = n^2 + 1
Using multiplication rule and drop smaller terms

n^2 (1+1/n^2) = drop smaller terms, goes to 0 as n-> infinity.
1 is O(1)
n^2 is trivially O(n^2)
O(n^2 * 1) = O(n^2)

OR
n^2 + 1 ≤ c . n^2 
1 ≤ c, pick constant = 2, n0 = 1


## Prove that 1 is O(n)
f(n) = 1
1 ≤ c . n, c = 1, n0, 1

## Disprove that n is O(1)
f(n) = n

n ≤ c . 1
as n -> infinity, there is no fixed constant we can apply, as cannot let depend on n. 

## Disprove that n^2 is O(n)
f(n) = n^2

n^2 ≤ c . n, divide by n both sides

n ≤ c . 1, see above.

## State following big-Oh behaviour
f(n) = IF even(n) THEN n + 3 ELSE n^2 + 5
must find c, n0 that satisfies both.
via domination of n^2 and big-Oh providing an upper bound, can look to O(n^2).

n + 3 ≤ c . n^2 / divide both sides by n
3 ≤ c . n, choose n0 = 3, c = 1

n^2 + 5 ≤ c . n^2, n0 = 3, c = 1 doesn't work. 
increase constant by 1, DONE. 

## 3n^3 + 10000n is O(n^3)
f(n) = 3n^3 + 10000n

3n^3 (1 + 10000n/3n^3 ) (or just factor out n^3)
As n grows -> infinity, grows closer to 0. Via drop smaller terms, becomes O(1)

3n^3 is n^3 via constant factor of 3, for any n0 >= 1

O(n^3 * 1) = O(n^3)

## n log(n) + 2n
f(n) = n log(n) + 2n

factor out largest term n:
n(log(n)/n + 2)
n-> infinity, O(1)
2 is trivially O(1), constant factor 2, to sketch a proof. 
n is also O(n)

O(n * 1) = O(n)

## n^3 + 2n^2 is O(n^3)
f(n) = n^3 + 2n^2

n^3 (1 + 2n^2/n^3)
n^3 (1 + 2/n)0 as n-> infinity
n^3 is trivially O(n^3)
O(n^3 * 1) = O(n^3)

## n^3 + 2n^2 log(n) is O(n^3)
n^3 (1+2n^2log(n)/n^3)
n^3 (1+2log(n)/n), n dominates log(n)/grows faster, as n -> infinity, grows to 0.
n^3 is trivially O(n^3)
O(n^3 * 1) = O(n^3)

## Prove that 2n + 1 is  Ω(3n) and hence 2n + 1 is $\Theta$(3n)

f(n) = 2n + 1

Omega
2n + 1 $\ge$ c . 3n
choose 0.5 c 
2n+1 $\ge$ 1.5n
0.5n + 1 $\ge$ 0,  n0 = 1, c = 0.5


Big-Oh
2n + 1 $\le$ c . 3 n
trivially satisfy with c' = 1

Theta 
![[Pasted image 20250224155230.png]]
$\Theta$(3n)

remember theta, 2 constants for both, but has to hold for same n0 value. 


## Prove that 5 is Ω(1) and hence 5 is $\Theta$(1)
f(n) = 5

5 $\ge$ c . 1, c = 1

5 $\le$ c . 1, c' = 5

hence
n is irrelevant, thus
is $\Theta$(1), choose any n0 as not relevant.


## Prove that 4 is  Ω(2) hence 4 is Θ(2)
f(n) = 4

4 $\ge$ c . 2, c = 1

4 $\le$ c . 2, c' = 2

hence
choose any n0
is Θ(2).


## Prove that 2n + 1 is Ω(n) hence 2n+ 1 is Θ(n)

2n + 1 $\ge$ c . n, trivially pick c = 1, n0 = 1.
to prove further(if needed)
n+1 $\ge$ c . 0


2n + 1 $\le$ c . n, c = 3.
1 $\le$ (c-2)n, need c as least as great as 2, so pick 3.

hence
c = 1
c' = 3
n0 = 1
Θ(n)

## Prove that n^2 is Ω(2n^2) and Θ(2n^2)
f(n) = n^2 

n^2 $\ge$ c . 2 n^2, c = 0.5
n^2 $\ge$ n^2, for any n0

n^2 $\le$ c . 2 n^2, c' = 1

hence
c = 0.5
c'=1
n0 = 1

## Prove that n^2 - 3 is Ω(n^2)
f(n) = n^2 - 3

n^2 - 3 $\ge$ c . n^2 WHEN LIKE THIS, TRY AND GET A CONSTANT ON ONE SIDE

n^2 - 3 $\ge$ c . n^2, n0 = 3, c = 0.5, just plug in values. 
![[Pasted image 20250224161942.png]]

## Prove that n^2 - 5n is Ω(n^2) hence Θ(n^2)

f(n) n^2 - 5n 

n^2 - 5n $\ge$ c . n^2, GET CONSTANT ON OWN
n^2 $\ge$ c . n^2 +5n, added 5
n^2 - c . n^2 $\ge$ 5n, removed LHS 
n - c . n $\ge$ 5n, choose 0.5 for c, choose 10 for n0
FOLLOW THESE STEPS BIG THETA PROOF WITH A MINUS


## Prove that n^2 + 1 is Ω(n^2) 
f(n) = n^2 +1
n^2 +1 $\ge$ c . n^2, subtract
1 $\ge$ c . 0, c = 1, n0 = 1


## Prove or disprove that 5 is o(1)
f(n) = 5

5 < c . 1, does not work for constant < 5, so disproven

## Prove or disprove that 5 is o(n)
f(n) = 5

1. Compute limit:
2. lim: n->infinity : 5/n
Since this limit is 0, proves.

OR, to get n value, divide by c on both sides
 5 < c . n
 5/c < n
 n0 = 5/c + 1
Just + 1, to make the n always bigger.

## Prove or disprove that n is o(n^2)
n < c . n^2
n/c < n^2
1/c < n
n0 = 1/c+1

OR
lim: n-> infinity n/n^2 -> 1/n -> 0


## Prove or disprove that 1 is o(log n)
1 < c . log n
1/c < log n
to undo a log, need to exponentiate both sides, by the base of the log
2^1/c < n
n0 = 2^1/c

OR 
lim: n -> infinity, 1/log n -> 0


## Prove or disprove that log n is o(1)
lim
n->infinity  

log n / 1 = log n, not 0. 



## Prove or disprove that 1 is Ω(n)
1 $\ge$ c . n, choosing c = 1
when n >= 2, fails, as no longer equal, cannot bound this with a low enough constant and constant cannot equal 0.

## Prove or disprove that n Ω(1)
n $\ge$ c . 1, choosing c = 1, n0 = 1, immediately obvious that n will equal to 1 on first value of n, so this is proven trivially.

## Prove n^2 is Ω(n)
n^2 $\ge$ c . n ,
n $\ge$ c, pick c = 1, n0 = 1, thus exceeds. 

## Prove or disprove that n is o(n log n)

n < c . n log n
1 < c . log n
1/c < n log n
2^1/c < n
n0 = 2^1/c + 1

n/n log n = 0, as n -> infinity, thus proved via use of computing limit.

## Solve
f(n) = n^2 if even
	n if odd

O(n^2) behaviour
n^2 $\le$ c . n^2, c = 1, n0 =1
n $\le$ c . n^2, c = 1, n0 =1

Ω(n^2)
n $\ge$ c . n, c =1, n0 = 1
n^2 $\ge$ c . n, c1, n0 = 1

Nothing useful to say about big theta behaviour - ie its behaviour is undefined. 

## Prove or disprove that n log n is o(n^2)
Provable for limiting behaviour. 
n log n < $\forall$c . n^2
log n < c . n
log n/c < n
n0=log n/c+1

## Prove or disprove that n is o(n^2)
n < c . n^2 
1 < c . n
1/c < n
n0 = 1/c + 1

## Prove that n^2 + n is O(n^2)

n^2 (1+n/n^2), n->infinity

## Prove that 2n^2 + 2n is O(n^2) and $\Omega$(n^2) , and o(n^2) and without using rules
2n^2 + 2n $\le$ c . n^2, c = 2, n0 = 2

2n^2 + 2n $\ge$ c . n^2, c = 1, n0 = 2, to prove big theta

2n^2 + 2n < c . n^2 
2n^2/n^2 +2/n^2
2n^2/n^2 
2, not 0, so does not work for all constants >0


## Prove the following
f(n) = n^2 + 2n if n is even
n if n is odd

O(n^2)
n^2 + 2n $\le$ c . n^2 , c=2, n0 = 2
n < c . n^2, c = 2, n0 = 2

$\Omega$(n^2)
n^2 + 2n $\ge$ c . n^2 c=1, n0 = 2,
n $\ge$ c . n^2, fails, chosing any c > 1 results in the even case no longer holding, as seen in O(n^2), and so there is no c which can bound the growth of n^2 below the linear growth of n. 

Does not have useful big theta


### Practice q's
#### 5n+ 3 = O(n)
5n + 3 $\le$ c . n
3 $\le$ (c-5) . n, choose c = 6, holds for n0 = 3,

#### n^2 + 10n = O(n^2)
n^2 + 10 n $\le$ c . n^2 
10n $\le$ (c - 1) . n^2, c = 2, n0=10

#### 2^n +n^3 
2^n + n^3 $\le$ c . 2^n 
n^3 $\le$ (c-1) . 2^n, pick c = 2.
given n^3/2^n = 0, we know is dominated by 2^n as n approaches infinity. n0 = 10

#### 3n^2 - 7n = Ω(n^2)
3n^2 - 7n $\ge$ c . n^2 
3n^2/ n^2 
3 - 7n $\ge$ c . n^2 












# Primitive operation counting rules and examples

Assignment = 1, place value at location in memory.
Array access = 2, 1 to get the start location, 1 to compute the location in the array using the index.
Moving pointers = 1.
Checking equality = 1
Variable declaration = 0, compiler directive. 


