

> Formalism, define languages, more general, than regular expressions can define. More languages definable by CFGs than finite automaton and regular expressions.


# Definition of CFG
A CFG $G = (N, T, P, S)$ 
- Finite set of **non-terminal symbols**, $N$ 
- Finite set of **termina**l symbols, $T$ 
- $N \cap T =\emptyset$ 
- A finite set of productions, $P \ , P \Subset N \times (N \cup T)^*$ . A production, where $A \in N$ and $a \in (N \cup T)*$, (sequence of nonterminal and terminal symbols). Written as $A \to a$ 
- Distinguished start symbol, $S \in N$ 
Terminals are alphabet
Non terminals, can be thought of as variables, like digit in backus naurform, define recursively, often. Abstracts patterns/rules.

RHS of production may be empty $A \to \epsilon$ 

Permissible, same non terminal to occur on left and right of arrow in production. $A \to Aa$, called immediately left recursive. $A \to aA$, called immediately right recursive, rightmost position. 

Recursion may be indirect e.g., LHS non-terminal of production can be reached from RHS of one or more other productions. Complex, abstract relationships permitted. 

Example: 
$G_{arith} = (\{E, T, F\}, \{(,), a, +, *\}, P, E)$
Where $P$ is given by: 
	$P =$
	$E → T \ | \ E + T$
	$T → F \ | \ T ∗ F$
	$F → a \ | \ (E)$

# Meaning of CFGs
How to check, if a word $w \in T^*, \in L(G)$.
1. Start with start symbol, non-terminal
2. Apply productions, replace encountered non-terminals with longer/shorter string of terminals/nonterminals given matching productions.
3. Continue applying, until string is made up of entirely terminals, via all possible derivations
4. If final string(s) match $w$, $w \in L(G)$ 

Let us consider $G_{arith}$ , where the start symbol is $E \in (N \cup T)^*$. Here is one possible derivation. 

$G_{arith} = (\{E, T, F\}, \{(,), a, +, *\}, P, E)$
Where $P$ is given by: 
	$P =$
	$E → T \ | \ E + T$
	$T → F \ | \ T ∗ F$
	$F → a \ | \ (E)$

 E ->E + T 
		T + T
		F + T
		a + T
		a + F
		a + (E)
		a + (T)
		a + (T * F)
		a + (F * F)
		a + (a * F)
		a + (a * a)
1 possible derivation, called leftmost derivation, as always replaced leftmost non-terminal at each step. 


## Directly derives
Given Grammar $G$, define relation **directly derives**
$$ \implies_G \ \Subset (N ∪ T)^∗ \ × \  (N ∪ T)^∗$$
Relates pair of productions.
$$αAγ \implies G αβγ \iff A → β ∈ P$$
If a string (any string of symbols(nonterminals + terminals)) contains a nonterminal $A$, you can replace it with $\beta$ in one step, if and only if $A \to \beta$ $\in P$.


## Derives in 0 or more steps
Given Grammar $G$, define a relation **directly derives in 0 or more steps.**
$$ \implies_G^* \ \Subset (N ∪ T)^∗ \ × \  (N ∪ T)^∗$$
$$\alpha_0 \implies_G^* \ \ \alpha_n \iff \alpha_0 \implies_G \ ... \ \alpha_{n-1} \implies_G \ \alpha_n$$
Where $n \in \mathcal{N}$.
$a_0$ can get to $a_n$ iff there exists direct a sequence of directly derivations  from $a_0$ to $a_n$


## Sentential Form
A string $a \in (N \cup T)^*$ such that $S \xRightarrow{*}_G \alpha$ is called **sentential form**. The language of a grammar consists of all **TERMINAL SENTENTIAL FORMS**. (can go from start symbol, to that set of terminal symbols in 0 or more derivations).

# Language of  CFG
$$L(G) = \{w \in T^* \ | \ S \xRightarrow{*}_G w\}$$
For all words formed of terminals, if can go from start symbol, to that word then the word is in the language. 


# Relation between regular and context free languages
- A grammar in which each production has at most one non-terminal on RHS is **LINEAR**. For example:$$G_1 = (\{S\}, \{0,1\}, {S → \epsilon \ | \ 0S1}, S)$$
	Is a linear grammar
1. A grammar is ***left-linear*** if each RHS nonterminal is the leftmost symbol on the RHS. 
2. A grammar is ***right-linear***  if each RHS nonterminal is the rightmost symbol on the RHS
	![[Pasted image 20250413022227.png]]

Collectively, **left-linear** grammars and **right-linear** grammars, are regular languages, they directly describe regular languages. 
Easy to see, RIGHT LINEAR GRAMMARS Correspond directly to NFAs.

![[Pasted image 20250413022337.png]]

Thus CFG can be used, describe all regular languages. For example:
$a^*b^*$ can be translated into 
![[Pasted image 20250413023017.png]]

But also describe languages which are context free e.g., $\{0^n1^n|n \in N\}$, irregular, but can be described by following CFG:
![[Pasted image 20250521162830.png]]

# Derivation Trees
A derivation in CFG, induces corresponding ***derivation tree***, reflects **structure** of the derivation: how each non-terminal was replaced. Derivation tree for a + (a * a) for this CFG:

$G_{arith} = (\{E, T, F\}, \{(,), a, +, *\}, P, E)$
Where $P$ is given by: 
	$P =$  
	$E → T \ | \ E + T$ 
	$T → F \ | \ T ∗ F$ 
	$F → a \ | \ (E)$
![[Pasted image 20250429194835.png]]

A tree is a **derivation tree** for a $CFG G = (N, T, P, S)$ iff:
1. Every node has a label from $N \cup T \cup \{\epsilon\}$ 
2. The label of the root node is $S$ IMPORTANT
3. Labels of internal nodes belong to $N$ 
4. If a node $n$ has a label $A$ and nodes $n_1, n_2,...,n_k$ are children of $n$, from left to right, with labels $X_1, X_2,...,X_k$, then $A \to X_1, X_2, ... X_K$ are productions in $P$.
5. If a node $n$ has a label $\epsilon$ then $n$ is a leaf node and the **only child of its parent.** 


Through the notion of **yield** of derivation tree, relationship between a **derivation tree** and and **corresponding derivations** can be **made precise.** 
	- The string of **leaf labels** read from left to right removing any $\epsilon$ (unless on its own), = the yield
		- Leaf nodes may be labelled with either terminal or nonterminal symbols
	- For a CFG G = $(N, T, P, S)$, a string $a \in (N \cup T)*$ is the yield of some derivation tree iff $S \xRightarrow{*}_G \alpha$ 
		- Given that this of sentential form, the string may not necessarily be in $L(G)$, as not made up of purely terminals. 


## Examples
![[Pasted image 20250429200332.png]]

![[Pasted image 20250429200348.png]]

![[Pasted image 20250429200401.png]]
# Ambiguity
A CFG G = (N, T, P, S) is ambiguous iff there is at least one word $w \in L(G)$ such that there are:
- Two different derivation trees OR
- Two different leftmost derivations OR
- Two different rightmost derivations.
More than one way to interpret word, semantic ambiguity. 

![[Pasted image 20250429200719.png]]

![[Pasted image 20250429200812.png]]

![[Pasted image 20250429200837.png]]

Also easy to see, one-to-one correspondence between derivation tree and leftmost and rightmost derivation.
![[Pasted image 20250429201215.png]]

1 word, 2 leftmost derivations, corresponding to derivation trees, leads to semantic ambiguity: Want to assign meaning beyond word itself, e.g., result of evaluating expression. First tree suggests should be read as 1+(2 * 3) and the second suggests should be read as (1+2) * 3 which evaluates to 9, On word, 2 different interpretations. 
Also difficult to parse, depending on method, efficient grammar = efficient parser, less decisions. 
# Applications of CFGs
- Specifying syntax of programming languages and languages where one must derive meaning of words.

# QUESTIONS
All in book.





