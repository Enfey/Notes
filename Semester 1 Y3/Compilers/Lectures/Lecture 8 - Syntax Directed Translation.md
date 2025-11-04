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
- Auxillary metadata

## Embedded actions in ANTLR
Supports **embedded actions** in grammar rules
An embedded action $\{...\}$  inside a rule becomes part of the generated parser method and executes immediately at that point in parsing.
![[Pasted image 20251103204445.png]]
These run inside the parser thread, heavy work is discouraged here. 

## Visitor Pattern
Software design pattern that permits the separation of algorithms from the objects on which they operate.  One supplies a `Visitor` object with a method for every node type; the tree *accepts* the visitor and dispatches to the correct visit method. This gives a place to add behaviour and keep the AST separate. 

The classic `Visitor` variant is one `visitX` method per node type. 

Bottom up/Top-down visitors exist, traversal order is important, some visitors will perform a post-order walk(useful for synthesised attributes), others pre-order(useful for inherited context).

An example visitor interface is supplied below (java-like):
```java
interface Node {
	<T> T accept(Visitor<T> v);
}

interface Visitor<T> {
    T visitBinary(Binary node);
    T visitLiteral(Literal node);
    T visitVar(Variable node);
    T visitIf(IfStmt node);
    // ... one method per node type
}

class Binary implements Node {
    Node left, right;
    Op op;
    <R> R accept(Visitor<R> v) {return v.visitBinary(this);}
}

class IRGenVisitor implements Visitor<IRFragment> {
    IRFragment visitBinary(Binary n) {
        IRFragment L = n.left.accept(this);
        IRFragment R = n.right.accept(this);
        Temp t = newTemp();
        emit(t+"="+L.place+" "+n.op+" "+R.place);
        return new IRFragment(t);
    }
    //...
}

```
Return type used for synthesised attributes. One can pass arguments if necessary for inherited attributes. Visitors produce new annotated structures rather than mutating nodes. 

It is good to break large visitors into focused visitors e.g., IR builder, SymbolCollector, TypeResolver etc. 

### ANTLR Visitor Pattern
The Visitor pattern is ANTLR's preferred method for semantic elaboration and translation over embedded actions. When run with the `-visitor` option, ANTLR will generate:
- `XVisitor<T>` interface where `X` is the name of the grammar, defines methods for 'visiting' a parse tree node, performing some computation and return a value of type `T`
- `XBaseVisitor<T>` class which has the default implementations of the `XVisitor<T>` interface
- `XListener` - an interface that defines methods for 'entering' and 'exiting' nodes of a parse tree, which can perform a computation but do not return anything
- `XBaseListener` - abstract class providing empty implementations of all `XListener` methods.
The compiler author is then to extend `XBaseVisitor<T>` and `XBaseListener`
	`T` is the type that all visit methods should return. 

The TreeVisitor can and should be augmented with extra class members for auxillary data structures needed during semantic elaboration e.g., symbol table, which can be then be woven into the independent `VisitX` methods. 
#### TreeListener methods
For each nonterminal defined in grammar, the listener defines two methods. For a nonterminal called `Thing`, the methods are:
- `void enterThing(ThingContext ctx)
- `void exitThing(ThingContext ctx)` 
The actions carried out in these methods are at the discretion of the compiler author, and would be a natural place to perform attribute related computations and passing. 

#### TreeVisitor methods
The visitor defines, for each nonterminal, a method of type `T visitThing(ThingContext ctx)`. 

The visitor also has a general visit method which takes an arbitrary context object and defers to the appropriate specific method depending on the concrete type of the context. 

The default implementations of the visit methods simply visit all the child nodes of the given node and return their result. If the programmer overrides a visit method, this does not happen and the programmer is responsible for manually specifying how to visit any child nodes and what order to do this in. 

An example workflow would be to produce a grammar, implement the Visitor methods to construct an AST node, on top of this one may implement a number of AST Visitors that perform different semantic jobs e.g., emit IRFragments, check types, collect declarations. 

Pass context explicitly into visit calls for inherited attributed needs, and returns for synthesised attributes. 
## Memory
### Lifetimes
A lifetime is the duration which a variable is valid or in scope i.e., from its creation, to its destruction.

Three useful categories

1. Automatic(stack) lifetimes
	- Created when control enters a scope(function, block)
	- Destroyed when control exits that scope
	- For optimised code, stored in registers only, otherwise present in a stack frame
2. Static lifetimes(compile-time/global lifetime)
	- Exist from program load until program termination
	- Stored in program data segments e.g., `.data`, `.rodata` or `.bss`
3. Irregular/Dynamic lifetime(heap-like)
	- User controlled via allocation/deallocaion, or managed by runtime environment(GC)
	- Lifetime not tied to lexical scope, compiler/runtime must track and free or collect
	- Examples include objects, big arrays allocated/extended on demand.
### Storage layout 
Every type has size and alignment. 
The compiler must ensure that each value is placed at an address that is a multiple of its alignment.  Primitives subject to rounding function to ensure value placed at address that is multiple of its alignment. 
#### Arrays
1D array address computation:
	`addr(E[i]) = base + i * sizeof(element)`
2d array address computation(row-major) (`A[R][C]`)
	`addr = base + (i * C + j) * sizeof(element)`

#### Strings
Null terminated contiguous sequence of bytes, compatible with C API. OR can do length-prefixed strings, length is extra word near the string to support O(1) length queries. 

#### Structs
Laid out contigously with alignment/padding. Compilers may provide pragmas to reduce alignment and enforce tighter packing where possible. 





