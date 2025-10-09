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
### Hopcroft's Algorithm
An algorithm which performs **$DFA$ Minimisation**. Transforms a given DFA into an equivalent DFA that has a minimum number of states. Two DFAs are equivalent if they recognise the same regular language.

Three states of classes that can be removed or merged from the original $DFA$ without affecting the language it accepts.
1. Unreachable States - states that are not reachable from the initial state of the DFA, for **any** input string. These states can be removed.
2. Dead states are the states from which no final state is reachable. These states can be removed unless the automaton is required to be complete (transition defined for every state.)
3. Nondistinguishable states - those that cannot be distinguished from one another for **any** input string. These states can be merged.

Hopcroft's Algorithm O(n log n)is concerned with merging the nondistinguishable states of a DFA and is based on partition refinement - partitioning the DFA states into groups by their behaviour. 


Initially, the set of final states and non-final states are placed into two separate partitions.
The algorithm repeatedly refines these partitions based on input symbols, until no finer refinement is possible.

Each final partition represents one state in the minimised DFA, representing equivalence classes whereby two states are equivalent if they have the same behaviour for every input sequence. That is for two states $q_1$ and $q_2$ that belong to the same block of partition $P$, and every input word $w$, the transitions determined by $\hat{\delta}$ with respect to $w$ should alaways take states $q_1$ and $q_2$ to either states that both accept or states that both reject. It should not be possible for $w$ to take $q_1$ to an accepting state and $q_2$ to a rejecting state and vice-versa.

Alternatively if $q_i \to_c q_x$ and $q_j \to_{c} q_y$ and $q_{i}, q_{j} \in p_{s}$, then $q_x, q_y$ must be in the same set $p_t$. This property holds for every set $p_s \in P$, for every pair of states. Thus the states in a given $p_s$ have the same behaviour with respect to input characters.

This is made concrete below:
![[Pasted image 20251009181709.png]]
The first one does not split, all initially in same set, all have transition defined for a, and lead to the same partition.
However in second case, $d_x$ is in a different partition, and will not have the same behaviour as $p_3$. Thus $p_1$ must be split, reflected on the RHS to account for this. Example below.

![[Pasted image 20251009182100.png]]

1. First examine the set $\{s3,s5\}$. Since neither state has an exiting transition, the state does not split on any character.
2. In the second step, it examines $\{s0,s1,s2,s4\}$; on the character $e$, it splits {s2,s4} out of the set. 
3. In the third step, it examines $\{s0,s1\}$ and splits it around the character f. 
4. At that point, the partition is $\{ \{s3,s5\}, \{s0\}, \{s1\}, \{s2,s4\} \}.$ The algorithm makes one final pass over the sets in the partition, splits none of them, and terminates.

![[Pasted image 20251009183612.png]]
![[Pasted image 20251009183729.png]]

### Limitations of Hopcroft
- In a scanner, different final states correspond to different lexical categories. If we merge final states, we will be unable to effectively classify lexemes.
- Solution is to simply not merge sets of final states when finishing the algorithm.


## Implementing Scanners
Scanner construction is a tool where the theory of formal languages has produced tools that can automate implementation. For most languages, a compiler writer can produce an acceptably fast scanner directly from a set of regular expressions. The compiler creates a regular expression for each syntactic category and gives the $REs$ as input to a scanner genertator. The generator constructs an $NFA$, then creates a corresponding $DFA$ and minimises the $DFA$. The scanner generator must convert the $DFA$ into executable code.

There are three common implementation strategies for scanners.  All of them operate in the same manner - by simulating the $DFA$. 
### Table-Driven Scanners


### Direct-coding


### Hand-Coded scanner