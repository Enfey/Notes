# Heuristics
- Problem-specific, inexact methods, find good solution for optimisation problem in reasonable amount of time
- Define neighbourhood of solution, specific to certain representations, can be highly specialised to certain problems
## Metaheuristics
- Provides high-level, problem independent guideline for developing heuristic optimisation algorithms, more general and adaptable approach
- But they require:
	- Problem specific details to be design/implemented/configured/tuned
	- Domain expertise to design the algorithmic components of metaheuristic
- Very trial and error, significant degree of analysis required to determine whether metaheuristic is valid guideline for problem instance/domain. 

## High-level solvers
- What if we could design a **high-level** solver which:
	- **Does not require problem specific details.**
	- **Is low-cost**
	- **Can be used for solving different problem domains**
- And
	- **Does not require problem specific details**
	- **Does not require tuning/re-configuring for solving different problem domains**
	- **Has a high performance across different problem domains.**


## Hyper-Heuristics
- **Hyper-Heuristics** are high-level search methods or learning mechanisms for selecting or generating low-level heuristics to solve computationally difficult problems
- Goal: Automate process of choosing or designing heuristics/metaheuristics for a specific problem domain
- Do not directly solve problem - operate on search space of LLH, whereas MH still operate on solution space, just more thoroughly and calculated.
- Motivation: Raise level of generality, aim toward general solvers which can perform best for a given problem domain, and can generalise to others, whilst staying away from problem specific details.
- Existing methods are highly specialised, have good performance for a specifi problem, but may not generalise to other domains, let alone even being able to be applied e.g., representation unsuitable.

### Problem Domain Barrier
- Hyper-Heuristics, introduce, layer of abstraction, aka **problem domain barrier**, sitting between problem domain and decision making process.
- Enforce separation between domain info and logic used to guide LLH search process.
- E.g., selection HH, select among set of LLHs without interacting with problem specific info, use predefined LLHs such as mutation, crossover, local search, manipulate candidate solutions based on rule of thumb
- Achieves general purpose problem solving - but lack domain info - must learn/infer effective control strategies based on limited feedback e.g., solution quality. **Domain independence vs performance tradeoff**


### Classification of Hyper-Heuristics
- **Feedback**
	- **Online learning** - HH adapts during solving, using domain-independent feedback to improve decisions.
	- **Offline learning** - learns prior to deployment, applies learned strategy.
	- **No-Learning** - uses fixed strategy to choose heuristics
- **Nature of heuristic Search Space**
	- **Heuristic Selection** - choose from pool of pre-defined
		- **Constructive Heuristics** - build solution step by step
		- **Perturbative** - operate, existing solution
	- **Heuristic Generation** - generate new heuristics, often by combining components from diff LLH
		- **Constructive heuristics** 
		- **Perturbative** 

### HyFlex
#### Selection HH pseudocode
![[Pasted image 20250415212333.png]]
#### Information flow
- Information flow restricted - feedback is purely objective function result, solutions never fully visible to HH layer.
- HH layer passes down  which solution index, which heuristic to apply, and index of where to store resultant solution in list of solutions. ![[Pasted image 20250415212357.png]]

#### HyFlex General Info
- Facilitate, abstraction, problem specific details + HH, allowing reuse of components
- Any problem can be solved by any hyper-herustic without modification
- Provides six domain implementations
- Each domain, set of LLHs specific to domain.
- Heuristics may be parameterised, HH may control these settings, and select LLHs based on type or tag, these are:
	- **Mutation**
	- **Local Search**
	- **Crossover**
	- **Ruin-Recreate**
- Solutions are always complete

##### Example Domain Components - 1D bin packing
- Given set of items with sizes $s_i \in \mathbb{Z}\{1, C\}$, for $i = 1, ... n$ where $C$ is a fixed capacity of each bin, pack each item into a set of bins while never exceeding the capacity of any bin. 

###### Instance
- Given set of $n = 7$ items, with sizes $S = \{8, 1, 7, 6, 2, 2, 4\}$ and $C=10$ 
- $< 1, 2, 2, 3, 1, 2, 3 >$
- ![[Pasted image 20250415214234.png]]

##### Mutation Heuristics
- $MU_0$( - (swap) - select 2 random items, swap their bin assignment, if space.
- $MU_1$ (Split) - select bin at random from set of bins with more items than avg bin, split into two bins, containing half num of items each. 
- $MU_2$ (Repack lowest filled) = remove all items, lowest filled bin, repack them into remaining bins with **best fit heuristic**. 
- IOM defined.
##### Hill-Climbing Heuristics
- $HC_0$ - First improvement IE with swap - swap 2 items at random if space, keeps new solution, if non-worsening.
- $HC_1$ - First improvement IE with swap from lowest filled bin - takes largest item from lowest filled bin, swaps it with smallest item from randomly selected bin.
- DOS defined.
##### Ruin-Recreate Heuristics
- $RC_0$ - destroy x highest bins, remove all pieces from these, repack using best fit.
- $RC_1$ - remove all pieces from x lowest filled bins, repack using best fit
	- ![[Pasted image 20250526191647.png]]
- IOM
- Exon shuffling crossover

### Selection Hyper-Heuristics
- Class of HH select one or more pre-defined LLH to be applied to solution in hand during search
- Non-learning selection HH may choose LLH arbitrarily
- Selection HH may contain learning mechanism for choosing best heuristic from finite set of heuristics during search.

#### Non-Learning Selection methods
##### Random Selection
Randomly apply LLH to solution in hand.
##### Greedy selection
Apply LLHs in turn, persist solution with best fitness.
![[Pasted image 20250415215759.png]]


#### Learning/Online selection methods
##### Reinforcement learning
- Psychology, punish poor heuristics, reward those who perform well
###### General template
![[Pasted image 20250415215847.png]]
If applying new heuristic results in better obj value, reward, and select based on score
###### Reward and punishment
- Can reward/punish LLHs by incrementing/decrementing scores
- Can update via:
	- Multiplication by factor
	- Asymmetric - reward + punishment have different values, altering their harshness
- What do if $f(s') = f(s)$ 
- Initialisation of scores
- Bounding of scores
- What if scores equal?
###### Reinforcement learning: Tournament selection
- Initial example, saw selection, best scoring LLH
- Choose best from tournament size randomly selected LLHs

###### Reinforcement learning: Roulette wheel selection
- Can randomly choose LLH based on probability scoring
- RWS, select randomly, odds of LLH, proportional to score.
- ![[Pasted image 20250415221244.png]]
- Reasonable to impose bounds on scores

###### Choice function
- Considers multiple performance metrics of LLH to assign a score.
- Individual performance, last time since applied, how well perform in association with others.
- $F(h_j) = af_1(h_j) + \beta f_2(h_k, h_j) + \delta f_3(h_j)$
- Often chooses, one, best score, can use, other mechanisms

 **Components of choice function: f1** 
- Individual performance score of a heuristic
- ![[Pasted image 20250416003451.png]]
	- $a^{n-1}$, where $a \in [0,1]$ 
	- $I_n (h_j)$ - change in solution quality
	-  $T_n (h_j)$ - time taken to run the heuristic
**Components of choice function: f2**
- How well LLH performed when applied after previously applied LLH
- ![[Pasted image 20250416002755.png]]
	- $\beta^{n-i}$, where $\beta \in [0,1]$ decay factor - more weight to recent pairs
		- e.g., 0.7, most recent has 0.7^0 weight, 0.7^1, 0.7^2 etc
	- $n$ is number of times $h_j$ has been applied after $h_k$ 
	-  $I_n (h_k, h_j)$ - change in solution quality when applying $h_j$ after $h_k$
	- $T_n (h_k, h_j)$ - time taken to run the heuristic when applying $h_j$ after $h_k$
**Components of choice function: f3**
-  $f_3(h_j) = τ(h_j$)
	- Where:
		- $τ(h_j)$: Time since heuristic $h_j$ was last used.

### Move acceptance for selection HH
- Selection HH, aim choose best LLH apply, may generally need guidance by move acceptance method, selection on own may not suffice.
	- **LLH may be stochastic in nature**
	- **Heuristic previously good, may not be reliably god**
	- **LLH may lead to stagnation and local optima**

#### Selection Hyper-heuristic framework for single-point search
![[Pasted image 20250416004144.png]]

### Misconceptions
- HH, no parameter tuning
- HH, always tested, under fair comparison
	- Training instances vs test instances
	- Timer resolution
	- Computational budget
- Apply HH new problem domain is easy, but must implement domain, components, classes of heuristics, respecting any API.


### An iterated multi-stage selection hyper-heuristic
- 2 HH, S1HH and S2HH
	- S1HH, fast but simple
	- S2HH, slow but sophisticated
	- S2HH applied based on random probability
##### S1HH
- Use reinforcement learning to score heuristics, selects LLH to apply using RWS, probabilities contributed from S2HH
- Moves based on adaptive threshold move acceptance method. 
- S1HH terminates if a duration is exceeded without any improvement in best solution found.

##### S2HH
- Adaptive threshold approach
- Execution, happens, stages
- Move accepted if either:
	1. New solution, better than current one $f(s')$ < $f(s)$
	2. OR new solution meets threshold criteria $f(s') < (1+\epsilon) \cdot f(s_{stagebest})$ 
- Threshold parameter calculated dynamically
		- $\epsilon = \frac{logf(s_{stagebest}) + C[i]}{f(s_{stagebest})}$ 
		- $C$ is a circular list of values where $C[i] \in \{C[0], ... C[n-1]\}$, updated depending on stage to adjust threshold - the larger C becomes, the threshold becomes more relaxed and willing to accept worse solutions, vice versa, dynamic component.
		- $\epsilon$ is updated with next value in the list if no improvement is found within a certain duration 𝑑.
		- The log term adjusts the threshold based on the magnitude of the objective function.
#### S2HH Overview
- Uses 'relay-hybridisation' to create a further $N^2$ LLHs. Considers all original LLHs plus the pairings of all LLHs to form a total of $N + N^2$ LLHs.
- Although, need strategey to reduce number of LLHs and assign probabilities, to pass to roulette wheel selection for next execution of S1HH.
- ![[Pasted image 20250416041014.png]]
- 