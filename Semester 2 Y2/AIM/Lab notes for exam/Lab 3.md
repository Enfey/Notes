### Purpose
- Experience implementing single-point(local-search) metaheuristic and apply this to solve combinatorial optimisation problem
	- Iterated local search implemented
- Understand issues of parameter settings which appear in metaheuristics
	- Understood tuning of strength of mutation and local search iteration
	- Understood issues that poor parameter choices have on performance of metaheuristics. 
- Understand backup solutions
- Boxplot comparisons

### Framework background
- For a single point-based search method, framework stores two solutions in memory.
- ![[Pasted image 20250311044247.png]]
- copySolution(int source_memory_index, int destination_memory_index)
### Psuedocode and implementation
2 parameters:
1. Depth of search
2. Intensity of mutation. 
![[Pasted image 20250311044450.png]]
![[Pasted image 20250311044459.png]]
- Apply mutation for IOM times(bitflip at random in bounds of problem length)
- Apply local search on mutated solution s' DOS times
- Compare s' to s
- If s' <= s 
	- Accept s'
	- Otherwise reject
-
Able to change mutation operator and local search operator e.g., using SDHC or use DBHC, or use a more extreme mutation operator. 

### Lab quiz
1. ![[Pasted image 20250311045220.png]]
2. ![[Pasted image 20250311045315.png]]
	1. ![[Pasted image 20250311045320.png]]
3. ![[Pasted image 20250311045333.png]]
	1. ![[Pasted image 20250314032443.png]]
	2. Choose mann-whitney cause the algorithms are different, and distribution not normal.
	3. Wilcoxon when paired, e.g., same algorithm, and not distrbuted normally
	4. Mann-whitney when not paired, and not distributed normally
	5. T test when focusing on mean difference and data is distributed normally, can be independent or paired samples
	6. Sign test when paired, focusing on direction of change, referring to whether values in dataset tend to increase or decrease between 2 related conditions. 
	7. ![[Pasted image 20250314032833.png]]
	8. ![[Pasted image 20250314032838.png]]
4. ![[Pasted image 20250311045408.png]]
5. ![[Pasted image 20250311045436.png]]
6. ![[Pasted image 20250311045506.png]]
	1. The local search mechanism SDHC here accepts non-worsening moves, which is problematic, as it is not likely to reach even a local optimum, as in a given pass, it would only modify [0,1] bits whereas DBHC could modify [0..n] bits in a single pass. So whilst having same computational budget, evaluating the bitstring the same amount of times, SDHC has an extreme limit on the amount of changes made in a single pass, thus for lower computational time, it is detrimental to the performance of ILS.

### Main takeaway
- Metaheuristics are high-level framework and optimisation algorithms for finding inexact solutions to impractical problems.
	- They guide subordinate heuristics to guide solutions space more efficiently, in this case enhancing local search by introducing mutation to escape local optima, and introduce more explorative properties, particularly effective for combinatorial optimisation problems. 
- Parameter tuning:
	- ![[Pasted image 20250311045138.png]]
	- ![[Pasted image 20250311045149.png]]
