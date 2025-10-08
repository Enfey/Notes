Easy way to analyse time complexity of recursive algorithms.
Applies to recurrence relations of the form
$$ T(n) = a \cdot T(\frac{n}{b}) + f(n)$$
Problems divided into a instances(recursive calls) of size n/b, where n is problem size reduced by a factor b in each recursive call, with f(n) representing the cost of trivial operations like dividing the problem itself to call the recursive function. 


## CASE 1 Recurrence dominates
> ***If f(n) is $O(n^c)$ with c < $log_b(a)$ THEN $T(n$) is $Θ(n^{log_ba})$***

HAS BIG OH, and $c < log_b(a)$
## CASE 2 Neither term dominates
> If f(n) is $Θ(n^c(logn)^k)$ with $c = log_b(a)$ and $k \ge 0$ THEN T(n) is $Θ(n^c(logn)^k+1)$ 

HAS WEIRD F(N), BOTH THE OTHER CASES DONT WORK

## CASE 3 f(n) dominates
> If $f(n)$ is $\Omega(n^c)$ with $c > log_b(a)$ then $T(n)$ is $Θ(f(n))$

HAS OMEGA, AND HAS $c > log_b(a)$


# PRACTICE Q's
## T(n) = 2 . T(n/2) + n and T(1) = 1
a = 2
b = 2
f(n) = n

log_b(a) = log_2(2) = 1
f(n^c) = f(n^1) = c = 1
Θ(n^1(log n)^k+1)

## T(n) = 2 . T(n/4) + n and T(1) = 1
a = 2
b = 4
c = 1

log_4(2) = 0.5

c > log_4(2) = case 3
Θ(n)

## T(n) = T(n-1) + 1, not in correct form, must prove by induction

## T(n) = 2 . T(n/4) + 1
a = 2, b = 4, c = 0

log_b(a) = 0.5

c < 0.5
c = 0.5
c > 0.5
1st case
so is Θ(n^0.5)


## T(n) = 4 . T(n/2) + n^2
a = 4
b = 2
c = 2

log_b(a) = log_2(4) = 2

2 = 2, case 2
Θ(n^2(log n)^k+1)
Θ(n^2 log n)


### T(n) = 3 . T(n/3) + n log n
a = 3
b = 3
log_b(a) = 1

c = 1

1 = 1
Θ(n log n(log n)^k+1)
Θ(n(log n)^2)

WHEN DOING MASTER THEOREM CASE 2, IF HAVE A LOG in THE f(n) THEN K IS THE POWER OF THE LOG, AND GET RID OF THE LOG

## T(n) = 2 . T(n/2) + 2n^2
a = 2 
b = 2
log_b(a) = 1
c = 2 

2 < 1, fails
2 = 1, fails
2 > 1, case three
Θ(2n^2)
Θ(n^2), discard due to definition

## T(n) = 2 . T(n/2) + n(log n)^2
log_b(a) = 1
as takes the form Θ(n^c(log n)^2), then use case 2, where c = 1

1 = 1
 Θ(n(log n)^2+1)
  Θ(n(log n)^3)


## T(n) = 2 . T(n/16) + n^2 
a = 2
b = 16

log_16(2) = log_2(2) / log_2(16)![[Pasted image 20250518170205.png]]
= 1/4

c = 2

2 < 1/4, fails
2 = 1/4, fails
2 > 1/4, case three
Θ(n^2)


## T(n) = 8 . T(n/2) + n^3 log n

a = 8
b = 2
log_b(a) = 3
c = 3

3 = 3, matches master case, and formula

Θ(n^3(log n)^2)

## T(n) = 16 T(n/4) + n
a= 16
b = 4

log_4(16) = 2
c = 1

1 < 2, 1st case

Θ(n^2)



log_16(4) = log_2(4) / log_2(16) = 2/4 = 0.5

## 2 T(n/16) + n^2 
a = 2
b = 16

log_16(2) = log_2(2) / log_2(16) = 1/4
c = 2

2 > 1/4, third case
$\Theta{n^2}$ 
# RESOLVING EXACT SOLTUIONS FROM RECURRENCE RELATIONS
1. T(n) = T(n-1) + 1 and T(1) = 1
	T(2) = T(2-1)+ 1 = 2
	T(3) = T(3-1) + 1 = 2 + 1 = 3
	T(4) = T(4-1) + 1 = 3 + 1 = 4
	T(n) = n
2. T(n) = 2 T(n-1) and T(1) =1
	T(2) = 2T(2-1) = 2T(1) = 2
	T(3) = 2T(3-1) = 2T(2) = 4
	T(4) = 2T(4-1) = 2T(3) = 8
	T(n) = 2^n-1
3. T(n) = 2 T(n/2) and T(1) =1
	T(2) = 2T(2/2) = 2
	T(4) = 2T(4/2) = 4
	T(8) = 2T(8/2) = 8
	T(16) = 2T(16/2) = 16
	T(2^k) = 2^k, of this form as dividing by 2.
4. T(n) = 3 T(n-1) and T(1) = 1
	T(2) = 3T(2-1) = 3T(1) = 3
	T(3) = 3T(3-1) = 3T(2) = 9
	T(4) = 3T(4-1) = 3T(3)= 27
	T(n) = 3^n-1
5. T(n) = 3T(n/3) and T(1) =1
	T(3) = 3T(3/3) = 3T(1) = 3
	T(9) = 3T(9/3) = 3T(3) = 9
	T(27) = 3T(27/3) = 3T(9) = 27
	T(3^k) = 3^k
 6. T(n) = 2T(n/4) and T(1) =1
	 T(4) = 2T(4/4) = 2T(1) = 2
	 T(16) = 2T(16/4) = 2T(4) = 4
	 T(64) = 2T(64/4) = 2T(16) = 8
	 T(4^k) = 2^k
	![[Pasted image 20250518174437.png]]
	
![[Pasted image 20250518174507.png]]
REMEMBER THIS
$log_b(a) = 1 / log_a(b)$
# INDUCTION PROOFS FOR RECURRENCE

## Given: T(n) = T(n-1) + 1 and T(1) = 1
Prove T(n) = n

T(1) = 1, base case
Induction step: assume T(k) = k, prove T(k+1) = k+1
T(k+1) = T(k+1-1)+1 = T(k)+1 = k + 1

## Given: T(n) = 2T(n-1) and T(1) = 1
PROVE T(n) = 2^n-1 
T(1) = 2^1-1 = 2^0 = 1, holds
Induction step: assume T(k) = 2^k-1, prove T(k+1) = 2^k-1+1 = 2^k
T(k+1) = 2T(k+1-1) = 2T(k) = 2 2^k-1 = 2^k

## Given T(n) = 2 . T(n/2) and T(1) =1
Prove T(2^k) = 2^k
T(2^0) = 1 = T(1), so holds
Induction step: Assume holds for T(2^k) = 2^k, prove T(2^k+1) = 2^k+1
T(2^k+1) = 2 . T(2^k+1/2)
2 . T(2^k) 
2 2^k, via ih
2^k+1

## Given T(n) = 3 . T(n-1) and T(1) =1 
Prove T(n) = 3^n-1
T(1) = 3^1-1 = 3^0 = 1, holds
Induction step: assume holds for T(k) = 3^k-1, prove T(k+1) = 3^k+1-1 = 3^k
T(k+1) = 3.T(k+1-1)
3.T(k)
3 . 3^k-1 
3^k 

## Given T(n) = 3 . T(n/3) and T(1) = 1
Prove: T(3^k) = 3^k
T(3^0) = T(1) = 1, so holds
Induction step: assume holds for T(3^k) = 3^k, prove that T(3^k+1) = 3^k+1
T(3^k+1) = 3T(3^k+1/3)
3T(3^k)
3 3^k
3^k+1, proved

## Given T(n) = 2 . T(n/4) and T(1) = 1
Prove: T(4^k) = 2^k
T(4^0) = T(1) = T(2^0) = T(1) = 1, so holds
Induction step: assume T(4^k) = 2^k holds, then prove that T(4^k+1) = 2^k+1
T(4^k+1) = 2T(4^k+1/4)
2T(4^k)
2 . 2^k
2^k+1



# Find exact solution, then prove by induction 

## T(n) = 4 . T(n/4)
T(4) = 4 . T(4/4) = 4T(1) = 4
T(16) = 4 . T(16/4) = 4T(4) = 16
T(4^k) = 4^k, WRITTEN IN THIS FORM

Prove T(4^k) = 4^k

T(4^0) = T(1) = Solved
T(4^k+1) = 4 . T(4^k+1/4)
4 . T(4^k)
4 . 4^k
4^k^1, solved


## 4 . T(n/2)
T(2) = 4 . T(1) = 4
T(4) = 4 . T(2) = 16
T(8) = 4 . T(4) = 64
T(2^k) = 4^k

T(2^0) = T(1) = 1 = 4^0
4T(2^k+1) = 4T(2^k+1/2)
4T(2^k)
4 4^k
4^k+1

##  T(n) = T(n-1) + n
T(2) = T(2-1) + 2 = 3
T(3) = T(3-1) + 3 = T(2) + 3 = 6
T(4) = T(4-1) + 4 = T(3) + 4 = 10
T(5) = T(5-1) + 5 = T(4) + 5 = 15
T(n)


 


# FELIX STRAT
WHEN DOING MASTER THEOREM CASE 2, IF HAVE A LOG in THE f(n) THEN K IS THE POWER OF THE LOG, AND GET RID OF THE LOG














