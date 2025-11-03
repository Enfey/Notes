# Context Sensitive Analysis
Phase in compiler pipeline, also referred to as semantic elaboration or semantic analysis, which is responisble for added semantic information to the parse tree, and often begins construction of a symbol table.

Performs semantic checks such as scope checking, function call validation, type checking, use of undeclared variable and so on and so forth, rather than just verifiying a program overall structure is sound, it verifies that the program context is logically coherent. 

Such a phase is a necessity. Before a compiler can emit executable target-environment code for computations involving some arbitrary variable $x$, it must have tha answers to many questions:
- *What kind of value is stored in x* - types
- *How big is x* - must be known to be manipulated
- *If x is a procedure, what arguments does it take, what does it return*
- *How long must x's value be preserved*
- *Who is responsible for allocating space for x and initialising it*
These questions stretch beyond the ability of examining the validity of a program's form, thus yielding a final stage in the compiler frontend. 

# Ad Hoc Syntax Directed Translation
This is a compiler design