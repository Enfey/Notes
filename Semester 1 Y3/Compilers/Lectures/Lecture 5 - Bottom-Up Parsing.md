
# Paradigm summary
> *Bottom Up parsers* begin with the leaves and grow the tree toward the root, applying productions in 'reverse' (this process is called a reduction - perform series of these to reach root). At each step, a bottom-up parser identifies a contiguous substring of the parse tree's upper fringe that matches the right hand side of some production; it then builds a node for the rules left-hand side and connects it into the tree.

## High-level description

To build a derivation from the lower fringe leaves, adds layers of nonterminals atop the leaves in a structure dictated by both the grammar and the partially completed portion of the parse tree.

To extend the frontier upward, looks in current frontier for a substring that matches the right-hand side of some production $A \to \beta$ . If it finds $\beta$ in the frontier, with its right end at $k$ , can replace $\beta$ with $A$, to create a new frontier.

A handle is a pair of a production rule and an index into a sentential form. 

$[A \to \beta ,k]$, means the first $k$ tokens of $\gamma_i$ form $\beta$ and can be replaced with $A$ in $\gamma_{i-1}$. (the part of the string you reduce at each reduction is the handle).
 
This replacement is called a **reduction**, because it reduces the number of symbols on the frontier, unless cardinality of $\beta$ is 1. If the parser is building a tree, it builds a node for $A$, adds that node to the tree, connects the nodes representing $\beta$ as $A's$ children.

Finding *handles* is the key issue that arises in bottom up parsing. 

The process of reductions after finding a handle repeats until either (1): the frontier has been reduced to a single node that that represents the grammar’s goal symbol, or (2) it cannot find a handle. In the first case, the parser has found a derivation; if it has also consumed all the words in the input stream (i.e. the next word is $eof$), then the parse succeeds. In the second case, the parser cannot build a derivation for the input stream and it should report that failure.

Bottom-up parser effectively discovers the steps of a derivation in reverse order. For a derivation: $$Goal = \gamma_0 \to \gamma_1 \to \gamma_2 \to \dots \to \gamma_{n-1} \to \gamma_n = sentence$$
The bottom up parser discovers $\gamma_i \to \gamma_{i+1}$ before it discovers $\gamma_{i-1} \to \gamma_i$. 

A bottom up parser scans the input left to right, but is building the derivation in reverse. Thus produces a **reversed rightmost derivation**.
	![[Pasted image 20251021201633.png]]
	
Informally, we will say that a language has the $LR(1)$ **property** if it can be parsed in a single left-to-right scan, to build a reverse-rightmost derivation, using only one symbol of lookahead to determine parsing actions.
## LR(1) parsing algorithm
The critical step in a bottom-up parser, such as a table-driven $LR(1)$ parser, is to find the next handle. Efficient handle finding is the key to efficient parsing!
An LR(1) parser uses a handle finding automaton encoded into two tables, called *Action* and *Goto*.

The skeleton LR(1) parser interprets the *Action* and *Goto* tables to find successive handles in the reverse rightmost derivation of the input string. When it finds a handle $[A \to \beta ,k]$ , reduces $\beta$ at $k$ to $A$ in the current sentential form($k$ is just the lookahead symbol used). 

Rather than build an explicit parse tree, skeleton parser keeps the current upper frontier of the partially constructed tree on a stack, interleaved with states from the handle-finding automaton that permit reductions. 

At any point in the parse, the stack contains a prefix of the current frontier, beyond this, the frontier consists of solely leaf nodes. 
The variable $word$ in the example holds the first word in the suffix that lies beyond the stack contents; the lookahead symbol.

To find the next handle, shift symbols onto the stack until automaton finds right end of a handle at stack top. Once it has a handle, the parser reduces by the production in the handle. To do so, it pops the symbols in $\beta$ from the stack and pushes the LHS, $A$ onto the stack. The *Action* and *GoTo* tables thread together shift and reduce actions in a grammar driven sequence, producing a reverse rightmost derivation.

![[Pasted image 20251021213521.png]]
![[Pasted image 20251021213638.png]]
![[Pasted image 20251021213653.png]]
LR(1) parsers take time proportional to the length of the input (one shift per word returned from scanner) and length of derivation (one reduce per step in derivation). 

![[Pasted image 20251021214424.png]]

## Tables and Closure
The key to LR parsing lies in the construction of the **Action** and **GoTo** tables. The tables encode all of the legal reduction sequences that can occur in a reversed rightmost derivation for the given grammar. 
Can do by hand, but very tedious! There is an algorithm that can be used. With $LR(1)$ parser generators, the compiler author's role is to define the grammar and ensure it preserves the $LR(1)$ property.

NOTE: The ideas that underlie $LR(1)$ parsers actually define a family of parsers that vary in amount of lookahead they use $LR(k)$.

### Table construction
#### LR(1) Items
An LR(1) item $[A \to \beta \cdot \gamma, a]$ consists of a production $A \to \beta , \gamma$; a placeholder, $\cdot$, that indicates the position of the parser's stacktop(already have $\beta$ on the stack, need to find $\gamma$ in this example) in the production's right hand side; and a specific terminal symbol/token as a lookahead symbol.

Building an LR(1) parser means finding all reachable items and grouping them into states. 

The **canonical collection** $\mathcal{CC} = \{\mathcal{cc_{0}}, \mathcal{cc_{1}},\mathcal{cc_{2}}\dots \mathcal{cc_{n}}\}$ is designated as the set of sets of $LR(1)$ items. Each set $\mathcal{cc_i}\in \mathcal{CC}$ represents a different state.

For a production $A \to \beta \gamma$ and a lookahead symbol $a$, the placeholder can generate three distinct items, each with own interpretation.

1. $[A \to \cdot \beta \gamma, a]$ indicates that an $A$ would be valid and recognising a $\beta$ next would be one step toward discovering an $A$. We call such an item a possibility, as it represents a possible completion.
2. $[A \to \beta \cdot \gamma, a]$ indicates the parser has progressed from the state $[A \to \cdot \beta \gamma, a]$ by recognising $\beta$ . A valid next step is to recognise $\gamma$ . We call such an item partially complete. 
3. $[A \to \beta \gamma \cdot, a]$ indicates the parser has found $\beta \gamma$ in a cotext where an $A$ followed by an $a$ would be valid. If the lookahead symbol is an $a$, then the item is a handle, and the parser can reduce $\beta \gamma$ to $A$. Such an item is complete.
![[Pasted image 20251021221507.png]]
![[Pasted image 20251021221500.png]]

To construct the canonical collection of sets from $LR(1)$ items, $\mathcal{CC}$, a parser generator must start from the parser's initial state, and construct a model of all the possible transitions that can occur. The algorithm represents each possible configuration, or state, of the parser as a set of $LR(1)$ items. Rely on 2 fundamental operations on these sets: taking a closure, and computing a transition. 

A closure of an item is all the other items that item implies. Give n some core set of $LR(1)$ items, it adds to that set any related $LR(1)$ items that they imply. For example, anywhere that $Goal \to List$ is legal, the productions that derive a $List$ are legal too. 
	Thus, the item $[Goal→• List,eof]$ implies both $[List→• List Pair,eof]$ and $[List→• Pair,eof]$, with respect to the **placeholder.** 
To build the closure of $[A \to \beta \cdot C \delta, a]$ , find all productions $C \to \gamma$ and for each symbol $s \in first(\delta a)$ (each terminal that can appear as initial symbol in $\delta a$), create item $[C \to \cdot \gamma, s]$. We use the closure to create $cc_0$, the initial item set for the grammar.

For the parentheses grammar, the initial item is $[Goal→• List,eof]$. Applying closure to that set adds the following items: $$[List → • List Pair,eof], [List → • List Pair,( ], [List → • Pair,eof ], [List → • Pair,( ], [Pair → • ( Pair ),eof ], [Pair → • ( Pair ),(], [Pair → • ( ),eof] [Pair → • ( ),(]$$

To model the transition the parser would make from a given state on some grammar symbol $x$, the algorithm computes the set of items that would result from recognising an $x$. To do so, the algorithm selects the subset of of the current **LR(1)** items where $•$ precedes $x$ and advances the $•$ past the $x$ in each of them. The GoTo procedure implements this function. 

#### GoTo Function
Takes as input a model of a parser state, represented as a set $\mathcal{CC_i}$ and a grammar symbol $x$ . From these, it computes a model of the parser state that would result from recognising an $x$ in state $i$. 

It iterates over the items in $cc_i$, when find an item in which $\cdot$ immediately precedes $x$, creates a new item moving $\cdot$ rightward past $x$ to complete the item. This item represents the parser config after recognising $x$. **GoTo** places these new items in a new set, takes its *closure* to complete the parser state, and returns that new state.
Repeatedly do this to compute canonical closure.


FINISH THIS OFF TOMORROW!