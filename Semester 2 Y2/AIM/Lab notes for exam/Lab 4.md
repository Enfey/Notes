## Purpose
- Implement move acceptance method
	- Implemented simulated annealing using 2 different cooling/decrement mechanisms to solve combinatorial optimisation problems
		- Lundy and mees
		- Geometric cooling
- Discover parameter setting sensitivity issues of such methods across mutliple problems instances, and understand impact that poor parameter choices have in inexact methods more generally. 

### Framework background
- Maintains two solutions in memory
	1. Solution currently operating on
	2. A backup of solution started current iteration with, can be reused later if reject any modifications to current solution. !![[Pasted image 20250310021508.png]]
### Pseudocode
#### Simulated annealing 
![[Pasted image 20250311062727.png]]
![[Pasted image 20250311062838.png]]
In single pass:
- Decides bit to flip, via random, 1 to problem.getNumberOfVariables()
- Flips that bit
- Calculate difference between s' and s
- Produce double between 0 and 1.
- If difference/delta less than 0 (strict improvement, minimisation) OR probability less than boltzmann probability (e^-d/t)
	- Keep the bit flip by applying to backup solution (same as copying, but just cheaper)
- Otherwise, revert the bit flip
- Alter/reduce temperature using cooling schedule

![[Pasted image 20250313220815.png]]


THINGS HANDLED BY FRAMEWORK:
Initial solution generated
Initial best value setting
Updating the best value found

#### Geometric cooling
- Adjusts temperature by fixed constant multiplier on each pass, $a$ 
- ![[Pasted image 20250311063510.png]]
- Typically set to 0.9, such that on each pass, temperature lowered, chance of worse move being accepted by boltzmann probability is lowered increasingly with each pass, but allows a sane amount at the start to permit exploration rather than honing in on local optima. 
#### Lundy and mees cooling
- Adjusts temperature in this instance via 2 parameters
	- $c$ = constant multiplier used to set initial temperature based on fitness of initial solution
		- If high, start with higher temperature, allows more extensive exploration at the beginning, assuming $\beta$ isn't an absurd value such as 999. 
	- $\beta$ = constant used in decrement mechanism
- ![[Pasted image 20250311063833.png]]
- ![[Pasted image 20250311064349.png]]
- The higher $\beta$ is set, the faster the cooling becomes, reducing exploration and leading to quicker convergence, but may risk getting stuck in local optima
- The lower $\beta$ is set, the slower cooling becomes, more exploration, but harmful on computational time. 
	-![[Pasted image 20250311064556.png]]

### Lab quiz
- ![[Pasted image 20250311064648.png]]
	- Temperature drops too quickly, but not quick enough to the point of only improving, and doesn't start off high enough to be only improving. 
- ![[Pasted image 20250311064816.png]]