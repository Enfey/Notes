## Questions
- When does genetic algorithm perform better than a memetic algorithm
- Is the choice of meme important?
- Which memes have better performance
- Can we somehow combine several memes to obtain a synergy leading to improved performance?
## Benchmark functions
- Artifical landscape/testbed for performance comparison of meta/hyperheuristic optimisation algorithms
	- These functions themselves are the problem definition and the evaluation
	- Can be easily computed
	- Ech function is recognised to have certain characteristics potentially representing a different real world problem
	- Known global optimum, measure how close get
### Classification of benchmark function
Common Classification:
- **Continuity**
	- Discontinuous
	- Continuous
	- ![[Pasted image 20250526163226.png]]
- **Dimensionality** 
	- Can extend to have as many variables as want
	- ![[Pasted image 20250526163239.png]]
- **Separability**
	- **Separable vs non separable**
	- Whether functions variables are independent or dependent, i.e., changing one variable affects the other variable or not
	- Can optimise variables independently or not
	- ![[Pasted image 20250526163250.png]]
- **Modality**
	- Unimodal - one global minima, relatively simple
	- Mutlimodal - few local minima, global minima
	- Multimodal - exponential number of local minima, and global minima


### TSP
- Binary representation and classic random initialisation, crossover and mutation operators are not suitable for encoding permutation?
- Could cause illegal tours to form e.g., visit self, straight after visiting self. 

#### Generic permutation based genetic operators
##### Partially mapped crossover
- Build offspring by:
	- Choose subsequence of tour from one parent.
		- Choosing two random cut points to serve as swapping boundaries for segment
		- Swap segment between parents
	- Preserving order and **position** of as many cities possible from other parent
	- If conflicted position, use swapping rule defined in segment to replace
	- ![[Pasted image 20250309005344.png]]
##### Order crossover
- Build offspring by:
	- Choose subsequence of tour (segment) from one parent, copy it to corresponding child
	- Starting from 2nd cut point position of either parent, cities are copied into corresponding offspring in same order, preserving relative order of tour. 
	- ![[Pasted image 20250309010025.png]]

##### Cycle Crossover
- Create offspring:
	- Aim to preserve **absolute positions** in elements from both parents
	- Key idea, each element in child must come from same position in one of the parents
	- Randomly select starting point in $p1$ 
	- Copy cities from $p1$ until cycle achieved
	- Rest filled in from other parent![[Pasted image 20250309010537.png]]
		Start at pos 0, value = 1 from p2
		Look at pos 0 in p2, value = 4, where is this in p1, position 3, so placed at position 3.
		Look at pos 3 in p2, value = 8, where is 8 in p3, pos 7 ![[Pasted image 20250526165027.png]]
		Then fill in rest from p2
#### GA vs MA for TSP experiments
- Representation: permutation
- Fitness function: path length
- Selection: tournament selection
- Crossover: PMX, 2PTX, OX
- Mutation: Exchange, Inversion
- Local search: TG, SS with strong elitism
- Replacement:RMHC using ISM and/or EM.

## Multimeme memetic algorithms
### Self adaptation for genetic operators
- **Self adaptation** - Deciding which operators and settings to use on fly, adaptively, during evolutionary search process, rather than fixed settings
- Davis, used varying probabilities, of applying different ops, to achieve this
	- Update probabilities, dependeing on performance of operator, during last few generations
- May use an additional bit to decide whether to apply 2PTX or UX to an individual
- Could encode
	- Mutation Rate
	- Crossover Type
	- As part of some chromosome, generated randomly, and pass to offspring, converging on setting which leads to optimal solutions.

### Memetic material
- Introduce memetic material for each individual;
	- Each individual, carries set of instructions, determine how solution/individual will improve, evolve alongside invididual
	- Genetic operators, governed by rules themselves that are subject to evolution
	- **A meme encodes, how to apply an operator (which one to apply, max no of iterations, acceptance criteria), when to apply, where to apply, how frequently to apply**
	- Memes of differing operators, can be combined under a memeplex. 
	- So individual, has both genetic, and memetic material, encoding both solution, and operator governed by rules, all of which evolve generationally.
	- Memes of differing operators, can be combined under a memeplex.

#### Memeplex
- Memeplex combined memes.
- Memeplex = $<Meme>;<Meme>:<Memeplex>$ 
- Meme = $(<Where>, <When>, <How>, <Frequency)$
- Where = $<Location> Crossover; <Location> Mutation$ 
- Location = $After ; Before$
- When = $< BooleanCondition>$
- How = $GeneralHillClimber <Move> <Strategy> <Iterations>;$ $\newline BoltzmanAdaptiveHillClimber <Move> <Strategy <Iterations>$
- Frequency = Always; For $<Iterations>$
- Move = 2-exchange, 3 exchange
- Strategy = NextAscent, SteepestAscent
- Iterations = $<Num>$
- Encodes two hill climbing algorithms, one applied after crossover, the other before mutation, given that a boolean condition is satisfied, always applying for num iterations, and having distinct strategies.
#### Features of multimeme memetic algorithms
- Memes represent instructions for self improvment
	- Set of rules, programs, heuristics, strategies, behaviours
- Interactions between memes and genes are not direct
	- Mediated by individual
- Memes themselves can evolve and change, by means of nontraditional genetic transformations

### MAX-SAT example
![[Pasted image 20250309032958.png]]
![[Pasted image 20250309032540.png]]
Singular meme, encoded via integer representation (or can use binary, either or)

Or can use memeplex
![[Pasted image 20250309032640.png]]
Then co-evolve this memetic material, according to outline strategy. Can have stochastic element to this as well during crossover. 


#### Mutating memes during evolution
- **Innovation rate** (IR) $\in [0,1]$ is probability of mutating the memes.
- Mutation randomly sets meme option to one of the other options. 
- IR = 0, no innovation, if meme option not introduced in initial generation, not reintroduced again.
	- Still permits inheritance of parent memes and memeplexes.
- All different strategies implies by the available $M$ memes may be equally used. 

### Measure for evaluating meme performance
- Concentration of a meme ($c_i, t$) is total number of individuals that carry the meme $i$ at a given generation $t$ 
	- Crude measure, gives no info about **continual usage** of meme throughout successive generations, could be due to stochastic element. 
- Evolutionary activity of a meme ($a_i$, $t$) is the acccumulation of a meme concentration until a given generation $t.$ 