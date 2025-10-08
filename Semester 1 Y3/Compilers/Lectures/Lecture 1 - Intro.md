## Compiler Definition
Compilers are programs which translate a program written in one language into a program written in another language. At the same time, a compiler is a vast system with many internal, varying components and algorithms. Compilers that target programming languages rather than the instruction set of a CPU are often called source-to-source translators or transpilers. 

### Compilers further
A good compiler contains a microcosm of computer science, making practical use of greedy algorithms, heuristic search, graph algorithms, dynamic programming, automata theory. Deals with problems such as spatial and temporal locality, memory hierarchy management, pipeline scheduling. Most software is compiled, and the correctness of that process and the effiiciency of the resulting code have a direct impact on our ability to build large systems.

## Principles of compilers. 
***The compiler must preserve the meaning of the program being compiled***
	Correctness is a fundamental concern in computer science. A compiler must preserve correctness by faithfully implementing the meaning of its input language in the target language. 
***The compiler must improve the input program in some discernible way***
	A traditional compiler improves the input program by making it directly executable on some target machine. Other compilers improve their input in different ways. e.g., optimising parts of the program, targeting a language with greater generality/ability to be executed e.g., to JVM bytecode.
## Compiler Structures
Varying ways to structure a compiler. 

### Single box model
Must understand the source program takes as input, and map its functionality to the target machine. Distinct nature suggests a division of labour, and decomposes compilation into two major pieces: a **front-end** and **back-end**.
![[Pasted image 20250929025421.png]]

#### Front-end
Focuses on understanding the source-language program. Must encode its knowledge of the source program in some structure for later use by the backend. This intermediate representation, whatever form it may take becomes the compiler's definitive representation for the code it is translating. At each stage in compilation, the compiler will have a definitive IR, there may be multiple at any given stage, but one will be the definitive.

#### Back-end 
Responsible for mapping the IR to the instruction set and finite resources of the target machine. Because the backend only processes the IR created by the front end, it can assume the IR contains no syntactic or semantic errors. 

### Three-Phase Compiler
Due to IR, makes it possible to add more phases to compilation. This structure adds a middle section, the optimiser, which takes an IR program as input and produces a semantically equivalent IR program as its output to pass to the backend, with minimal disruption other than an increase in both compilation time and compiler complexity. Represents the classic optimising compiler. In practice, each phases is divided internally into a series of passes. Front end consists of two or three passes that handle the details of recognising valid source-language programs and producing the initial IR form of the program. The middle section contains passes that perform different optimisations.
Backend consists of a series of passes, each taking IR program one step closer to target machine's instruction set. 

### Optimiser in three-phase compiler
IR to IR transformer that attempts to improve the IR program in some way. The optimiser can make one or more pases over the IR, analyse the IR, and rewrite the IR, technically making it a compiler. It may have other objectives other than to rewrite the IR in a way that will produce a faster target program from the backend, or a smaller target program from the backend. It may have other objectives such as a program that produces fewer page faults or uses less energy.

## Compiler Components
### Front-end
Focuses on understanding the source-language program's form/syntax and meaning/semantic details. Determines whether input is well-formed, in terms of both syntax and semantics. If it finds that the code is valid, it creates a representation of the code as IR; if not, it reports back to the user with diagnostic error messages. 


### Back-end

### IR

### Optimiser