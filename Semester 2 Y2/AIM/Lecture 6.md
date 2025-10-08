### Questions
- When does a genetic algorithm perform better than a memetic algorithm?
- Is the choice of meme (hill climbing/local search algorithm) important?
- Which meme has better performance in a memetic algorithm
- Can somehow combine several memes to obtain a synergy leading to improved performance
## Benchmark functions
- Serve as a testbed for performance comparison of meta/hyper heuristic optimisation algorithms
	- These functions themselves are the problem definition and the evaluation.
	- Can easily be computed
	- Each function is recognised to have certain characteristics potentially representing a different real world problem
	- Have a known global minimum/optimum, can measure how close get.
### Classification of benchmark functions
Common classification:
- Continuity
	- Discontinuous vs continuous
- Dimensionality
	- Scalability - can extend to have as many variables as want
- Separability
	- Separable vs non separable
	- Whether function variables are independent or dependent, ie changing one variable affects the other variable or not
	- ![[Pasted image 20250309002911.png]]
	- Can take one variable, optimisie it, then do another, can do this in parallel.
- Modality
	- Unimodal - one global mimima, relatively simple
	- Multimodal with few local minima, and global mimima
	- Multimodal with exponential number of local minima, and global minima
#### Sphere function - FINISH THIS SHIT OFF ANOTHER DAY

![[Pasted image 20250309001514.png]]


### Travelling salesman problem
- Binary representation and classical random initialisation, cross-over and mutation operators are not suitable for encoding permutation
- E.g., TSP, could cause illegal tours to form
	- Not all cities visited
	- Undefined city codes
	- Cities visited more than once etc
#### Example
Given n = 5 cities 
3 bits per city
![[Pasted image 20250309004738.png]]
#### Generic permutation based genetic operators
##### Partially mapped crossover
- Builds offspring by
	- Choosing a subsequence of a tour from one parent
		- Choosing two random cut points to serve as swapping boundaries for a segment
		- Swap segments between parents
	- Preserving order and position of as many cities possible from other parent
	- If conflicted position, use swapping rule defined in segment to replace. 
	![[Pasted image 20250309005344.png]]
- Exploits similarities in the value and ordering simultaneously??
##### Order crossover
- Builds offspring by
	- Choosing subsequence of tour(segment) from one parent, copy it to child
	- Starting from 2nd cut point position of initial parent, cities from other parent copied in the same order, removing cities appearing in the copied segment
	- Then place these elements in order
	- ![[Pasted image 20250309010025.png]]
	- ![[Pasted image 20250309010033.png]]
	- 
##### Cycle crossover
- Creates offspring by:
	- Aims to preserve absolute positions in elements from both parents
	- Key idea, each element in child must come from same position in one of the parents
	- Randomly select starting point in parent 1
	- Copy cities from p1 until 'cycle' is achieved
	- Rest filled in from other parent, or do more cycles
- ![[Pasted image 20250309010537.png]]
- p1 has 1, p2 has 4, look at p1 position 4, etc until cycle.

#### GA vs MA for solving TSP experiments
- Representation: permutation
- Fitness function: path length
- Crossover: PMX, 2PTX(with repair), OX
- Mutation operators: **Exchange mutation**, **Insertion** mutation
- Mate selection: Tournament
- Replacement: TG, SS ?
- Hill climbing: RMHC using ISM and/or EM.
- Hill climbing step applied as long as max num of iterations exceeded and indiv. is improved.


## Multimeme memetic algori
thms
### Self adaptation for genetic operators
- **Self Adaptation**: Deciding which operators and settings to use on the fly whenever receiving feedback during evolutionary search process, rather than fixed settings
- Davis used v**arying probabilities** of **applying different operators** to achieve this
	- **Updating probabilitie**s based on **performance of the operator** within **last few generations**
- May use an **additional bit** to decide whether to **apply 2PTX** or **UX to an individual.**
- **Could encode mutation rate, crossover type, as part of chromosome, generated randomly, and pass to offspring if outperform others, converging on 1 fixed setting which leads to optimal solution.**
### Memetic material
- Introduce memetic material for each individual
	- Each individual carries **set of instruction**s that determine **how solution will be improved**, that **evolve alongside individual**
	- **Genetic operators** are **governed by rules** that **themselves are subject to evolution**
	- A meme **encodes how to apply an operator** (**which one to apply,** **max no of iterations,** **acceptance criteria**), **when to apply**, **where to apply**, **how frequently to apply.**
	- **Memes of differing operators can be combined under a memeplex**
#### Memeplex
- Memeplex = $<Meme>;<Meme>:<Memeplex>$ 
- Meme = $(<Where>, <When>, <How>, <Frequency)$
- Where = $<Location> Crossover; <Location> Mutation$ 
- Location = $After ; Before$
- When = $< BooleanCondition>$
- How = $GeneralHillClimber <Move> <Strategy> <Iterations>;$ $\newline BoltzmanAdaptiveHillClimber <Move> <Strategy <Iterations>$
	- Basically saying we're gonna apply hill climber, after/before crossover/mutation, when boolean condition applies
- Frequency = Always; For $<Iterations>$
- Move = 2-exchange, 3 exchange
- Strategy = NextAscent, SteepestAscent
- Iterations = $<Num>$
### Features of Multimeme memetic algorithms
- Memes represent instructions for self improvement
	- **Set of rules, programs, heuristics, strategies, behaviours**
- Interaction between memes and genes are not direct
	- **Mediated by individual (genetic and memetic material)**
- **Memes themselves can evolve, change by means of nontraditional genetic transformations and metrics**

### Example - MAX-SAT
- ![[Pasted image 20250309032540.png]]
- With singular hill climbing meme and 2 options, which we can evolve, and is tied to our individual
- Or can use memeplex, like above, 
- ![[Pasted image 20250309032640.png]]
- Where and when and some other things omitted, some memes do not have these whereas some do
- Then evolve this memetic material, e.g.,
- ![[Pasted image 20250309032844.png]]
- Second one has better fitness
- ![[Pasted image 20250309032918.png]]
- Copy memetic material to offspring
- ![[Pasted image 20250309032958.png]]


#### Mutating memes during evolution
- Inovation rate (IR) $\in [0,1]$ is probability of mutating the memes
- Mutation randomly sets meme option to one of the other options
	- ![[Pasted image 20250309033104.png]]
- IR = 0, no innovation, if a meme option is not introduced in initial generation, it will not be reintroduced again.
	- Still permits inheritance of memes ofc.
- All different strategies implied by the available $M$ memes might be equally used. 

### Measure for evaluating meme performance
- Concentration of a meme $(c_i,(t)$ is the total number of individuals that carry the meme $i$ at a given generation $t$.
	- Crude measure of success, gives no info about **continual usage** of a meme throughout successive generations
- Evolutionary activity of a meme $(a_i(t))$ is the accumulation of a meme concentration until a given generation



FINISH OFF THE BIT ABOUT FUNCTIONS AFTER DIOGO EXPLAINS IT