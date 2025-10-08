> Finite automaton augmented with a stack, a data structure which can store infinite set of symbols, only accessed in LIFO manner. Recognise context-free languages.

# Pushdown Automaton Definition
A **PDA** $P = (Q, \Sigma, \Gamma, \delta, q_0, Z_0, F)$:
- A finite set of states, $Q$ 
- A finite set $\Sigma$ alphabet of input symbols
- A finite set $\Gamma$ of stack symbols
- A transition function $\delta : Q \times (\Sigma \cup \{\epsilon\}) \times \Gamma \to \mathcal{P} (Q \times \Gamma^*)$ 
	- Takes state, input symbol, top of stack symbol, returns finite set of possible moves:, including next state and replacement symbol for top of stack, if any. 
- An initial state $q_0 \in Q$ 
- An initial stack symbol $Z_0$ 
- A set of final states $F \Subset Q$ 

To recognise all even length-palindromes over $\Sigma = \{0,1\}$: 
$$L = \{w \in \{0,1\}^* \ | \ w = ww^R\}$$
Define following PDA.

$$P_0 = (\{q_0, q_1, q_2\}, \{0,1\},\{0,1,\#\}, \delta, q_0, \#, \{q_2\}) $$
PDA compares input, what is on stack, pushes and pops symbols accordingly, if reaches EOI according to transition function and states, precisely when stack is empty, then $w \in L$. Guesses midpoint, and then starts popping, via empty transition. 


Transition function given by following equations.
![[Pasted image 20250430205041.png]]
- At q0: Any symbol + any stack symbol = q0 and push input symbol onto stack
- At q0: Empty transition + any stack symbol = q1, leave stack alone
- At q1: Symbol X + symbol x on top of stack = q1, remove x from stack
- At q1, empty transition + empty stack = q2 + empty stack
- Anything else invalid
![[Pasted image 20250430205502.png]]

![[Pasted image 20250430210138.png]]
Draw transition diagram, label each transition with a triple $x, Z, \gamma$ with $x \in \Sigma, Z \in Γ, \gamma \in Γ^*$ (gamma represents resulting stack)
==LABEL WITH INPUT SYMBOL, **CURRENT TOP OF STACK**, AND RESULTING STACK.==  

# How does PDA work.
- At any time, state of computation of PDA $P$ = $(Q, \Sigma, \Gamma, \delta, q_0, z_0, F)$ is given by a triple:
	1. The State $q \in Q$ PDA is in
	2. The input string left to be processed, $w \in L^*$ that still has to be processed.
	3. The whole contents of the stack $\gamma \in \Gamma^*$
- Such a triple $(q, w, \gamma) \in Q \times \Sigma \times \Gamma^*$ is called an **INSTANTANEOUS DESCRIPTION (ID).** 
- Define relation between IDs that describe, how PDA change from one ID to next. RELATION BECAUSE PDAS ARE DETERMINISTIC, MAY BE MORE THAN ONE POSSIBILITY FOR GIVEN ID.
	-  $⊢_p \Subset ID \times ID$ 
	- Two possibilities for $⊢_p$ 
		1.  $(q, xw, z\gamma) ⊢_p (q', w, a\gamma) \ if \ (q', a) \in \delta(q, x ,z)$ 
			Consult transition function via curr state, input symbol, top of stack, get new state, new stack symbol pushed on replacing z.
		2. $(q, w, z\gamma) ⊢_p (q', w, a\gamma) \ if \ (q', a) \in \delta(q, \epsilon ,z)$ 
			Consult transition function via curr state, empty word, and top of stack, get new state and new stack symbol


For PDA $$P_0 = (\{q_0, q_1, q_2\}, \{0,1\},\{0,1,\#\}, \delta, q_0, \#, \{q_2\}) $$ With $\delta$ 
![[Pasted image 20250430205502.png]]
![[Pasted image 20250430210138.png]]

Consider word $0110$. What are possible sequences of IDs for this? 

(q0, 0110, #) -> (q0, 110, 0#) -> (q0, 10, 10#) -> (q1, 10, 10#) -> 
(q1, 0, 0#) -> (q1, epsilon, #) -> (q2, epsilon, \epsilon)








$$

\begin{array}{rll}

(q_{0},0110,\#) & \vdash_{P_{0}}(q_{0},110,0\#)  & \text{1. with }(q_{0},0\#) \in \delta(q_{0},0,\#) \\

 & \vdash_{P_{0}}(q_{0},10,10\#) & \text{1. with }(q_{0},10) \in \delta(q_{0},1,0) \\

 & \vdash_{P_{0}}(q_{1},10,10\#) & \text{2. with }(q_{1},1)\in \delta(q_{0},\epsilon,1) \\

 & \vdash_{P_{0}}(q_{1},0,0\#) & \text{1. with }(q_{1},\epsilon) \in \delta(q_{1},1,1) \\

 & \vdash_{P_{0}}(q_{1},\epsilon,\#) & \text{1. with }(q_{1},\epsilon) \in \delta(q_{1},0,0) \\

 & \vdash_{P_{0}}(q_{2},\epsilon,\epsilon) & \text{2. with }(q_{2},\epsilon) \in \delta(q_{1},\epsilon,\#)

\end{array}

$$


We write $(q, w, \gamma) \overset{*}{\vdash}_P (q', w', \gamma)$ if the PDA can move from $(q, w, \gamma)$ to $(q', w', \gamma)$ in 0 or move moves. Above, shown that:$$ (q_0, 0110, \#) \overset{*}{\vdash}_P (q_2, \epsilon, \epsilon)$$
This is not only possible sequence of IDs for this input may guess the middle wrong, and empty transition at 110, and fails. 
![[Pasted image 20250430212204.png]]

# Language of a PDA
Two ways to define language of a PDA $P = (Q, \Sigma, \Gamma, \delta q_0, Z_0, S)$.

## Acceptance by final state
$$L(P) = \{w \ | \ (q_0, w, Z_0)\overset{*}{\vdash}_P(q, \epsilon, \gamma) \wedge q \in F\}$$
PDA accepts word w if there is any sequence, IDs, starting $(q_0, w, Z_0)$ and leading to ($q, \epsilon, \gamma$) where $q \in F$ is one of the final states. Contents of stack are irrelevant.

Previous PDA would accept 0110 because $q2 \in F$(although the stack is also empty.)

## Acceptance by empty stack
$$ L(P) = \{w \ | \ (q_0, w, Z_0)\overset{*}{\vdash}_P(q, \epsilon, \epsilon)\}$$
PDA that accepts a word if there is any sequence of id's leading to $(q, \epsilon, \epsilon)$ i.e., the stack is empty. The state is irrelevant, the stack for $w \in L(P)$ must be empty. 

Just leave out F when specifying PDA and use acceptance by empty stack instead.

# Deterministic PDAs
PDAs, nondeterministic machines, several alternatives for given ID, how to continue. Deterministic PDA never have a choice however.


To be precise, say that a PDA $P$  = ($Q, \Sigma, Γ, \delta, q_0, Z_0, F)$ is deterministic iff $$|\delta(q, x, z)| + |\delta(q, \epsilon, z)| \le 1$$
NO EQUIVALENT EMPTY TRANSITIONS, AND FOR GIVEN STATE, INPUT SYMBOL AND TOP OF STACK, CAN ONLY HAVE 1 RESULTING STATE AND REPLACEMENT STACK SYMBOL.

There is no way to translate nondeterministic PDA into a deterministic one. NPDA more powerful than DPDA. 

The 2 acceptance methods are not equivalent for DPDAs: acceptance by final state makes it possible to define a bigger class of languages. So always use acceptance by final state if have to use DPDA.

# CFG and PDA relationship.
For a language $L \Subset \Sigma^*$ the two following statements, equivalent:
1. $L$ is given by a **CFG** $G, L = L(G)$
2. $L$ is given by a **PDA** $P, P = L(P)$
Context free languages can be described by CFGs and processed by PDAs. Both recognise same thing.


## Construct PDA Given Grammar
Given a CFG $G$ = $(N, T, P, S)$ , define a **PDA** 
	$P(G) = (\{q_0\}, T, N \cup T, \delta, q_0, S)$ 
We do not give set of final states, use acceptance by empty stack. 
STACK HAS EITHER TERMINALS OR NON TERMNALS, INPUT IS TERMINALS, S IS FIRST THING ON STACK.

$\delta$ defined as follows:
$$
\begin{array}{rll}

\delta(q_{0},\epsilon,A) & = & \{ (q_{0},\alpha)\mid A\to\alpha \in P \} & \text{for all }A \in V \\

\delta(q_{0},a,a) & = & \{ (q_{0},\epsilon) \} & \text{for all }a \in \Sigma

\end{array}$$
1. GIVEN Q0, EMPTY WORD, NONTERMINAL ON TOP OF STACK, RESULTS IN Q0, AND PUSHING BODY OF PRODUCTION FOR THAT NONTERMINAL ONTO STACK.
2. GIVEN Q0, TERMINAL SYMBOL, TERMINAL SYMBOL ON TOP OF STACK, RESULTS IN Q0, AND REPLACES TERMINAL SYMBOL ON TOP OF STACK WITH EMPTY WORD, for all terminals, consumes input.



GIVEN q0, empty word, and A on stack, results in q0, and possible body of production for nonterminal replacing nonterminal at top of stack
Given q0, terminal, and terminal on top of stack, results in q0, and replacing terminal symbol on stack with empty word, for all terminals, consumes input.
##### Example
$$

G=(\{ E,T,F \},\{ (,),a,+,* \},E,P)

$$

Where the set of **productions** $P$ are given by

$$

\begin{array}{rll}

E & \to & T & \mid & E+T \\

T & \to & F & \mid & T*F \\

F & \to & a & \mid & (E)

\end{array}

$$

Now we can define the **PDA** $P(G)$ for $G$:

Where $\delta$ is given by:

$$

\begin{array}{rll}

\delta(q_{0},\epsilon,E) & = & \{ (q_{0},T),(q_{0},E+T) \} \\

\delta(q_{0},\epsilon,T) & = & \{ (q_{0},F),(q_{0},T*F) \} \\

\delta(q_{0},\epsilon,F) & = & \{ (q_{0},a),(q_{0},(E)) \} \\

\delta (q_{0},(,(\ ) & = & \{ (q_{0},\epsilon) \} \\

\delta(q_{0},),)\ ) & = & \{ (q_{0},\epsilon) \} \\

\delta(q_{0},a,a) & = & \{ (q_{0},\epsilon) \} \\

\delta(q_{0},+,+) & = & \{ (q_{0},\epsilon) \} \\

\delta(q_{0},*,*) & = & \{ (q_{0},\epsilon) \} \\

\delta(q,x,z) & = & \emptyset \ \ \text{everywhere else}

\end{array}

$$
![[Pasted image 20250430230903.png]]
	
MAKES SENSE.
# Questions
ID = (state, word, entire stack)
Transition function takes state, symbol, top of stack, returns state, and replacement for top of stack, if any. 
Transition drawn with symbol, top of stack, resulting stack symbol replacement, and resulting state. 



## IDs
S = E | ¬E
E = | T v E | T ∧ E | T
T = (E) | Z
Z = p | q | r, | true | false


G = (N, T, P, S)
	Where N = {S}
	T = {a,b}
	Where P consists of following productions
	S -> aSb | epsilon

P = (Q, Sigma, Gamma, delta, Q0, Z0

P(G) = (q0, T, N union T, delta, q0, S)
	= (q0, {a,b}, {a,b S}, delta, q0, S)
	delta(q0, epsilon, S) = (q0, aSb)
	delta(q0, epsilon, S) = (q0, epsilon)
	delta(q0, a, #) = (q0, epsilon)
	delta(q0, b, a) = (q0, epsilon)
	delta(q, x, gamma) = emptyset


how does PDA accept aabb

ID  = (q0, aabb, S) -> (q0, aabb, aSb) -> (q0, aabb, aaSbb) -> (q0, aabb, aaSbb) -> (q0, aabb, aabb) -> (q0, aabb, aabb) -> (q0, abb, abb) -> (q0, bb, bb)
-> (q0, b, b) -> (q0, epsilon, epsilon)


## CFG -> PDA
S = E | ¬E
E = | T v E | T ∧ E | T
T = (E) | Z
Z = p | q | r, | true | false


G = (N, T, P, S)
	Where N = {S}
	T = {a,b}
	Where P consists of following productions
	S -> aSb | epsilon

P = (Q, Sigma, Gamma, delta, Q0, Z0

P(G) = (q0, T, N union T, delta, q0, S)
	= (q0, {a,b}, {a,b S}, delta, q0, S)
	delta(q0, epsilon, S) = (q0, aSb)
	delta(q0, epsilon, S) = (q0, epsilon)
	delta(q0, a, #) = (q0, epsilon)
	delta(q0, b, a) = (q0, epsilon)
	delta(q, x, gamma) = emptyset


how does PDA accept aabb

ID  = (q0, aabb, S) -> (q0, aabb, aSb) -> (q0, aabb, aaSbb) -> (q0, aabb, aaSbb) -> (q0, aabb, aabb) -> (q0, aabb, aabb) -> (q0, abb, abb) -> (q0, bb, bb)
-> (q0, b, b) -> (q0, epsilon, epsilon)



