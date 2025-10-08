### Decision support
A term used in variety of contexts (e.g., tools, systems) with regard to supplementing decision making. Decision making relates to the process of making a choice between alternatives.

### System
A system can be defined as a number of related elements which perform an activity, function, or task. 
There are two classes of systems:
1. **Open Systems**  - interacts with environment and exchanges resources/information.
2. **Closed systems** - self-contained and does not interact with its environment, does not exchange information with outside world.
And 2 typical measures to evaluate systems:
1. Effectiveness - the degree to which goals are achieved (think sudoku solvers)
2. Efficiency - a measure of the use of inputs/resources (time taken, memory etc) to achieve an output.

### Solving problems by searching
We can solve problems by:
- Systematically searching from paths to goals e.g., tree/graph search, **or**
- Searching for solutions (the process of optimisation)

#### Searching for paths to goals
Involves finding a set of actions that will move from a given initial state towards a goal state. Central to many AI problems such as game playing and path finding. 
- BFS
- DFS
- UCS
- A*
- Branch and Bound

#### Search for solutions(the process of optimisation)
More general than searching for paths to goals. Involves efficiently finding a solution to a problem in a large search space, focusing on optimisation and finding best solution, rather than just any valid path.  Subsumes searching through paths to goals (a path through a search tree can be encoded as a candidate solution in an optimisation process.) 

#### Solving a single-objective optimisation problem
Such problems can be formulated as mathematical optimsation problems. The goal is to minimise or maximise a single objective function, which evaluates the quality of a given solution.
 $$min Z = f(X)$$
Where:
- Z is the objective function value - a single measure of the quality of that solution
- F(X) is the function that takes input X, represented as a vector of variables $<x_1, x_2,...x_n>$
- X represents the decision variables that can be adjusted to optimise Z.
Find the best possible configuration of X that results in smallest possible value of Z. 

Optimisation problems may be subject to a number of constraints e.g.,
- Variables may or may not be bounded e.g., -5 to +5 or negative infinity to positive infinity 
- May be continuous or discrete - e.g., could hold any value or may only take integer values
- Could have an infinitely large search space - see above
- Systems of equations generally indifferentiable
How do we find the optimal solution when given such constraints?
Must choose the optimal methods given the set of constraints e.g., searching through continuous spaces vs discrete spaces. 


### Local vs Global optimum
Common issue of search algorithms to get stuck somewhere which is locally optimum, especially in large search landscape.
#### Local optimum
Better than all other solutions in a given neighbourhood N.
#### Global optimum
Better than all other solutions in the entire search space. 


E.g., 1 dimensional search space
N defined as ±$\delta$,means small interval around a point, given as:
$$ N(x_0) = {x | x \in [x_0 - \delta, x_0, + \delta]} $$![[Pasted image 20250216214506.png]]


### Continuous vs Discrete Search Spaces 
#### Continuous search space
Continuous search space means that the variables/solutions can take any value within a certain range or domain. E.g., in a problem where the objective function is to minimise a certain quantity, the variables can vary and take any real value e.g., $x \in [0,10]$. 

#### Discrete search space
Discrete search spaces mean the variables/solutions can only take specific, distinct, countable values with no intermediate values. 



### Problem and problem instance
A **problem** is the high level question/optimisation issue to be solved; general description of task or challenge trying to solve e.g., TSP, doesn't specify at high-level which cities or how many where are. 
An instance of a problem is a concrete expression which represents the input for a decision or optimisation problem e.g., 50 cities TSP. 

#### Combinatorial optimisation problems
Require finding an optimal solution from a finite set of solutions.
For NP-Hard COPs, this finite set grows exponetially with the instance size, hence becomes impossible to exahustively search through all possible solutions for anything other than smallest of instances. 

### Optimisation/Search methods and techniques 
Broadly classified into two approeaches 
#### Exact/exhaustive/systematic
- Dynamic programming, branch and bound, constraint satisfaction
- Systematically traverse the search space
- Has optimality guarantees

### Inexact/approximate/local search methods (focus here)
- Heuristics/Metaheuristics/Hyperheuristics
- Sample search space via neighbourhood operators to find reasonable quality solutions in reasonable amount of time
- No optimality guarantees

#### Search paradigms
- Single points vs multi point
	- Explore search space starting from single solution, either move toward better solution or iterativelt imporve current
	- Explore search space using multiple points, provide broader exploration, these points can evolve independently or interact with each other 
- Pertubative heuristics operate on complete solutions
	- Operate by taking complete solution, make small changes to it to explore new solutions
- Constructive heuristics operate on partial solutions
	-  Operate by gradually building up solution from scratch, start with empty/partial solution to form complete solution
- Deterministic vs stochastic
	- Approach search same way every single time via prescribed set of instructions, same solution each time  
	- Explore search space differently based on some random choice, never guaranteed same solution. 
- Systematic vs local search
	- Explore entire search space in exhaustive manner, structured approach
	- Searching in limited area of search space, often make incremental improvements to solution
- Sequential vs parallel
	- Searching solution space in single sequence/step by step
	- Searching multiple solution paths at same time. 
- Single objective vs multi-objective
	- Measure using multiple objectives to measure multiple solution aspects.



# Heuristic search and optimisation
> A heuristic is a rule of thumb method derived from human intuition. 

Have an idea about how we might approach some problem, and just try it. 

Heuristics are:
- **Problem dependent** search methods which seek good solutions in reasonable computational budget
- Good for solving ill-structured problems or well-structured problems that are complex
- Not able to guarantee optimality (unless can mathematically prove otherwise)
Have to be designed for each problem trying to solve. 


Some heuristics for TSP:

### Nearest neighbour

- Constructive
- Stochastic
- Systematic

1. Choose random city (stochastic)
2. Apply NN to construct complete solution 
3. Compare new solution to best solution found so far and update if improved
4. Repeat until some max number of iterations reached
5. Return best solution found
### Pairwise exchange
- Pertubative
- Stochastic
- Local search

1. Create a random current solution
2. Apply pairwise exchange to form a new solution (remove 2 edges, replace them with 2 different edges)
3. Compare new solution to current solution, replacing current solution with new solution if improvement made
4. Repeat until some max number of iterations reached by going back to step 2
5. Return current solution


## Caveats of heuristic search
No optimality guarantees in constrast to exact methods
Usually require designing new heuristics when applying to a new problem.
Often parameterised e.g., number of iterations and very sensitive to their settings
May result in poor quality solutions when: 
- Used for different problem instances
- Applied second time (stochastic methods)


### Psuedo-random number generators
Generate a sequence of numbers in a deterministic way but appear to be random. 

FINISH THIS OFF LATER