# Code Shape
> Encapsulates all the design choices, large and small, that the compiler author makes regarding the representation of computation in both intermediate representation.

These decisions have a direct impact on the efficiency of the final compiled code, as well as the compiler's ability to analyse and optimise the code in later passes.

This is the stage that sits between semantics and optimisation, permitting the realisation of semantics in a more grounded form - code shape is merely concerned with ensuring structurally reasonable code. 

It should be noted that the shape of code constrains optimisations.
## Expression Evaluation Order
Expression evaluation is an area where code-shape decisions influence optimisation opportunities. 

For the expression $x+y+z$ where $x,y,z : int$, mapping the operation into a sequence of binary additions exposes the impact of the evaluation order. One may choose to represent this via three address code, or via AST, these are equivalent for commutative and associative operations, but the compiler must choose one. 

The best evaluation order often depends on the context from the surrounding code e.g., whether x+y was computed recently and the values have not changed.  Of course, we are not just limited to the order of evaluation and its structure, must also consider its storage strategy. 
	![[Pasted image 20251104174354.png]]
## Storage Assignment
As part of translation, the compiler must assign a storage location(memory or register) to each value produced by the code. The compiler must understand the value's type, its size, its visibility and its lifetime, all defined by the source-language rules. 

The compiler must take into account the link-time, load-time, and runtime layout of memory, any source-language constraints on the layout of data areas and data structures, and any target-processor constraints on the placement or use of data such as alignment. 

A key code shape choice is defining conventions for storing data according to the above.

The compiler must decide for each value, whether to keep it in a register, or keep it in memory. In general, compilers adopt a memory model. With a memory-to-memory model, the compiler assumes all values reside in memory. Values are loaded into registers as needed but the code stores them back to memory, use physical register names in IR. 

In register-to-register, the compiler assumes that it has enough registers to express the computation. It invents a distinct name, a virtual register, for each value that can legally reside to a register. The compiled code will store a virtual register's value to memory only when absolutely necessary. Register allocation is a mandatory phase here rather than an optimisation, and maps the virtual register names onto physical ones. 

### Layout for data areas - Memory-to-Memory
The compiler groups together storage for values with the same lifetimes and visibility, creating distinct data areas for them. For example, the compiler can place procedure-local automatic storage inside the procedure's stack frame, and address such storage relative to the base pointer for that stack frame/activation record, precisely because the lifetimes of these variables match the lifetime of the stack frame.

Variables with either static lifetimes or global visibility are placed into the static region of memory, .bss if zero initialised. 

Values that are stored in the heap have lifetimes that the compiler cannot easily predict. A value can be heap allocated by two distinct mechanisms; the programmer can explicitly allocate storage from the heap, or the compiler can place a value on the heap when it detects that the value may outlive the procedure that created it. A value in the heap is direct addressed. 

In the case of local, static, and global data areas, the compiler must assign each name an offset inside the data area, often mirroring alignment. Padding may be required to achieve alignment compliance. To minimise wasted space, the compiler should order the variables into groups, from those with the most restrictive alignment rules to those with the least. Then assign offsets accordingly, keeping track of next available offset at each point. 


#### Addresses
Linkers and loaders necessitate that addresses should be kept abstract until as long as possible. It is generally more efficient to use relative addresses according to some base e.g., addressing .text relative to its fixed position in ELF files when loaded into distinct virtual address space on x86.

To address some memory, one must generate the code to calculate the address, and generate the code to load data from the address.

The symbol table records the storage location and size of variables; the term storage location in practice can refer either to file offsets or memory addresses. 

##### Variable access in TAM
For simple variable access, the code shape follows a two step pattern:
1. Calculate the address - TAM stores variabls at known offsets from register-based addresses(SB for globals, LB for Stack frame)
2. Load the data - use $LOAD(n)$ to fetch $n$ words from that address onto the stack. e.g., $LOAD(1) 3[LB]$ loads one word from LB+3 onto stack. The symbol table should track each variable's offset and size so these addresses can be generated correctly. 

##### Array indexing in TAM
Arrays require address arithmetic. The code shape must:
1. Generate code to get the index
2. Multiply the index by the element size
3. Add this to the array's base address
4. Load from computed address
This naturally depends on whether we are using row-major, or column-major order. 


## Binary Operations
Generating code for a binary operation is as follows:
1. Generate code to compute first operand
2. Generate code to compute second operand
3. Generate code to compute result and leave result on stack
Optimisations further on may permit us to skip some of these steps e.g., register allocation.
### Binary Operations in TAM
For `a + b`:
``` 
LOAD(1) [addr of a] ; left operand on stack 
LOAD(1) [addr of b] ; right operand on stack 
CALL(0) 0[PB]       ; add primitive, push result 
```


### Short Circuit Evaluation in TAM
Boolean operators require special handling. For $a \&\& b$, you shoudn't evaluate both operands, if $a$ is false, then $b$ shouldn't be evaluated at all. 
``` 
LOAD(1) [addr of a]   ; evaluate first operand 
JUMPIF(0) label_false ; if false, skip second operand LOAD(1) [addr of b]   ; evaluate second operand 
CALL(0) 17[PB]        ; and operation 
JUMP label_end 
label_false: 
LOADL 0               ; push false 
label_end:            
; result is on stack 
```

## Control Flow Patterns
Generally we wish to:
1. Generate code for a basic block
2. Identify all possible destinations that can follow the block
3. Generate jumps/branches with appropriate conditions
CFG can make this straightforward, but there are alternatives. 
### Conditionals
The code shape must conditionally skip blocks of code.
In linear assembly, one executes a branch by jumping over the code you don't want to run. 
```
LOAD(1) [condition_var]
JUMPIF(0) else_label
JUMP end_label

else_label:
...

end_label:
...
```

### Loops
After iteration, need to jump back to start of the loop.
Label first instruction of loop condition, and next instruction after end of loop body. 

```
cond:
LOAD(1) [condition_var]
JUMPIF(0) loop_end
...
JUMP cond

loop_end:
...
```

### Switch statements
Case statements are a special case of conditional execution. Label first instruction of each case, and generate code to test input and branch to appropriate selection
Larger statements may generate a lookup table or binary search to find correct destination.
![[Pasted image 20251104194608.png]]

## Procedures
The method for generating procedure calls is aligned with the calling convention and ABI for the specific architecture and OS. The shape of code must reflect this agreement. Discussed more next lecture. 
## General TAM info
Abstract emulated device for teaching compilers, with stack based computation. Maintains separate code and data memory areas, and uses 16 registers to track addresses. Somewhat fusion between PDP-11 separate I and D areas and permanent register-indirect addressing. 
### Memory Layout and Registers

![[Pasted image 20251104190501.png]]

Similar to segment registers in x86, registers in TAM track addresses of different segments in memory. 

Addresses are word-aligned, no shifting is involved to derive a byte address. Furthermore, whilst the address a register holds is always a 16 bit unsigned int, the displacement/offset is in fact signed.

The most used registers:
- CB(code base) - address of first word in code memory
- PB(primitives base) - address of first word in primitive segment
- SB(stack base) - address of first word in data memory
- LB(local base) - address of first word in current stack frame
- ST(stack top) - address of word after last word in stack

Heap always begins at 65335 and grows downward, stack begins at 0 and grows upward.

### Instruction Set
All instructions are 32 bit wide and follow the same structure:
- 4 bit opcode $op$ 
- 4 bit register index $r$
- 8 bit unsigned operand $n$
- 16 bit signed operand $d$
$Op(n)$  $d[r]$ 

#### Loading Values
- $LOAD(n) \ \ \ r[d]$ 
	- Starting from $r[d]$, copy $n$ words of data to the top of the stack
- $LOADA \ \ \ r[d]$
	- Push the address $r[d]$ to the stack
- $LOADI (n)$
	- Equivalent to $LOAD(n)$, pops address from stack however
- $LOADL(d)$
	- Push the value $d$ to the stack

#### Storing Values
- $STORE(n) \ \ \ r[d]$ 
	- Pop $n$ words from the stack and store them in memory beginning at address $r[d]$ 
- $STORE(I)$
	- Equivalent to $STORE(n)$ but pops the address from stack
	- Address should be above the data to store on stack
#### Function Calls and Calling Convention
- $CALL(n) \ \ \ r[d]$
	- Call the subroutine that begins at address $r[d]$ with the value of the register $n$ as the static link. 
- $CALLI$
	- Pop the call address and the static link in that order and call the subroutine at the specified address
- $RETURN(n) \ \ \ d$ 
	- Pop an $n$ word result value, unwind the stack frame, pop $d$ words of parameters, push the result value and update CP to the return address.
Static link, dynamic link and return address all handled by emulator. The callee is to allocate space for local variables in prologue. Caller stores the return value in postcall sequence. 
#### Stack manipulation
- $PUSH(n)$
	- Advance the stack top by $n$ words but don't add any data
- $POP(n) \ \ \ d$ 
	- Pop $n$ words from the stack, then pop $d$ further words, then push the original $n$ words back on the stack
#### Control Flow
- $JUMP \  \  \ r[d]$
	- Unconditionally changes the code pointer to $r[d]$ 
- $JUMPI$
	- Pop an address from the stack and set the code pointer to that address
- $JUMPIF(n) \  \  \  r[d]$
	- Pop a value from the stack, and if it is equal to $n$, change code pointer to $r[d]$
- $HALT$ 
	- Immediately terminate program



