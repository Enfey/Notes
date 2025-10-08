
## Graph-based Hyper-heuristics - A Constructive Approach
- Constructive Hyper-Heuristic
- Low-level heuristics are all constructive here - build solution step-by-step instead of mutating existing one e.g., greedy strategies
- LLHs are simple graph-colouring heuristics
- The hyper-heuristic is trying to find **optimal sequence** of constructive LLHs to generate a complete solution
- The completed coloured graph represents a solution to a timetabling problem
## Graph Colouring
- Given a graph $G$ containing a set of vertices $V$ and edges $E \Subset V \times V$
- Is it possible to colour each vertex, such that no two vertices connected via an edge share the same colour?![[Pasted image 20250524174035.png]]
- Vertices here could represent exams.
- Edges could be constraints caused by students taking multiple exams
- Trivial with |V| colours, but what if the number of exam slots is less than |V|?

## Graph Colouring Problems
### $K$ colouring problem
> Can the vertices of the graph be coloured using $k$ colours such that no two vertices with the same colour are connected via an edge.

### Minimum colouring problem
> Given a graph $G = (V, E)$, colour the vertices of the graph using the minimum number of different colours, such that no two vertices with the same colour are connected via an edge.


## Properties of vertices
- Number of edges connected to a given vertex $v$ is called its ***degree*** $d$
- The number of differently coloured vertices connected to a given vertex $v$ is called its ***saturation degree***
- These properties can be used to create some constructive heuristics for our graph.

## Graph Colouring Heuristics - Largest Degree
- Given a graph $G = (V, E)$, order the vertices based on their degree, from highest to lowest.
- Given a list of colours $c₁ (🟡), c₂ (🔴), c₃ (🟢)$, pick the highest-degree-uncoloured vertex, assign it the lowest-numbered colour/first colour not used by its neighbours. Repeat until all vertices are coloured.
**Example**
![[Pasted image 20250524175422.png]]
$V = \{(A,3), (B,2), (C,2), (D, 1)\}$, vertices ordered by degree. So would colour $A$ first using first colour etc.
	Colour $A$ - No neighbours coloured, so pick lowest, assign $c₁ (🟡)$ to A
	Colour $B$ - Neighbours: $A(c₁ (🟡))$, $C(uncoloured)$, $c₁ (🟡)$ used, so assign next lowest, $c₂ (🔴)$ to $B$
	Colour $C$ - Neighbours: $A(c₁ (🟡))$, $B(c₂ (🔴)$), so assign next lowest, $c₃ (🟢)$ to $C$
	Colour $D$ - Neighbours: $A(c₁ (🟡))$, so assign next lowest $c₂ (🔴)$ to D
**Final Colours (Largest Degree):**
- $A: 🟡$
- $B: 🔴$
- $C: 🟢$
- $D: 🔴$

## Graph Colouring Heuristics - Saturation Degree
1. For a given graph $G = (V, E)$, order the vertices based on their saturation degree; assume highest to lowest.
2. Given a list of colours $c₁ (🟡), c₂ (🔴), c₃ (🟢)$, pick the highest saturation degree, and assign it the lowest/first colour not used by neighbours. Repeat **from step 1**, until all coloured. So keep reordering and recolouring until all coloured. 
**Example**
![[Pasted image 20250524175422.png]]
$V = \{(A,0), (B,0), (C,0), (D, 0)\}$, ordered by saturation degree
	Pick vertex with highest **degree** among ties
		**Step 1:**
			Colour A, highest degree, assign $c₁ (🟡)$ to A
			Update $SDs$ 
			All neigbours of A now have SD = 1, they see 🟡
		**Step 2**:
			Pick from any $\{B, C, D\}$, same SD, B wins, highest degree, assign colour unused by neighbour, A is neighbour, so assign $c₂ (🔴)$ 
			Update $SDs$
		**Step 3:**
			$\{C, SD = 2, D, SD = 1\}$, assign $C$ colour unused by neighbours $A$ or $B$, pick $c₃ (🟢)$ 
			Update $SDs$
		**Step 4**:
		${D, SD = 1}$, assign colour unused by neighbour $A$, so pick $c₂ (🔴)$ 

**Done**.



## Examination Timetabling problem
- Given a set of exams $E (e_1, e_2, ..., e_E)$ taken by students $S (s_1, s_2, ... , s_S)$ schedule each student and exam into a limited time period $T(t_1, t_2, ...t_T)$ within a limited number of rooms $R (r_1, r_2,...r_R)$.
- **Hard constraints** include exams taken by the same student cannot be assigned to the same time period, capacity of room cannot be exceeded, amongst many others.
- **Soft constraints** include time separation between exams, and scheduling exams with large cohorts earlier.

### Designing a local search metaheuristic for Examination Timetabling
- **Representation** - can use list of pairs of integers, each representing time period assignment, and room assignment. Size of list and indexes represent number of examinations. Each time period is an integer in range $[1, T]$ and each room has an integer value in range $[1, R]$ 
	- [(2, 1), (3, 2), (1, 1), (2, 2)] e.g., exam 1, time period 2, room 1.
- **Initialisation** could randomly assign each exam a time period and room.
- **Objective function** a measure of hard and soft constraint violations
- **Neighbourhood operators** are **perturbation operators:**
	- **OP1:** randomly choose exam, assign it to different time period
	- **OP2:** randomly choose exam and assign it to different room
	- **OP3:** Randomly choose exam and assign it to both a different time period and different room
		- All of above could be parameterised to pick $x$ random exams.
- Possible to integrate more LLHs here etc.

### Modelling Examination Timetabling as a Graph
Consider simplified exam timetabling problem, allocating exams to time periods with 7 exams $E = (e1, e2,... e7)$  and 5 students taking the exams $S = (s1, s2, ... s5)$ 
![[Pasted image 20250524183526.png]]

- **Nodes** represent exams
- **Edges** connect nodes/exams to represent exams with common students
- **Colours** will represent different time periods
- **Objective**: Colour the graph using the minimum number of colours/time periods. 
- ![[Pasted image 20250524183627.png]]
## Graph Based Hyper Heuristic For Examination Timetabling 
![[Pasted image 20250524183721.png]]
Use constructive LLHs.

### Graph Colouring LLHs for the GHH
- Largest Degree $(LD)$ - Aims to schedule exams with most potential conflicts first
- Saturation Degree $(SD)$ - prioritises scheduling exams with the highest number of available time slots? 
- Colour Degree $(CD)$ - opposite of saturation degree
- Largest Enrolment $(LE)$ - schedules exams with the **highest number of students first**
- Largest Weighted Degree $(LWD)$ - combines $LD$ and $LE$, by assigning weights to each edge as the number of students involved.
- Random Ordering $(RO)$ - orders exams that are not yet scheduled randomly 

### GHH Example
Hybrid of multiple heuristics used in passes to assign $N$ exams to time slots.
#### (Pass 1)
$h_1 = \{LE, SD, LD, LWD...\}$ 
Exams = $\{𝑒1, 𝑒2, 𝑒3, 𝑒4, 𝑒5, 𝑒6, 𝑒7\}$ 
$N = 2$ (schedule 2 exams per pass)

1. Choose next heuristic from $h_1$ as $LE$ 
2. Order remaining exams by $LE$ 
	exams = $\{ e4, e2, e3, e5, e1, e6, e7\}$
![[Pasted image 20250524185908.png]]
3. For $I = 0$ to $N-1$ schedule $exams[i]$
Exam slots = SLOT1 $[e4, e2]$, no conflict with each other

#### (Pass 2)
$h_1 = \{SD, LD, LWD...\}$ 
Exams =$\{𝑒1, 𝑒3, 𝑒5, 𝑒6, 𝑒7\}$
$N = 2$ 

1. Choose next heuristic from $h_1$ as $SD$ 
2. Order remaining exams by $SD$ 
	$exams = \{ e7, e5, e6, e1, e3\}$, ordered by lowest to highest
	![[Pasted image 20250524191944.png]]
3. For $I = 0$ to $N-1$ schedule $exams[i]$
Conflict! cannot schedule e5 in same time slot as e4, thus exam slots:
Exam slots = [(e4,e7), (e2, e5)]


#### Pass 3
$h_1 = \{LD, LWD...\}$ 
Exams = $\{e1, e3, e6\}$
N = 2

1. Choose next heuristic from $h_1$ 
2. Order remaining exams by $LD$ 
	Exams = $\{e3, e1, e6\}$ 
	![[Pasted image 20250524192136.png]]
3. For $I = 0$ to $N-1$ schedule $exams[i]$
$Exam slots = [ (e4,e7), (e2, e5), (e3, e1) ]$, due to conflicts


#### Pass 4
$h_1 = \{LWD...\}$ 
Exams = $\{e6\}$
N = 2

1. Choose next heuristic from h1 as LWD
2. Order remaining exams by LWD
	exams = {e6}
	![[Pasted image 20250524192508.png]]
3. . For $I = 0$ to $N-1$ schedule $exams[i]$
$Exam slots = [ (e4,e7), (e2, e5), (e3, e1, e6) ]$

Constructed a complete and feasible solution with 3 colours, optimal. For more difficult problems, may repeat this process multiple times.


![[Pasted image 20250524192518.png]]


# Genetic Programming
- Goal of genetic programming is to get a computer to do a given task without telling it how to do it
- Similar in many ways to the desired result of machine learning methods, and the methods themselves, but uses evolutionary process to create new programs
- Uses parse trees as a flexible representation of a computer program. 

## Example of a known program, and using trees to represent programs
```
int foo (int time) { 
	int temp1, temp2; 
	if (time > 10) 
		temp1 = 3; 
	else 
		temp1 = 4; 
	temp2 = temp1 + 1 + 2; 
	return (temp2); 
}
```

If run this program across different time inputs, get a mapping of inputs to outputs. Using genetic programming, can we then create a program that is the same as the known program.

Can construct the following tree to compute the same outputs as known program:
![[Pasted image 20250524193132.png]]
![[Pasted image 20250524193136.png]]
Trivial.

## Genetic operators for GP
Evolutionary algorithm containing the same components as seen before:
- **Initialisation** of the population could be by randomly generating a program for each individual/program
- **Crossover** can be used to combine two solutions to produce new solutions/programs
- **Mutation** can be used to create new solutions/programs making some small change to existing solutions.
Methodology inspired by biological evolution. It is a type of genetic algorithm, where individuals in the population are programs or expressions represented as trees, rather than bit strings or arrays.
Fitness measures how well a program solves the given task. 

## Random Generation of Programs
**Example**: Randomly generate a program that takes two arguments and uses the basic arithmetic operators:
- **Function set**(internal nodes/nodes with children): $\{+, -, *, /\}$
- **Terminal set**(leaf nodes): $\{X, Y, integer_values\}$
Random generation, randomly selects a function of a terminal to represent the program. If a function is chosen, recursively generate a random program until all leaf nodes are terminals, i.e., terminates and is valid.

### Random Generation Example 
1. Chosen binary `+`
2. Recurse on each child node
	a. Chosen binary `*`
	b. Recurse on each child node
		i. Chosen Terminal `X`
		ii. Chosen Terminal `Y`
	c. Chosen terminal `1`
![[Pasted image 20250524193750.png]]


## Mutation in GP
1. Select a random node and remove it from the tree
	If the node was an internal node/function, then remove all children as well
2. Insert a randomly generated program in place of the node just deleted.
![[Pasted image 20250524193853.png]]

## Crossover in GP
For two programs $p1$ and $p2$ select a subtree from each program and swap them
![[Pasted image 20250524193943.png]]
![[Pasted image 20250524193950.png]]



## Genetic Programming Hyper Heuristic - Solving Bin Packing Problems with Genetic Programming
Given a set of items 𝐼, $|𝐼| = 𝑛$, containing items of various sizes $𝑠_𝑥$, and an unlimited number of bins $𝑎_𝑥$, each with fixed capacity 𝐶, where $0 ≤ 𝑥 ≤ 𝑛…$ 
What is minimum number of bins required to pack all items without exceeding capacity of any of the bins?

### GP for bin packing
- Basic idea, generate a program to calculate some score/metric of the open bins to decide which bin to place the next item in
	- The following program/tree is a way of calculating the space left in the bin, after packing the new item
	- ![[Pasted image 20250524204455.png]]
		 Capacity - (Piece Size + Bin Fullness(size of items alr in bin))
		 ![[Pasted image 20250524204559.png]]
- Imagine, following tree, where $C$ is a fixed capacity , $C = 150$, $S$ is the size of the item to be paced, and $F$ is the bin fullness.
	- ![[Pasted image 20250524204840.png]]
	- C / ( (C-F) - S)
- Items $i_0...i_3$ are already packed, with 4 items remaining with size 70, 85, 30, and 60, respectively
	- ![[Pasted image 20250524204937.png]]
	- ![[Pasted image 20250524204944.png]]
	- as 150 / ( (150 - 90) - 70) = 15, etc, metric.
- Choose highest, which is $a_3$ , so place $i_4$ in $a_3$ 
	- ![[Pasted image 20250524205156.png]]
- Repeat
- ![[Pasted image 20250524205206.png]]
- ![[Pasted image 20250524205236.png]]
- ![[Pasted image 20250524205302.png]]
- ![[Pasted image 20250524205310.png]]