### Purpose
- Experimentation with psuedorandom number generators
	- Understanding their consequences for computational experiments
- Introduction to maximum satisfiability problem and can explain how heuristics modify the solutions to MAX-SAT problems

### Framework background
- getNumberOfVariables of the problem
- bitFlip(index) of the problem
- ![[Pasted image 20250311033934.png]]

### Lab test answers
1. ![[Pasted image 20250311034149.png]]
2. ![[Pasted image 20250311034202.png]]
3. ![[Pasted image 20250311034211.png]]
4. ![[Pasted image 20250311034223.png]]
5. ![[Pasted image 20250311034234.png]]
	- No variation in pertubation, always resulting in same exploration of solution space, is essentially deterministic.
6. ![[Pasted image 20250311034328.png]]
	![[Pasted image 20250311034338.png]]
	
### Main takeaways and definitions
- Psuedorandom number generators, algorithms that generate sequences of numbers predicated on an initial seed, always generate same sequence of numbers when initialised with same seed
	- Allows experiments to be replicated, and tested, and is a useful property in debugging. 
	- Some seeds may lead to highly structured outputs rather than appearing truly random, need to experiment with this as well
- Maximum satisfiability problem, combinatorial optimisiation variant of boolean satisfiability problem.
	- Given boolean formula in CNF, goal of MAX-SAT is to find assignment of variables that maximises number of satisfied clauses, or minimises number of unsatisfied clauses.
- MAX-SAT is NP-hard, exact solutions are impractical for large instances, use inexact heuristics
	- In lab 1, use a stochastic method, simply flip the a random bit in solution, performing random walk of search space until time limit reached. 
![[Pasted image 20250313235625.png]]
