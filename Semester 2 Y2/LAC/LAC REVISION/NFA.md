NFA have transition functions, map given state and input symbol to **ZERO OR MORE SUCCESSOR STATES.** Can be thought of as having a choice, whenever there are two or more possible transitions given input symbol for a given state. 
NFA may have multiple initial state
Accepts word $w$ if there is at least one possible way, get from initial state, to a final state, using nondterministic transition function.
Can always be determined whether or not $w$ belongs in language. 
Using subset construction, every NFA can be translated into a DFA that accepts the same language. 

![[Pasted image 20250220193417.png]]
NFA that accepts all words over {0,1} where 1 is second to last symbol.

## Definition of NFA
An **NFA** $A = (Q, \Sigma, \delta, S, F)$ 
- Q, set of states
- $\Sigma$ set of input symbols
- $\delta$ : $Q \times \Sigma \to \mathcal{P}(Q)$ 
	- Input: Current state and input symbol
	- Output, set of possible next states, including empty state due to powerset. 
- $S$ set of starting states $S \Subset Q$ 
- F set of accepting states $F \Subset Q$ 

## Example NFAs and Transition Functions


![[Pasted image 20250220193417.png]]

![[Pasted image 20250220202508.png]]
Transition table for the Above NFA, now uses sets instead of just singular $Q$ from $\delta$ 
Still marked using arrow and star to denote starting and final state. 


![[Pasted image 20250220203204.png]]
$Q = \{0,1,2,3,4,5\}$ 
![[Pasted image 20250220203213.png]]
Multiple initial states, multiple final states, final state can also be initial state like in DFA.




## Language accepted by NFA
To determine whether $w \in \Sigma^*$ is accepted by an NFA, must consider all possible transitions from its current state to new states, based on symbol consumed. After reading entire input word $w$, if at least one of the possible states is a final state, then word is accepted, $w \in L(A)$, otherwise not. 


See how below NFA rejects 100. 

![[Pasted image 20250220204047.png]]
Initial state q0.
Can go to q0, q1.
Look at marked states. Remove old markers, put new ones at all reachable states given input symbol. 
![[Pasted image 20250220204248.png]]
q0, q1, marked/reachable, must consider both states and their possible transitions. 
Can go to q0, q2
![[Pasted image 20250220204326.png]]
Reading 0, can go to q0. 
q2 unmarked after reading 0, no transition defined, returns emptyset, $\emptyset \notin F$ 
![[Pasted image 20250220204402.png]]
So fails. 

bacac

![[Pasted image 20250520173902.png]]
MARK ALL INITIAL STATES.
1, 4 marked

![[Pasted image 20250520173924.png]]
UNMARK IF NO TRANSITION DEFINED
2 marked

![[Pasted image 20250520173936.png]]
3, 4 marked

![[Pasted image 20250520175015.png]]
mark 3, mark 5

![[Pasted image 20250520175023.png]]
mark 1


![[Pasted image 20250520175030.png]]
unmark 1, fails


NO FINAL STATE MARKED, SO FAILS. ALL WORDS WITH PREFIX BACAC FAIL.

## Extended transition Function For NFAs
Use generalisation of the union operator$$
\bigcup \{A_1, A_2, ..., A_n\} = A_1 \cup A_2\ ... \cup \ A_n$$
E.g.,
$\bigcup\{\{1\}\{2,3\}, \{1,3\}\} = \{1\} \cup \{2,3\} \cup \{1,3\} = \{1,2,3\}$

**LAWS:**
$\bigcup\emptyset = \emptyset$
$\bigcup\{A\} = \{A\}$


We define the extended transition function: 
$\hat{delta} : \mathcal{P}(Q) \times \Sigma^* \to \mathcal{P}(Q)$ such that $\hat{\delta} (P, w)$ is the set of states that are reachable from one of the states in P on the word $w$. 

$\hat{\delta}(P, \epsilon) = P$ 
$\hat{\delta}(P, xw) = \hat{\delta}(\bigcup\{\delta(q,x) \ | \ q \in P\}, w)$ 
Compute and union all possible states NFA could be in after consuming x. And if $F$ $\in$ Result, then accepted. 

$$\begin{array}{l l l r} \hat{\delta}_{C}(\{ q_{0} \},100) & = & \hat{\delta}_{C}(\bigcup \{ \delta_{C}(q,1)\mid q \in \{ q_{0} \} \},00) & \text{by }(4) \\ & = & \hat{\delta}_{C}(\delta_{C}(q_{0},1),00) \\ & = & \hat{\delta}_{C}(\{ q_{0},q_{1} \},00) \\ & = & \hat{\delta}_{C}(\bigcup \{ \delta_{C}(q,0)\mid q\in \{ q_{0},q_{1} \} \},0) & \text{by }(4) \\ & = & \hat{\delta}_{C}(\delta_{C}(q_{0},0)\cup\delta_{C}(q_{1},0),0) \\ & = & \hat{\delta}_{C}(\{ q_{0} \}\cup \{ q_{2} \},0) \\ & = & \hat{\delta}_{C}(\{ q_{0},q_{2} \},0) \\ & = & \hat{\delta}_{C}(\bigcup \{ \delta_{C}(q,0)\mid q\in \{ q_{0},q_{2} \} \},\epsilon) & \text{by }(4) \\ & = & \hat{\delta}_{C}(\delta_{C}(q_{0},0)\cup\delta_{C}(q_{2},0),\epsilon) \\ & = & \hat{\delta}_{C}(\{ q_{0} \}\cup \emptyset,\epsilon) \\ & = & \{ q_{0} \} & \text{by }(3) \end{array}$$



Example for bacac in book
JUST KEEP READING THIS







