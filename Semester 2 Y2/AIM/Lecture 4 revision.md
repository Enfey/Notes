# Move acceptance - acceptance strategies
Thus far taken naive perspective, focus has been on how to make neighbourhood moves, rather than which ones to accept, simply doing IE or strict thus far.

## Naive approach
What happens if accept some random percentage of worst cost moves.
![[Pasted image 20250306023116.png]]
Changing the setting of P can significantly affect the performance of Naive acceptance.
![[Pasted image 20250306023124.png]]
![[Pasted image 20250306023130.png]]
![[Pasted image 20250306023130.png]]

Broadly classified into **2 categories**

1. Nature of the accept/reject decision
2. Nature of how algorithmic parameters are set.
![[Pasted image 20250306023202.png]]

## Algorithmic parameter setting move acceptance
- Move acceptance methods often **parameterised,** governing overall behaviour of algorithm e.g., tabu tenure, temperature, neighbourhood size
- Way parameters set:
	- **Static** - behave same throughout the search, either no parameters or fixed parameters.
	- **Dynamic**- methods may behave differently in time, but this change is predetermined, values of parameters adjust over time, depend on current iteration
	- **Adaptive** - methods may behave differently based upon search history, not predetermined. 
## Nature of acceptance decision move acceptance
- Refers to how algorithm decides whether to accept or reject a newly generated solution, within the move acceptance method.
	- **Basic Non-Stochastic** - Objective value of candidate solution can be compared to known solution values
	- **Threshold Non-Stochastic** Compare to other calculated values, not necessarily equivalent to cost of a solution
	- **Stochastic** - Acceptance is in some way based on randomised probability, non-deterministic/stochastic

## Move acceptance so far
Only used static, non-stochastic basic criteria to **accept neighbourhood moves**
- : 𝑓 𝑠′ ≤ 𝑓 ( 𝑠 ) - improving or equals 
- : 𝑓 𝑠′ < 𝑓 ( 𝑠 ) - only improving
## Late acceptance
A basic **adaptive** and **non-stochastic** move acceptance method, accepting worse cost moves, if not worse than the solution accepted $L$ steps previous. 
![[Pasted image 20250306024836.png]]
Contains a list of length $L$ of previous accepted solution costs:
Can initialise the list with:
- Initial solution cost - most common
- Infinity - random walk for $L$ steps
- Zero
Setting of list length is vital on search performance
- Short list length has fast convergence with sub-optimal results
- Longer list length has slow convergence with better results generally.

## Non-Stochastic threshold methods
- Determine acceptance of neighbourhood moves based on comparison with calculated value (may not be obj value of solution)
- May be a dynamic or a fixed value, that defines how much worse a solution can be before it is rejected. ![[Pasted image 20250306025258.png]]
- Even if worse than current solution, as long as less than threshold, will be accepted.
- Threshold could be based on combination of an $\epsilon$ value with cost of (initial/current/best found so far) solution


### Further info about nature of parameter settings for non-stochastic threshold acceptance methods
- Static methods can only use a fixed $\epsilon$ value in relation to cost of current solution to guide acceptance.
- Dynamic methods generally decrease an $\epsilon$ value over time toward a known target
- Adaptive methods may adapt $\epsilon$ value over time, and combine this with memory to determine a threshold:
	- **Record-to-record Travel - where T = $\epsilon$ + $f(s^{best}_i)$ - based on memory + dynamic $\epsilon$** 
	- **Extended Great Deluge**
	- **Flex Deluge** 
#### Great deluge
**Dynamic**, **non-stochastic** threshold move acceptance method parameterised on water-level. Accepts worst cost moves if not worse than a '*water-level'* that decreases to specified target over time.
![[Pasted image 20250306044203.png]]

#### Extended Great Deluge
An adaptive, non-stochastic threshold method extending great deluge by updating the water level and decay rate based on feedback from the search. Key idea is to allow restarts when the search is though to be stuck. 

### Stochastic move acceptance
**Stochastic criteria** determines acceptance of neighbourhood moves based on **randomised** component. 
![[Pasted image 20250306044354.png]]
#### Stochastic move acceptance - Nature of parameter settings
- Static methods
	- Use fixed probability $P$, naive acceptance with this.
- Dynamic methods
	- Adjust probability over time, lowering it as hone in on optima.
- Adaptive methods
	- Adjust probability based on feedback from search.
	- Simulated annealing with reheating - increases/decreases the temperature setting to balance exploitation/exploration
#### Simulated annealing
A **stochastic** local search algorithm, easy to implement, can be either dynamic or adaptive, often adaptive parameters. Requires good parameter setting, achieves good performance given sufficient running time. 

##### Overview
A dynamic stochastic move acceptance method, accepts worst cost moves, based on probability that decreases both over time and based on a measure of change. 
![[Pasted image 20250306045008.png]]
Non-improving moves accepted using **boltzmann probability**, accept if random number is less than this value.
![[Pasted image 20250306045346.png]]
IE acceptance.

##### Effects of cooling
- Over time, dynamically reduce temperature to a ground state
- A higher temperature = higher magnitude of inferior moves accepted
- A lower temperature = reject inferior moves, accept a few minor worst cost moves
- Decreasing $T$ decreases probability, given worst cost move, accepted
- Can observe this, given $T$, as $\Delta$ increases.
- ![[Pasted image 20250306045657.png]]

###### Cooling schedules
Many ways, can decrease temperature. Each method, use unique cooling schedule, specifiying:
- Initial starting temperature $T_0$ 
- A final target temperature $T_{final}$ 
- A mechanism for decreasing the temperature
- Number of iterations per temperature adjustment
###### Starting temperature
- $T_0$ needs to be hot enough, allow worst cost moves, but not hot enough to perform random walk
- Initial temp settings can be calculated using computational methods, to accept target percentage of worst moves.
- Needs to be done, every problem instance.
###### Final temperature
- Usually assumed to be 0
- Should be low enough, close to 0 chance, worst cost move, accepted, end of search
- Parameter of temperature decrement mechanism can be specified to reach this value

###### Decrement mechanisms
- **Linear cooling** - reduce temperature by fixed amount each iteration
	- $T_{i+1} = T_i - T_\delta$ 
- **Geometric cooling** reduce temperature by multiplying current temp by a value $\alpha$ close to 1, problem instance specific.
	- $T_{i+1} = T_i \cdot \alpha$ 
- **Lundy & mees cooling** uses parameter $\beta$ close to 0 and usually around 0.0001 $T_{i+1} = T_{i+1}\cdot \frac{T_i}{1+\beta T_i}$ 

###### Iterations per temperature adjustment
Standard approach, decrease temp, every iteration. Other mechanisms do exist.
- Reduce $T$ after fixed num iterations
- Reduce $T$ after some num of iterations that is changed based on where we are in search
Adaptive approaches
- Reduce $T$ after some number of accepted moves in memory
- Simulated annealing with reheating increases temp at certain points based on feedback

###### Using simulated annealing for solving MAX-SAT problem
![[Pasted image 20250307214938.png]]
- $T_0$ = 0.9
	- Geometric cooling
		- $T_i+1 = T_i \cdot \alpha$ 
		- $\alpha = 0.9$ 
	- Initial solution: $s_0$ = <100100> with $f(s_0) = 3$ 
	- randomInts = $[2,3,1,6]$ randomFloats = $[0.470, 0.089]$
- **Step 1**
	- $s1' = <110100> with $f(s')$ = 4
	- Δ = $f(s'_1)-f(s_0)$ = 4 - 3 = 1
	- 𝑃 (Δ, 𝑇) = 𝑒 −1\0.9 = 0.329
	- accept = $\Delta < 0 || random < 𝑃 (Δ, 𝑇)$
	- $T_1$ = $T_0 . a = 0.81$
- **Step 2**
	-  $s^*$ = < 100100 >, $f(s^*)$ = 3
	- s' = <  101100>, $f(s') = 1$
	- Δ = $f(s')-f(s^*)$ = 3 - 1 = -2
	- 𝑃 (Δ, 𝑇) = 𝑒 0\0.81 = 1
	- accept = Δ < 0 || random < P(Δ, T) = 1
	- accepted, as Δ < 0
	- $T_2 = T_1 . a = 0.729$

### Single-stage search methods
- Combine multiple move acceptance methods through group decision making
- Final decision, based on majority vote
- Decisions happen, in one phase
### Multi-stage search method
- Break search process into distinct phases
- Can use entirely different acceptance methods, dependent on stage
- May have multiple sequential stages of exploration, then exploitation, using diff move acceptance based on whether exploring or exploiting to refine solutions.

## Parameter configuration
- Heuristic techniques, sensitive, parameter settings
- Most meta/hyper heuristic and move acceptance methods contain parameters which adjust behaviour/performance
- Example techniques:
	- ILS - IOM and DOS
	- Tabu search - tabu tenure
	- Late accepance - List length
	- Simmulated annealing - temperature
	- GD - t final, decay rate
### Types of parameters
- Categorical/symbolic/structural
	- Choice of initialisation method, choice of mutation
- Ordinal parameters
	- Neighbourhoods (small, medium, large)
- Numerical parameters
	- Integer, real values
	- Pop sizes, evaporation rate etc

### Parameter setting methods
#### Tuning
- Occurs before search process starts
- Aims to find best initial settings for parameters
- Methods include
	- **Sequential tuning** - Fix all but one parameter, test all variations of that, using best setting, unfix another parameter, until best achieved.
	- **Design of experiments** - structured approach to testing parameter combinations
	- **Meta optimisation** - using another optimisation algorithm to find best parameters
#### Control
- Occurs during search process, online, adapt parameters as search progresses
	- **Dynamic** - change based on predetermined rules
	- **Adaptive** - change based on feedback from search process
		- **Self adaptive** - parameters themselves evolve as part of solution
#### Examples - tuning - sequential tuning
- Assume metaheuristic with 2 parameters, discretised some options:
	- $a \in \{0.0, 0.3, 0.5, 0.8, 1.0\}$
	- $\beta \in \{20, 40, 60, 80, 100\}$
- Sequential tuning, fix all but one parameter e.g., $\beta = 60$, try all settings of $\alpha$ and vice versa
- Could trial and error with some intuition, arbitrary choice, suggested settings from theoretical studies etc.
#### Examples - tuning - Design of Experiments
- Systematic method (controlled experiment)
- Determine relationship between controllable parameters, and uncontrollable factors e.g., stochastic element, their levels (number of settings parameter can take) and the quality of solutions obtained.

##### Fractional factorial design
- Specific type of experimental design strategy within DoE framework, address challenge, sampling, large parameter spaces
- Imagine $k$ factors, in $n$ level factorial design, $n^k$ designs
- Fractional factorial design - reduce number of required configurations, by systematically sampling parameter space to draw valuable conclusions
- 2 params, 8 levels/settings for each = 2^8 = 256 configurations
- Fractional designs reduce this, testing only carefully selected subset.

##### DoE sampling methods
- Three main sampling methods used to implement experimental designs like fractional factorial design
- **Random**
	- Randomly select $m$ configurations from parameter space
- **Latin Hyper-Cube**
	- Divides each parameter range into $m$ intervals, selects one sample from each interval, for each parameter, better distribution
- **Orthonality** 
	- Extension to latin hypercube, make sure each subspace even sampled - divides sample space into equally probably subspaces.
- ![[Pasted image 20250307231658.png]]


##### Taguchi Orthoganal arrays method
- Structure statistical DoE method/design for determining best combination of parameter settings to achieve certain objectives
- Works well when there 3-50 parameters/factors, and when there are few interactions between factors, or only a few contribute significantly to overall performance
- Highly fractional orthogonal design
- Orthogonal array - table where each col represents parameter, each row is an experment to run.
- Can estimate main effect after few runs.

###### Using this
1. Select control parameters
2. Select possible settings for each parameter
	![[Pasted image 20250307233222.png]]
	![[Pasted image 20250307233227.png]]
	![[Pasted image 20250307233239.png]]
	![[Pasted image 20250307233402.png]]![[Pasted image 20250307233434.png]]![[Pasted image 20250307233442.png]]