Correspond, to a computer, with finite, fixed amount of memory. Accepts certain words, and rejects others, the set of accepted words is the **language of the automaton**. Recongise the regular languages.


# Deterministic Finite Automaata
## What is a DFA
A DFA A = $(Q, \Sigma, \delta, q_0, F)$, given by:
1. $Q$, set of states
2. $\Sigma$ input symbols, alphabet
3. $\delta : Q \times \Sigma \to Q$
	Input, current state + input symbol
	Output next state
4. Initial state $q_0 \in Q$ 
5. A set of final states $F \Subset Q$ 
Final states = accepting states, initial states = accepting states

## Representing DFAs
Consider DFA D = $$(\{q_0, q_1, q_2)\}, \{0, 1\}, \delta, q_0, \{q_2\}$$ $\delta$ = 
	$\delta(q0, 0) = q1$
	$\delta(q0, 1) = q0$
	$\delta(q1, 0) = q1$
	$\delta(q1, 1) = q2$
	$\delta(q2, 0) = q2$
	$\delta(q2, 1) = q2$
	
May also use transition table
![[Pasted image 20250220181249.png]]
Have name of function, states on left, columns for symbols, and resulting states in the table, denoting initial and end symbol with arrow and star, respectively. 

### Transition Diagram
Given DFA D and $\delta$, over $Q$ and $\Sigma$ 
![[Pasted image 20250220181643.png]]
Incoming arrow for initial state, loop if stay in one place. 
If $\delta(q, x) = q'$  then there is arrow from $q \to q'$ labelled $x$ 
May not use double outline, may use outgoing arrow from final state in DFA also. 


Example of larger DFA over $\Sigma=\{a,b,c\}$
$Q=\{A,B,C,D,E,F,G\}$
![[Pasted image 20250220182202.png]]
With transition table: DO THIS IN THE EXAM
![[Pasted image 20250220182457.png]]


## Language of  DFA
How DFA accepts or rejects words over $\Sigma$ 
For a **DFA $A$** with alphabet $\Sigma$, $L(A) \Subset \Sigma*$

To determine whether $w \in L(A)$ , start in initial state (show which state in by underlining state name). Transition to new state on input symbol x, according to transition function. Once all symbols read, if the last state transitioned to was final, then the word is accepted. Only method of acceptance. 

![[Pasted image 20250220182938.png]]

$w = 101$
q0 x 1 = q0
q0 x 0 = q1
q1 x 1 = q2
Final state, so accepted.

Can see DFA amounts to:
$$L(D) = \{w | w \ contains \ substring \ 01\}$$ 
### Extended transition function
To make idea of language of DFA precise, give formal definition of $L(A)$.
First, define extended transition function, determines state reached after reading given **STRING** from initial state. $\hat{\delta} \in Q \times \Sigma^* \to Q$.
	Input = Initial state(or any state)
	Output: State reached.
	Intuitively: $\hat{\delta}(q, w) = q'$ if machine starting from state $q$ ends up in $q'$when reading word $w$.

Formally defined via recursion:
	 $\hat{\delta}(q, \epsilon)$ = $q$
	  $\hat{\delta}(q, xw)$ = $\hat{\delta}(\delta(q,x),w)$ 
	  Do transition function for first symbol, use that output state with rest of the word. 
	  Where $x \in \Sigma$ and $w \in \Sigma^*$.

$\hat{\delta}(q0, 101)$ = $q2$
	= $\hat{\delta}(\delta(q0, 1)01)$ 
	= $\hat{\delta}(q0, 01)$ 
	= $\hat{\delta}(\delta(q0, 0)1)$ 
	= $\hat{\delta}(q1, 1)$ 
	= $\hat{\delta}(\delta(q1, 1)\epsilon)$ 
	= $\hat{\delta}(q2, \epsilon)$ 
	= q2

Therefore $L(A) = \{w \ | \ \hat{\delta}(q, w) \in F \}$ 

Thus in example, $101 \in L(D)$ because $\hat{\delta}(q_0, 101) = q2 \, and \ q2 \in F$


# Questions
1. Let the alphabet ΣA = {a, b} and consider the following DFA A $$ A = (Q_a = \{0,1,2,3\}, \Sigma_A, \delta_a, q_0 = 0, F_A = \{1,2\}\}$$ $$ \delta_A = \{(0, a) = 1, (0, b) = 2, (1, a) = 0, (1, b) = 3, (2, a) = 3, (2, b) = 0, (3, a) = 2, (3, b) = 1 $$
	Draw transition diagram
	Determine which of the following words belong to L(A)
	1. $\epsilon$, $\epsilon \notin L(A)$ 
	2. b, b $\in$ L(A)
	3. $abaab \in L(A)$
	4. LOOK AT BOOk

	Calculate extended transition function
		$\hat{\delta}_A(0, abba)$ 
			= $\hat{\delta}_A(\delta(0, a)bba)$
			= $\hat{\delta}_A(1, bba)$ 
			= $\hat{\delta}_A(\delta(1, b)ba)$
			= $\hat{\delta}_A(3, ba)$ 
			= $\hat{\delta}_A(\delta(3, b)a)$
			= $\hat{\delta}_A(1, a)$ 
			= $\hat{\delta}_A(\delta(1, a) \epsilon)$ 
			= $\hat{\delta}_A(0, \epsilon)$ 
			= q0
			
			
	Describe language it recognises in english
	 Recognises words where the amount of a's and b's differ by an odd number. 


2. Construct a DFA B over $\Sigma_B$ = {a,b,c,d} accepting all words where the number of a's is a multiple of 3.   
FUCK THIS


