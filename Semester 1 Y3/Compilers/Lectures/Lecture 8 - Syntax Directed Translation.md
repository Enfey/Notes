# Context Sensitive Analysis
Phase in compiler pipeline, also referred to as semantic elaboration or semantic analysis, which is responsible for added semantic information to the parse tree, and often begins construction of a symbol table.

Performs semantic checks such as scope checking, function call validation, type checking, use of undeclared variable and so on and so forth. Rather than just verifying a program overall structure is sound, it verifies that the program context is logically coherent, enforcing language rules that cannot be checked by grammar alone. Often includes:
- Type checking and inference
- Control flow and reachability
- Semantic checks for language features
- Scope and symbol management
- Check accessibility and visibility


Such a phase is a necessity. Before a compiler can emit executable target-environment code for computations involving some arbitrary variable $x$, it must have the answers to many questions:
- *What kind of value is stored in x* - types
- *How big is x* - must be known to be manipulated
- *If x is a procedure, what arguments does it take, what does it return*
- *How long must x's value be preserved*
- *Who is responsible for allocating space for x and initialising it*
These questions stretch beyond the ability of examining the validity of a program's form, thus yielding a final stage in the compiler frontend which turns syntactic structure into semantically meaningful IR. 

## Data Structures
- **Symbol table and Entries and Sheaf of Tables**
	- Entries contain name, kind(var/type/func/namespace), type descriptor, attributes(storage class, visibility), unique id(index into string table perhaps)
	- Sheaf of tables - new symbol table is created upon entering each new lexical scope, when a reference to a name occurs, must search sequentially and access each table starting at current scope and moving outward to lower nesting levels, until most recent declaration is found. The result of this search is the name's static coordinate, a pair $[l,o]$ where $l$ is the lexical nesting level where the name is declared and $o$ is the offset within the data area for that level. 
		- Often search and other mechanisms implemented around a stack:
			![[Pasted image 20251103182710.png]]
- **Control Flow Graph**
	- Used for flow-sensitive analysis e.g., reachability and subsequent optimisation and transformation
- **AST Nodes**
	- Annotated with semantic attributes(inferred type, resolved symbol)

# Syntax Directed Translation
Compiler design technique utilised for implementing context-sensitive analysis during compilation. Attaches semantic actions so that at parse time some semantic effect - building AST, generating intermediate code e.g., three address, computing attributes - is produced.

An attribute grammar is an augmented context-free grammar with *attributes* associated with terminal and nonterminal symbols of the grammar. *Semantic* rules compute attribute values and are associated with the productions of the grammar. Attributes are used to carry semantic information. 

When a semantic function defines the value of an attribute of the symbol on the left hand side of the rule, the attribute is called **synthesised**, otherwise it is called **inherited**.

There are two fundamental attribute kinds:
- **Synthesised attributes**
	- A synthesised attributed is computed from the values of attributes of the children, since the values of the children must be computed first, this is an example of bottom-up propagation. ![[Pasted image 20251103184242.png]]
- **Inherited attributes**
	- An inherited attribute at a node in a parse tree is defined using the attribute values at the parent or siblings. Inherited attributes are convenient for expressing dependence.
		- Take $S \to ABC$ where $A$ can get values from $S, B,$ and $C$.

There are a few attribute grammars that we can construct based on this:
- **S-attributed Grammars** (only synthesised) - easy to evaluate bottom-up ideal for LR/bottom-up parsers or a postorder AST walk
- **L-attributed Grammars** - use both synthesised and inherited attributes, more flexible evaluation order, often left-to-right traversal of parse tree(suitable for LL/recursive descent) - inherited attributes restricted to depend only on parent node and attributes of its left siblings. 
- **General attribute Grammars** - arbitrary dependencies, need dependency graph. 

Every attribute evaluation can be modeled as nodes in a directed graph where edges point from the attribute it depends on to the attribute being computed. 
## Action Placement
Refers to where in grammar the semantic actions are placed that compute attributes or emit code. 

Pure parsing can be kept separate from heavy semantic work. Can also avoid building AST altogether, and emit code here. 
### Top Down
Evaluate inherited attributes just before calling the production for that child.

Evaluate synthesised attributes after parsing the child(when control returns). 
![[Pasted image 20251103192359.png]]
Take this grammar, in LL(1) form. A recursive descent parser follows:
![[Pasted image 20251103192502.png]]
During the processing of the Login command, the parser gathers the username and password returned from the children nodes and uses that information to create a connection attribute to pass up the tree; the username, password, and connection are all acting as synthesised attributes. The connection is saved and later passed downward into other commands, used as an inherited attribute when processing *Get* and *Logout*, receiving from left siblings.

## Practical Generation Strategies
1. Emit-while-parse
	- Semantic actions produce IR directly during parse, inconvenient for optimisation and maintaining any for of structure, but is immediate. 
2. Build AST then translate
	Grammar actions only build AST nodes; after parsing, run separate passes. It is up to the compiler author how much semantic elaboration is to be detailed at parse time. AST->IR lowering takes place after all of these passes. 
3. Hybrid
	One may choose to build AST for high-level constructs, but emit an IR like TAC for expressions during parse, preserving structure for future compilation phases. 

## What can SDT emit?
SDT actions can produce many different artifacts:
- Symbol table entries and scope
- AST nodes/annotated AST
- Semantic diagnostics
- IR fragments
- Auxilary metadata

## Embedded actions in ANTLR

## Visitor Pattern

## Memory

