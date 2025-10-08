## DFA minimisation
As a final refinement to the $RE \to DFA$ conversion, we can add an algorithm to minimise the number of states in the $DFA$. The $DFA$ that emerges from the subset construction can often have a large set of states, often needlessly complex, which increases the size of the recogniser in memory. Speed of memory accesses often governs the speed of computation given 'that famous bottleneck'. Thus! A smaller recogniser may fit better into cache memory. 

To minimise the number of states in a $DFA$, we need a technique to detect when two states are equivalent - that is, when they produce the same behaviour on any input string. From these equivalences we can construct a minimal $DFA$. 

Also remove dead and unreachable states, which accelerates the merging of non-distinguishable states. 

### Hopcroft's algorithm
This is an algorithm which merges the nondistinguishable states of a $DFA$. Two states $p$ and $q$ in a DFA are **equivalent** if, for every possible input string $w$:

$$δ^*(p,w)∈F  ⟺  δ^*(q,w)∈F$$

That means:

- Starting from either $p$ or $q$, reading any string www will **either both end in a final state** or **both end in a non-final state**.
    

If that’s true, they’re **indistinguishable** and can be **merged**. 

The algorithm constructs a set partition(a collection of nonempty, disjoint subsets of $P$ whose union is exactly $P$) $P = \{p_1, p_2,...,p_m\}$ of the $DFA$ states.

The partition $P$, is responsible for constructing groups of $DFA$ states according to their behaviour. 

Each partition must follow two rules: 
1. For each state in a partition, every transition must lead to another state in the same partition.
2. The states in a partition must be either all accepting or all nonaccepting.

**Algorithm:**
1. Initialise the partition
	$P = \{F, Q-F\}$ 
2. Refinement loop
	Choose a set in the partition, called $p$. For each $c \in \Sigma$, determine all states that can reach a state in $p$.
	For each set $q$ in the partition that has one of those states:
		Let $q_1$ = 