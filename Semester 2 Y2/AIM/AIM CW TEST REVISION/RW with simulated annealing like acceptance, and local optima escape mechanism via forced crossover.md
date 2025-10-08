- I implemented a learning, selection-based hyper-heuristic.
## Features and design choices
### IOM initially set to 0
- This does not prevent exploration, just merely reduces it initially when explorative heuristics are selected.
- IOM is later increased via the local optima escape mechanism; where IOM is set to 0.6 and crossover is forcibly executed, as this performed better over multiple trials.

### Reinforcement learning
- Heuristics are assigned scores s, $1 < s \le 10$, which are updated based on whether their application improves the solution or not.
- This is an increment/decrement by one point, based on whether the new objective value is lower than the previous (minimisation).
- Via use of objective value, provide a mechanism with which to learn online to improve decisions, while the bound also provides the ability for heuristics that may not perform well to be selected.
	- This is useful as found when decreased the lower bound, stuck in local optima, as mutation and crossover heuristics were not selected anywhere near as often. 
### Roulette wheel selection of LLH
- Using the scores assigned via reinforcement learning, heuristics are selected, based on their performance![[Pasted image 20250508201004.png]]
- By bounding scores between 1 and 10, reward heuristics which provide better solutions more, giving them a higher chance to be selected, whilst giving heuristics with a poor performance (according to immediate improvement) still a chance to be selected by not setting the bound to 0, so still allowing worsening heuristics to be selected.
### Strictly accepting
- The HH only accepts solutions which improve strictly, which does reduce explorative capabilities by not allowing solutions of equal value to be permitted. 
### Local optima escape mechanism
- Due to the strict acceptance mechanism, a local optima escape mechanism has been implemented.
- If the solution quality has worsened or is equal to the previous solution, then the $stagnationCounter$ is incremented.
- When 1000 iterations is reached with no strict improvement, IOM is increased to 0.6 and one of the crossover heuristics is randomly applied to the solution.
	- This is because apart from inversion mutation, the crossover heuristics have the greatest (and more or less equal, using PMX and OX) potential for modifying the solution, providing the explorative capabilities needed to escape local optima. 
- Justify this over IE? IE may lead to excessive random walks, and may slow convergence to an optima, strict is more exploitative, but risks getting stuck in local optima. The stagnation mechanism provides a controlled and targeted way to escape local optima, injecting diversity when the search is stuck, rather than continously.
- Heuristic score is updated all the same, and the IOM and stagnation counter are reset after applying one of the crossover heuristics.
### Benefits
- Occasionally forcing crossover and increasing intensity of mutation when a solution has not changed for $STAGNATION_{}LIMIT$ iterations prevents premature convergence to a local optimum, and forces exploration.
- **Online adaptation** - heuristic selection probabilities are adapted during the run based on whether the heuristic improves a given solution or worsens it after application. Allows HH to respond to characteristics of each specific instance, leveraging the most suitable heuristics for that particular instance, improving performance.
- **Simple and transparent reinforcement learning mechanism** - reinforcement learning mechanism is simple, and easy to extend if needed by providing a more meaningful measure of scoring heuristics.
- **Low computational overhead** - score update and selection mechanism is computationally inexpensive generally, and as such, given the features listed above, the HH will return a quality solution even given a low computational budget
- **Maintains exploration via bounding of scores** - by bounding heuristic scores above zero, even heuristics which seem to return poor solution quality retain a nonzero probability of selection, permitted exploration, even without the stagnation mechanism. 
- Diversity of heuristics - heuristics are well balanced, with choices from multiple classes of heursistics, and variance even in those e.g., adjacent swap vs inversion mutation and their capability for disruptiveness. 
- Use zero clones, and try to perform array operations in place instead of making deep copies where possible to increase performance in each heuristic


## Drawbacks
- **Parameter sensitivity** -  the effectiveness of stagnation limit and score bounds may vary significantly depending on the problem instance. As such, HH performance  may degrade on instances with different landscape features e.g., more plateaus, larger search spaces. 
- **No transfer of knowledge between runs** -  must relearn, but potentially also beneficial as each starts with equal probability of heuristic to be selected. 
- **Diversity of heuristics** - The performance of the HH is tied directly to the set of heuristics it works with, if the heuristics are not well-suited to a particular instance, there is little a selection hyper heuristic can do to compensate for this.
- **Score issue** - Scores are updated based on immediate improvement rather than the long term benefit, perhaps using something like f-scores, which is a more holistic mechanism to determine the most performant heuristic. Also, RWS sensitive to range and distribution of scores, if all perform similarly, selection becomes random again. If one heuristic dominates, others may rarely be chosen. 
- **Stagnation limit too low** - can lead to escaping from the global optimum, and depending on how intense the crossover applied, it may be difficult to get back there due to how much the solution has changed. Just generally escaping from good solutions unnecessarily.
- **Parameter adaption** - adaptive parameter control based on search project may improve robustness of the HH when generalising to other problem domains or solution spaces with unique features that the current parameters may not adjust well to. 




![[Pasted image 20250508221303.png]]


## Set of heuristics
- Chose 3 Local Search, 3 mutation, 2 crossover, one global search metaheuristic
### Local Search LLH
- The 3 local search aim to exploit the search space and fine-tune solutions, exploiting local optima, but whilst also providing a small degree of explorative capabilities.
- For example DHC has IE acceptance, and thus tries to introduce explorative features to an otherwise purely exploitative class of heuristic, which the others do not. It is this kind of variance that provides a diverse set of heuristics. 
#### NextDescent
- Iteratively examines neighbours via application of adjacent swap in place, accepts first improving move found, essentially wrapper around adjacent swap that ensures exploitation.
- Fast, stops searching neighbourhood as soon as found
#### DavissHillClimbing
- Systematic evaluation of all possible neighbours, moves to best improving neighbor, if any.
- Generates non-repeating permutation of indices to use to index into solution representation, performing adjacent swaps at random.
- Accepts swap IE
- Reverts swap if not IE
- Continues, only accepting best.
- Checks [0..n]
- Computationally expensive for large neighbourhoods.
### TwoOPT
- Selects two indicies, reverses the segment between them
- Can make larger, more disruptive changes than simple swaps, allowing escape from some local optima unreachable by single swaps
- Slower than simple swaps due to large neighbourhood operator
- Can still get stuck in local optima, but due to disruptive operator more likely to escape.
### Mutation LLH
- The 3 mutation methods provide differing degrees of solution modification, and aim to provide explorative capabilities. 
- Adjacent swap, with IOM=0 can only make [2] modifications, whereas inversion can make [2..n] modifications. It is this kind of variance that provides a diverse set of heuristics, and allow for the escape of local optima in line with the features of the hyper heuristic. 
#### Adjacent swap
- Simply swaps with forward index, 1...n-2(should not swap first and last locations according to speciciation)
- Only has the potential to make [2] x num swaps modifications per application.
- Purely explorative.
#### Inversion
- Inverts a subset of the solution representation
- Purely explorative
- Can make $[2..n] \times numInversions$ modifications/affected poisitons per application
- Provides a great degree of exploration. 
#### Reinsertion
- One element is removed from its position in solution representation and inserted elsewhere.
- Only has the potential to make [2] x num swaps modifications per application.
### Crossover LLH
- The 2 crossover heuristics provide a great degree of exploration, whilst also preserving beneficial characteristics from parent solutions. 
- OX preserves relative order of cities from parent 2 outside of the copied segment, and an absolute segment from first parent, thus retain what could be a good subsequence or partial route.
- Forcibly used in stagnation, reason being is not to apply explorative capabilities without thinking, but to preserve beneficial qualities whilst also exploring. 
#### Order Crossover (OX)
- 2 crossover points, copy the segment between these points from parent 1, and then starting from parent 2 crossover point, copy positions that are not in the segment, in relative order.
- Thus preserve relative order of cities from parent 2 outside the copied segment, and maintain the absolute order of elements inside the segment.![[Pasted image 20250508224354.png]]
- ![[Pasted image 20250508225909.png]]
- Does change, even if same parent. 
#### Partially mapped crossover (PMX)
- 2 Crossover points, swap segments, use mapping between swapped elements to fix duplicates outside the segment, maintaining more of the genetic material of the parents. 
- In code, perturb parent 2 before applying, as the mapping would result in the exact same solution, OX only works due to unique filling approach taken. 
- ![[Pasted image 20250508230108.png]]
### Metaheuristic LLH
#### Simulated annealing
- Global search metaheuristic, accepts worsening, uses the inversion operator as has the highest disruptive capability, and so increases exploration more than a single mutation heuristic would. 
- Uses a geometric cooling mechanism, simple, but fixed value, may have benefitted from adaptive parameter tuning, but of little consequence when considering learning nature of hyperheuristic as if this performs poorly simply wont be selected.,
- Balances exploration (higher temp), searches widely
- Exploits at lower temperatures, refining a given solution and converging faster
- Geometric cooling, not computationally expensive and easy to implement, but not adaptive and does not consider the search landscape by providing a fixed value.

Minimising use of clone, e.g., using arrayCopy to perform shallow copies of the array, shifting elements right, instead of working on a different array. 


![[Pasted image 20250509132553.png]]
Strict acceptance less of a drawback when using constructive, as solution is already good, whereas in random, cannot move across plateaus and may get stuck more easily in early stages, and this may also be affected the learning mechanism, which takes time to identify good heuristics with regard to the solution space of that instance. 

![[Pasted image 20250509132749.png]]
