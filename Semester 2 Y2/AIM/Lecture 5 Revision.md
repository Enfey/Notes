# Evolutionary algorithms
## Preface
- Nature, problem solver, evolution
- Nature, complexity, achieved in short time
- Want to solve complex problems as quickly and reliably as this on computer
## Evolutionary background
- Heritable traits, passed one generation, to next, via DNA
- Change/genetic variation comes from 
	- **Mutations**: changes in DNA sequence
	- **Crossover** - reshuffling of genes through sexual reproduction and migration between populations
- Evolution, driven by natural selection. Genetic variations that enhance survival, become + remain more common in successive generations of population

## Evolutionary algorithms
- Simulate natural evolution of individual structures at genetic level, using survival of fittest, via processes of **selection**, **mutation** and **reproduction**
- An **individual(chromosome)** represents candidate solution for problem
- A collection of individuals currently active is called **population**
- Population **evolved** from **one generation** to next depending on **fitness of individual**s in given environment
- ***Hope: Last generation contains best solution***
### List of EAs
- Genetic algorithms
	- Memetic algorithms
- Genetic programming evolves programs in tree form
- Evolutionary programming, evolves parameters of program or heuristic with a fixed structure.

### Genetic algorithms
![[Pasted image 20250308010923.png]]

Generate multi-point population, select parents, crossover, mutate, evaluate, enact replacement strategy.

#### Basic Components of Genetic Algorithms
- **Genetic Representation**/Encoding for candidate solutions
- An **initialisation scheme** for first population
- A **fitness function**
- A **scheme for selecting mates for crossover**
- **Crossover mechanism** - exchange genetic material, produce offspring
- **Mutation mechanism** - perturb individuals, often children
- **Replacement strategy** to select surviving individuals for next generation
- **Termination criteria**
- **Values for various GA parameters e.g., pop size**

##### Component - Representation
- **Haploid structure** - **each individual**, **one chromosome**/representation
- Each **individual is evaluated**, has a **fitness value**
- Chromosomes contain fixed num of genes: **Chromosome length**
- Traditionally, **binary encoding**, each gene: **$\text{Allele value} \in \{0,1\}$
- A population contains fixed number of individuals: pop size
- Each **iteration** is referred to as a **generation**

##### Component - Initialisation
- **Random initialisation**
- **Pop size** num of individuals, created randomly
- Each gene at a locus/index of individual, assigned allele value randomly
	- e.g., if random val > 0.5 then 0, otherwise 1.
###### Example - MAX-SAT: Initialisation
![[Pasted image 20250308012617.png]]
![[Pasted image 20250308012651.png]]
Chromosome size = 6, pop size 4, assign allele value as 1 at locus of individual if random val between 0 and 1 exceeds 0.5, 0 otherwise.

##### Component - Fitness Function
- Fitness value indicates:
	- **How fit individual is to survive + reproduce under current generation/conditions**
	- **How much current solution, meet, requirements, problem**
- Obtained, applying eval function, to individual.
- **genotype (101110) to phenotype e.g., 1 mapping**
###### Example - MAX-SAT Fitness calculation
![[Pasted image 20250308013129.png]]

##### Component - Reproduction/Crossover selection
- **Reproduction**
	- **Selecting individuals**: apply **selection pressure,** **considering fitness of individuals in population.**
		- Higher fitness, more likely selected.
	- **Select 2 parents using same method, then undergo crossover.**
###### Example - Roulette Wheel Selection
- **Fitness level** used to associate **probability of selection** $prob_i$ with each individual chromosome/individual
- Issue of diversity
$$prob_i = \frac{fitness_i}{\sum_{j}fitness_j} \ ,j = 1 ... pop.size$$
Fitness divided by sum of all fitness values, so better solutions get larger slices(maximisation)
Get 2 parents via spin twice
![[Pasted image 20250308014901.png]]

###### Example: Tournament selection
- Run tourments, among randomly chosen individuals of size $tour$, selecting one with best fitness at end
- Repeat for each parent selected for crossover.
###### Tournament selection MAX-SAT
- Tour size = 3
- Throw random number between 1 and pop size for 3 times
- Select individuals, return the one with best out of those as parent.
- ![[Pasted image 20250308015249.png]]
- ![[Pasted image 20250308015256.png]]
- ![[Pasted image 20250308015300.png]]

##### Component - Recombination/Crossover
- Selected pairs/mates via selection are recombined, form new individuals
- Crossover, applied with **crossover probability**, $p_c$, in general, chosen close to 1.0.
###### One Point Crossover (1PTX)
- Generate random number $\in [0,1]$, if smaller than crossover probability $p_c$ then
	- Select random crossover site in $[1, chromosome.length]$ 
	- Split individuals at site
	- Exchange segments between pairs, to form two new individuals ![[Pasted image 20250308015724.png]]
###### 2 Point Crossover (2PTX)
- Generate random number $\in [0,1]$, if smaller than crossover probability $p_c$ then
- Select 2 random non-equal crossover sites in $[1, chromosome.length]$
- Split individuals at sites
- Exchange segment between pairs ![[Pasted image 20250308015742.png]]
	K point is the same
###### Uniform crossover
- Considers each bit in parent strings with a probability of 0.5, throw random num, if exceeds 0.5, then swap, otherwise keep.

##### Component - Mutation
- Any offspring/recombined parents, may be exposed, mutation
- Loop through all genes of individual, if that gene selected for mutation give probability $p_m$, can either change by small amount, or replace with small value
	- Bit flip to opposite allele for given locus, binary.
- Mutation, provides diversity, increases exploration, explore different regions of search space
- Mutation rate $p_m$ chosen to be very small typically
- $p_m = 1/chromosome.length$ implies single gene mutated for individual
- ![[Pasted image 20250308020206.png]]
##### Component - Replacement strategy
- Determines how next gen of individuals created, from current population + offspring
- **Generation gap** $\alpha$ represents proportion of population, replaced, each generation. 
	- $\alpha \in [1/n, 1.0]$ 
	- Num of offspring produced in each generation is $g = \alpha \cdot N$ 
		- E.g., $g = 1/100 \cdot 100 = 1$ 
		- E.g., $g = 1.0 \cdot 100 = 100$ 
		- Create at least 1, or n size, bound to generation gap

###### Trans Generational GA replacement (g > 2 a > 2/n)
- $N$ individuals produce produce $a \cdot N$ offspring 
- Replace worst of $N + (a \cdot N)$ via sorting of fitness values and choosing $N$ best, elitism.
- Individuals from both parent and offspring set of individuals can survive, not strict replacement. ![[Pasted image 20250308202416.png]]
- ![[Pasted image 20250525232321.png]]
- ![[Pasted image 20250525232327.png]]
- 
###### Steady state replacement
- Replace small number of individuals, usually 2
- $a = 2 / N$, thus $g = 2$ 
- **Method 1**: two offspring replace two parents
- **Method 2**: two offspring replace worst two of population
- **Method 3**: best two of parents and offspring replace two parents(elitism)
- **Method 4:** best two of parents and offspring replace two of population(strong elitism)


##### Component - Termination criteria
- Evolution(main loop), continues, until termination criteria is met:
	- Predefined number of generations exceeds
	- A goal is reached
		- Expected fitness value
		- Population converges
	- Best fitness does not change for $x$ iterations
	- A condition is satisfied based on combination of above.
###### Convergence
- **Defined as progression toward uniformity**
- **Gene convergence**
	- A location in chromosome is converged when 95% of individuals have same gene value for that location
- **Population convergence**
	- A population is converged when all genes have converged. (individuals all alike - can still have different fitness)
- **Phenotypic Convergence** 
	- Average fitness of population approaches best individual in population
#### Key Features of EAs
- Population based search approaches
	- Independent of initial starting points
		- Start search from many points in search space
	- Conduct search in parallel over search space, implicit parallelism
	- Balance exploration and exploitation
	- May be used together with other approaches

### Memetic Algorithms
- Key idea, improve GAs, embedding local search
- MAs make use of exploration capabilities provided by GAs and leverage exploitation provided by local search algorithms
- Shown to be much faster and accurate than GAs on some problems![[Pasted image 20250308204724.png]]
- Perform local search prior to evaluating population and replacing current geneation

## Example - Memetic algorithm for MAX-SAT
- **Representation** - Binary, length $N$ 
- **Initialisation** - Random
- **Fitness function** - $C_{max}$, num of satisfied clauses
- **Selection** - tournament selection, tour size of $2$
- **Crossover** - $1PTX$, crossover probability $0.99$
- **Mutation** - random bitflip, mutation probability $1/N$ 
- **Local search** - DBHC
- **Replacement** - Steady state GA, best two of parents and offspring replace worst 2 individuals in population, harsh elitism.
![[Pasted image 20250308205235.png]]
![[Pasted image 20250308205243.png]]
![[Pasted image 20250308205254.png]]
![[Pasted image 20250308205254.png]]![[Pasted image 20250308205305.png]]![[Pasted image 20250308205322.png]]![[Pasted image 20250308205402.png]]![[Pasted image 20250308205645.png]]![[Pasted image 20250308205711.png]]