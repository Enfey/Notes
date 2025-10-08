> 	***Pushdown automata**.* PDAs are finite automata with a stack, i.e., a data structure that can be used to store an arbitrary number of symbols (hence PDAs have an infinite set of states) but can only be accessed LIFO manner. Languages recognised by PDA are context-free languages. 

### Pushdown automaton
A pushdown automaton $P$ = ($Q, \Sigma, Γ, \delta, q_0, Z_0, F)$ is given by the following 7-tuple
- A finite set $Q$ of states
- A finite set $\Sigma$, alphabet of input symbols
- A finite set $Γ$ of stack symbols
- A transition function $\delta \in Q \times (\Sigma \cup \{\epsilon\}) \times Γ → \mathcal{P}_{fin}(Q \times Γ^*)$ 
	- $\mathcal{P}_{fin}(A)$ are the finite subsets of a set.
	- Essentially, given current state, next input symbol, top of stack symbol, returns finite set of possible moves including next state and replacement for top of stack
- An initial state $q_0$ 
- An initial stack symbol $Z_0$ 
- A set of final states $F \Subset Q$ 

To recognise all even length-palindromes over $\Sigma = \{0,1\}$: $L = \{w \in \{0,1\}^* \ | \ w = ww^R\}$ (where $w^R$ means reverse of the word).
We define the following pushdown automata:$$P_0 = (\{q_0, q_1, q_2\}, \{0,1\},\{0,1,\#\}, \delta, q_0, \#, \{q_2\}) $$
PDA compares the input with what is on the stack, and pushes and pops symbols respectively. If it reaches the end of the input according to the transition function and states, precisely when the stack is empty, the input word $w \in L$ . Guesses the midpoint and starts popping from there by just doing an empty transition. 

![[Pasted image 20250430205041.png]]
- at q0: Any symbol + any stack symbol = q0
- at q0 Empty word/transition + any stack symbol = q1(move here to guess when getting to middle)
- at q1 Symbol x + symbol x = q1 
- at q1 Empty word + empty stack symbol = q2, accept
![[Pasted image 20250430205502.png]]

We draw the transition diagram of $P$ by labelling each transition with a triple, $x, Z, \gamma$ with $x \in \Sigma, Z \in Γ, \gamma \in Γ^*$ (gamma represents resulting stack due to that transition.)
![[Pasted image 20250430210138.png]]

### How does the PDA work?
- At any time the state of the computation of a PDA $P$ = ($Q, \Sigma, Γ, \delta, q_0, Z_0, F)$ is given by:
	- The state $q \in Q$ that the PDA is in
	- The input string $w \in L^*$ that still has to be processed
	- The contents of the stack $\gamma \in Γ^*$ 
- Such a triple ($q, w, \gamma$) $\in Q \times \Sigma^* \times Γ^*$ is called an Instantaneous Description (ID). 
- We define a relation between IDs that describe how the PDA can change from one ID to the next one. Because PDAs in general are nondeterministic, this is a relation (not a function), may be more than one possibility for a given ID. 
	- $⊢_p \Subset ID \times ID$ 
	- There are 2 distinct possibilities for $⊢_p$ 
		1. $(q, xw, z\gamma) ⊢_p (q', w, a\gamma) \ if \ (q', a) \in \delta(q, x ,z)$
			- Reads an input symbol and consults transition function $\delta$ to calculate new possible state $q'$ and sequence of stack symbols $a$ that replaces the current symbol on top z.
		2. $(q, w, z\gamma) ⊢_p (q', w, a\gamma) \ if \ (q', a) \in \delta(q, \epsilon ,z)$
			- PDA ignores the input (doesn't consume as seen) and silently moves into a new state and modifies the stack as above, based on this silent transition defined in $\delta$.
- Consider the word 0110. What are possible sequences of IDs for : $$P_0 = (\{q_0, q_1, q_2\}, \{0,1\},\{0,1,\#\}, \delta, q_0, \#, \{q_2\}) $$
	Starting with $(q_0, 0110, \#)$
		![[Pasted image 20250430211830.png]]
		We write $(q, w, \gamma) \overset{*}{\vdash}_P (q', w', \gamma)$ if the PDA can move from $(q, w, \gamma)$ to $(q', w', \gamma)$ in 0 or move moves. Above, shown that:$$ (q_0, 0110, \#) \overset{*}{\vdash}_P (q_2, \epsilon, \epsilon)$$
		But this is not the only possible sequence of IDs for this input, PDA may guess the middle wrong for example. 
		![[Pasted image 20250430212204.png]]
		Moves state too early and will get stuck after the last transition mentioned in text above. 
### The language of a PDA
Two ways to define language of a PDA $P$ = ($Q, \Sigma, Γ, \delta, q_0, Z_0, F)$ as  both of which specify the same class of language as a PDA that uses a particular acceptance method can be changed to use the other. there are two notions of acceptance:

#### Acceptance by final state
$$L(P) = \{w \ | \ (q_0, w, Z_0)\overset{*}{\vdash}_P(q, \epsilon, \gamma) \wedge q \in F\}$$
That is the PDa accepts the word $w$ if there is any sequence of IDs starting initially from$(q_0, w, Z_0)$ and leading to $(q, \epsilon, \gamma)$, where $q \in F$ is one of the final states. The contents of the stack are irrelevant.

In the previous example, PDA $P_0$ would accept 0110 because $(q_0, 0110, \#) \overset{*}{\vdash}_P (q_2, \epsilon, \epsilon)$, where $q_2 \in F$. 
#### Acceptance by empty stack
$$ L(P) = \{w \ | \ (q_0, w, Z_0)\overset{*}{\vdash}_P(q, \epsilon, \epsilon)\}$$
That is the PDA accepts the word $w$ if there is any sequence of IDs starting from $(q_0, w, Z_0)$ and leading to $(q, \epsilon, \epsilon)$. The final state plays no role, but the stack must be empty for $w \in L(P)$ . 

Previous automaton also works. But we just leave out $F$ when specifying PDA and use acceptance by empty stack instead. 


### Deterministic PDAs
Introduced PDAs as nondeterministic machines, that may have several alternatives how to continue. Deterministic Pushdown Automata (DPDA) never have a choice. 
To be precise, say that a PDA $P$  = ($Q, \Sigma, Γ, \delta, q_0, Z_0, F)$ is deterministic iff $$|\delta(q, x, z)| + |\delta(q, \epsilon, z)| \le 1$$
Essentially, the $|\mathcal{P}_{fin}(Q \times Γ^*)|$ returned by $\delta$ must be $\le 1$ and that only one of the above transition functions is permitted a transition. If both, then exceeds one, and is not deterministic as there are multiple possible moves considering the empty transition. 
![[Pasted image 20250430220129.png]]
There is in general no way to translate nondeterministic PDA into a deterministic one. Indeed, there is no DPDA that recognises the language $L$!. **Nondeterministic PDAs are more powerful that deterministic PDAs.**
![[Pasted image 20250430220258.png]]
![[Pasted image 20250430220305.png]]
![[Pasted image 20250430220310.png]]
In contrast to PDAs in general, the two acceptance methods are not equivalent for DPDAs: acceptance by final state makes it possible to define a bigger class of languages. Consequently always use acceptance by final state for DPDAs in the following. 

### Context-free grammars and pushdown automata
***For a language $L \Subset \Sigma^*$ the following two statements are equivalent:***
	1. $L$ is given by a CFG G, $L = L(G).$
	2. $L$ is given by a PDA P, $L = L(P)$ 
To summarise, context free languages can be described by CFGs and processed by PDAs.
Here show how to construct PDA from a grammar.

Given a **CFG** $G = (V, \Sigma, P, S)$ we define a **PDA** $$

P(G)=(\{ q_{0} \},\Sigma,V\cup \Sigma,\delta,q_{0},S)

$$
(stack can have either terminals or non terminals, input is terminals, use only one state)

Where $\delta$ is defined as follows:

$$
\begin{array}{rll}

\delta(q_{0},\epsilon,A) & = & \{ (q_{0},\alpha)\mid A\to\alpha \in P \} & \text{for all }A \in N \\

\delta(q_{0},a,a) & = & \{ (q_{0},\epsilon) \} & \text{for all }a \in T

\end{array}$$

Here we do not give $F$ the set of **final states** because we use acceptance by **empty stack**.

Only use one state

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

$$

P(G)=(\{ q_{0} \},\{ (,),a,+,* \},\{ E,T,F,(,),a,+,* \},\delta,q_{0},E)

$$

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

$$![[Pasted image 20250430230903.png]]
Just keep reading this, makes enough sense.
![[Pasted image 20250430230950.png]]
