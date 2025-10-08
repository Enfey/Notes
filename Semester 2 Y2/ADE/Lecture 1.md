# Analysis of Algorithms
Initially focusing on measure of runtime, but can also analyse other measures such as space used etc.
## Running time
- Consider batch algorithms here, not interactive; simply transform input data into output data. Naive.
- The running time of algorithm typically(not always) grows with input size.
- Even given size, runtime usually not fixed. 
	- Can have "best", "average", "worst" cases
- Usually focus on worst case running time at given size since:
	- Worst-case usually most useful and easiest to analyse
	- Average case time is often difficult to determine, and average may refer to *mean* or *median* or other measures
	- Best case rarely matters.

n^k not exponential, this is power law, exponential means ends in the exponent??

### Experiments to analyse running time
- Implement algorithm, and then record running time, for worst, average and best case
- Requires implementation of the algorithm, may be difficult or time-consuming.
- Results may not indicate the running time, running time may depend on hardware, ordering of operations at instruction level, etc, may miss worst real case.
- To compare two algorithms this way, same hardware and software environments must be used.
- Creates need for platform agnostic way to analyse algorithms without implementing them.
### Limitations of theory
- Necessary to implement the theory, may be time consuming.
- Results may not be indicative of typical running time on inputs encountered in real world. 

## Theoretical analysis:
- AIM: Characterise **running time** as a **function** of **input size n**. 
- Use a high-level description of algorithm, not directly implemented, so that can take into account all inputs and evaluate speed of algorithm independently of environmental constraints. 
	- Use psuedocode to describe algorithms, more structured than english prose, less detailed than program, preferred notation for describing algorithms. 
```c
//get max of a non-empty array int 
getMax(int[] A) { // A has size n 
	max = A[0] 
	for (int k = 1; k < n ; k++ ) 
		if ( A[k] > max ){
			max=A[k]
		}
 return max
 ```
 To analyse programs theoretically, we will count 'Primitive Operations'.

### Primitive operations
- Basic computations performed by an algorithm
	- Identifiable in psuedocode
	- Largely independent from programming language
	- Exact definition not important
- Assumed to take constant amount of time in RAM model (see below)
	- Close to assembly language
	- No hidden expenses. 
#### Examples
- Assign value to variable
- Compare two numbers
- Perform one arithmetic operation
- Call a method
- Return a value
- Value lookup/access
#### Random Access machine. 
- CPU
- Potentially unbounded bank of memory cells, each of which can hold 'arbitrary' number of character
- Memory cells numbered 0, 1...n and accessing any cell in memory takes unit time(some fixed time.)
IGNORE ALL BIGNUM ISSUES, ALL NUMBERS ARE OF EQUAL SIZE, AS ALL FIT IN REGISTER OF CPU. CANT REALLY EXPECT TO STORE 938566359286151800351666177735777777177177374717717471 5717777761365661618161616


### Counting primitive operations
getMax(int[] A) { 
	max = A[0] OPERATIONS = 2(assignment + access)
	for (int k = 1; k < n ; k++ ) OPERATIONS = 1 (k =1)
		if ( A[k] > max ){ OPERATIONS = 2 (access + comparison) * n
			max=A[k]  OPERATIONS = 2 (assignment + access) * n
		}
		{increment k} OPERATIONS = 1 * n
		{test k < n} OPERATIONS = 1 * n
 return max OPERATIONS = 1
6n - 2 = total operations
6n-6 tied to loop, + 4 from outside it. 


Counting is underspecified, full process for c = A[i]:
- get A, store in register
- get I, store in register
- compute A+i which is pointer to location of A[i], store back in register
- Get value of A+i from RAM and store value in register. 
Important to know this to justify counting of primitive operations. Real timing would depend on compiler, CPU architecture, pipelining, cache misses. 


Found that getMax uses 6n-2 primitive operations in worst case, but what about actual worst-case running time - TW(n)?
- Not all primitive operations take same time, so cannot say directly the time taken is proportional to (6n-2)
Instead, suppose the quickest primitive operation takes time q, and the slowest time s, then we have q(6n-2) <= TW(n) <= s(6n-2)
TW(n) sandwiched between two linear functions and can say worst case running time is linear


#### Estimating running time: Best
- For best case, line 'max = A[k]' never executes, getMax uses 4n primitive operations
- For best case running time TB(n)
	- q(4n) <= TB(n) <= s(4n)
	Also can say best case is linear

Growth rate of running time not affected by changes to environment, the linear growth rate of running times is a property of the algorithm getMax itself. 



### Another prim ops example
```
int RecDiv(int n)
	int m = 0 OPERATIONS = 1(assignment)
	while(n >= 2 ) OPERATIONS = 3 (access n, comparison, 2)
		n = n/2 OPERATIONS = 3(assignment, get n, division)
		m++ OPERATION = 2 (assignment, increment)
	return m OPERATION = 1
```
Could reduce number of operations e.g., use of register allocation as compiler optimisation technique.

### How many passes?
Case $N$ = 1 = $2^0$, passes = 0
Case $N$ = 2 = $2^1$, passes = 1
Case $N$ = 4 = $2^2$, passes = 2
Case $N$ = 8 = $2^3$, passes = 3
Case $N$ = $2^m$ 
	$N$ = $2^m, 2^{m-1}, 2^{m-2},...2$ terminates!
	 $m$ passes through the loop
But $N$ = input, not m, so:
$m = log2(n)$

So total = $8log2(n) + 5$ 
![[Pasted image 20250210015703.png]]
Iteratively halving something gives you a logarithm, describes number of passes. 


