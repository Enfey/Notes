## What do linkers and loaders do?
- The basic job of any linker and loader is simple at a glance; it binds more abstract names to more concrete names, permitting programmers to write code using the more abstract nams.
- Takes a name written by a programmer e.g., `getline` and binds it to the location 612 bytes from the beginning of the executable of the executable code in the module `iosys`.

## Address binding: historical perspective
- Earliest computer, programmed in machine language.
- Write out symbolic programs on punch cards and hand assemble them into machine code.
- If programmer used symbolic addresses at all, the symbols were bound to addresses as the programmer did their hand translation.
- If it turned out that an instruction had to be added or deleted, the entire program had to be hand-inspected and any addresses affected by the added or deleted instruction adjusted.
- The problem was that names were bound to addresses to early. Assemblers solved this by letting programmers write programs in terms of symbolic name, with the assembler binding the names to machine addresses. If program changed, just had to reassemble, the work of assigning addresses pushed from programmer to computer.
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
#### Code relocation
#### Symbol resolution