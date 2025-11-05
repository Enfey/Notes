# ABI
> An **application binary interface** is defined as the set of programming conventions that operating systems ensure applications follow in order to run under that system. 

It represents the contract between code emission and consumption. An ABI must specify a number of distinct constraints with regard to procedures:
- **Parameter passing**: where parameters go(stack or register), order, who allocates/pushes them
- **Return Values**: how many words, where are they placed(register or stack), and how a caller reads them
- **Call/Return sequence**: exactly what bytes/instructions constitute a call and return(including how return addresses and links are stored)
- **Frame layout/stack discipline**: where locals live, where saved registers live, alignment rules, base/stack pointer conventions
- **Register preservation**: which registers the callee must preserve vs which registers the caller must preserve
- **Unwinding model**: how to unwind stacks
If everyone follows the ABI, modules compiled independently will be guaranteed to interoperate. 

Even for a simple stack virtual machine such as in our case, a simple convention must be chosen. 

## Procedure Calls
The procedure is a central abstraction in most programming languages, maintaining a controlled execution environment with a new protected namespace. 

The linkage convention of a procedure specifies an agreement between the compiler and OS that defines the actions taken to call a procedure or function. 

We can split the stages of a procedure call distinctly, and the actions they take directly mirror the linkage convention/preservation of ABI compliance. 
1. Precall Code
2. Function Prologue
3. Function Epilogue
4. Postcall Code

### Activation Records/Stack Frames
> An activation record is a region of storage associated with a single, isolated invocation of a procedure. 

Procedures access their AR frequently, thus it is often that an activation record pointer/frame pointer holds the address of the current AR; this is a conceptual abstraction, in most real implementations, the frame pointer acts as the ARP. 

The stack pointer points to the top of the stack, and changes frequently during function execution, whilst the AR/FP stays fixed. 

A stack frame provides space for:
- **Parameter Area** - Holds actual parameters from the call site
- **Return Address slot** - Stores the runtime address where execution should resume when the procedure terminates
- **Caller's ARP** - stores the pointer to the caller's AR, needed to restore caller's environment upon return
- **Local Data Area** - holds storage for variables declared in local scope
- **Return-Value Slot** - space to communicate data back to the caller


### TAM's Stack and Register Model
$SB$ - Stack Base, start of data memory, usually 0
$ST$ - Stack Top, points to the next free word after the stack
$LB$ - Local Base, points to base of current activation record
The stack grows upwards, and each function call creates a new activation record above the old one. 

#### Procedure Steps
Here we detail the transfer of control and data between a caller and callee, handled by four distinct sequences of code.
##### Precall Code
This sequence is the caller's responsibility. It begins constructing the callee's runtime environment. It evaluates the actual parameters and determine the return address. If a call by reference parameter is currently in a register, the precall sequence must store its value back to the caller's AR so that its address can be passed to the callee.

In TAM, we push the parameters in order onto the stack. If the callee is nested, we compute the address of the enclosing frame and place it in the $r$ field of the $CALL$ instruction. 
##### Function Prologue
This sequences completes the creation of the callee's environment. It creates space for local variables in the AR and initialises them. It may also store values passed in registers into the callee's newly created AR space. 

![[Pasted image 20251104210934.png]]

The callee issues a $PUSHK$ instruction to reserve space for $k$ local variables.
##### Function Epilogue
Evaluate and store return value in specified location(the stack).
Deallocate memory.
Transfer control back to caller by jumping to return address.

$RETURN$ instructions reset LB to caller's AR, pop all local values from the stack, and loads the return value on top of the stack, then jump to where execution should resume. 
##### Postcall Code
Store the return value in appropriate location and use as necessary.
Clean up any memory not cleared by callee.

To reduce overall code size, compilers often try to shift work from the precall and postcall sequences (which are generated per call site) into the prologue and epilogue sequences (which occur once per procedure).

#### Passing Parameters
One can pass by value: the caller evaluates the actual parameter and copies its value into the designated location in the callee during the precall phase. Any subsequent modification is therefore not local nor visible to the caller.

One can pass by reference: the caller passes the address to the formal parameter, thus any change made is reflected in the caller. 

