https://colab.research.google.com/drive/1AUkk7CKcqlvs8pgSbvrO7Pmt4l86es2o?usp=sharing#scrollTo=Ap7Vs6r-62_s 
# Z3
An efficient SMT solver with specialised algorithms for solving background theories. SMT is a decision problem for logical formulas with respect to multiple representations e.g., arithmetic, bit vectors, boolean, and arrays.
The main purpose of Z3 is software verification, i.e., verification that a program will always work correctly. Z3 is based on first order logic, and that is how it will be used. 
Z3 takes its input in SMT-LIBv2 format -  astandardised language for describing logical formulae and constraints. Symbols are declared i.e., constants, functions, variables. Then add constraints i.e., assertions, such as *x > 10*, or placing boolean constants into a logical arrangement. Then ask z3 to solve.
Z3 has bindings which are used for bindings. 

### SMT-2 Brief summary
- To comment out a line place a semicolon `;Z3 will ignore this line`
- Every command in Z3 is a function call, and has the following format:
	`(<function name> <parameter 1> <parameter 2> ...)`
	![[Pasted image 20251001122949.png]]
Translate each of the following formulas into the SMT2 language and use Z3 to find the values of the constants that satisfy each of them (edit the `smt2program` content). Check the answers by substituting them into the original formulas.
1. $A \vee \neg B$ 
	(declare-const A Bool)
	(declare-const B Bool)
	(assert (or A (not B)))
2. $(A \vee \neg B \vee \neg C ) \wedge B \wedge C$ 
	(declare-const A Bool)
	(declare-const B Bool)
	(declare-const C Bool)
	(assert (and (or A (not B) (not C)) B C))
3. $((A \implies B) \iff (A \wedge B)) \wedge \neg B$ 
	(declare-const A Bool)
	(declare-const B Bool)
	(assert (and (= (=> A B) (and A B)) (not B)))
4. $((A \wedge \neg B) \vee (\neg A \wedge B)) \wedge (A \iff B) \wedge (C \iff B)$ 
	(declare-const A Bool)
	(declare-const B Bool)
	(declare-const C Bool)
	(assert (and (or (and A (not B)) (and (not A) B)) (= A B) (not (= C B)) ))

Z3 can also handle non-boolean variables. It can handle integers, just use `int` instead of `bool` when defining a constant. 
The supported functions are `+, -, *, <, >,` etc. 
![[Pasted image 20251001125942.png]]
![[Pasted image 20251001130739.png]]
![[Pasted image 20251001130709.png]]
![[Pasted image 20251001130716.png]]


## Encoding Z3 Programs without SMT 2
Have seen thus far that Z3 can solve mathematical problems, given they are formulated correctly with respect to SMT 2. However, this language is not user friendly. We now learn how to use Z3 directly from within a python program; a useful combination which allows us to integrate Z3, a general solver, with python, a general purpose programming language.

Below is an example of a python program that encodes a single rule $A \wedge \neg B$ and calls Z3 to find values of $A$ and $B$ that satisfy the rule:

```python
import z3

s = z3.Solver()
A = z3.Bool(A)
B = z3.Bool(B)
s.add(z3.And(A, z3.Not(B)))

if s.check() = z3.unsat:
	print('unsat')
else:
	print(f'A = {s.model.eval(A)}')
	print(f'A = {s.model.eval(B)}')
```
The add function is equivalent to assert in the Z3 language; it adds an expression to the knowledge base. Functions Not, And etc are provided by the Z3 library to encode logical expressions. 