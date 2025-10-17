## Parsing
Second stage of compiler front end. Works with the program transformed by the scanner; sees stream of tokens, derives syntactic structure for the program, fitting the words into a grammatical model of the source programming language, often an **AST**. Parser reports problem and appropriate diagnostic information if the input stream is not a valid problem. Low-level implementation detail affects performance with regard to parsing.

Need a formal mechanism for specifying the syntax of the source language, and a systematic method of determining $w \in L(G)$. 

For a stream of words $s$ and a grammar $G$ we say that the parser building a constructive proof that $s$ can be derived in $G$ is called **parsing**. 
## Grammars - CFG
A formalism to define languages that are more general than regular expressions; that is there are more languages definable by CFGs than by regular expressions and finite automata. Defines the *context-free languages*.

### Definition
A context free grammar $G = (N, T, P, S)$ is given by:
- A finite set $N$ of *nonterminal symbols* or *nonterminals*
- A finite set $T$ of *terminal symbols* or *terminals*
- $N \cap T = \emptyset$
- A finite set $P$, $P \Subset N \times (N \cup T)^*$  of productions. A production ($A$ $a$) where $A \in N$ and $a \in (N \cup T)^*$ is a sequence of nonterminal and terminal symbols. It is written as $A → a$.
- $S \in N$ the distinguished start symbol. 
The terminals are the alphabet of the language.

Non terminals can be thought of as variables like digit in BNF, define recursively, often. Abstracts patterns/rules.

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


## Derivation
How can we check a word $w \in T^*$, $w \in L(G)$.
1. Start with start symbol $S$, which is a string in $(N \cup T)^*$ (specifically the string consisting of one nonterminal).
2. Apply productions, replace a nonterminal with a longer/shorter string of terminals/nonterminals given matching productions
3. Continue applying until string is made up entirely of terminals
4. Check if final string matches $w$, then $w \in L(G)$ 
Replacing leftmost non-terminal = **leftmost derivation.**
Replacing rightmost non-terminal = **rightmost derivation.** 

Often represent derivations via parse trees.

Given any grammar $G$ = $(N,T,P,S)$ we define the relation *directly derives in grammar G* as follows: $$\implies_G \ \Subset (N ∪ T)^∗ \ × \  (N ∪ T)^∗$$Relates pairs of strings that can go from one to the other using one derivation step, $(α,β)$ where we can turn $a$ into $\beta$  $$αAγ \implies G αβγ \iff A → β ∈ P$$ replace a nonterminal $A$ with $\beta$ , via derivation, means the production on the right is in P.

The relation derives in *Grammar G, derivation in 0 or more steps* is defined as:
![[Pasted image 20250413015404.png]]
where $n \in N$. Essentially saying can go from one string to another in zero or more steps, applying production rules. 
So: 
	if $\alpha$ = $\beta$, thats zero steps
	if $\alpha \implies g \ \beta$, thats one step
Reflexive, transitive closure of the direct derivation.

A string $a \in (N \cup T)^*$ such that $S \xRightarrow{*}_G \alpha$ is called a sentential form. The language of a grammar, $L(G) \Subset T*$ consists of all terminal sentential forms: 
$$ L(G) = \{w \in T^* \ |\ S \xRightarrow{*}_G w
\}$$
## Derivation Trees
A derivation in a context-free grammar induces a corresponding ***derivation tree*** that reflects the **structure** of the derivation: how each nonterminal was rewritten/replaced. As an example, consider the tree representation of the derivation of $a + (a * a)$ in grammar $G_{arith}$.

$G_{arith} = (\{E, T, F\}, \{(,), a, +, *\}, P, E)$
Where $P$ is given by: 
	$P =$
	$E → T | E + T$
	$T → F | T ∗ F$ 
	$F → a | (E)$

![[Pasted image 20250429194835.png]]
A tree is a **derivation tree*** for a CFG $G = (N, T, P, S) \iff$: 
1. Every node has a label from $N \cup T \cup \{\epsilon\}$ 
2. The label of the root node is $S$.
3. Labels of internal nodes belong to $N$.
4. If a node $n$ has a label $A$ and nodes $n_1, n_2,...,n_k$ are children of $n$, from left to right, with labels $X_1, X_2,...,X_k$, respectively, then $A \to X_1, X_2,...,X_k$ is a production in $P$ 
5. If a node $n$ has a label $\epsilon$, then $n$ is a leaf node and the **only child of its parent**.


## Grammars in the context of compilers
- Visible how a CFG $G$ can be used as a rewriting system to generate sentences that are in $L(G)$. 
- In contrast, a compiler must infer a derivation for a given input string, or determine that no such derivation exists. The process of constructing this derivation is called parsing. It is useful to visualise this derivation as a syntax tree, the root is simply $S$. 
- The leaves are also known; they must match, in order from left to right, the stream of words returned by the scanner. 
- The grammatical connection between the leaves and the root is yet to be discovered. 
- There exists two distinct and opposite approaches for constructing the tree:

1. ***Top-Down Parsers*** 
	A key insight makes TD parsing efficient: a large subset of CFGs can be parsed without backtracking. There are transformations that can convert an arbitrary grammar into one suitable for backtrack-free TD parsing. Two common distinct techniques for constructing top-down parsers: **hand-coded recursive-descent parsers** and **generated LL(1)** parsers.
2. ***Bottom-Up Parsers***

## Top-Down Parsers
- Begin with root, and grow the tree toward the leaves, at each step, select a node for some nonterminal on the lower fringe of the tree and extends it with a subtree that represents RHS of production that rewrites the nonterminal.
- Process continues until fringe of parse tree contains only terminal symbols, the input stream is exhausted, or clear mismatch between fringe of partially built parse tree and input stream. 
- If input stream exhausted, parser may have selected wrong production at some earlier step in the process, in which cause it can backtrack, sytematically reconsidering earlier decisions. For an input string that is a valid sentence in the source language, backtracking will lead the parser to a correct sequence of choices and let it construct a correct parse tree.
- The main portion of a TD parser consists of a loop that focuses on the leftmost unmatched symbol on partially built AST lower fringe.
	![[Pasted image 20251014211109.png]]
The implementation of backtrack can be relatively simple (though exhaustive and inefficient). Set focus to parent in partially built AST and disconnect children. If an untried rule remains with focus on the LHS, parser expands focus by that rule. Builds children for each symbol on RHS of production, pushes those symbols, and sets focus to point at first child. If no untried rule remains, the parser moves up to another level and tries again, when it runs out of possibilities it reports a syntax error and quits.

When backtracking, must also rewind input stream.


### Transforming a Grammar for Top-Down Parsing
Efficiency of top-down parser depends on its ability to pick correct production each time it expands nonterminal. If always make correct choice, top-down parsing is efficient. Worst case, parser does not terminate. 

#### Eliminating left-recursion
- A production like $A \to A \alpha \ | \ \beta$ leads to $A$ calling itself immediately with no token consumption $\to$ infinite recursion. Must rewrite productions to remove left recursion, as parser can loop indefinitely without generating leading terminal symbol to advance fringe search. This is standard - reformulating left-recursive grammar so that it uses right recursion instead.
##### Direct left recursion elimination
The translation from left recursion to right recursion is mechanical. For direct left recursion, we can rewrite the individual productions to use right recursion.

For all productions of the form $$ A\to A a_{1} \ | \ A a_{2} \ | \dots \ | \ \beta_{1} \ | \beta_{2} \ | \ $$
Where each $\alpha$ is a nonempty sequence of nonterminals and terminals and
each $\beta$ is a sequence of nonterminals and terminals that does not start with $A$.

Replace these with two sets of productions, one set for $A$:
$$A \to \beta_{1}A' \ | \ ... \ | \beta_m A'$$
and another set for the fresh non-terminal $A'$
$$A' \to \ | \ \alpha_1A '\ | \ a_nA' \ | \ \epsilon$$
Repeat this until no direct left recursion remains.

For example consider the ruleset:
$Expression \to Expression + Expression \ | \ Integer \ | \ String$ 

Could be rewritten to avoid recursion as
$Expression  \to Integer \ Expression'  \ | \ String  \ Expression'$
$Expression' \to +Expression  \ Expression'  \ |  \  \epsilon$ 

This simple transformation eliminates direct left recursion. Must also eliminate indirect left recursion, 
##### Indirect left recursion
Indirect left recursion occurs when the definition of left recursion is satisfied via several substitutions. It entails a set of rules following the pattern

$A_{0}→β_{0}A_{1}α_{0}$

$A_1→β_1A_2α_1$

$A_n→β_nA_0α_n$

where $β_0,β_1,…,β_n$ are sequences that can each yield the [empty string](https://en.wikipedia.org/wiki/Empty_string "Empty string"), while $α_0,α_1,…,α_n$ may be any sequences of terminal and nonterminal symbols at all. Note that these sequences may be empty. The derivation

$A_0⇒β_0A_1α_0⇒+A_1α_0⇒β_1A_2α_1α_0⇒+⋯⇒+A_0αn…α_1α_0$

then gives A0 as leftmost in its final sentential form.

We incorporate an additional step: forward substitution to convert indirect left recursion into direct left recursion, and then apply our previous mechanical transformation to produce the desired CFG. This algorithm assumes the original grammar has no cycles and no $e$ productions. 

![[Pasted image 20251014220222.png]]
The algorithm may be veiwed as establishing a topological ordering on nonterminals. Algorithm highly sensitive to nonterminal order.

![[Pasted image 20251014222502.png]]


#### Left-factoring
When two alternatives share a common prefix, a single lookahead token may not decide which alternative to choose, thus the grammar must be factored.

$A \to \alpha \ \beta_1 \ | \ \alpha \ \beta_2$ 
becomes
$A \to \alpha \ A'$
$A' \to \beta_1 \ | \ \beta_2$
Allows a predictive parsers (those which compute set of terminal symbols that can starts strings derived from each alternative, the **first set**, if there is a choice between two or more alternatives, insist the first sets are disjoint (a grammar restriction just implemented)) to consume $\alpha$ and then decide between $\beta_1, \beta_2$ using subsequent tokens, making such a choice deterministic with limited lookahead window.

### Backtrack-free parsing
Major source of ineffeciency in leftmost TD parsing arises from need for backtracking. If expand lower fringe with wrong production, encounters mismatch when reach AST leaves, which correspond to words returned by scanner. The act of expanding, retracting, re-expacting fringe of AST wastes time and effort.

Some parsers can enjoy a simple modification via the addition of a lookahead symbol, used to disambiguate all of the choices that arise in parsing the right-recursive expression grammar. Thus, we a say that a grammar is backtrack free with a lookahead of one symbol.

We can formalise the property that makes right-recursive expression backtrack free. At each point in the parse, the choice of an expansion is obvious because each alternative for the leftmost nonterminal leads to a distinct terminal symbol (we have left factoring to thank for ensuring that first sets of a production's RHS are disjoint.)


Define $First(\alpha)$ as set of terminals that can appear at the start of a sentence derived from $\alpha$. If $\alpha$ is terminal, $\epsilon$ or $eof$ then $First(\alpha)$ has exactly one member, $\alpha$. (include $\epsilon$ if can be derived) For a nonterminal $A$, $First(A)$ contains the complete set of leading terminal symbols of sentential forms derived from $A$.

Define $Follow(A)$ as the set of terminals that can appear immediately after nonterminal $A$ in some sentential form (or $\$$ if $A$ can appear at the end.)

Formally:

#### First and Follow
Develop the notion of first and follow sets. For a **CFG** $G$ = $(N, T, P, S)$:$$

\begin{array}{rll}

first(\alpha) & = & \{ a \in T\mid\alpha \Rightarrow_{G}^*a\beta \} \\

follow(A) & = & \{ a \in T \mid S \Rightarrow_{G}^*\alpha Aa\beta \}

 \cup \{ $ \ |S \Rightarrow_{G}^*\alpha A \}

\end{array}

$$
where $\alpha,\ \beta \in(N\cup T)^*,\ A \in N$, and where $\$$ is a special "***end of input***" marker.

**Follow set:** Follow set of some nonterminal $A$ consists of all terminals $a$ that can appear immediately after $A$ in some derivation from the start symbol. The End of input marker $\$ \in follow(A)$ if there exists a derivation where $A$ appears at the end of a string derived from $S$.


##### Worked example
if $A \to \beta C$
- If $\beta$ is a terminal, add $\beta$ directly to $First(A)$
- If $\beta$ is a nonterminal, add $First(\beta)$ to $First(A)$
if $C \to \beta A \delta$
- add $First(\delta)$ to $follow(A)$
- if $\delta$ can derive the empty string, add $Follow(\delta)$ to $Follow(A)$
if $C \to \beta A$
- add $Follow(C)$ to $Follow(A)$
![[Pasted image 20251015000946.png]]


#### Start sets
$Start(A \to B \ C)$ is the set of terminals that indicate that a production can be applied. 
If $A \to B \ C$:
	add $First(B)$ to start $(A \to B \ C)$
	if $B$ can derive the empty string, also add $First(C)$
	if all RHS can derive empty string, add $follow(A)$
Each alternative for a nonterminal has a separate start set.


### Hand-Coded Recursive Descent Parsers


### LL(1) Parsers
