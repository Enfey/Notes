## Scanner
Converts stream of characters into a stream of meaningful lexical tokens via aggregation of these characters into words, which are then assigned a pre-defined syntactic categories. Those categories include identifiers, operators, language keywords etc, based on a lexical grammar.

This is the only pass in the compiler that manipulates every character of the input character. 

Automatic tools for scanner generation are common. These process a mathematical description of the languages lexical syntax, and produce a fast lexer.


More typically, according to wikipedia, lexing can be divided into 2 stages: scanning, which segments input string into syntactic units called *lexemes* and categorises these into token classes. Evaluating, which converts lexemes into processed values.

![[Pasted image 20251007232113.png]]

Scanning returns tokens - a lexical token is a string with an assigned and thus identified meaning. It consists of a token name, and an optional token value. The token name is the corresponding lexical category.

### Recogniser
The simplest explanation of an algorithm to recognize words is often a character-by-character formulation. This has a parallel with ***finite automaton.*** 
![[Pasted image 20251007231921.png]]
Refer to LAC, cyclic transitions, formal definition of finite automaton. 

The set of words accepted by a finite automaton $F$ forms a language denoted $L(F)$. This is not intuitive however. A specification that humans find more intuitive are regular expressions, which denote the same class of language as finite automaton, and can thus be used in their place. 

It is because of this, that regular expressions are used to specify the lexical grammar of a lexer. 
A lexical grammar thus consists of a list of regular expressions, and the token classes they correspond to. When a scanner recognises a string matching one of the regular expressions, it returns a token consisting of the matched lexeme, and the token name(the corresponding lexical category.)


Formally(and according to the reference book), a regular expression is built up from three basic operations:
	![[Pasted image 20251007232958.png]]
Although there is technically 7 as REs are defined inductively, this is ignored here.

Regular expressions vary depending on the regex engine being used - it is not a universal standard when it comes to software implementations.

The goal of working with finite automata is to automate the derivation of executable scanners from a collection of $REs$. Now develop the constructions that transform an $RE$ into an $FA$ that is suitable for a direct implementation, and an algorithm that derives an $RE$ for the language accepted by an $DFA$.

Via **Thompson's construction**, we can convert a regular expression to an $NFA$, then via the **subset construction** we can convert the $NFA$ to a $DFA$, now suitable for direct implementation. Hopcroft's algorithm may also be used to minimise a $DFA$. Kleene's construction derives an RE from a $DFA$


### Thompson's Construction
Method of transforming a regular expression into an equivalent $NFA$. This $NFA$ can be used to match strings against the regular expression.

The following rules are depicted:
- The empty word $ε$ is converted to:
	![[Pasted image 20251007234417.png]]
- A symbol $a$ of the input alphabet is converted to:
	![[Pasted image 20251007234437.png]]
- The union expression $s|t$ is converted to:
	- ![[Pasted image 20251007234514.png]]
	State q has a silent transition to the start state of either $N(s)$ or $N(t)$. Their final states become intermediate states of the whole $NFA$ and merge via two silent transitions into the final state of the $NFA.$
- The concatenation expression $st$ is converted to
	- ![[Pasted image 20251007234811.png]]
	The initial state of $N(s)$ is the initial state of the whole $NFA$. The final state of $N(s)$ becomes the initial state of $N(t)$. The final state of $N(t)$ is the final state of the whole NFA.
- The Kleene star expression $s^*$ is converted to:
	![[Pasted image 20251007234954.png]]
	An empty transition connects the initial state and the final state of the NFA with the sub-NFA N(s) in between, also connected via an empty transition. Another empty transition from the inner final to the inner initial state of $N(s)$ allows for repetition of expression $s$ according to the star operator.
		![[Pasted image 20251007235346.png]]

### Subset construction
Given an $NFA$ $A$ = $(Q,\Sigma, \delta, S, F)$ we construct the equivalent $DFA$: 

$D(A)$ = $(P(Q), \Sigma,\delta_{D(A)}, S, F_{D(A)})$
 - $P(Q)$ = power set of $Q$, each state in $DFA$ corresponds to a subset of states from $NFA$
- Input alphabet $\Sigma$
- The transition function for the $DFA$ $\delta_{D(A)}$
- The start state of the $DFA$
- The set of final states for the $DFA$
where:
	$\delta_{D(A)}(P, x) = \bigcup\{\delta(q,x) \ | \ q \in P\}$
		![[Pasted image 20250220225913.png]]
	$F_{D(A)} = \{P \in P(Q) \ | \ P \cap F \not = \emptyset\}$
Basic idea is to define $DFA$ whose states are sets of $NFA$ states. A set of possible $NFA$ states thus becomes a single $DFA$ state. A $DFA$ state is final if that set contains at least one final $NFA$ state. 
DO TRANSITION TABLE, ONLY PUT INITIAL STATE(S) IN FROM NFA, AND THEN DO REST OF TABLE, ADDING STATES TO LHS OF TABLE AS THEY APPEAR FROM CALCULATING INITIAL STATE.

WHEN HAVE MULTIPLE INITIAL STATES, ALL GO TOGETHER IN A SET OF STATES, AND START FROM THERE.



![[Pasted image 20251008000533.png]]

Must factor in the ε closure with regard to the transition function.
