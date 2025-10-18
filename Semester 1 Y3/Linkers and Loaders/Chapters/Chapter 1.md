## What do linkers and loaders do?
- The basic job of any linker and loader is simple at a glance; it binds more abstract names to more concrete names, permitting programmers to write code using the more abstract nams.
- Takes a name written by a programmer e.g., `getline` and binds it to the location 612 bytes from the beginning of the executable of the executable code in the module `iosys`.

## Address binding: historical perspective
- Earliest computer, programmed in machine language.
- Write out symbolic programs on punch cards and hand assemble them into machine code.
- If programmer used symbolic addresses at all, the symbols were bound to addresses as the programmer did their hand translation.
- If it turned out that an instruction had to be added or deleted, the entire program had to be hand-inspected and any addresses affected by the added or deleted instruction adjusted.
- The problem was that names were bound to addresses too early. Assemblers solved this, let programmers write programs in terms of symbolic name, with the assembler binding the names to machine addresses. If program changed, just had to reassemble, the work of assigning addresses pushed from programmer to computer.
- Libraries of code compound the address assignment issue. Basic operations computers do are simple, useful programs are composed of subprograms, reuse these.
- Programmers were using libraries before they even used assemblers. John Mauchly, led the ENIAC project, wrote about loading programs along with subprograms selected from a catalog of programs stored on tapes - and of the need to relocate the subprogram's code to reflect the addresses at which they were loaded. 
- These 2 basic linker functions, relocation and library search, appear to predate even assemblers! The relocating loader allowed authors and users of subprograms in this era to write each subprogram as though it would start at location zero, and defer the actual address binding until the subprograms were linked with a particular main program!
- With the advent of operating systems, relocating loaders separate from linkers and libraries became necessary. Before OS, monoprogramming model with static code relocation at compile time. 
- With OS, program had to share memory with the OS and perhaps even other programs, depending on memory model and time period. This meant the actual addresses at which the program would be running weren't known until the OS loaded the program in memory, deferring final address binding past link time to load time. Dynamic code relocation at load time. 
- Linkers and loaders now divided up the work, linkers did part of the address binding, assigning *relative* addresses within each program, and loader doing final relocation step to assign actual addresses.
- As systems increased in complexity, linkers took on more complexity. Fortran programs used multiple subprograms and common blocks, up to linker to lay out storage and assign addresses for both the subprograms and the common blocks. Linkers increasingly had to deal with object code libraries, including application libraries written in Fortran and other languages.
- Program's quickly became larger than memory, linkers provided **overlays**, a technique that let programmers arrange for different parts of a program to share the same memory, with each overlay loaded on demand when another part of the program called into it. They’re still used in memory limited embedded environments, and may yet reappear in other places where precise programmer or compiler control of memory usage improves performance.
- With the advent of hardware relocation and virtual memory, linkers and loaders actually got less complex, since each program could again have an entire address space. Programs could be linked to be loaded at fixed addresses, with hardware rather than software relocation taking care of any load-time relocation. Segmentation then came along, and paging and so and so forth. Compilers+assemblers, began to emit **object code divided into these segments**, rather than one monolithic block.
	- - `.text` — code
    - `.data` — initialised global variables
    - `.bss` — uninitialised globals
    - (later others like `.rodata`, `.heap`, etc.)
- Linker now had to be able to combine the .text, .data, .bss sections from each input module, rather than treating all code together and all data together in one block, which would violate segmentation.
	- ![[Pasted image 20251017023300.png]]
- In the simpler static shared libraries, each library is bound to specific addresses at the time the library is built, and the linker binds program references to library routines to those specific addresses at link time. Static libraries turn out to be inconveniently inflexible, since programs potentially have to be relinked every time any part of the library changes, and the details of creating static shared libraries turn out to be very tedious. Systems added dynamically linked libraries in which library sections and symbols aren’t bound to actual addresses until the program that uses the library starts running. Sometimes the binding is delayed even farther than that; with full-fledged dynamic linking, the addresses of called procedures aren’t bound until the first call. Furthermore, programs can bind to libraries as the programs are running, loading libraries in the middle of program execution. This provides a powerful and high-performance way to extend the function of programs, essentially lazy loading. Microsoft Windows in particular makes extensive use of runtime loading of shared libraries (known as DLLs, Dynamically Linked Libraries) to construct and extend programs.

## Linking vs Loading
Perform several related, but conceptually separate tasks.
#### Program loading
- Copy a program from disk into main memory so that it is ready to run. 
- Loading a program involves either memory mapping or copying the contents of the executable file containing the program instructions into memory and then carrying out other required preparatory tasks such as setting protection bits and arranging for virtual memory to map virtual addresses to disk pages. 
- This is entirely the job of a loder.
#### Code relocation
- Compilers + assemblers emit object code with program addresses often starting at address zero, but do not load into memory at location zero.
- If a program is created from multiple subprograms, must load at non-overlapping addresses.
- Relocation is the process of adjusting addresses, i.e., providing load addresses in a program so it can execute correctly when loaded at a memory address different from the one it was originally compiled for, adjusting code and data in the program to reflect the assigned addresses.
- In many systems, happens more than once:
	- Common for linker to emit program from multiple .obj files, which starts at program address zero, and will then be relocated according to architecture and OS. 
- Can be done by either linker or loader, or both (or neither depending on system constraints)
#### Symbol resolution
- Compiler identifiers symbols and emits object code with placeholders for addresses e.g., address of a function.
- Linker then performs symbol resolution between object files - finds the placeholders for the symbols defined across different modules and assigns them the proper addresses such that external symbol references are valid. 
- Entirely the job of the linker.


Considerable overlap between linker and loader, but reasonable to mark distinction. 

The distinction between relocation and symbol resolution is murky. Linkers can already resolve references to symbols, thus one way to handle code relocation, is to assign a base address symbol to the base address of each part of the program, and treat relocatable addresses as references to the base address symbols. Just now needs to calculate relocation for the start of each segment rather than entire program, and the rest of the addresses are expressed relative to that base.


### Two Pass Linking
We now turn to the general structure of linkers. Linkers are fundamentally subject to a 2-pass process. 
**INPUT** - command line arguments, object files, linker control files, shared libraries, normal libraries
	Each input file also contains symbol table (compiler data structure, IR after optimisation and such)
**OUTPUT** - debug symbol file, executable file, link/load map

#### First Pass
Collects data.
1. Scans all object files and notes
	- Scans input files and finds sizes of segments to be placed in output file
	- Finds symbol definitions
	- Finds symbol references
2. Builds tables
	- Symbol table - constructed from symbols imported
	- Segment table - each section's name, size, base address
3. Assigns numeric addresses
	- After knowing all segment sizes and placements, linker can compute numeric address of every symbol, now corresponding to a specific number each, but this hasn't been patched into the code yet.

Using this data, determines the size and locations of the segments in the output address space, and figures out where everything will be placed in the output file e.g., merging segments to avoid violating memory constraints. 

#### Second pass
Uses data from first pass to read and relocate object code, **replace each symbolic reference with its actual numeric address**, patch into code, with respect to relocated segment addresses. Writes this relocated code to output file, generally with header information, and the symbol table information.


If the program uses dynamic linking, the symbol table contains the info the runtime linker will need to resolve dynamic symbols. In many cases, the linker itself will generate small amounts of code or data in the output file, such as "glue code" used to call routines in overlays or dynamically linked libraries, or an array of pointers to initialisation routines that need to be called at program startup time.

Some object formats are relinkable, that is, the output file from one linker run can be used as the input to a subsequent linker run. This requires that the output file contain a symbol table like one in an input file, as well as all of the other auxiliary information present in an input file.


Nearly all object formats have debugging symbols, such that when program run under control of debugger, can use those symbols to let programmer control the program in terms of the line numbers and names used in the source programs. 


### Object code libraries
All linkers support object code libraries in some form, most provide support for various kinds of shared libraries. A library in principle is just a set of object code files. After linker in first pass processes all of the regular input files, if any imported names remain undefined, it runs through the library and links in any of the files in the library that export one or more undefined names.
![[Pasted image 20251017212701.png]]

### Relocation and code modification
When compiler or assembler generates object files, generates code using unrelocated addresses of code and data defined within the file. 

Patch object code to reflect addresses assigned. Consider x86 snippet.
moves the contents of variable a to variable b using the eax register. 
	mov a,%eax 
	mov %eax,b
If a is defined in same file at location 1234 hex and b is imported from somewhere else, generated object code will be:
	A1 34 12 00 00 mov a,%eax 
	A3 00 00 00 00 mov %eax,b
Each instruction contains a one-byte operation code followed by a four-byte address. The first instruction has a reference to 1234 (reversed, since x86 uses little-endian) and the second reference is 0 as the location of b is unknown. 

Assume the linker links this code such that the section in which $a$ is in is relocated by hex 10000 bytes, and b is at hex 9A 12.
The linker modifies the code to be:
	A1 34 12 01 00 mov a,%eax 
	A3 12 9A 00 00 mov %eax,b
The modification process can get complex depending on the system. Modern computers, including all RISC's, require considerably more complex code modification. No single instruction contains enough bits to hold a direct address, so the compiler and linker have to use some bullshit tricks to handle data at arbitrary addresses. Can also concoct addresses using instructions which each contain part of the address, and bit manipulation to combine them.

Some systems require position independent code that will work correctly regardless of where in the address space it is loaded. Linkers generally have to provide extra tricks to support that, separating out the parts of the program that can’t be made position independent, and arranging for the two parts to communicate.


### Compiler drivers
In most cases, operation of linker invisible to the programmer as it run automatically as part of compilation process. Most compilers have compiler driver that automatically invokes phases of compiler as needed. 

E.g., 2 C source files, compiler driver will run C preprocessor on file A, then compiler on preprocessed file, then assembler on assembled file, then object file, then does the same for File B. Then invokes linker on the two object files. 
Compiler drivers are often much cleverer than this. They often compare the creation dates of source and object files, and only recompile source files that have changed (***make*** is the classic example. )


### Linker command languages
Every linker has some sort of command language that can be used to control the linking process. At the very least, need list of object files and libraries to link. Generally long list of possible options: whether to keep debugging symbols, whether to use shared or static libraries, which of several output formats to use. 

Most linkers permit some way to specify the address at which the linked code is to be bound; useful when using a linker to link a system kernel or other programs that don't pass control to the operating system. In linkers that support multiple code and data segments, a linker command language can specify the ordering in which segments are linked etc.

4 Common techniques to pass commands to a linker.

- *Command line* - Pass mixture of file names and flags - or pass file with directives for linker.
- *Intermixed with object files* - IBM mainframe linkers accepted alternating object files and linker commands in a single input file. 
- *Embedded in object files* - some object formats, notably Microsoft's, permit linker commands to be embedded inside object files, permitting compiler to pass any options needed to link an object file inside the file itself. E.g., C compiler passes commands to search the C standard library.
- *Separate configuration/DSL* - some linkers have full fledged configuration language to control linker, the GNU linker which can handle range of object file formats, architectures, and address space conventions, has a complex control language allowing specification of segment linking order and combination order and segment addresses, defining overlays etc.

### Linking example
2 C source files, `m.c` and `a.c`.

`m.c`
```c
extern void a(char *)

int main (int ac, char** av){
	static char string[] = "Hello, world!\n";
	
	a(string);
}
```

`a.c`
```c
#include <unistd.h>
#include <string.h>

void a(char *s){
	write(1, s, strlen(s))
}
```

Apparently, according to author, m.c compiles via GCC into 165 byte object file in classic a.out object format. Includes fixed length header, 16 bytes of `.text` segment containing read only program code, 16 bytes of `data` segment containing the string. Following that are two relocation entries, one that marks the pushl instruction that puts the address of the string on the stack in preparation for the call to `a`, and one that marks the call instruction that transfers control to `a`. The symbol table exports the definition of `_main`, imports `_a` and contains some symbols for the debugger. Each global symbol is prefixed with an underscore purposely. 

![[Pasted image 20251018032012.png]]

A.c compiles into 160 byte obj file, 28 byte text segment, no data. Two relocation entries mark the calls to `strlen` and `write`. Symbol table exports `_a` and imports `_strlen` and `_write`.

To produce a binary executable, linker must combine these two object files with a startup init routine for C programs, and the necessary routines from the C library, creating the following executable:

![[Pasted image 20251018032913.png]]
Linker combined corresponding segments from each input file. One combined text segment, one combined data segment, one bss segment(not that either file used these). Each segment is padded out to a 4k boundary

