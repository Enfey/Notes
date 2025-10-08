A **turing machine*** is a generalisation of a **PDA** that uses a tape instead of a stack. They are an abstract version of a computer: they have been used to defineformally what is *computable.*

Turing machines behave differently from the previous machine classes seen: they may run forever, without stopping. TO say a language is accepted by a Turing machine means the TM will stop in an accepting state for each $w \in L$. If the word is not in the language, the Turing machine may stop in a non-accepting state or loop forever. In this case, we can never be sure whether the given word is in the language i.e., the TM doesn't decide the word problem.

We say that a language is ***recursive***(or ***decidable***) if there is a TM that can determine, for any word, whether the word belongs to the language or not. There *type-0* languages that are not recursive, the most famous one is the halting problem.

There is no type of grammars that capture all recursive languages, but there is a subset of recursive languages, called the *context-sensitive languages* that can be characterised by context-sensitive grammars.
These are grammars where the LHS of a production is always shorter than the RHS?

## Turing machine definition
A **Turing machine** $M = (Q, \Sigma, \Gamma, \delta, q_0, B, F)$ is:
- A finite set of states, $Q$ 
- A finite set of symbols, $\Sigma$ 
- A finite set of tape symbols $\Gamma$, $\Gamma \Subset \Sigma$, this is the case because we also use the tape for input
- A transition function - $\delta \in Q \times \Gamma \to \{stop\} \cup Q \times \Gamma \times \{L, R\}$ 
	Defines how machine behaves if in state q and the symbol on the tape is $x$.
		If $\delta(q,x)$ = stop then machine stops
		Otheriwse if $\delta(q,x) = (q', y, d)$ the machine gets into state $q'$, **writes $y$ on the tape**(replacing $x$) and moves left if $D = L$, or right if $D = R$
- An initial state $q_0 \in Q$ 
- The blank symbol $B, B \in \Gamma$ but $B \notin \Sigma$
- A final set of states $F \Subset Q$ 
The above defines deterministic turing machines; for nondeterministic TMs the type of the transition function is changed to:$$\delta \in Q \times \Gamma \to \mathcal{P} (Q \times \Gamma \times \{L, R\})$$
The empty set being returned from this plays the role of stop. There is no difference in the strength of deterministic or nondeterministic TMs.

As for PDAs, we define instantaneous descriptions for Turing machines: ID = $\Gamma^* \times Q \times \Gamma^*$. An element $(\gamma_L, q, \gamma_R) \in ID$ describes a situation where the TM is in state Q, the non-blank portion of the tape on the left of the head is $\gamma_L$ and the non-blank portion of the tape on the right, including the square under the head is $\gamma_R$ .
We define the next-state relation $\vdash_M$ similarly to PDAs.
![[Pasted image 20250504015851.png]]
Cases 3 to 6 are only needed to deal with situation of having reached the end of non-blank part of the tape.

We say that a **TM** $M$ *accepts* a word if it goes into an accepting state i.e., the language of a **TM** is defined as$$L(M) = \{w \in \Sigma^* \ | \ (\epsilon, q_0, w) \vdash^{*}_M (\gamma_L, q', \gamma_R) \wedge q' \in F\}$$
	$(\epsilon, q_0, w)$ initial ID where head is at leftmost position of $w$. 
The TM automatically stops if goes into accepting state, but may also stop in non-accepting state if $\delta$ returns stop or $\emptyset$.



WE SAY THAT A GRAMMAR IS CONTEXT-SENSITIVE IF THE RHS of a production is at least as long as the LHS. May be multiple symbols on LHS of the production
![[Pasted image 20250504022400.png]]
![[Pasted image 20250504022759.png]]
![[Pasted image 20250504022824.png]]
![[Pasted image 20250504023312.png]]

