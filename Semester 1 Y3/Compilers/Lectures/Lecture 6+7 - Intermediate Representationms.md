> An IR is a machine-agnostic representation placed between front-end(parsing+semantics) and back-end(codegen).

A well formed IR permits one to:
- Apply optimisations cleanly (e.g., constant folding, dead-code elimination)
- Separate language concerns from machine concerns and prep for linking
- Make retargeting easier: many backends can consume the same IR. 

## A Taxonomy of IRs
1. **Graphical IRs** - encode the compiler's knowledge in a graph. The algorithms are expressed in terms of graphical objects: nodes, edges, lists, or trees. Provides abstract view of code and provides way to trivially navigate logically connected components. 
2. **Linear IRs** - resemble pseudocode for some abstract machine, and impose a clear, useful ordering on operations. Often compact and human readable. The algorithms iterate over simple, linear sequences of operations. 
3. **Hybrid IRs** - combine elements of both graphical and linear IRs.
The structural organisation of an IR has a strong impact on how the compiler writer will consider analysis, optimisation, and code generation. E.g., treelike IRs lead naturally to passes structured as some form of treewalk. Similarly, linear IRs lead naturally to passes that iterate over the operations in order.

Can vary in degree of abstraction offered. 

### Graphical IRs
#### Abstract Syntax Tree
The **Abstract Syntax Tree** is a concise, near-source-level representation that retains the essential structure of the parse tree but omits most nodes for nonterminal symbols. Due to close correspondence to parse tree, can build AST directly following syntax analysis.
![[Pasted image 20251022113659.png]]
##### Construction
The parse tree derived from syntax analysis will have nodes omitted for many nonterminals nodes. This corresponds to the AST - chuck away extraneous nodes that serve no purpose in remainder of compiler. 
An Ad Hoc Syntax-Directed Translation scheme fits neatly into the task of building an AST(covered next lecture).
#### Directed Acyclic Graphs
A **Directed Acyclic Graph** is a contraction of the AST achieved through sharing; identical subtrees are instantiated once, potentially having multiple parents, making a DAG more compact than a corresponding AST. DAGs are often employed to expose redundancies. (Acyclic, never form closed loop)
![[Pasted image 20251022114017.png]]
Essentially an optimisation over AST, particularly regarding literal subexpressions. Can get more complex with expressions with assignments as must prove that the value cannot change e.g., final. 
##### Construction 
DAG generally constructed from AST by leveraging sharing; identical subtrees are instantiated only once, potentially having multiple parents. Pretty straightforward.

### Linear IRs
![[Pasted image 20251022170122.png]]
#### Stack Machine IR
Stack-machine code is a form of **one address** (uses single explicit memory address for its operation) code that utilises an implicit stack for operands. Most operations consume operands from the stack and push the result of their computation back onto it. Highly compact, Java's Bytecode is similar in nature to stack-machine code.

![[Pasted image 20251022170053.png]]

#### Three-address code IR
Models a machine where most operations take two operands and produce a result, typically written in the form $i ←j \ op \ k$ . Format is attractive as it is reasonably compact, and the distinct names for operands and results grant the compiler freedom to control the reuse of names and values. Three-address codes are often implemented as a sequence of quadruples, which are records with four fields: an operator, two sources, and a destination. 
![[Pasted image 20251022170100.png]]

### Hybrid IR

#### Control Flow Graphs
A Control Flow-Graph is a directed graph $G = (N, E)$ that models the flow of control between basic blocks in a program. Each $n \in N$ represents a **basic block**, which is **a maximal length sequence of guaranteed branch-free/in-order code.** Each edge $e = (n_i, n_j) \in E$ corresponds to a possible transfer of control from block $n_i$ to block $n_j$. The CFG is fundamental for control flow analysis and global optimisation, dead code elimitation, global register allocation etc. CFGs often constructed from a linear IR.  

![[Pasted image 20251022115054.png]]

##### Construction and use case
Often constructed from linear IR, which requires two main steps:
1. Finding leaders and defining basic blocks
	The initial operation of a block is called a **leader**. An operation is a leader if:
		It has the first operation in the procedure
		It has a label that is potentially the target of some branch
	
2. Adding edges
### Symbol Table
Central repository for facts about names and values, integral component of compiler's IR. Maps a name into various annotations including type, size, address information needed for code generation. The symbol table essentially coalesces information derived from different parts of the source code, providing central access and simplifying the code required to reference declarations. 

For languages with nested lexical scopes, symbol table may be implemented as a linked list or stack of tables, where each table represents a different scope. When searching for an identifier, check current scope first, if not there, proceed to previous table in chain, creating hierarchal search; aids further in error detection as can also determine invalid scope from this. Each entry in the hash table is a $n-ary-record$. 

Referenced very frequently during linking and code generation e.g., alignment, code relocation, symbol resolution. Hash table implementations offer amortised $O(1)$ lookup. Tree-based tables guarantee $O(log n)$ lookup. Table elements should try to be compact as possible, though this is a lesser concern.

![[Pasted image 20251022160102.png]]

### Namespaces
Container that holds set of identifiers and allows compiler to distinguish between identical names in different contexts. Each procedure typically creates a new, protected name space, insulating local declarations from the surrounding context. Typically during parsing/CSA, scope is recorded.

Lexical scope, scopes nest in the order they are encountered in the program text. A name refers to the definition that is lexically closest to its use - the definition in the closest surrounding scope. 

Language specific scopes, C defines multiple levels: global, local(procedure-level), block-level({}), and file level for static names. Fortran uses single global scope, along with a local scope for each procedure. 

Name resolution in OOLs involve searching the lexical scopes, then the class hierarchy, and finally the global name space.

Via a sheaf of tables, where a new symbol table is created upon entering each new lexical scope, when a reference to a name occurs, must search sequentially and access each table starting at current scope and moving outward to lower nesting levels, until most recent declaration is found. The result of this search is the name's static coordinate, a pair $[l,o]$ where $l$ is the lexical nesting level where the name is declared and $o$ is the offset within the data area for that level. 
### Static Single Assignment
Widely used IR and naming discipline employed to encode detailed info about **data flow** and **control flow**.

In SSA form, every name is defined by exactly one operation. This is achieved by renaming original variables with numerical subscripts e.g., $x_0, x_1...x_n$ such that each new name corresponds to a unique definition - each operation stores its result in a new address(unsure whether this will actually pass over to to symbol table, and to linking and code gen). 

$\phi$ functions preserve the single definition rule.

The primary usefulness of SSA comes from how it simultaneously simplifies and improves the results of a variety of compiler optimisations by simplifying the properties of variables. For example, consider this piece of code:

y := 1
y := 2
x := y
In SSA form, would not have to perform *reaching definition analysi*s to determine whether the first assignment is necessary, immediate in SSA form.

To convert to SSA form, simply a matter of replacing the target of each assignment with a new variable, and replacing each use of a variable with the 'version' of the variable **reaching** that point. 

A definition d is a reaching definition for a point p if theres a path from d to p where d is not overwritten along that path.
![[Pasted image 20251022163345.png]]

![[Pasted image 20251022163132.png]]
![[Pasted image 20251022163138.png]]
Clear to see which version of which definition each use is referring to, except in the bottom basic block, could be either $y_1$ or $y_2$ depending on which path the control flow took. 

To resolve this, a statement is inserted in the last block, called a $\phi$ *function*. This statement will generate a new definition of $y$ called $y_3$ by 'choosing' (essentially executing concurrently) either $y_1$ or $y_2$.

Since processors do not execute $\phi$ functions, code must be translated back into an executable form, generally involves replacing each $\phi$ function with a sequence of copy operations inserted along appropriate incoming edges to convergent basic block. Sure!
#### Construction overview
##### Dominance Frontiers
In a control-flow graph, a node $A$ is said to **strictly dominate** a different node $B$ if it is impossible to reach $B$ without passing through $A$ first. $A$ is said to dominate $B$ if $A$ strictly dominates $B$ or $A=B$.

A node which transfers control to a node $A$ is called an **immediate predecessor** of $A$.

The **dominance frontier** of a node $A$ is the set of nodes $B$ where $A$ *does not* strictly dominate $B$, but does dominate some immediate predecessor of $B$.

```python
[1] x = random()
if x < 0.5
    [2] result = "heads"
else
    [3] result = "tails"
end
[4] print(result)
```
Node $1$ strictly dominates $2, 3, 4$. The immediate predecessors of node $4$ are $2$ and $3$. 

Dominance frontiers define the points at which $\phi$ functions are needed, if can calculate, then can insert $\phi$ functions.

##### Variable renaming
Variables are systematically renamed using a DFS of the dominator tree. Algorithm uses a stack and counter for each base name to track most recently defined SSA name, ensuring that every use refers to single definition.

## Call Graph 
Graphical representation used for interprocedural analysis and optimisation. Control-flow graph representing calling relationships between procedures. Each node represents a procedure and each edge $f, g$ indicates that procedure $f$ calls procedure $g$. Thus a cycle indicates recursive calls. 