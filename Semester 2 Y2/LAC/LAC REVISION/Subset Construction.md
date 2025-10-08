DFAs = Special case of NFA, one start state, and for which transition function always returns singleton sets, $\delta(q, x) = \{q'\} \ , \forall q \in Q \wedge x \in \Sigma$ 

Opposite true. Can construct equivalent DFA which accepts same lang as NFA for all NFAs, thus NFAs no more powerful, but more intuitive and simpler to construct often in first place, needing fewer states and transitions to achieve the same goal.


Given NFA $A = (Q, \Sigma, \delta, S, F)$, construct equivalent DFA:

$D(A) = (\mathcal{P}(Q), \Sigma, \delta_{D(A)}, S, F_{D(A)})$ 
	$\mathcal{P}(Q)$ = power set of Q, each state in DFA corresponds to a subset of states from NFA.???
	Alphabet $\Sigma$ 
	Transition function for DFA
	The start state of the DFA
	The set of final states for the DFA
Where the transition function is $$\delta_{D(A)}(P, x) = \bigcup\{\delta(q,x) \ | \ q \in P\}$$
And $F_{D(A)}$ is defined as $$F_{D(A)} = \{P \in P(Q) \ | \ P \cap F \not = \emptyset\}$$ IF A SUBSET OF Q HAS A FINAL STATE IN, IS FINAL.


So define DFA whose states are sets of NFA state. A set of possible NFA states thus becomes a single DFA state. The DFA transition function is given by considering all possible reachable states from a given state using a given input symbol, and then unioning them. The resulting set of possible NFA states from this is just a single DFA state. 


![[Pasted image 20250220231347.png]]
KEEP REREADING TS
![[Pasted image 20250220231440.png]]



![[Pasted image 20250520185659.png]]
5 states, 2^5 = 32, possible states for D(A). 
Tactic, start from $S_N=\{q_0\}$, and compute transition function
$\delta_{D(A)}(q_0, x) = \bigcup(q, x) | q \in S_n$ for each symbol. 

![[Pasted image 20250520185952.png]]
ADD NEWLY UNCONSIDERED STATES TO TABLE

![[Pasted image 20250520190011.png]]

![[Pasted image 20250520190147.png]]

![[Pasted image 20250520190158.png]]

![[Pasted image 20250520190205.png]]

![[Pasted image 20250520190215.png]]



# Questions
![[Pasted image 20250520192224.png]]
Construct a DFA D(B) equivalent to B using the “subset construction” and draw the transition diagram for D(B), ignoring unreachable states. Clearly show each step of your calculations, e.g. in a transition table.

Answer in book


DO TRANSITION TABLE, ONLY PUT INITIAL STATE(S) IN FROM NFA, AND THEN DO REST OF TABLE, ADDING STATES TO LHS OF TABLE AS THEY APPEAR FROM CALCULATING INITIAL STATE.

WHEN HAVE MULTIPLE INITIAL STATES, ALL GO TOGETHER IN A SET OF STATES, AND START FROM THERE. EXAMPLES IN DISCORD AND IN BOOK.
