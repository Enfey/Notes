 ### Purpose
- Framework experience for handling population oriented metaheuristic implementation
- Implementation of a multimeme memetic algorithm
	- Observing how co-evolving mems can adapt to problem being solved, and explain the process of co-evolution, explaining usefulness of various parameter settings. 

### Framework background
- A multimeme memetic algorithm is an extension to memetic algorithms that incorporates multiple memes within the evolutionary process.
- MA apply single local search to all children, MMMA allows each individual or a grouping of individuals to have their own local search heuristics, which can evolve over time, rather than being fixed settings
- Thus each individual carries set of instructions, that determine how solution will be improved, evolving alongside individual.
- A meme encodes how to apply an operator(which one to apply, max no of iterations, acceptance criteria), when to apply, where to apply, and how freqeuntly. 
- These memes are then combined under a memeplex for a given individual or group of individuals. 

![[Pasted image 20250314002006.png]]
Each individual meme (operator used to improve the solution) points to a specific setting (type of operator used from that set of operators)

The memeplex is intrinsic to each individual, and can be accessed using getMeme(int solutionIndex, int memeIndex). Memes are represented by a meme object, in which you can get and set the current meme option. A meme option is integer value meme is assigned to, which declares the specific genetic code a meme employs. 

For example if getMemeOption() from solution index 0 and meme from meme index 2, return 5 as the meme option. ![[Pasted image 20250314002233.png]]
![[Pasted image 20250314004110.png]]
Thus memes can be operators, or they can be discrete values also, which evolve to determine operator behaviour. 

#### Implementation
##### MMMA
![[Pasted image 20250314013847.png]]

##### Apply mutation
![[Pasted image 20250314013940.png]]
##### Apply local search given DOS meme and local search meme
![[Pasted image 20250314013950.png]]


##### Mutate memeplex for offspring post-inheritance.
![[Pasted image 20250314014037.png]]
### Lab test answers
1. ![[Pasted image 20250314014202.png]]
	- Initially, innovation rate = 0, always inherit best, and tournament always picks best parents.
2. ![[Pasted image 20250314014341.png]]
	- Never possible to inherit best meme option given that occurs post-inheritance of memes from parents
3. ![[Pasted image 20250314014438.png]]
	- Remember, smaller perturbations are better.
4. ![[Pasted image 20250314014458.png]]
5. ![[Pasted image 20250314014509.png]]
	- No-one heuristic is better than another i guess.
6. ![[Pasted image 20250314014624.png]]
	- IDFK anymore
7. ![[Pasted image 20250314014637.png]]
	- Frequencies say nothing about each other when independent. 