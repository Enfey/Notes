> An IR is a machine-agnostic representation placed between front-end(parsing+semantics) and back-end(codegen).

A well formed IR permits one to:
- Apply optimisations cleanly (e.g., constant folding, dead-code elimination)
- Separate language concerns from machine concerns and prep for linking
- Make retargeting easier: many backends can consume the same IR. 

## A Taxonomy of IRs
1. **Graphical IRs** - encode the compiler's knowledge in a graph. The algorithms are expressed in terms of graphical objects: nodes, edges, lists, or trees. The parse trees used to depict derivations in Chapter 3 are a graphical IR
2. **Linear IRs** - resemble pseudocode for some abstract machine. The algorithms iterate over simple, linear sequences of operations. 
3. **Hybrid IRs** - combine elements of both graphical and linear IRs.
The structural organisation of an IR has a strong impact on how the compiler writer will consider analysis, optimisation, and code generation. E.g., treelike IRs lead naturally to passes structured as some form of treewalk. Similarly, linear IRs lead naturally to passes that iterate over the operations in order.

Can vary in degree of abstraction offered. 

### Graphical IRs
#### Abstract Syntax Tree

#### Directed Acyclic Graphs

### Control Flow Graphs

### Linear IRs
#### Stack Machine IR
### Three-address code IR

### Hybrid IR
#### Symbol Table

## Static Single Assignment



