# Metaheuristic
> A **metaheuristic** is a high-level problem-independent framework providing guidelines/strategies for how to develop heuristic optimisation algorithms.


![[Pasted image 20250304013218.png]]
## Types of metaheuristics
- **Local search** metaheuristics are **single-point** based trajectory methods that **improve a single solution** over time
- **Population** based metaheuristics **evolve** a set of solutions over time
- **Constructive** metaheuristics construct new solutions.

## Main components of metaheuristics
Include basic components of heuristic search methods seen previously:
- **Solution representation**
- **Initialisation method**
- **Objective function**
- **Neighbourhood operators**
Now need **additional** components
- **Mechanism for escaping local optima**
- **Termination condition**
- **Multiple move operators**
### Mechanisms for escaping local optima
Key issue in heuristic optimisation algorithms. ![[Pasted image 20250304030440.png]]
- **Change the current solution or restart the search procedure when a local optima is detected**
	- Move to a random solution in the neighbourhood
	- OR generate a random solution - invole reinitialisation
- **Change the search landscape**
	- **Can change the objective function itself,** add/remove criteria.
	- Can use mix of different neighbourhood operators e.g., hill climbing, iterations randomly select different neighbourhood operators
- Allow acceptance of non-worsening moves to permit random walk of plateaus which may lead to escape.
- Maintain a memory to prohibit specific moves which would lead to being stuck.

### Termination criteria
Exceeding a fixed number of the following, can be classed as terminatin criteria:
- **Iterations**
- **Moves**
- **Obj function evaluations**
- **CPU time, can be problematic e.g., VRT vs real-time.**
Other ideas involve looking at progress of search itself:
- Consecutive iterations since solution last improved 
- Evidence we are at the optimum
- No feasible solution obtained within a fixed limit. 
### Feasibility of solutions
Not all solutions are feasible. Do not satisfy hard constraints e.g., 0/1 knapsack, solution exceed weight, but has highest value.

#### Dealing with infeasibility
Instead of outright discard infeasible solutions in optimisation problems

##### Repair
Have problem specific repair operators, e.g., 0/1 knapsack, remove random item from knapsack until weight not exceed limit.
##### Penalise
A fixed penalty can be applied to any infeasible solution. If a solution $s$ is infeasible, its modified objective function $f'(s)$ is calculated using a fixed penalty.
$$f'(s) = min(p_0, ...p_4)/2 = $0.50$$
Note $p_n$ represents fixed penalty values assigned to different constraint violations.

A variable penalty can be assigned to any infeasible solution, depending on how much the constraints are violated. 

Can ensure feasibility is always preferred by adding strong penalty so that infeasible solution is strictly worse than feasible solution by modifying objective function again. 
![[Pasted image 20250304032139.png]]

## General guidelines/Art of search
Effective search techniques, provide a balance between **exploration** and **exploitation**, can escape from local optima, and are independent of the initial configuration. 

### General template for local search metaheuristics
![[Pasted image 20250304032333.png]]
Use explorative/exploitative operator, decide whether to move (move acceptance can be based on various criteria such as feasability and modified objective functions, could accept worsening solution to escape local optima etc.)

### Metaheuristics - Iterated local search
A metaheuristic that seeks to enhance simple local search algorithms like hill-climbing. Enforces balance between **exploration** and **exploitation**, visiting sequence of locally optimal solutions. 

- Perturbs current local optimum (exploration), then applies hill climbing to get to new local optima. 
- Perturbation strength = crucial
	- Too strong = lose good properties of local optima
	- Too weak may lead to cycles
		![[Pasted image 20250304033901.png]]
- Acceptance criteria
- Memory for restarts

#### Designing ILS for solving MAX-SAT
Binary representation
Minimisation obj function, num of satisfied clauses.
Need to choose each of following to create ILS algorithm
- GenerateInitialSolution - Random Initialisation
- Perturbation - Random bit flip
- Hill Climbing - **Steepest descent hill climbing** using 1-bit-flip neighbourhood operator. 
- Move Acceptance - accepting **non-worsening** moves

![[Pasted image 20250304034841.png]]
1. Generate Initial solution
	6 variables, binary string of length 6.
	Assign each variable to true or false based on random probabilities.![[Pasted image 20250304035323.png]]
	So gives $s_0 = <100100>$ with $f(s_0) = 3$ 
2. Repeat Loop
	Apply perturbation - 100100 -> 110100, f(s') =4
	Apply SDHC - 110100 -> 111100, f(s') = 1
	Check move acceptance, accepted, as minimised.
3. Repeat loop
	Apply perturbation - 111100 -> 101100, f(s') = 1
	Apply SDHC - 101100 -> 101000, f(s')=0
	Check move acceptance f(s') $\le$ f(s*), accepted.
Known global optimum found. 

![[Pasted image 20250304035925.png]]

#### Designing ILS for solving TSP
Permutation encoding
- Generate initial solution - NN
- Perturbation - exchange operator, applied n times
- Hill climbing - first improvement with adjacent swap
- Move acceptance - accepting improving moves
#### Guidelines for designing ILS
- Solution initialisation generally irrelevant for larger burdgets
- Perturbation and acceptance criteria interations can be important, and determine the relative balance between exploration/exploitation, large perturbations only useful if they can be accepted.
- Local search should be fast and effective
- Ideally, choice of perturbation operator, should be, steps not easily undone by local search operator
- Advanced methods, adapt perturbation strength during search. 
## Metaheuristics - Tabu Search
Maintain memory/list of prohibited 'tabu' moves to enhance local search techniques, avoiding specvific moves for certain number of iterations. 
![[Pasted image 20250306004832.png]]
***Determine non tabu neighbours of current solution, choose best, restrict solutions and free some, ALWAYS ACCEPT.***


### Components of Tabu Search
- **Forbidding Strategy**
	- Controls which moves/solutions are in **tabu list**
- **Freeing strategy**
	- Controls when moves/solutions are removed from **tabu list,** valid again
- **Short-term strategy**
	- Manages inter-play between forbidding and freeing strategies.
### Tabu Search visually
ISSUE:
Without a tabu list, become trapped in local optimum
	E.g.. GIVEN:
	Initial solution $x_0$:
	1. Determine all neighbours of $x_0$ 
		$N' = \{x_1, x_2\}$
		$s^* = x_2, \text{best in N'}$ 
	2. Determine all neighbours of $x_2$ 
		$N' = \{x_0, x_3\}$
		$s^* = x_0, \text{best in N'}$ 
	Stuck moving between these.
![[Pasted image 20250306005810.png]]

SOLUTION:
With a tabu list, can remember previously visited solutions and consider them as tabu - note in practice, forbid attributes, rather than complete solutions. 

Given complete solution $x_0$ and $T = \emptyset$ 
1. Determine admissible neighbours of $x_0$ 
	- $N' = \{x_1, x_2\}$ 
	- $s^* <- x_2$ 
	- Add $x_0$ to tabu list, $T = \{x_0\}$ 
2. Determine admissible neighbours of $x_2$ 
	$N' = \{x_0, x_3\}$ 
	$s^* = x_3$ as $x_0$ is forbidden
	Add $x_2$ to tabu list: $T = \{x_0, x_2\}$
Escaped local optima
![[Pasted image 20250306010231.png]]


### Guidelines for Tabu Search
- ***Tabu tenure*** $t$ needs to be set appropriately
	- Too large, overly restricts search, lose good quality solutions
	- If $t$ is too small, risk cycling.
	- Guideline: $t = 7$, or $t =$ $\sqrt{n}$ 
- Aspiration criteria needs to override a solution's tabu state - a simple method to do this would be to allow solutions which are better than currently known best solution ? 
### Designing TS for solving MAX-SAT
- Solution representation = binary encoding
- Initialisation = random initialisation
- Neighbourhood  = 1 bit flip operator
- Memory - associate moves as **tabu** if they have been changed in the previous $t$ steps.![[Pasted image 20250306011655.png]]
- Ran for several steps. Take valid neighbourhood, $x_0, x_2$ tabu, take best, mark $x_3$ as tabu, remove $x_0$

## Performance comparison of algorithms
How to determine which metaheuristic/heuristic is **best**?

Must be **precise**
	What is meant by best?
	Best for what?
Formulate hypothesis
	*DBHC finds better solutions than SDHC for solving some specific MAX-SAT problem instance $\varphi$ when given a computational time budget of 1 second.*
Make sure comparison fair, i.e., computational time, number of evaluations, same for all algorithms. Number of trials ensure reliability of results. May perform totally different with a stocahstic element. Data used, must be drawn from same sample, e.g., same set of problem instances.

Given results obtained after performing 30 trials on problem instance $1$ of problem $X$, which algorithm performs **best** for solving problem $X$.
	Each algorithm, compute basic state:
	- **Average** - mean objective value over all trials
	- **Std** - standard deviation associated with the avg
	- **Median** - median objective value over all trials
	- **Min** - the objective value of best solution found over all trials

### Statistical testing
Basic stats okay for indication, but lack **significance.**
Many statistical tests can be used to conclude stronger, statistically significant observations. 

All statistical tests include:
- Null Hypothesis ($H_0$) - hypothesis want to test against, assumed true, unless evidence is strong enough to reject it in favour of $H_1$. States no effect.
- P-value - probability value, indicating how likely it is that data occurred by random chance, thus, statistical evidence can reject null hypothesis.
	- Determines **level of significance** our results hold, more stringent evaluation of p is e.g., p <= 0.0001, more significant the results
	- Typically use $p \le 0.05$ - 95% confident results weren't due to chance
- Alternative Hypothesis - hypothesis want to prove, negation of $H_0$ 

#### Which statistical tests to use
- Typically, data/results from heuristic methods do not come from a normal distribution -> use non-parametric test.

##### Non-Parametric tests
**Pairwise Comparisons**
- Sign Test - paired data(same set of data), focus on direction, median value
- Wilcoxon Signed Rank Test - paired data, not gaussian distribution, median value
**Independent**
![[Pasted image 20250525174547.png]]


# Scheduling
Many real world problems, scheduling problems:
- Project planning
- Scheduling for CPUs
- Semiconductor manufacturing
Scheduling problems deal with allocation of finite resources to **tasks** over given **time periods** with goal of optimising one or more objectives.

## Scheduling framework and notation
All scheduling problems, assume number of **jobs** and **machines** are finite.
- ***Jobs*** $j$= $1 .. n$
- ***Machines*** $m$ = $1..m$ 
$(i,j$) denotes a processing step of **job** $j$ on **machine** $i$.
**Scheduling problems can be classified based on three criterion. They are expressed in the form $\alpha \ | \ \beta \ | \ \gamma$**  
- **$\alpha$ = *machine characteristics***
- **$\beta$ = *job characteristics***
- **$\gamma$ = optimality criteria/objective to be minimised**

### Machine Characteristics $\alpha$ 
For a single stage problem $\alpha$ could be:
- 1 where we have a single machine
- $Pm$ where we have $m$ identical machines that can be used in parallel, and a job $j$  can be processed on any of the $m$ machines. 
- $Qm$ where we have $m$ machines that can be used in parallel, but machines may differ in speed.
- $Rm$ where we have unrelated machines that can be used in parallel, but have different speeds for different jobs
- ![[Pasted image 20250527142737.png]]


### Job characteristics $\beta$ 
- Processing time $p_{ij}$ of a **job** $j$ on **machine** $i$
- Due date $d_j$ of a **job** $j$
- A weight $w_j$ can indicate importance of a **job** $j$ relative to others
- Release date of a **job** $r_j$, earliest time at which job can start processing
- A precedence $prec$ define a precedence relation between jobs
- Sequence dependent setup times $s_{jk}$ define setup time between **jobs** $j$ and $k$ 
- Breakdowns can be specified for machines that are not continuously available

### Optimality criteria $\gamma$ 
For each **job** $j$ we define 
- **Completion Time** - $C_{ij}$ time to complete **job** $j$ on **machine** $i$ 
- **Exit Time** - $C_j$ time at which **job** $j$ exits system
- **Makespan** - $C_{max}$ time difference between start and end, when last job exits system, time jobs been running for.
- **Lateness** - $L_j$ = $C_j - d_j$ (exit time - due date)
- **Tardiness** - $T_j = max(L_j, 0)$ 
- **Unit penalty** - $U_j$ = $1 \ if \ C_j > d_j$, $otherwise$ $0$ 

### Example scheduling problem
![[Pasted image 20250306022308.png]]
![[Pasted image 20250525180703.png]]
Construct solution choosing smallest processing time

<4, 1, 2, 3>
$T_j \ 4 = 4 - 12 = -8$
$T_j \ 1 = 14 - 4 = 10$
$T_j \ 2 = 24 - 2 = 22$
$T_j \ 3 = 37 - 1 = 36$

![[Pasted image 20250306022318.png]]
Maybe come back to this

