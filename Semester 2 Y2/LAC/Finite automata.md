Correspond to a computer with a fixed finite amount of memory. An automation will accept certain words (sequences of symbols of a given alphabet $\Sigma$) and reject others. The set of accepted words is called the ***language of the automaton***. The class of languages that are accepted by DFAs and NFAs are the same, they recognise the regular languages.

## Deterministic finite automata
### What is a DFA
A deterministic finite automaton (DFA) A = ($Q$, $\Sigma$, $\delta$, $q0$, $F$) is given by:
	1. A finite set of states $Q$
	2. A finite set of input symbols, the alphabet $\Sigma$
	3. A transition function $\delta : Q \times \Sigma -> Q$ 
		- Input: Current state and input symbol
		- Output: Next state
	4. An initial state $q0 \Subset Q$
	5. A set of final states $F \Subset Q$
The initial states are sometimes called start states, and the final states are sometimes called accepting states.

### Representing DFAs
Consider following automaton: 
D = ({q0, q1, q2}, {0, 1}, $\delta d$, q0, {q2})

Can define the transition function case by case:
	δD(q0, 0) = q1 
	δD(q0, 1) = q0 
	δD(q1, 0) = q1 
	δD(q1, 1) = q2 
	δD(q2, 0) = q2 
	δD(q2, 1) = q2
A DFA may also be represented by a transition table 
![[Pasted image 20250220181249.png]]
A transition table represents transition function $\delta$ i.e., the value of $\delta(q,x)$is given by the row labelled q in the column labelled x. (just look above lol.) Final states are labelled using a * and initial states are labelled using → .
E.g., a variation D' of DFA D where q0 is final:
![[Pasted image 20250220181525.png]]

May also represent DFA via a transition diagram. The transition diagram for DFA D is:
![[Pasted image 20250220181643.png]]
Initial state identified with incoming arrow. Final states are drawn with double outline. If $\delta(q,x)=q'$ then there is an arrow from state $q$ to $q'$ that is labelled $x$. 
Transition diagram for DFA D':

![[Pasted image 20250220181916.png]]
Alternative to double outline for final state is outgoing arrow from the final state. 
![[Pasted image 20250220182016.png]]

Example of larger DFA over $\Sigma=\{a,b,c\}$
![[Pasted image 20250220182202.png]]
$Q=\{A,B,C,D,E,F,G\}$
Commonly use symbols $q_i, i \in \mathbb{N}$, to name states, or use natural numbers purely to name the states.

Transition table for above DFA.  
![[Pasted image 20250220182457.png]]

### Language of a DFA
How DFA accepts or rejects words over its alphabet of input symbols. The set of words accepted by a DFA A is called the language $L(A)$ of the DFA. Thus for a DFA A with alphabet $\Sigma$, $L(A) \Subset \Sigma^*$.

To determine whether a word $w \in L(A)$, machine starts in initial state. We indicate the state of a DFA at any given point by underlining the state name. 
![[Pasted image 20250220182938.png]]
Whenever input symbol read from $w$, machine transitions to a new state by following edge labelled with this symbol. Once all symbols in input word $w$ read, $w$ is accepted if the state is final, $w \in L(A)$ otherwise $w \notin L(A)$.

E.g., $w = 101$ NFA = $D$, start from $q0$
	$δD(q0, 1) = q0$
	$δD(q0, 0) = q1$
	$δD(q0, 1) = q2$
As the state is final, $w \in L(D)$. 
Some other words: $0 \notin in L(D), 110 \notin L(D)$ but $011 \in L(D)$. We can see that: 

$L(D) = \{w | \text{ w contains substring } 01\}$

To make idea of language of DFA precise, give formal definition of L(A). First, define extended transition function which determines state reached after reading given string from initial state $\hat{\delta} \in Q \times \Sigma^* -> Q$, takes initial state (or any state) and a string, and outputs state reached.
Intuitively: $\hat{\delta}(q, w) = q'$ if machine starting from state $q$ ends up in $q'$when reading word $w$.

Formally, $\hat{\delta}$ is defined by primitive recursion: 
	$\hat{\delta}(q, \epsilon)$ = $q$
	 $\hat{\delta}(q, xw)$ = $\hat{\delta}(\delta(q,x),w)$
	 Where $x \in \Sigma$ and $w \in \Sigma^*$.
$xw$ thus stands for nonempty word where first symbol is x, and rest is w. E.g., if 010, $x = 0$ and $w = 10$. $w$ may also be empty.

$\hat{\delta}(q0, 101)$ = $q2$
	= $\hat{\delta}(\delta(q_0,1),01)$
	= $\hat{\delta}(q_0,01)$ - because $\delta(q_0, 1) = q_0$
	= $\hat{\delta}(\hat{\delta}(q_0,0),1)$
	= $\hat{\delta}(q_1,1)$ - because $\delta(q_0, 0) = q_1$
	= $\hat{\delta}(\hat{\delta}(q_1,1),\epsilon)$
	= $\hat{\delta}(q_2, \epsilon)$ - because $\delta(q_1, 1) = q_2$
	= $q2$

Using extended transition function, define the language $L(A)$ of a DFA $A$ formally:
	 $L(A) = \{w \ | \ \hat{\delta}(q_0, w) \ \in F\}$
Thus in example, $101 \in L(D)$ because $\hat{\delta}(q_0, 101) = q2 \, and \ q2 \in F$




### Nondeterministic finite automata
NFA have transition functions that map given state and input symbol to *zero or more* successor states. This machine can be thought of as having a choice whenever there are two or more possible transitions from a state on an input symbol. NFAs may also have more than one initial state, can be thought of as choosing where to begin. NFA accepts a word $w$ if there is at least one possible way to get from one of the initial states to one of the final states along edges labelled with the symbols of $w$ in order. 

While NFAs have nondeterministic transition functions, can always be determined whether or not $w$ belongs to its language. Every NFA can be translated into a DFA that accepts the same language. 

Here is example of NFA C that accepts all words over $\Sigma \{0,1\}$ such that symbol before the last is 1:

![[Pasted image 20250220193417.png]]

A non deterministic finite automaton (NFA) $A$ $=$ $(Q, \Sigma, \delta, S, F)$
1. A finite set of states $Q$
2. A finite set of input symbols, the alphabet $\Sigma$
3. A transition function $\delta \in Q \times \Sigma -> P(Q)$
	1. Input: Current state and input symbol
	2. Output: set of possible next states. 
4. A set of initial states $S \Subset Q$
5. A set of final states $F \Subset Q$

Example transition table for NFA C:
	 ![[Pasted image 20250220202508.png]]
Table entries = sets of states, these sets may also be empty $\emptyset$. Initial state marked with → and final state marked with *.

Another example of NFA, over alphabet $\Sigma = \{a,b,c\}$ and with states $Q = \{0,1,2,3,4,5\}$ $\subset N$. 
![[Pasted image 20250220203204.png]]
With transition table: 
![[Pasted image 20250220203213.png]]
NFA has multiple initial states, multiple final states, one initial state that is also final, some cases there are no possible successor states, and in other cases more than one.


### Language accepted by an NFA
To determine whether word $w \in \Sigma^*$ is accepted by an NFA $A$, must consider all possible transitions from its current states to new states based on symbol being consumed. After reading entire input word $w$ if at least one of the possible states is a final state, then word accepted, $w \in L(A)$ otherwise rejected, $w \notin L(A)$

Let us see how NFA C rejects word 100. May be multiple marked states at once, but for this just $q_0$ initially.

![[Pasted image 20250220204047.png]]
Each time symbol read, look at mark states, remove old markers and put markers at all the states that are reachable via edge marked with current input symbol. May be a state that was marked previously.
E.g., read 1, leaves 2 marked states, 2 arrows reachable.
![[Pasted image 20250220204248.png]]
After reading 0, q0 and q2 marked: can reach q0 from q0 and q2 from q1.
![[Pasted image 20250220204326.png]]
Only one of the marked states is final, so 10 accepted, But still have 0 left. 
![[Pasted image 20250220204402.png]]
Marks q0 and unmarks q2 as cannot be reached from any marked states, and since this only possible state is not final, the NFA rejects the word.

![[Pasted image 20250220211120.png]]
For bacac. Accepts abcc, abcca. 
There are no marked states left at all: this means that NFA reject all words that start $bacac$.


To define extended transition function $\hat{\delta}$ for NFAs we use a generalisation of the union operation. 

$\bigcup\{A_1, A_2,...A_n\} = A_1 \cup A_2...\cup A_n$

In special cases of empty set and singleton
$\bigcup\emptyset = \emptyset$
$\bigcup\{A\} = \{A\}$

E.g.,
$\bigcup\{\{1\}\{2,3\}, \{1,3\}\} = \{1\} \cup \{2,3\} \cup \{1,3\} = \{1,2,3\}$

We define $\hat{\delta} \in P(Q) \times {\Sigma^*} -> P(Q)$ such that $\hat{\delta} (P, w)$ is the set of states that are reachable from one of the states in P on the word $w$. 
E
$\hat{\delta}(P, \epsilon) = P$ 
$\hat{\delta}(P, xw) = \hat{\delta}(\bigcup\{\delta(q,x) \ | \ q \in P\}, w)$ 
Compute all possible states the NFA could be in after consuming x. 

$$\begin{array}{l l l r} \hat{\delta}_{C}(\{ q_{0} \},100) & = & \hat{\delta}_{C}(\bigcup \{ \delta_{C}(q,1)\mid q \in \{ q_{0} \} \},00) & \text{by }(4) \\ & = & \hat{\delta}_{C}(\delta_{C}(q_{0},1),00) \\ & = & \hat{\delta}_{C}(\{ q_{0},q_{1} \},00) \\ & = & \hat{\delta}_{C}(\bigcup \{ \delta_{C}(q,0)\mid q\in \{ q_{0},q_{1} \} \},0) & \text{by }(4) \\ & = & \hat{\delta}_{C}(\delta_{C}(q_{0},0)\cup\delta_{C}(q_{1},0),0) \\ & = & \hat{\delta}_{C}(\{ q_{0} \}\cup \{ q_{2} \},0) \\ & = & \hat{\delta}_{C}(\{ q_{0},q_{2} \},0) \\ & = & \hat{\delta}_{C}(\bigcup \{ \delta_{C}(q,0)\mid q\in \{ q_{0},q_{2} \} \},\epsilon) & \text{by }(4) \\ & = & \hat{\delta}_{C}(\delta_{C}(q_{0},0)\cup\delta_{C}(q_{2},0),\epsilon) \\ & = & \hat{\delta}_{C}(\{ q_{0} \}\cup \emptyset,\epsilon) \\ & = & \{ q_{0} \} & \text{by }(3) \end{array}$$
The language of NFA can now b defined using $\hat{\delta}$: 
L(NFA) = $\{w \ | \ \hat{\delta}(S, w) \ \cap F \not = \emptyset\}$

Thus, 100 $\notin L(C)$ because: 
	$\hat{\delta}_C (S_C , 100) \cap F_C = \hat{\delta}_C ({q0}, 100) \cap \{q2\} = \{q0\} \cap \{q2\} = \emptyset$

### Subset construction
DFAs can be viewed as a special case of NFAs, for those which there is precisely one start start ($S = \{q_0\}$) and for which the transition function always return singleton sets $\delta(q,x) = \{q'\}$ for all $q \in Q \ and \ x \in \Sigma$.

Opposite also true, NFAs are just DFAs in disguise. Can systematically construct equivalent DFA, i.e., a DFA which accepts the same language as the given NFA. NFAs are thus no more powerful than DFAs as they describe the same class of language. In some cases however, NFAs need a lot fewer states than the corresponding DFA and may be more intuitive and simpler to construct in the first place. 

The subset construction. Given an NFA $A$ = $(Q,\Sigma, \delta, S, F)$ we construct the equivalent DFA: 

D(A) = $(P(Q), \Sigma,\delta_{D(A)}, S, F_{D(A)})$
- P(Q) = power set of Q, each state in DFA corresponds to a subset of states from NFA
- Input alphabet $\Sigma$
- The transition function for the DFA $\delta_{D(A)}$
- The start state of the DFA
- The set of final states for the DFA
where 
	$\delta_{D(A)}(P, x) = \bigcup\{\delta(q,x) \ | \ q \in P\}$
		![[Pasted image 20250220225913.png]]
	$F_{D(A)} = \{P \in P(Q) \ | \ P \cap F \not = \emptyset\}$
		
Basic idea is to define DFA whose states are sets of NFA states. A set of possible NFA states thus becomes a single DFA state. The DFA transition function is given by considering all reachable NFA states for each of the current possible NFA states for each input symbol(marked). The resulting set of possible NFA states is again just a single DFA state. A DFA state is final if that set contains at least one final NFA state. 

![[Pasted image 20250220231347.png]]
![[Pasted image 20250220231423.png]]
![[Pasted image 20250220231440.png]]
Some of the states cannot be reached from the initial state, means they can be omitted without changing the language.
Thus obtain the following automaton:
![[Pasted image 20250220231523.png]]
