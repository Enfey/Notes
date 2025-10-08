# Move acceptance - acceptance strategies for local search metaheuristics 
![[Pasted image 20250306022900.png]]
Focus so far has been on how to make such neighbourhood moves, rather than which ones to accept. Been rejecting worse cost moves, taking greedy-locally optimal perspective (except in case of tabu-search perhaps.) Are there advantages to accepting these?

## A Naive approach
What happens if accept some random percentage of worse cost moves![[Pasted image 20250306023116.png]] ![[Pasted image 20250306023124.png]]
![[Pasted image 20250306023130.png]]
![[Pasted image 20250306023202.png]]
![[Pasted image 20250306023232.png]]

Broadly classified methods into 2 categories: nature of accept/reject decision, and the nature of how algorithmic parameters are set

### Algorithmic parameter settings
- Move acceptance methods often parameterised, governing overall behaviour of algorithm e.g., tabu tenure, temperature, neighbourhood size. 
- The way parameters are set/controlled, captured in previous taxonomy:
	- **Static** - behave the same throughout the search, either contain no parameters or parameters are fixed, tabu tenure fixed. 
	- **Dynamic** - methods may behave differently in time, but this change in behaviour is predetermined, values of parameters change in time and depend on current iteration, reject on ith iteration. 
	- **Adaptive** - methods may behave differently based upon the search history/memory, not predetermined. 
![[Pasted image 20250306023558.png]]

### Nature of acceptance decision
- Refers to how algorithm decides whether to accept or reject a newly generated solution, within the move acceptance method. 
- The objective value of a candidate solution can be directly compared to known solution values(basic)
- Or can be compared to other calculated values(threshold) not necessarily equivalent to 'cost' of a solution
- Or acceptance is in some way determined based on randomised probability, non deterministic/stochastic. 
![[Pasted image 20250306024232.png]]


### Move acceptance so far
So far, only been using static, non-stochastic basic criteria to accept neighbourhhood moves.
- : 𝑓 𝑠′ ≤ 𝑓 ( 𝑠 ) - improving or equals 
- : 𝑓 𝑠′ < 𝑓 ( 𝑠 ) - only improving

### Late acceptance 
A basic **adapative** and **non-stochastic** move acceptance method accepting worse cost moves if not worse than the solution accepted $L$ steps previous. 

![[Pasted image 20250306024836.png]]
Contains a list of length $L$ of previous accepted solution costs.
Can initialise the list with:
- Initial solution cost - most common
- Infinity - (random walk for $L$ steps) ??
- Zero
Setting of list length is vital on search performance:
- Short list length has fast convergence with sub-optimal results
- Longer list length has slow convergence with better results generally. 

### Non-stochastic threshold methods 
- Non-stochastic threshold methods determine acceptance of neighbourhood moves based on a comparison with some calculated value (may not correspond to an objective value of a solution)
- May be a dynamic or fixed value, that define how much worse a solution can be before its rejected. ![[Pasted image 20250306025258.png]]
- Even if worse than current solution, as long as less than threshold, will be accepted.  
- Threshold could be based on a combination of an $\epsilon$ value with the cost of the (initial/current/best-found-so-far) solution.

#### Nature of parameter settings 
- Static methods can only use a fixed $\epsilon$ value in relation to cost of current solution
- Dynamic methods generally decrease a $\epsilon$ value over time towards a known target
	- Great Deluge
- Adaptive methods may adapt an $\epsilon$ value over time and combine this with memory to determine a threshold, as described above.
	- Record-to-record Travel - where T = $\epsilon$ + $f(s^{best}_i)$ - based on memory + dynamic $\epsilon$ 
	- Extended Great Deluge
	- Flex Deluge 
##### Great Deluge
A dynamic, non-stochastic threshold move acceptance method parameterised on a water-level. Accepts worst cost moves if not worse than a water-level that decreases toward a specified target over time.
![[Pasted image 20250306044203.png]]
Parameterised on T, but predictable, so dynamic. 

##### Extended Great deluge
An adaptive, non-stochastic threshold move acceptance method extending Great Deluge by updating the water level and decay reate based on feedback from the search. Key idea is to allow restarts when the search is thought to be stuck.


#### Stochastic move acceptance
Stochastic criteria determines acceptance of neighbourhood moves based on randomised component. Nature of acceptance decision.
![[Pasted image 20250306044354.png]]

##### Stochastic move acceptance - Nature of the parameter settings 
- Static methods used a fixed probability, P.
	- Naive acceptance, with a fixed P.
- Dynamic methods adjust the probability over time.
	- Simulated annealing, dynamically decreasing temperature parameter over time.
 - Adaptive methods adapt the probability based on feedback from the search.
	 - Simulated annealing with reheating - increases/decreases the temperature setting to balance exploitation/exploration.

### Simulated Annealing
A **stochastic local search algorithm** inspired by the physical process of annealing. Easy to implement, required good parameter setting for improved performance. Achieves good performance given sufficient running time. 
![[Pasted image 20250306044813.png]]

#### Overview
A **dynamic stochastic move acceptance method** that accepts worse cost moves based on a probability that decreases both over time and based on measure of change. 
![[Pasted image 20250306045008.png]]
Improving moves always accepted.
Non-improving moves accepted using the Boltzmann probability, accept if random number is less than this value. 
![[Pasted image 20250306045346.png]]


#### Effects of cooling
- Through time, ie dynamically, the temperature of the system is decreased to a 'ground state'. 
- A higher temperature allows a higher magnitude of inferior moves to be accepted.
- A low temperature rejects inferior moves, accepting a few minor worst cost moves.
- Decreasing T decreases probability that a given worse cost move is accepted.
- Can observe this for a given T as Δ increases.
![[Pasted image 20250306045657.png]]

#### Cooling schedules
Many ways in which can decrease the temperature, each method using a specific cooling schedule, typically specifying:
- Initial starting temeprature, $T_0$
- A final target temperature, $T_{final}$ 
- A mechanism for decreasing the temperature
- The number of iterations per temperature setting?
##### Starting temperature
- $T_0$ needs to be hot enough to allow most worst cost moves, but not 'hot' enough to perform random walk. 
- Initial temp settings can be calculated using computational methods to accept a target percentage of worse moves. 
- Needs to be done for every problem instance. 
##### Final temperature
- Usually assumed to be (close to) 0 
- Should be low enough such that there is a close to 0 chance a worse cost move accepted near end of the search. 
- Parameter of temperature decrement mechanism can be specified to reach this value
##### Decrement mechanisms
- **Linear cooling** reduces the temperature by a fixed amount each iteration: $T_{i+1} = T_i - T_{\delta}$ 
- **Geometric** cooling reduces temperature by multiplying current temperature by a value ($a$) close to 1, usually $a = 0.9$ but is problem instance specific: $T_{i+1} = T_i \cdot a$
- **Lundy and mees** somewhat similar, uses a parameter $\beta$ close to 0 and usually around 0.0001. $T_{i+1} = T_{i+1} \frac {T_{i}} {1+\beta T_i}$
##### Iterations per temperature
Standard approach, decrease temp after each iteration.
Other mechanisms do exist:
- Reduce T after some number of fixed iterations
- Reduce T after some number of iterations that is changed based on where we are in the search (reduce often to begin with, slow rate of change toward end)
Adaptive approaches:
- Reduce T after some number of accepted moves in memory
- Simmulated Annealing with Reheating increases temperature at certain points based on feedback.
#### Using simulated Annealing for solving MAX-SAT problem
![[Pasted image 20250307214938.png]]
- $T_0 = 0.9$
	-Using geometric cooling: 
	-$T_{i+1} = T_i \cdot a$ , where a = 0.9
- Initial solution: $s_0 = <100100> \ with \ f(s_0) = 3$ 
- randomInts = $[2,3,1,6]$ randomFloats = $[0.470, 0.089]$
Step 1:
- $s'_1$ = <110100 with $f(s')$ = 4
- Δ = $f(s'_1)-f(s_0)$ = 4 - 3 = 1
- 𝑃 (Δ, 𝑇) = 𝑒 −1\0.9 = 0.329
- accept = Δ < 0 || random < P (Δ, 𝑇) 
- first fails, second fails as use 0.470 < 0.329
- $T_1$ = $T_0 . a = 0.81$
Step 2:
- $s^*$ = < 100100 >, $f(s^*)$ = 3
- s' = <  101100>, $f(s') = 1$
- Δ = $f(s')-f(s^*)$ = 3 - 1 = -2
- 𝑃 (Δ, 𝑇) = 𝑒 0\0.81 = 1
- accept = Δ < 0 || random < P(Δ, T) = 1
- accepted, as Δ < 0
- $T_2 = T_1 . a = 0.729$
Etc etc, until reaches final temperature.

### Single-stage search methods
- These combine multiple move acceptance methods through group decision making
- Final decision on whether to accept a move is based on a majority vote
- Decisions happen in one phase
### Multi-stage search methods
- Break the search process into distinct phases
- Can use entirely different move accepteance methods dependent on stage
- May have multiple sequential stages of exploration, then exploitation, using diff move acceptance based on whether perturbing/exploring or exploiting to refine a solution. 

## Parameter configuration
- Heurstic techniques are sensitive to their parameter settings
- Most meta/hyper heuristic and move acceptance methods contain parameters which adjust their behaviour and performance.
- Example techniques seen thus far
	- ILS - Perturbation strength/depth of search
	- Tabu search - Tabu tenure
	- Late acceptance - List length $(L)$
	- Simulated annealing - $T_0, \ a, \ T_{final}$ etc
	- GD - $T_final$
### Types of parameters
- Categorical/symbolic/structural parameters
	- Choice of initialisation method, choice of mutation
- Ordinal parameters
	- Neighbourhoods (small, medium, large)
- Numerical parameters.
	- Integer, real values
	- Population sizes, evaporation rates, etc
	- Values may be dependent on setting of categorical or ordinal parameters.
### Parameter setting methods 
#### Tuning
- Occurs before search process starts
- Aims to find best initial settings for parameters
- Methods include
	- Sequential tuning - Fixes all but one parameter, and tests all variations of that. Then using that best setting, unfix another parameter, until best achieved.
	- Design of Experiments = Structured approach to testing parameter combinations (examples later.)
	- Meta-optimisation - using another optimisation algorithm to find best parameters
- Think great deluge, or ILS changing mutation strength
#### Control
- Occurs during search process, ie, online, adapts parameters dynamically as search progresses
- Methods include
	- Dynamic - change based on predetermined rules
	- Adaptive - change based on feedback from search process
		- Self-adaptive - parameters themselves evolve as part of solution
-  Think great deluge, final target, or ILS changing mutation strength

#### Examples - tuning - sequential/simple
- Assume some metaheuristic with 2 parameters, discretised some options
- $a \in \{0.0, 0.3, 0.5, 0.8, 1.0\}$
- $\beta \in \{20, 40, 60, 80, 100\}$
- Sequential tuning would fix all but one parameter, e.g., fix $\beta=60$, and try all settings of $a$. Then using best setting for $a$ try configurations for $\beta$ 
- Could also trial and error with some intuition, arbitrary choice, suggested settings from theoretical studies, or a combination of the former(s).

#### Examples - tuning - Design of Experiments 
- Systematic method (controlled experiment), to determine relationship between controllable(parameters) and uncontrollable factors (e.g., stochastic element, affecting a process(algorithm), their levels(number of settings a parameter can take) and the response of that process (quality of solutions obtained - performance of an algorithm.)
##### Fractional factorial design
- Specific type of experimental design strategy within DoE framework to address challenge of sampling large parameter spaces
- Imagine have $k$ factors in $n$ level factorial design, then have $n^k$ designs.
- Fractional factorial designs, reduce number of required configurations by systematically sampling parameter space to draw valuable conclusions
- Consider 2 params, 8 levels/settings for each, $2^8$ = 256 configurations
- Fractional designs can reduce this, testing only carefully selected subset of full factorial design e.g., 1/2,$ $2^8$, 128

#### DoE sampling methods
- Three main sampling methods used to implement experimental designs like fractional factorial design. 
- **Random** - randomly selects m configurations from entire parameter space
- **Latin Hyper-cube** - divides each parameter range into m intervals and selects exactly one sample from each interval for each parameter, ensuring better distribution.
- **Orthagonality** - ensures better spread in the sample by subdiving the sample space into equally probable subspaces
	- Extension to latin hypercube that makes sure each subspace is **evenly sampled**
- ![[Pasted image 20250307231658.png]]

##### Case study using Taguchi orthogonal arrays method
- **Structured statistical DoE method**/**design** for determining best combination of parameter settings to achieve a certain objectives
- Works well when there are 3 - 50 parameters/factors, and when there are few interactions between parameters or only a few contribute significantly to overall performance.
- Highly fractional orthogonal design.
- Orthogonal array - special table where each col represents parameter, each row is experiment to run
- Mathematically designed so all parameter levels tested in balanced way.
- ![[Pasted image 20250307233110.png]]
Can estimate main effect after few runs.

###### Using for parameter tuning - Planning stage
1. Select control parameters
2. Select possible settings for each parameter
3. Select appropriate orthogonal array
![[Pasted image 20250307233222.png]]
![[Pasted image 20250307233227.png]]
![[Pasted image 20250307233239.png]]
(generated for us)
###### Using for parameter tuning - Experiment stage
![[Pasted image 20250307233336.png]]
##### Analysis stage
![[Pasted image 20250307233402.png]]
![[Pasted image 20250307233434.png]]
![[Pasted image 20250307233442.png]]
![[Pasted image 20250307233450.png]]




![[Pasted image 20250307232433.png]]
