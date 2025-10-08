When employing subset construction, or converting from RE to NFA, etc, resulting automaton not always as minimal as possible, and thereofre not as efficient. Need to construct equivalent, but smaller automaton, accepting the same language. Use something called the **table-filling algorithm**


# Table filling algorithm
For a DFA $(Q, \Sigma, \delta, q_0, F)$, $p, q \in Q$ are **equivalent states**  if for all $w \in \Sigma^*$, $\hat{\delta}(p, w) \in F \iff \hat{\delta}(q, w) \in F$. If two states are **not equivalent**, then they are **distinguishable.** 

![[Pasted image 20250521140358.png]]
The states 1 and 2 are distinguishable $\hat{\delta}(1, \epsilon) = 1, \ 1 \notin F$ whereas $\hat{\delta}(2, \epsilon) = 2, \ 2 \in F$. 

The states 0 and 1 are distinguishable.
The states 4 and 5 are not distinguishable on any word however, **not possible to reach an accepting state from either.** 


Table filling algo, recursively constructs set of distinguishable pairs of states for DFA. When all distinguisable pairs identified, any remaining pairs of states must be equivalent, and so can be merged, minimising the automaton. 


**Basis**
For $p, q \in Q$ if $(p \in F \wedge q \notin F) \vee (p \notin F \wedge q \in F)$, then (p,q) is a distinguishable pair of states. THE PAIR IS DISTINGUISHABLE ON THE EMPTY WORD. 

**Induction**
![[Pasted image 20250521141021.png]]
IF DISITNGUISHABLE ON A WORD W, THEN THE STATES ARE EQUIVALENT ON AW.

DISTINGUISHABLE MEANS BOTH THE RESULTS OF THEIR EXTENDED TRANSITION FUNCTIONS FOR ALL VALID WORDS, ARE EQUIVALENT, JUST SCROLL UP. 


# Example usage
![[Pasted image 20250521141217.png]]
Construct table over all pairs of distinct states:
	List all states except last one in ascending order on top of table
	List all states except first one in descening order on left hand side  of table.
		![[Pasted image 20250521141641.png]]
		Then mark the state pairs that are distinguisable according to the basis. 
		![[Pasted image 20250521142913.png]]
		![[Pasted image 20250521143015.png]]
		Then do it for the induction. IF cannot show that a pair is distinguishable like (3,2) and (4,5), then they are equivalent. We merge these states by placing them on top of each other, and dragging along the edges
		![[Pasted image 20250521143152.png]]
		
		


# Table filling question
BASIS, THEN INDUCTION, SO DO FOR EMPTY WORD, THEN TRIAL AND ERROR, STATES IN ASCENDING ORDER, EXCEPT LAST ON TOP OF TABLE, STATES IN DESCENDING ORDER, EXCLUDING 1ST ON LEFT, THEN FORM TRIANGLE. MERGE STATES WHEN DONE. 