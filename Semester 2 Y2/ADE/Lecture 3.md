## Rules for finding big-Oh of a function
Reverting to the concrete mathematical definition each time is consuming and error prone for complex cases, better to develop a set of rules that allow us to very quickly find big-Oh.

### Multiplication rule
Suppose: 
- f1(n) is O( g1(n) )
- f2(n) is O( g2(n) )
Then from the definition, there exist positive constants $c1, c2$ such that:
- f1(n) $\le$ c . g1(n) $\forall n \ge n1$
- f2(n) $\le$ c . g2(n) $\forall n \ge n2$
Let n0 = max(n1, n2) (choose max of either n1, n2), then multiplying gives
- f1(n)f2(n)$\le$ (c1 c2 = c) . g1(n) g2(n)
Thus f1(n)f2(n) is O( g1(n) g2(n))

E.g.,
What is big-Oh of f(n) = 5 n
5 is O(1)
n is O(n)

5 * n is O(1 * n)

### Drop smaller terms
Suppose:
- f(n) = (1 + h(n) )
- with h -> 0 as n->infinity
Then f(n) is O(1)
Hence there exists n0, such that

h(n) $\le$ 1 for all n $\ge$ n0
So f(n) $\le$ 2 for all n $\ge$ n0

To use this however, need to have some rules for limits of various functions e.g., what do we do with (1 + (log(n) / n) )
Does h(n) = log(n) / n go to 0  as n -> infinity?

e.g.,
What is big-Oh of f(n) = n^2 + n
FACTOR OUT, APPLY DROP SMALL TERMS, APPLY MULTIPLICATION RULE
![[Pasted image 20250222034306.png]]

![[Pasted image 20250222033524.png]]

Therefore if f(n) is a polynomial of degree d (with positive largest term) then f(n) is O(n^d), i.e.,
1. Drop lower order terms
2. Drop constant terms
E.g., f(n) 5n^4 + 3n^2 is degree 4 and so will be O(n^4)

### Important functions
- Constant : 1
- Logarithmic : log(n) 
- Linear : n • 
- “n-log-n” : n log(n) 
- Quadratic : n^2 
- Cubic : n^3
- Exponential : 2^n

### Exponents vs powers
Exponentials grow faster as n->infinity than any power:
For any fixed k > 0 and b > 1
	$b^n$ /$n^k$ -> infinity
	$n^k$ / $b^n$ -> 0
E.g., n^2 / 2^n -> 0
E.g., 2^n / n^k -> infinity.

E.g.,
What is the big Oh of f(n) 2^n + n^2.
2^n (1 + (n^2/2^n) )
g(n) = 0
Hence f(n) is O(2^n)

### Powers vs logs
Powers grow faster (as n-> infinity) than any power of a log 
n/(log n) -> infinity
log(n) / n -> 0

More generally: (log n)k / n k’ → 0 for any fixed k, k’ > 0
E.g., (log n)^100 / n^0.1 = 0

E.g., What is the big Oh of f(n) = (n log n) + n^2
Factor out biggest
n^2 * ( ( log n )/n + 1)
log n / n = 0
can drop it
f(n) = O(n^2)

PRACTICE
![[Pasted image 20250222035829.png]]

### Big-Oh conventions
Use slowest growing (reasonable) possible class of functions to classify a function upper bound:
	 2n is O(n) instead of 2n is O(n^2)
Use the simplest expression of the class
- 3n+5 is O(n) instead of O(3n)
Both of the second cases correct, but doesn't say much.

From the definition true that any f(n) is O(f(n))
But such an answer is inappropriate, conventions demand we want a useful answer, not a trivial one, want the tightest nice function that defines the growth rate, not just one that is formally correct.


![[Pasted image 20250222040453.png]]
2^n/100 * (1 + n^200/2^n/100) = O(2n/100)

### Big Oh algorithm usage
Big-Oh definitions themselves just classify growth rate of a function, and do not classify algorithms.

Their usage for runtimes of algorithms has further choices.
One can use big-Oh to describe any of
- Worst case runtime w(n)
- Best case runtime b(n)
- Average case runtime b(n)
- ...
If simply say algorithm X is O(.) then usual convention is that is refers to the worst case. But we do have the freedom to define cases for the runtimes, classifying them differently depending on runtime grows with input.

Consider saying 'Merge-sort' is O(n log n)'
Expands:
'If running this algorithm on n integers, then the worst-case runtime over all possible inputs, is a member of the set of functions, that is no worse than some fixed constant times n log(n) for all values of n that are at least some fixed value'


### Asymptotic analysis of algorithms
Asymptotic analysis is a method of describing limiting behaviour. 
To perform this for algorithms:
- Find the case we are aiming to describe(best case etc) in terms of primitive operations executed as a function of the input size. 
- Express this function with big-Oh notation. 
A1 executes at most 6n^2 + 3n - 4
We say that algorithm A1 runs in O(n^2) time.
