> A formalism to define languages that are more general than regular expressions; that is there are more languages definable by CFGs than by regular expressions and finite automata. Defines the *context-free languages*.

## Definition
A context free grammar $G = (N, T, P, S)$ is given by:
- A finite set $N$ of *nonterminal symbols* or *nonterminals*
- A finite set $T$ of *terminal symbols* or *terminals*
- $N \cap T = \emptyset$
- A finite set $P$, $P \Subset N \times (N \cup T)^*$  of productions. A production ($A$ $a$) where $A \in N$ and $a \in (N \cup T)^*$ is a sequence of nonterminal and terminal symbols. It is written as $A → a$.
- $S \in N$ the distinguished start symbol. 
The terminals are the alphabet of the language. 

Non terminals can be thought of in variables, e.g., like digit in backus naur form, abstracts patterns/structures in the language. Never appear in $T$, only appear in production rules. 

Right hand side of a production may be empty, known as $\epsilon$ production, written $A →\epsilon$ 

Permissible for same nonterminal to occur both on left and right of the arrow in a production. A production for a nonterminal $A$ where the same nonterminal is the first symbol of the right-hand side, in the leftmost position, is called immediately *left-recursive*. $A → Aa$ 

A production for a nonterminal A where the same non-terminal is the last symbol of the right-hand side, in the rightmost position is called immediately right-recursive $A → aA$ 

Recursion may also be indirect: the left-hand side non-terminal of a production can be reached again from the right-hand side of one or more other productions.

Example: 
$G_{arith} = (\{E, T, F\}, \{(,), a, +, *\}, P, E)$
Where $P$ is given by: 
	P =  
	E → T | E + T 
	T → F | T ∗ F 
	F → a | (E)
### The meaning of CFGs
How can we check if a word $w \in T^*$, a target made only of terminals is in the language of a grammar?
1. Start with start symbol S, which is a string in $(N \cup T)^*$ (specifically the string consisting of one nonterminal)
2. Apply productions, replace a nonterminal in the with a longer/shorter string of terminals/nonterminals given matching productions
3. Continue applying until string is made up entirely of terminals
4. Check if final string matches $w$, then $w$ is in the language
Let us consider $G_{arith}$ , where the start symbol is $E \in (N \cup T)^*$. Here is one possible derivation. 
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
Called leftmost derivation, as always replaced leftmost non-terminal, but can choose to replace any nonterminal. 
Given any grammar $G$ = $(N,T,P,S)$ we define the relation *directly derives in grammar G* as follows: $$\implies_G \ \Subset (N ∪ T)^∗ \ × \  (N ∪ T)^∗$$Relates pairs of strings that can go from one to the other using one derivation step, $(α,β)$ where we can turn $a$ into $\beta$  $$αAγ \implies G αβγ \iff A → β ∈ P$$ replace a nonterminal $A$ with $\beta$ , via derivation, means the production on the right is in P.

The relation derives in *Grammar G, derivation in 0 or more steps* is defined as:
![[Pasted image 20250413015404.png]]
where $n \in N$. Essentially saying can go from one string to another in zero or more steps, applying production rules. 
So: 
	if $\alpha$ = $\beta$, thats zero steps
	if $\alpha \implies g \ \beta$, thats one step
Reflexive, transitive closure of the direct derivation.
![[Pasted image 20250413020710.png]]

A string $a \in (N \cup T)^*$ such that $S \xRightarrow{*}_G \alpha$ is called a sentential form. The language of a grammar, $L(G) \Subset T*$ consists of all terminal sentential forms: 
$$ L(G) = \{w \in T^* \ |\ S \xRightarrow{*}_G w
\}$$
### The relation between regular and context-free languages
A grammar in which each production has at most one non-terminal symbol in its right-hand side is linear. For example: 
$$G_1 = (\{S\}, \{0,1\}, {S → \epsilon \ | \ 0S1}, S)$$
is a linear grammar. There are two special cases of linear grammars:
1.  A linear grammar is ***left-linear*** if each right-hand side nonterminal is in the leftmost symbol on the right-hand side
2. A linear grammar is ***right-linear*** if each right-hand side nonterminal is the rightmost symbol on the right-hand side:
- ![[Pasted image 20250413022227.png]]
Collectively, left-linear and right-linear grammars are called regular languages are called because the languages they describe are regular. It is easy to see how right-linear grammars correspond directly to NFAs.
![[Pasted image 20250413022337.png]]
Thus context-free grammars can be used to describe at least some regular languages. However some of these languages shown to be regular are context-free. E.g., $G_1$ above describes the language $\{0^n1^n | n \in N\}$, regularity disproven via the pumping lemma. 
etc etc![[Pasted image 20250413022702.png]]
So what is the relation between regular and context-free languages? The answer is that all regular languages are context-free and can be defined by context free grammars. For example $a^*b^*$  can be translated into:
![[Pasted image 20250413023017.png]]

### Derivation trees
A derivation in a context-free grammar induces a corresponding ***derivation tree*** that reflects the **structure** of the derivation: how each nonterminal was rewritten/replaced. As an example, consider the tree representation of the derivation of a + (a * a) in grammar $G_{arith}$.

$G_{arith} = (\{E, T, F\}, \{(,), a, +, *\}, P, E)$
Where $P$ is given by: 
	P =  
	E → T | E + T 
	T → F | T ∗ F 
	F → a | (E)
	
![[Pasted image 20250429194835.png]]
A tree is a **derivation tree*** for a CFG G = (N, T, P, S) iff: 
1. Every node has a label from $N \cup T \cup \{\epsilon\}$ 
2. The label of the root node is $S$.
3. Labels of internal nodes belong to $N$.
4. If a node $n$ has a label $A$ and nodes $n_1, n_2,...,n_k$ are children of $n$, from left to right, with labels $X_1, X_2,...,X_k$, respectively, then $A -> X_1, X_2,...,X_k$ is a production in $P$ 
5. If a node $n$ has a label $\epsilon$, then $n$ is a leaf node and the **only child of its parent**.
Through the notion of the *yield* of a derivation tree, the relationship between a derivation tree and corresponding derivations can be made precise:
- The string of ***leaf labels*** read left to right, removing any $\epsilon$ bar one if it is the only remaining symbol, constitute the **yield.**
	- Leaf nodes may be labelled with either terminal or nonterminal symbols(THEY ARE NOT PARENTS)
- For a **CFG G** = $(N, T, P, S)$, a string $a \in (N \cup T)^*$ is the yield of some derivation tree iff $S \xRightarrow{*}_G \alpha$
	- Given that this yield is of sentential form:
	- ![[Pasted image 20250429200201.png]]
	- The string may not necessarily be a word in the language of the context free grammar. 
![[Pasted image 20250429200332.png]]
![[Pasted image 20250429200348.png]]
![[Pasted image 20250429200401.png]]

### Ambiguity
A **CFG G** = $(N, T, P, S)$ is ***ambiguous*** *iff* there is at least one word $w \in L(G)$ such that there are:
- Two different **derivation trees**, or equivalently
- Two different **leftmost derivations,** or equivalently
- Two different **rightmost derivations**
for $w$. This entails there are more than one way to interpret a word i.e., it leads to semantic ambiguity.
![[Pasted image 20250429200719.png]]
![[Pasted image 20250429200812.png]]
![[Pasted image 20250429200837.png]]
As per the definition of ***ambiguity***, another way to demonstrate ambiguity is to find two **leftmost** or two **rightmost** derivations for a word. It is easy to see there is a **one-to-one correspondence** between a **derivation tree** and a **leftmost** and **rightmost** derivation. Here are the leftmost derivation steps that correspond to the first, and second trees respectively. (Replace left non-terminal in each step).
![[Pasted image 20250429201215.png]]
As there is one word, and 2 different leftmost derivations, we say that the **grammar is *ambiguous***.
There are multiple reasons ambiguity is problematic:
1. Semantic ambiguity: Suppose we wish to assign a meaning to a word beyond the word itself. The meaning might be the result of evaluating the expression in our case. Then the first tree suggests the expression should be read as 1+(2 * 3), which evaluates to 7, while the second tree suggests the expression should be read as (1 + 2) * 3 which evaluates to 2. One word, with 2 different interpretations, leave the meaning ambiguous.
2. Parsing: Many methods for parsing do not work for ambiguous grammars, in particular, applies to efficient methods for parsing. But it is often possible to change an ambiguous grammar into an unambiguous one that is equivalent.
	 ![[Pasted image 20250429213419.png]]
	Note this tree corresponds to the reading 1 + (2 * 3), where multiplication has higher precedence than addition.
	To interpret as (1 + 2) * 3, must use explicit parenthesis
	![[Pasted image 20250429213617.png]]
	Only derivation tree for this word.

### Applications of context free grammars
- Specifying syntax of programming languages and languages where one must derive meaning of words.
- ![[Pasted image 20250429215211.png]]

## EXERCISES
- todo
- ![[Pasted image 20250429221049.png]]
- ![[Pasted image 20250429221056.png]]
- 