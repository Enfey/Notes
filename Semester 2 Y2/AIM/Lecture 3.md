# Metaheuristics
> A metaheuristic is a high-level problem-independent framework providing guidelines/strategies for how to develop heuristic optimisation algorithms.

![[Pasted image 20250304013218.png]]
## Types of Metaheuristics
- **Local search** metaheuristics are single-point based trajectory methods that improve a single solution over time
- **Population** based metaheuristics evolve a set of solutions over time
- **Constructive** metaheuristics construct new solutions.

## Main components of metaheuristics
Includes the basic components of heuristic search methods seen previously: 
- Solution representation
- Initialisation method
- Neighbourhood relations(move operators)
- Objective function
Now also need additional components:
- **Mechanism for escaping local optima**
- **Termination condition**
- **Multiple move operators.**

## Mechanisms for escaping local optima
A key issue in heuristic optimisation is how to escape from local optima. 
![[Pasted image 20250304030440.png]]
- Change the current solution or restart the search procedure when a local optima is detected
	- Move to a random solution in the neighbourhood
	- Generate a random solution - invoke reinitialisation
- Change the search landscape
	- Can change the objective function
	- Can use a mix of different neighbourhood operators e.g., hill climbing, iterations randomly select a different neighbourhood operator.
- Maintain a memory to prohibit specific moves which would lead to being stuck
- Allow acceptance of non-worsening moves?

## Termination criteria
Exceeding a fixed number of the following, can be classed as termination criteria:
- Iterations
- Moves
- Objective function evaluations
- CPU time(this can be problematic, and hard to represent, CFS scheduler, VRT vs real-time?)
Other ideas involve looking at the progress of the search itself:
- Consecutive iterations since solution last improved
- Evidence that we are the optimum
- No feasible solution obtained within a fixed limit. 
## Feasibility of solutions
Not all solutions to problems are feasible; **infeasible solution**
0/1 knapsack, solution may exceed weight but have highest value. 

### Dealing with infeasibility 
Instead of outright discarding infeasible solutions in optimisation problems. 
#### Repair
Should have **problem specific repair operators** to deal with infeasibility.
0/1 knapsack, remove a random item from knapsack until the weight does not exceed a limit. 

#### Penalise
A fixed penality can be assigned to any infeasible solution. If a solution $s$ is infeasible, its modified objective functions $f'(s)$ is calculated using a fixed penalty.
$$f'(s) = min(p_0, ...p_4)/2 = $0.50$$
Note $p_n$ represents fixed penalty values assigned to different constraint violations.

A variable penalty can be assigned to any infeasible solution, depending on how much the constraints are violated. 
$$f'(s) = min(p_0,...p_4)/(2 \ \times \  (total \ weight \ - C))$$
Can ensure feasibility is always preferred by adding strong penalty so that infeasible solution is strictly worse than feasible solution by modifying objective function again. 
![[Pasted image 20250304032139.png]]

## General guidelines/Art of the search
Effective search techniques can be provide a balance between exploration and exploitation, can escape from local optima, and are independent of the initial configuration

### General template for local search metaheuristics
![[Pasted image 20250304032333.png]]
Move acceptance can be based on various criteria such as feasability and modified objective functions, could accept worse solution to escape local optima. 

### Metaheuristics - Iterated local search
A metaheuristic that seeks to enhance simple greedy local search algorithms like hill-climbing. Enforces the balance between **exploration and exploitation**, visiting a sequence of locally optimal solutions.

- Perturbs the current local optimum(exploration), then, applies hill climbing(exploitation) to get to the new local optimum
- Perturbation strength is crucial
	Too weak may lead to cycles
	Too strong could lose good properties of the local optima
- Acceptance criteria
- Memory for restarts 
![[Pasted image 20250304033901.png]]

#### Designing ILS for solving MAX-SAT
Representation and objective function follows previous examples
- Binary representation
- Minimisation(total unsatisfied clauses)
Need to chose each of the following to create an ILS algorithm:
- GenerateInitialSolution - Random initialisation
- Perturbation - Random bit flip(random walk)
- Hill Climbing - Steepest descent hill climbing using a 1-bit-flip neighbourhood operator
- Move acceptance - accepting non-worsening moves. 
![[Pasted image 20250304034841.png]]
Step 1: Generate initial solution
	Total variables is 6, need binary string of length 6: <10000>
	Assign each variable to True or False based on equal probabilities.
		![[Pasted image 20250304035323.png]]
		So gives $s_0 = <100100>$ with $f(s_0) = 3$ 
Step 2: Repeat loop
	Apply perturbation - 100100 -> 110100, f(s') =4
	Apply SDHC - 110100 -> 111100, f(s') = 1
	Check move acceptance, accepted, as minimised.
Step 3: repeat loop
	Apply perturbation - 111100 -> 101100, f(s') = 1
	Apply SDHC - 101100 -> 101000, f(s')=0
	Check move acceptance, f(s') $\le$ f(s*), accepted. 
Known global optimum found, return s*.

![[Pasted image 20250304035925.png]]
#### Designing ILS for solving TSP
Solution representation is permutation encoding; need to choose different components for ILS to be applicable to TSP.
- GenerateInitialSolution - Nearest neighbour
- Perturbation - exchange operator - applied n times
- Hill climbing - first improvement with an inserton neighbourhood relation
- Move acceptance, accepting improving moves

#### Guidelines for designing ILS
- Solution initialisation generally irrelevant for longer budgets?
- Perturbation and acceptance criteria interactions can be important and determine the relative balance between exploration/exploitation. E.g., large perturbations only useful if they can be accepted. 
- Local search should be fast and effective.
- Ideally choice of perturbation operator should be such that the steps are not easily undone by a local search operator e.g., neighbourhood.
- Advanced methods may adapt the perturbation strength during the search. 
### Metaheuristics - Tabu Search
Maintain a memory/list of tabu/prohibited moves to enhance local search techniques by using memory to avoid specific moves for a certain  number of iterations. 
![[Pasted image 20250306004832.png]]

#### Components of Tabu Search
- Forbidding Strategy
	- Controls which moves/solutions are in the tabu list
- Freeing strategy
	- Controls when moves/solutions are removed from the tabu list and are valid again. 
- Short-term strategy
	- Manages the inter-play between the forbidding and freeing strategies/
#### Tabu search visually
ISSUE:
Without a tabu list, become trapped in a local optimum. 
Given initial solution $x_0$ : 
1. Determine all neighbours of $x_0$
	- N' = $\{x_1,x_2\}$
	- s* <- choose best in N' as $x_2$
2. Determine all neighbours of $x_2$
	- N' = $\{x_0, x_3\}$
	- s* <- choose best in N' as $x_0$
Stuck moving between $x_0 \ and \ x_2$...

![[Pasted image 20250306005810.png]]

SOLUTION:
With a tabu list, we can remember previously visited solutions and consider them as tabu - note in practice, forbid attributes rather than complete solutions. 

Given a complete solution $x_0$ and $T = \emptyset$
1. Determine admissible neighbours of $x_0$
	- N' = $\{x_1, x_2\}$
	- s* <- $x_2$
	- Add $x_0$ to the tabu list; $T = \{x_0\}$
2. Determine admissible neighbours of $x_2$
	- N' = $\{x_0, x_3\}$
	- 𝑠* ← choose the best in 𝑁′ as $𝑥_3, \ as \ 𝑥_0$ forbidden
	- Add 𝑥2 to the tabu list: $𝑇 = \{𝑥_0, 𝑥_2\}$
Now escaped local optimum.
![[Pasted image 20250306010231.png]]
Assume no freeing strategy here yet!

#### Fundamentals of Tabu Search
- Exploits a memory to prohibit certain moves to guide the search process, avoiding cycles (local optima/neutral spaces).
- Explores admissible neighbours and selects the best
- The memory contains solution attributes, rather than a set of solutions, to stay memory efficient and introduce generalisability. 
	- Memory can be short term - ie a list of solutions recently considered
	- Or it can be rules intended to bias the search toward promising areas of the search space (intermediate-term memory), typically this in practice, but can overlap alsa.
- Tabu list is managed by a list length. 
- Aspiration criteria often used to allow the tabu status to be overriden. 

#### Guidelines for Tabu Search
- Tabu tenure (t) needs to be set appropriately
	- If t is too small, risk cycling
	- If t is too high, may overly restrict the search
- General guidelines for setting t: 
	- t = 7
	- t = $\sqrt{n}$ 
- Aspiration criteria need to override a solution's tabu state, a simple method to do this would be to allow solutions which are better than the currently-best known solution.
### Designing TS for Solving MAX-SAT
- Solution representation is binary encoding, need to alter components for TS to be applicable to MAX-SAT
- Initialisation - Random initialisation
- Neighbourhood - 1 Bit flip operator
- Memory - associate moves as tabu if they have been changed in the previous t steps 
- ![[Pasted image 20250306011655.png]]
- Prohibited moves due to tabu list: 001100 and 100100, as modify $x_0$ or $x_2$.
- $x_3$ added to tabu list, as accepted, and $x_0$ removed. 
## Performance comparison of algorithms
Seen how to design various heuristics and metaheuristics, but which is best? 

Need to be precise:
	What is meant by the best?
	Best for what exactly?
Need to formulate a hypothesis:
	DBHC finds better solutions that SDHC for solving some specific MAX-SAT problem instance $\Phi_x$ when given a computational time budget of 1 second.

Need to make sure comparison is fair, ie computational budget, nominal time, number of evaluations etc is the same for all algorithms.

Number of trials is important to ensure reliability of results. May perform totally different with a stochastic element, for example. Data used to make comparisons must be drawn from same sample e.g., same problem instance. 

Given results obtained after performing 30 trials on the problem instance 1 of the problem X, which algorithm performs the **best** for solving problem X?
	For each algorithm, compute some basic stats:
	- Avg - mean objective value over all trials
	- Std - standard deviation associated with the avg
	- Med - median objective value over all trials
	- Min - the objective value of the best solution found over all trials.
### Statistical testing
Basic stats are ok for indication, but lack significance. 
Many statistical tests that can be used to conclude stronger, statistically significant observations. 
All statistical tests include: 
- A null hypothesis($H_0$) - hypothesis want to test against, assumed to be true unless the evidence is strong enough to reject it in favour of $H_1$
- A p value - probability value indicating how likely it is that data would have occurred by random chance, thus, statistical evidence can reject null hypothesis.
	- Determines level of significance our results hold, the more stringent the evaluation of p is e.g., p <= 0.001, the more significant the results. 
	- Typically use p $\le$ 0.05 - 95% confident results weren't due to chance. 
- Alternative hypothesis ($H_1$) - hypothesis want to prove, negation of $H_0$ typically.
![[Pasted image 20250306014032.png]]
When comparing two algorithms over same problem instance, use wilcoxon signed rank test.
![[Pasted image 20250306014059.png]]
![[Pasted image 20250306014209.png]]
![[Pasted image 20250306014358.png]]
![[Pasted image 20250306014405.png]]
![[Pasted image 20250306014414.png]]
![[Pasted image 20250306014446.png]]


## Scheduling - intro
Many real world problems are what we call scheduling problems:
- Project planning
- Semiconductor manufactoring
- Task scheduling on CPUs

Scheduling problems deal with allocation of resources to tasks over given time periods with goal of optimising one or more objectives. 

### Scheduling framework and notation
All scheduling problems, assume number of jobs(tasks) and machines(resources) are finite.
- ***Jobs***: j = 1 .. n
- ***Machines***: m = 1 .. m
The notation (i, j) denotes a **processing step** of job *j* on machine *i*
Scheduling problems can be classified based on three criterion and are expressed in the form $a$ | $\beta$ | $y$
- $a$ = machine characteristics
- $\beta$ = job characteristics
- $y$ = optimality criteria/objective to be minimised

#### Machine characteristics
For a single stage problem, $a$ could be:
- 1 where we have a single machine
- Pm where we have m identical machnes that can be used in parallel; a job j can be processed on any of the m machines
- Qm where we have m machines that can be used in parallel, but have different speeds. 
- Rm where we have unrelated machines that can be used in parallel, but have different speeds for different jobs. 
#### Job characteristics
- The processing time $p_{ij}$ of a job j on machine i
- The due date $d_j$ of a job j is its, well due date duh.
- A weight $w_j$ can indicate the importance of a job j relative to others in system
- The release date of a job $r_j$ , earliest time at which job j can start processing
- A precedence *prec* defines a precedence relation between jobs
- Sequence dependent setup times $s_{jk}$ define setup time between jobs j and k.
- Breakdowns *brkdwn* can be specified for machines that are not continously available. ???
#### Optimality criteria
For each job *j* we define
- Completion time - $C_{ij}$, time to complete job j on machine i.
- Exit time - $C_j$ time at which job j exits system
- Makespan - $C_{max}$ time difference between start and end when last job exits system
- Lateness - $L_j = C_j - d_j$ (exit time - due date) for job j
- Tardiness - $T_j$ = $max(L_j, 0 )$ 
- Unit penalty - ![[Pasted image 20250306022214.png]]

### Examples of scheduling notation 
![[Pasted image 20250306022234.png]]
![[Pasted image 20250306022308.png]]
Look at Ts another day lol
![[Pasted image 20250306022318.png]]
