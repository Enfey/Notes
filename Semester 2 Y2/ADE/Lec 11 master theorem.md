Provides an easy way to analyse the time complexity of recursive algorithms, especially those that follow divide and conquer paradigm. 
Applies to recurrence relations of the form $$T(n) = a \cdot T(\frac{n}{b}) + f(n)$$
Which problems are divides into $a$ instances (recursive calls) of a problem of size $n/b$ , where n is problem size, reduced by a factor b in each recrusive call, with f(n) representing cost of dividing problem and combining results.
Does not cover all cases, but covers many useful cases.

## Case 1: Recurrence dominates
> IF $f(n)$ is $O(n^c)$ with $c < log_b \ (a)$ THEN $T(n)$ is $Θ(n^{log_ba})$
> 

![[Pasted image 20250317172243.png]]
1. T(n) = 2 . T(n/2)
	a = 2
	b = 2
	f(n) = 0
	$0 < log_2(2)$
	$T(n)$ is $Θ(n^{log_ba})$, with $Θ(n)$

## Case 2: Neither term dominates
> IF $f(n)$ is $Θ(n^c(log n)^k)$ with $c = log_ba$ and $k \ge 0$ THEN $T(n)$ is $Θ(n^c(log n)^{k+1})$

## Case 3: f(n) dominates.
> IF $f(n)$ is $\Omega(n^c)$ with $c > log_ba$ THEN $T(n)$ is $Θ(f(n))$











## Practice q's
### T(n) = 2 . T(n/2) + n and T(1) = 1
a = 2
b = 2
f(n) = O(n)
log_2(2) = 1
Case 1: O(n^1) with c < log_2(2) = 1, fails
Case 3: Ω(n^1) with c > log_2(2) = 1, fails. 
So case 2: f(n) = $\Theta(n^c(log n)^k)$=  $\Theta(n^1(log n)^1)$ with $c = log_ba$
so = $Θ (𝑛 \ log \ n)$

### T(n) = 2 . T(n/4) + n and T(1) = 1
a = 2
b = 4
log_4(2) = 0.5
f(n) = O(n) = O(n^1) = c = 1
1 < 0.5, false, case 1 fails
1 = 0.5, case 2 fails
1 > 0.5, case 3.

Θ(n)

### T(n) = T(n-1) + 1
Not in correct form, must prove by induction

### T(n) = 2 . T(n/4) + 1
a = 2
b = 4
log_4(2) = 0.5
f(n) = O(1) = O(n^0)  = c = 0 --need c remember
c < 0.5, so first case
T(n) = Θ(n^0.5)

### T(n) = 4 . T(n/2) + n^2
a = 4
b = 2
log_2(4) = 2
f(n) = O(n^2) = c = 2

2 = 2, so 2nd case
pick 0 for k
Θ(n^2(log n)^0)
Θ(n^2 log n)


### T(n) = 2 . T(n-1)
Induction only

### T(n) = 3 . T(n/3) + n log n
a = 3
b = 3
log_3(3) = 1
f(n) = O(n^1 log n^1) = c = 1
1 = 1, 2nd case
![[Pasted image 20250317191204.png]]

### T(n) = 2 . T(n/2) + 2n^2
a = 2
b = 2
log_2(2) = 1
f(n) = n^2
c = 2

2 > 1, so case 3.
Θ(n^2)

### T(n) = 2 . T(n/2) + n(log n)^2
a = 2
b =2
log_2(2) = 1
f(n) = n^1(log n)^2 = c = 1, k = 2, as takes the form![[Pasted image 20250317191516.png]]
1 = 1, so case 2,
T(n) is Θ(n(log n)^3)


### T(n) = 2 . T(n/16) + n^2 
a = 2
b = 16
log_16(2) = log_2(2) / log_2(16) = 1/4
f(n) = n^2, c = 2
2 > 1/4, so case 3
Θ(n^2)

### T(n) = 8 . T(n/2) + n^3 log n
a = 8
b = 2
log_2(8) = 3
c = 3, k = 1
3 = 3, case 2
Θ(n^c(log n)^k+1), Θ(n^3(log n)^2)


### T(n) = 16 T(n/4) + n
a = 16
b = 4
log_4(16) = 2
c = 1

1 < 2, case 1.
Θ(n^2)


## Recurrence proofs practice
### T(n) = 3T(n/3) + 2 and T(1) =1
Solution claimed to be T(3^k) = 2 * 3^k -1
Base case:
T(3^0) = T(1) = 1 = 2 * 1 - 1 = 1, so holds
Inductive step: 
T(3^k+1) = 3T(3^k+1/3) + 2
= 3T(3^k) + 2
= 3T(2 * 3^k - 1)
= 2 * 3^k+1 - 3 + 2
=  2 * 3^k+1 - 1

![[Pasted image 20250317192905.png]]

### T(n) = 2T(n/3) + 1 with T(1) = 1
T(3) = 2T(3/3) + 1 = 2T(1) + 1 = 3
T(9) = 2T(9/3) + 1 = 2T(3) + 1 = 7
T(27) = 2T(27/3) + 1 = 2T(9) + 1 = 15
T(n) = 2^k+1 -1



