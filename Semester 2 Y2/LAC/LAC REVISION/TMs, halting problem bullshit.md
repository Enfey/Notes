
TM, generalistation, PDA, uses tape instead of stack. Formally defines what is  computable. May run forever. TO say l==anguage is accepted byd by Turing machine, means TM stops in accepting state for each $w \in L$.== I==f not in lang, turing machine stop sin non-accepting state, or loop forever. Can never be sure whether word is in the language.==

We say that a language is ==decidable==, if there is a ==TM== that can ==determine,== for ==any word,== whether the ==word belongs to the language or not.== Halting problem, famous undecidable language.

==No type of grammars== that ==capture all decidable languages==, but there is subset of recursive langs called context sensitive languages that can be characterised by CSG.

# TM definition
A **Turing machine** $M = (Q, \Sigma, \Gamma, \delta, q_0, B, F)$ is:
- A finite set of states, $Q$ 
- A finite set of symbols, $\Sigma$ 
- A finite set of tape symbols $\Gamma$, $\Gamma \Subset \Sigma$, this is the case because we also use the tape for input
- A transition function - $\delta \in Q \times \Gamma \to \{stop\} \cup Q \times \Gamma \times \{L, R\}$ 
	Defines how machine behaves if in state q and the symbol on the tape is $x$.
		If $\delta(q,x)$ = stop then machine stops
		Otherwise if $\delta(q,x) = (q', y, d)$ the machine gets into state $q'$, **writes $y$ on the tape**(replacing $x$) and moves left if $D = L$, or right if $D = R$
- Initial state $q_0 \in Q$ 
- The blank symbol $B, B\in \Gamma,$ but $B \notin \Sigma$ 
- Finite set of final states $F \Subset Q$ 

The above defines deterministic turing machines; for nondeterministic TMs the type of the transition function is changed to:$$\delta \in Q \times \Gamma \to \mathcal{P} (Q \times \Gamma \times \{L, R\})$$
The empty set being returned from this plays the role of stop. There is no difference in the strength of deterministic or nondeterministic TMs.


Define, IDS. 

ID = $\Gamma^* \times Q \times \Gamma^*$. $(\gamma_L, q, \gamma_R) \in ID$, describes situation where in state Q, the non-blank portion of tape on the left of the head is $\gamma_L$ and the non-blank portion of the tap on the right, incl square under the head is $\gamma_R$. 
![[Pasted image 20250504015851.png]]

We say that a **TM** $M$ *accepts* a word if it goes into an accepting state i.e., the language of a **TM** is defined as$$L(M) = \{w \in \Sigma^* \ | \ (\epsilon, q_0, w) \vdash^{*}_M (\gamma_L, q', \gamma_R) \wedge q' \in F\}$$


# Halting problem
Given a $TM$ $M$ and an input $w$, does $M$ halt on $w$.
Theorem - the halting problem is undecidable - no TM can decide for all (M, w) whether M halts on w.
![[Pasted image 20250522020124.png]]
RECOGNISABLE, NOT DECIDABLE

![[Pasted image 20250522020056.png]]

# Chomsky hiearchy
## Type 0
**Grammar** - Unrestricted grammar
**Automaton** - Turing machine
**Languages** - Recursively Enumerable languages (decidable)
### Type 1
**Grammar**  - Context-sensitive grammar
**Automaton** - Linear bounded TM
**Languages** - Context-sensitive languages
#### Type 2
**Grammar** - Context-Free Grammar
**Automaton** - Pushdown Automaton
**Languages** - Context Free languages
#### Type 3
**Grammar** Regular Grammar
**Automaton** Finite automaton
**Languages** Regular languages

Each level is a strict superset of the next. E.g., type 0 methods can be used to recognise all the other 3. 
Initially introduced as a classification of grammars, the relation to automaton realised later. 
![[Pasted image 20250522015350.png]]

![[Pasted image 20250522015644.png]]





==Decidable languages==, those for which a ==TM exist==s that ==halts **on every input**== and ==accepts exactly those words $w \in L$== 
==L is recognisable==, if there ==exists a TM== that ==accepts words in L==, and ==may not halt== for ==strings not in L.==
A ==language is undecidable== if there is ==no TM== which ==can decide language membership==. It ==may be recognisable.==

# ![[Pasted image 20250522015806.png]]
