# Evolutionary algorithms
## Preface
- Nature as problem solver, evolution.
- Beauty-of-nature: Complexity achieved in *short* time.
- Can solve complex problems as quickly and reliably on computer.
## Evolutionary background
- Heritable traits passed from one generation to next via DNA
- Change/genetic variation comes from
	- Mutations: changes in DNA sequence
	- Crossover: reshuffling of genes through sexual reproduction and migration between populations.
- Evolution driven by natural selection, genetic variations that enhance survial become and remain more common in successive generations of population.
## Evolutionary algorithms (EAs)
- Simulate natural evolution of individual structures at the genetic level using survival of the fittest via processes of **selection**, **mutation,** and **reproduction**.
- An **individual** (chromosome) represents candidate solution for problem e.g., < 100101 >.
- A collection of individuals currently 'alive' called population
- Population is evolved from one generation(iteration) to next depending on the fitness of individuals in given environment
- Hope: last generation contains best solution
### List of EAs
- Genetic algorithms (evolves (bit) strings)
	- Memetic algorithms
- Evolutionary programming evolves parameters of a program with a fixed structure
- Evolution strategies
- Genetic programming evolves computer programs in tree form

### Genetic algorithms
![[Pasted image 20250308010923.png]]
#### Basic components of Genetic Algorithms
- Genetic representation/encoding for candidate solutions
- An initialisation scheme to generate first population of individuals
- A fitness/evaluation function
- A scheme for selecting mates for recombination
- Recombination/Crossover exchanges 'genetic material' between mates producing offspring
- Mutation perturbs an individual creating a new one
- Replacement strategy to select surviving individuals for next generation
- Termination criteria
- Values for various parameters that GAs use e.g., population size, probabilities of applying genetic operators etc.
##### Component - Representation
- **Haploid structure** - each individual contains one chromosome/representation
- Each individual is evaluated and has a fitness value
- Chromosomes contain a fixed number of genes: **Chromosome length** 
- Traditionally **binary encoding** used for each gene: **Allele value** $\in \{0,1\}$ 
- A population contains a fixed number of individuals: population size.
- Each **iteration** is referred to as a **generation**
##### Component - Initialisation
- Random initialisation
- Population size number of individuals created randomly
- Each gene at a locus/index of an individual is assigned allele value  0 or 1 randomly
	- Decide by flipping a coin e.g., if random val > 0.5 then 0, otherwise 1
###### Example - MAX-SAT: Initialisation
![[Pasted image 20250308012617.png]]
![[Pasted image 20250308012651.png]]
Chromosome size 6, assume population size 4, assign allele value as 1 at locus of individual if random value between 0 and 1 exceeds 0.5, 0 otherwise.
##### Component - fitness/evaluation function
- Fitness value indicates:
	- How fit individual is to survive and reproduce under current generation/conditions
	- How much does current solution meet requirements of objective function
- Obtained via applying eval function to individual/candidate solution - genotype (101110) to phenotype e.g., 1 mapping
###### Example - MAX-SAT: Fitness calculation
![[Pasted image 20250308013129.png]]
##### Component - Reproduction selection
- Reproduction consists of
	- **Selecting individuals**: apply selection pressure considering fitness of individuals in population -> e.g., roulette wheel selection, tournament selection, rank selection, etc
		- selection pressure means that individuals with better fitness have higher chance of being selected.
	- usually 2 parents are selected using same method, which will go under the crossover/recombination method. 
###### Example - Roulette Wheel Selection
- **Fitness level** is used to associate a **probability of selection**($prob_i$) with each individual chromosome/individual ($i$)
- While candidate solutions with a higher fitness, there is still a chance that they may be, which also introduces diversity(maximisation problem)
$$prob_i = \frac{fitness_i}{\sum_{j}fitness_j} \ ,j = 1 ... pop.size$$
- Probability of selection, is fitness divided by sum of all fitness values, so better solutions get larger slices (maximisation, this is. )
- Spin twice, get 2 parents. 
###### Roulette wheel MAX-SAT
![[Pasted image 20250308014901.png]]

###### Tournament selection
- Involves running a number of tournaments among randomly chosen individuals (of tour size), selecting the one with the best fitness at the end
	- Process repeated for selecting each parent for recombination/crossover
###### Tournament selection MAX-SAT
- Tour size = 3
- Throw random number between 1...population size, tour size times.
	- ![[Pasted image 20250308015249.png]]
	- ![[Pasted image 20250308015256.png]]
	- ![[Pasted image 20250308015300.png]]
##### Component - Recombination - Crossover
- Selected pairs/mates via reproduction are recombined to form new individuals - 'exchange of genetic material'
- Crossover is applied with a **crossover probability**, $p_c$ which in general is chosen close to 1.0
- **One Point Crossover(1PTX)**
	- Generate random number in [0,1], if it is smaller than a crossover probability $p_c$ then
		- Select random crossover site in [1, chromosome length]
		- Split individuals at the selected site 
		- Exchange segments between pairs to form two new individuals
- ![[Pasted image 20250308015724.png]]
- ![[Pasted image 20250308015742.png]]
##### Component - Mutation
- Any offspring/recombined parents may be exposed to mutation
- Loop through all the alleles of the individuals one by one, if that allele is selected for mutation with a given probability $p_m$ you can either change it by a small amount or replace it with a small value. 
	- Binary representation mutation corresponds to flipping allele at given locus. 
- Mutation provides diversity and increases exploration, allowing GA to explore different regions of the search space
- Mutation rate $p_m$ is chosen to be very small typically
- Choosing $p_m$ as (1/chromosome length) implies on average single gene will be mutated for an individual.
![[Pasted image 20250308020206.png]]
##### Component - Replacement strategy
- Determines how next generation of individuals is created from current population and offspring.
- Generation gap ($a$) parameter represents proportion of population that is replaced in each generation
	- $a \in [1/n, 1.0]$, ranges
	- Number of offspring produced in each generation is $g = a * N$
		- e.g., 1/N, g = 0.01 x 100, = 1 offspring
		- e.g., 1, g = 1 x 100, = 100 offspring
- (Trans-)Generational GA(g > 2, that is a > 2/N)
	- N individuals produce $aN$ offspring, so ($N + aN)$ -> N
	- replace worst of N + $aN$ via sorting of fitness values and choosing N best (elitism)
	- ![[Pasted image 20250308202416.png]]
	- Individuals from both parent and offspring set of individuals can survive, not a strict replacement, ensures population size remains constant. 
- Steady State GA (g = 2, a = 2/N)
	- Replace a small number of individuals, usually two offspring
	- **Method 1**: two offspring replace two parents
	- **Method 2**: two offspring replace worst two of population
	- **Method 3**: best two of parents and offspring replace two parents (elitism)
	- **Method** 4: best two of parents and offspring replace two of population (strong elitism)

###### Transgenerational GA replacement example (no elitism)
![[Pasted image 20250308203100.png]]
###### Transgenerational GA replacement example(elitism)
![[Pasted image 20250308203130.png]]

###### Steady State GA with elitism
![[Pasted image 20250308203205.png]]
etc

##### Component - Termination Criteria
- The evolution(main loop), continues until termination criteria is met, could be:
	- Predefined max number of generations exceeded
	- A goal is reached
		- Expected fitness value
		- Population converges
	- Best fitness does not change for a while
	- A condition is satisfied based on combination of above
###### Convergence
- Defined as the progression toward uniformity
- Gene convergence: a location in chromosome is converged when 95% of the individuals have same gene value for that location
- Population (genotypic) convergence: a population is converged when all the genes have converged (individuals all alike - they may have different fitness)
- Phenotypic Convergence - average fitness of population approaches to best individual in population (all individuals have same fitness)
##### Key features of EAs
- Population based search approaches
	- Be independent of initial starting points
		- Start search from many points in search space
	- Conduct search in parallel over search space
		- Implicit parallelism due to nature
	- Balance exploration and exploitation
	- May be used together with other approaches

## Memetic algorithms (MAs)
- Key idea, improve GAs by embedding local search
- MAs make use of exploration capabilities provided by GAs and leverage exploitation provided by local search algorithms.
- Shown to be much faster and accurate than GAs on some problems.
- ![[Pasted image 20250308204724.png]]
- Performs local search prior to evaluating population and replacing current generation. 
### Example - Designing Memetic algorithm for MAX-SAT
- Representation - Bit string of length N (truth assignment for each var)
- Initialisation - Randomly generate initial population, population size = N
- Fitness function - C, unsatisfied clauses, minimisation
- Mate selection/reproduction - Tournament selection with a tour size of 2
- Crossover - 1PTX, crossover probability 0.99 (basically guaranteed)
- Mutation - random bitflip, mutation probability = 1/N
- Local search - DBHC
- Replacement - Steady state GA, best two of parents and offspring replace the worst 2 individuals in the population. 
![[Pasted image 20250308205235.png]]
![[Pasted image 20250308205243.png]]
![[Pasted image 20250308205254.png]]
![[Pasted image 20250308205305.png]]
![[Pasted image 20250308205322.png]]
![[Pasted image 20250308205402.png]]
![[Pasted image 20250308205645.png]]
Not sure why keep 6, thought we were minimising but ig not lol
![[Pasted image 20250308205711.png]]