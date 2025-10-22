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
##### Construction and use case
#### Directed Acyclic Graphs
A **Directed Acyclic Graph** is a contraction of the AST achieved through sharing; identical subtrees are instantiated once, potentially having multiple parents, making a DAG more compact than a corresponding AST. DAGs are often employed to expose redundancies. (Acyclic, never form closed loop)
![[Pasted image 20251022114017.png]]
Essentially an optimisation over AST, particularly regarding literal subexpressions. Can get more complex with expressions with assignments as must prove that the value cannot change e.g., final. 
##### Construction and use case



### Linear IRs
#### Stack Machine IR
Stack-machine code is a form of **one address** code that utilises an implicit stack for operands. Most operations consume operands from the stack and push the result of their computation back onto it. This IR is highly compact because the stack creates an implicit name space? eliminating many explicit names
### Three-address code IR

### Hybrid IR

### Control Flow Graphs
A Control Flow-Graph is a directed graph $G = (N, E)$ that models the flow of control between basic blocks in a program. Each $n \in N$ represents a **basic block**, which is **a maximal length sequence of guaranteed branch-free/in-order code.** Each edge $e = (n_i, n_j) \in E$ corresponds to a possible transfer of control from block $n_i$ to block $n_j$. The CFG is fundamental for control flow analysis and global optimisation, dead code elimitation, global register allocation etc. CFGs often constructed from a linear IR.  

![[Pasted image 20251022115054.png]]

##### Construction and use case
#### Symbol Table

### Static Single Assignment



## Namespaces
## Context Sensitive Analysis