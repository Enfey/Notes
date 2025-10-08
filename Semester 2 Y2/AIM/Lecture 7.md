## Heuristics
- Heuristics are problem specific, inexact methods to solve a complex problem, often optimisation problem, in a reasonable amount of time. 
	- Define the neighbourhood of a solution, are specific to certain representations, and can be highly specialised to certain problems.
## Metaheuristics 
- A metaheuristic provides a high-level problem independent guidelines for developing heuristic optimisation algorithms. 
- But require problem specific details to be designed/implemented/configured/tuned
- Require domain expertise to design the algorithmic components e.g., temperature cooling mechanism
- Very trial and error, and significant degree of analysis required to determine whether metaheuristic is valid guideline for varying problem instances. 
## High-level solvers
- What if we could design a **high-level** solver which:
	- Does not require problem specific details.
	- Is low-cost
	- Can be used for solving different problem domains
- And
	- Does not require problem specific details
	- Does not require tuning/re-configuring for solving different problem domains
	- Has a high performance across different problem demains.
# Hyper heuristics - in search of high-level solvers
- **Hyper-heuristics** are high-level search methods or learning mechanisms for selecting or generating low-level heuristics to solve computationally difficult problems. 
- Goal is to automate process of choosing or designing heuristics or metaheuristics for a specific problem domain.
- Do not directly solve the problem - instead operate upon the search space of low-level heuristics - whereas MHs operate upon the search space of solutions. 
- Motivation: raise level of generality and aim toward the general solver which performs best for a given problem domain/instance. - - 
- Existing methods are highly specialised and have good performance for a specific problem:
	- but may not perform well for solving other problems
	- may not even be able to be applied for solving other problems.
## Problem Domain barrier and Initial Selection Hyper-Heuristic
- Hyper heuristics introduce a layer of abstraction known as the **problem domain barrier**, sitting between the problem domain the decision-making process. Enforces separation between domain-specific information and logic used to guide search process.
- For this HHS, select among a set of low-level heuristics without interacting with problem-specific info, instead rely on predefined set of low-level heuristics, encapsulated generic algorithmic components such as mutation, local search, crossover. Basic mechanisms, domain independent, for manipulating candidate solutions based on rule of thumb.
- This is how this framework aims to achieve general-purpose problem solving - but this abstraction lacks access to domain info, must learn/infer effective control strategies based on limited feedback e.g., solution quality. Domain independence vs performance.
### Classification of hyper heuristics
![[Pasted image 20250415211517.png]]
- **Feedback**
	- **Online learning** - hyper heuristic learns and adapts dynamically during solving, using limited, domain-independent feedback to improve its decisions e.g., swapping heuristic.
	- **Offline learning** - learns prior to deployment, and applies learned strategy without further adaptation.
	- **No-learning** - uses fixed strategy to choose heuristics.
- **Nature of the heuristic search space**
	- **Heuristic selection** - operate by choosing from a pool of existing heuristics
		- **Constructive Heuristics** - build a solution step by step e.g., greedy strategies
		- **Perturbative Heuristics** - operate on existing solution to explore search space e.g., mutation
	- **Heuristic Generation** - generate new heuristics, often by combining components, e.g., low-level operators, specific rules etc
		- **Constructive heuristics** - generates heuristics/procedures to construct solutions from components
		- **Perturbative heuristics** - constructs new ways to modify existing solutions.

## The Hyper-heuristics flexible interface (HyFlex) Framework
(for Selection Hyper-heuristics)
### Selection Hyper-heuristic Template
![[Pasted image 20250415212333.png]]
### Information flow
![[Pasted image 20250415212357.png]]
- Information flow restricted - feedback is purely objective function result, and solutions move back to the domain layer for further modification, depending on the decisions made by the HH.
### HyFlex general info
- Facilitates abstraction between problem specific details and general purpos HHs, allowing for reuse of components.
- Any problem can be solved by any hyper-heuristic without modification. 
- Provides six problem domain implementations
- Each domain has a set of low-level heuristics.
- Heuristics may be parameterised e.g., depth of search, intensity of mutation
- HH can control these settings and select low-level heuristics based on their types/tag. They are:
	- Mutation
	- Local search
	- Crossover
	- Ruin-recreate
- May not be available in all problem domains. 
- Solutions are always complete.
#### Example Domain Components: 1 Dimensional Bin Packing
- **Bin Packing (1D)** - given a set of items with sizes $s_i \in \mathbb{Z}\{1, C\}$, for $i = 1, ... n$ where $C$ is a fixed capacity of each bin, pack each item into a set of bins while never exceeding the capacity of any bin. 
##### Instance
- Given a set of $n = 7$ items with sizes $S = \{8, 1, 7, 6, 2, 2, 4\}$ and $C=10$ 
- Solution representation:
	- < 1, 2, 2, 3, 1, 2, 3 >
- ![[Pasted image 20250415214234.png]]
#### Mutation heuristics
- $MU_0$(Swap) - selects two random items and swaps their bins if there is space
- $MU_1$(Split a bin) - selects bin at random from the set of bins with more items than the average bin and splits it into two bins containing half the number of items each.
- $MU_2$(Repack lowest filled bin) - removes all items from lowest filled bin and tries o repack them into remaining bins with best-fit heuristic.
- Intensity of mutation defined how many time each LLH is repeated with 0.2 = repeat once through 1.0 = 5 times
#### Hill-climbing heuristics
- $HC_0$(First improvement (IE) with Swap) - Swaps two items at random if there is space and keeps new solution if results in **non-worsening** solution cost
- $HC_1$(First Improvement (IE) with Swap from lowest filled bin) - takes largest item from lowest filled bin and swaps it with smallest item from a randomly selected bin.
- Depth of search = defined how many times each LLH is repeated with 0.2 = repeat 10 times through 1.0 = 20 times
#### Ruin-recreate Heuristics
- $RC_0$-Destroy x highest bins - Remove all pieces from x highest filled bins and repack using best fit
- $RC_1$-Destroy x lowest bins - Remove all pieces from x lowest filled bins and repack using best fit
- Intensity of mutation - defines how many times each LLH is repeated with 0.2 = repeat 3 times through 1.0 = 15 times.
- Crossover XO_0 - exon shuffling crossover
### Selection Hyper-heuristics
- A class of hyper-heuristics that select from one or more pre-defined low-level heuristics to be applied to the solution-in-hand at a given point in the search
- Non-learning selection hyper-heuristics may choose from a set of low-level heuristics arbitrarily (mentioned earlier)
- Selection hyper-heuristics contain learning mechanisms for selecting best heuristic from a set of heuristics at any given point during the search.
#### Non-learning selection methods
##### Random selection
Applies a low-level heuristic(s) at random to the solution in hand
![[Pasted image 20250415215749.png]]
##### Greedy selection
Applies each low-level heuristic(s) in turn and chooses the one that generates the solution with the best objective value
![[Pasted image 20250415215759.png]]
#### Learning-based selection methods:
##### Reinforcement learning
- Psychology, punish poor heuristics and reward those which are good.
###### Reinforcement learning: General Template
![[Pasted image 20250415215847.png]]
###### Reward and punishment
- Can reward/punish LLHs by incrementing/decrementing scores.
- May also chose to update scores via:
	- Multiplication by some factor
	- Asymmetric - reward and punishment have different values, altering their harshness.
- What to do if f(s') == f(s)
- Initialisation of scores?
- Should scores be bounded?
- What to do in case of tied scores?
###### Reinforcement learning: Selection 1
- Initial example above saw selection of best scoring LLH
- Tournament selection, choose best from *tournament_size* randomly selected LLHs![[Pasted image 20250415221201.png]]
###### Reinforcement learning: Selection 2
- Can also randomly choose LLH based on probability scoring.
- Roulette wheel selection , selects randomly, but odds of LLH being chosen scales proportionately to their score.![[Pasted image 20250415221244.png]]

#### Choice function
- Considers multiple performance metrics, more general, of a LLH to assign a score
	1. **Individual performance**
	2. **How well the LLH performed when applied after the previously applied LLH**
	3. **The time since the LLH was last applied - 'age'**
- $F(h_j) = af_1(h_j) + \beta f_2(h_k, h_j) + \delta f_3(h_j)$ 
- Often chooses LLH with the best score but can use any of the other mechanisms seen previously.
##### Components of choice function: f1
- Individual performance score of a heuristic.
- ![[Pasted image 20250416003451.png]]
- $\alpha^{n-1}$, where $\alpha \in [0,1]$ 
- $I_n (h_j)$ - change in solution quality
- $T_n (h_j)$ - time taken to run the heuristic
##### Components of choice function: f2
- How well the LLH performed when applied after the previously applied LLH
- ![[Pasted image 20250416002755.png]]
- $\beta^{n-i}$, where $\beta \in [0,1]$ decay factor - more weight to recent pairs
	- e.g., 0.7, most recent has 0.7^0 weight, 0.7^1, 0.7^2 etc
- $n$ is number of times $h_j$ has been applied after $h_k$ 
- $I_n (h_k, h_j)$ - change in solution quality when applying $h_j$ after $h_k$
- $T_n (h_k, h_j)$ - time taken to run the heuristic when applying $h_j$ after $h_k$
- CALCULATE THE FRACTION, MULTIPLY BY DECAY FACTOR, SUMMATE.
##### Components of choice function: f3
- $f_3(h_j) = τ(h_j$)
	- Where:
		- $τ(h_j)$: Time since heuristic $h_j$ was last used.

### Move acceptance for selection HH
- While selection HH aim to choose best LLH to apply, generally need to be guided by move acceptance method, selection on own may not guarantee good performance over time.
	- Low-level heuristics may be stochastic in nature
	- A heuristic that was previously good may not be reliably good.
	- LLH may lead to stagnation and lead to local optima
- Need move acceptance to guide search along intelligent trajectory.
#### Selection Hyper-heuristic framework for single-point search
![[Pasted image 20250416004144.png]]
- May have stochastic element like simulated annealing to ocasionally accept worsening moves. 

#### Misconceptions
- **HH do not require parameter tuning**
- HH always tested under a fair comparison
	- Training instances vs test instances.
	- Computational budget set per machine
	- Timer resolution across operating systems - affects learning processes.
- Applying a hyper-heuristic to a new problem domain is easy, but must implement the new domain, components, classes of heuristics, while respecting any API.
#### Recent research
![[Pasted image 20250416004529.png]]

### An iterated multi-stage selection hyper-heuristic
- There are two hyper-heuristics, S1HH and S2HH
	- S1HH is fast but simple
	- S2HH is slow but more sophisticated than S1HH
	- S2HH is applied after S1HH based on a random probability![[Pasted image 20250416004722.png]]
#### S1HH overview
- S1HH uses reinforcement learning to score heuristics and selects the LLH to apply using Roulette Wheel Selection - probabilities come from S2HH(?)
- Moves are based on an adaptive threshold move acceptance method
- S1HH terminates if a duration is exceeded without any improvement in the best solution found
##### S1HH Move Acceptance Method
- Adaptive threshold approach
- Execution divided into stages
- Move accepted if either:
	1. The new solution is better than the current one - f(s') isBetterThan f(s)
	2. The new solution meets a threshold criteria -$f(s')$ isBetterThan $(1+\epsilon) . f(s_{stagebest})$
- Threshold parameter calculated dynamically.
	- $\epsilon = \frac{logf(s_{stagebest}) + C[i]}{f(s_{stagebest})}$
	- $C$ is a circular list of values where $C[i] \in \{C[0], ... C[n-1]\}$, updated depending on stage to adjust threshold - the larger C becomes, the threshold becomes more relaxed and willing to accept worse solutions, vice versa.
	- $\epsilon$ is updated with next value in the list if no improvement is found within a certain duration 𝑑.
	- The log term adjusts the threshold based on the magnitude of the objective function.
	- (YEAH IDK WHATS GOING ON)
	- ![[Pasted image 20250416040421.png]]
#### S2HH Overview
- Uses 'relay-hybridisation' to create a further $N^2$ LLHs. Considers all original LLHs plus the pairings of all LLHs to form a total of $N + N^2$ LLHs.
- Although, need strategey to reduce number of LLHs and assign probabilities, to pass to roulette wheel selection for next execution of S1HH.
- ![[Pasted image 20250416041014.png]]
### MSHH overview
![[Pasted image 20250416041008.png]]
![[Pasted image 20250416041032.png]]

## Parameter tuning for cross-domain search
- Know that parameter tuning is effective to improve performance of heuristic search methods for solving particular problem.
- Is parameter tuning of a single algorithm still beneficial for cross-domain search?
- ![[Pasted image 20250416041241.png]]
- ![[Pasted image 20250416041256.png]]
- 