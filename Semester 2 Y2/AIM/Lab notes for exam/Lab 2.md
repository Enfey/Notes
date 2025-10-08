### Purpose
- Implementing some hill climbing local search heuristics, get experience applying these to combinatorial optimisation problems
	- Steepest descent and Davis' bit hill climbing heuristics
- COMP2001 framework covered more


### Framework background
- getNumberOfVariables() of problem
- getObjectiveFunctionValue(index) of problem, using current solution index
- shuffle(arr, random),  shuffles array of ints using instance of random. 

### Psuedocode and implementation
#### DBHC
![[Pasted image 20250311035645.png]]
![[Pasted image 20250311035656.png]]
- when apply heuristic
	- get fitness of current solution
	- create random permutation of indices via shuffle() on list of indices 0 to numberofvariables in problem
	- flip random bit using permutation indices
	- If better then accept, **keep flipping new solution in future iterations**
		- If not, don't accept via flip back

#### SDHC
![[Pasted image 20250311035949.png]]
![[Pasted image 20250311040137.png]]
- when apply heuristic
	- Flip sequentially
	- If bit flip leads to improved solution
		- Set improved to true
		- Set bestEval
		- Remember bitFlip that led to improvement
	- Flip back, until done all bit flips
	- Accept best bit flip.

### Lab test answers
- ![[Pasted image 20250311040353.png]]
- ![[Pasted image 20250311040401.png]]
- ![[Pasted image 20250311040423.png]]
- ![[Pasted image 20250311040453.png]]
- ![[Pasted image 20250311040507.png]]
- ![[Pasted image 20250311040522.png]]
- ![[Pasted image 20250311040759.png]]
	- Where performance is indicative of time to complete i assume.
### Main takeaways and definitions
- DBHC - first improvement in a single pass
	- More likely to escape local optima, as it's greedyness means accepting solutions which may actually explore the landscape more. Balances exploration more by design, rather than pure exploitation
	- Needs less computation time; takes first improvement and flips it, and then works on that problem instance, not visiting that index again, whereas SDHC only accepts 1 bit in a single pass, albeit it is the most optimal. 
	- Does not explore the search space fully - takes first accepting move, which is naive and may lead to a poor result depending on problem instance. 
- SDHC - best improvement.
	- Needs more computation time
	- Can explore search space fully
	- Likely to get stuck in local optima with no operators to get out of it. 
- No one heuristic performs better than another given heuristic - this is dependent entirely on problem instance. 
