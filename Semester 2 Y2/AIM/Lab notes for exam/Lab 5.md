### Purpose
- Implementing population-based metaheuristic to solve combinatorial optimisation problem
	- Metaheuristic as framework
- Implement memetic algorithm
- Empirical evaluation of local search operators in memetic algorithms.
- Be able to implement and distinctly recognise components of evolutionary algorithms including:
	- Parent selection
		- Tournament
	- Crossover
		- UXO
	- Mutation
		- Not covered - likely just bit flip to perturb offspring based on some random chance
	- Local search
		- SDHC and DBHC
	- Population replacement. 
		- Trans generational replacement with elitism
### Framework background
- Memetic algorithm is a population-based search method
- Framework permits multiple solutions in memory, difference between single point methods that optimise a single solution, is maintenance of entire population
- Have 16 memory addresses to manage for population of size 8, 8 addresses for current population, another 8 for offspring. 
- Special methods needed for applying heuristics to correct solutions in memory. 
### Memetic algorithm psuedocode

![[Pasted image 20250310034404.png]]
![[Pasted image 20250313061128.png]]
![[Pasted image 20250310034409.png]]
#### Genetic algorithm pseudocode
![[Pasted image 20250310034534.png]]
Can implement local search wherever to form a **Memetic Algorithm**. 
![[Pasted image 20250314000012.png]]
- select parents, if parents index same, + 1 to index and mod by pop size, child indexes set, apply 1PTX or UXO, apply mutation to children, perform local search on children, perform trans-generational replacement with elitism (child population replaces parent generation whole, unless best is found to be in parents, then replaces worst of the offspring.)

#### Genetic operator: Parent Selection: Tournament
![[Pasted image 20250313061016.png]]
![[Pasted image 20250314143352.png]]
INTSTREAM.range.toarray
- Generate random indices for tournament size, perform evaluation on that indice, and compare to best seen, simply return the best index. 

#### Genetic operator: Crossover: Uniform (UXO)
![[Pasted image 20250313061022.png]]
![[Pasted image 20250313223225.png]]

#### Genetic operator: Population replacement : TGR with elitism
![[Pasted image 20250313061036.png]]
![[Pasted image 20250313061045.png]]
![[Pasted image 20250313223240.png]]

![[Pasted image 20250314033656.png]]

KEEP TRACK OF BEST SOLUTION
KEEP TRACK OF WORST SOLUTION IN OFFSPRING PORTION VIA INDICE CHECKING AND SOLUTION COST CHECKING FOR A GIVEN ITERATION

OUTSIDE OF LOOP, IF BESTINDEX LESS THAN POPULATION SIZE, 
![[Pasted image 20250314033913.png]]