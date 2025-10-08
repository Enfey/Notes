# Decision support
Term, used, variety of contexts (tools, systems) with regard to supplementing decision making. Decision making, relates to process of making choice between alternatives.

## Systems
A system = number of related elements, perfoming activity, function, or task.
1. Open systems - interact w environment, exchange resources and info
2. Closed systems - self contained, no interaction with environment.
2 measures to eval systems
3. Effectiveness - degree to which goals are achieved
4. Efficiency - measure of use of inputs/resources to achieve an output

## Solving problems by searching
Solve problems via searching:
- Systematically searching from paths to goals OR
- Searching for solutions, the process of optimisation

## Sewarching for paths to goals
Involves finding set of actions, move from given initial state, to a goal state. E.g., game playing, path finding. 


## Search for solutions
More general than path to goals. Involves efficiently finding a solution to problem in large search space, focus on optimisation and best solution, rather than any valid path. Subsumes searching through paths to goals. 


## Solving a single-objective optimisation problem
Can be formulated, mathematical optimsation problem. Goal is to minimise or maximiise single objective function, which evaluates quality of a given solution.

 $$min Z = f(X)$$
Where:
- Z is the objective function value - a single measure of the quality of that solution
- F(X) is the function that takes input X, represented as a vector of variables $<x_1, x_2,...x_n>$
- X represents the decision variables that can be adjusted to optimise Z.
Find the best possible configuration of X that results in smallest possible value of Z. 

Optimisation problems, may have number of constraints:
- Variables may or may not be bounded
- May be continuous or discrete, could hold any value, or may only take int values
- Could have infinitely large search space
How to find optimal solution given constraints? Must choose optimal methods given set of constraints e.g., searching through continuous spaces vs discrete spaces.


## Local vs Global optimum
Common issue, search algorithm get stuck somewhere locally optimal

Local optimum = better than all other solutions in neighbourhood $N$ 
Global optimum = better than all other solutions in search space.

N defined as ±$\delta$,means small interval around a point, given as:
$$ N(x_0) = {x | x \in [x_0 - \delta, x_0, + \delta]} $$
![[Pasted image 20250216214506.png]]


## Continuous vs Discrete search space
### Continuous search space
Coninuous search space, means that variables/solutions, can take any value, within certain range or domain.  E.g., in a problem where the objective function is to minimise a certain quantity, the variables can vary and take any real value e.g., $x \in [0,10]$. 

## Discrete search space
Variables/solutions, take only specific, distinct, countable values, no intermediate values.


## Problem and problem instance
### Problem 
high level question/optimisation issue to be solved. General description of task or challenge without concrete info. 


### Instance of a problem 
Concrete expression of a problem, represents input for a decision or optimisation problem. 


### Combinatorial optimisation problems
Require finding an optimal solution, from finite set of solutions. For NP-Hard COPs, finite set grows exponetially with the instance size, becomes impossible to exhaustively search through all possible solutions for anything other than smallest of instances.

## Optimisation/Search methods and techniques
### Exhaustive/Exact/Systematic
- Dynamic programming, branch and bound, constraint satisfaction
- Systematically traverse search space
- Has optimality guarantees
### Inexact/Approximate/Local search methods
- Heuristics/Metaheuristics/Hyperheuristics
- Samples search space via neighbourhood operators, find reasonable quality solutions, reasonable amount of time
- No optimality guarantees


### Search paradigms
#### Single Point vs Multi Point
- **Single Point** - Explore search space, starting from single solution, move toward better solution, or improve current.
- **Multi-point** - Explore search space using multiple solutions, provider broader exploration, points can evolve independently or interact w each other
#### Perturbative vs Constructive
- **Perturbative** - operate on complete solutions, make small changes to it, exploring new ones
- **Constructive** - gradually build up solution, start with empty/partial solution, form complete one
#### Deterministic vs Stochastic
- **Deterministic** - approach search same way every single time via prescribed set of instructions, same solution each time
- **Stochastic** - explore search space differently based on some random choice, never guaranteed same solution
#### Systematic vs local search
- **Systematic** - Explore entire search space, exhaustive manner, structured approach
- **Local search** - Search limited area of search space, often make incremental improvements to solution
#### Sequential vs Parallel
- Searching solution space in single sequence/step by step
- Searching multiple solution paths at same time ? 
#### Single objective vs Multi-objective
- Measure using multiple objectives to measure numerous solution aspects. 

# Heuristic search and optimisation
> A heuristic is just a rule of thumb method derived from human intuition.

Have an idea about how we might approach some problem, and just try it.

Heuristics are:
- **Problem dependent search methods** which seek **good solutions** in **reasonable computational budget**
- Good for solving **ill-structured problems**, or **well-structured problems** that are **complex**
- **Not able** to **guarantee optimality** (unless mathematically prove otherwise.)
Have 2 b DESIGNED for each problem trying to solve. 

TSP heuristics 
= NN = constructive, stochastic (random start city), systematic.
= Pairwise exchange = pertubative, stochastic, local search


## Caveats of heuristic search
- No optimality guarantees
- Usually require designing new heuristics when applying to a new problem
- Often parameterised
- May result in poor quality solutions when using stochastic methods or used on different problem instances. 



# Psuedo Random numbers
- Generate a sequence of numbers in deterministic way, appear to be random.
- Useful, via seeding, guarantees repeatability of results despite stochastic mechanisms and the like. 