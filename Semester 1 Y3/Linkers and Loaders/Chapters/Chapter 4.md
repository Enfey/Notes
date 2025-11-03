A linker or loader's first major task is **Storage Allocation**. It involves establishing a coherent model of the target address space and determining actual addresses for all code and data segments within the final output file.

Most of the symbols in a linkable object file are defined relative to the storage areas within a file, so the symbols cannot be resolved until the addresses of these areas are known. 

Details to handle peculiarities of computer architecture and programming language semantics can get complicated. Can handle most of storage allocation in an architecture agnostic manner, but invariably, there are some details that require ad-hoc machine-specific hackery. 

## Segments and Addresses
Input files contain a set of segments of various types. Differing kinds of segments are treated in different ways, all segments of a particular type are often concatenated into a single, larger segment in the output file. Sometimes the linker itself needs to create some segments and lay them out. Storage layout is generally a two-pass process, because the location of each segment cannot be assigned until the size of all segments that logically precede it are known. 

## Simple Storage Layout
In the simplest scenario, a linker input consists of a set of modules $M_1, M_2, ..., M_n$ , each of which consists of a single segment starting at location $0$ of length $L_1, L_2, ... , L_n$, with the target address space also starting at $0$.

The linker or loader examines each module in turn, allocating storage sequentially. The starting address of $Mi$ is the sum of all logically preceding segments i.e., $L_1$ through $L_{i-1}$, and the length of the linked program is the sum of all segment length $L_1 + L_2 + ... + L_n$.

Most architectures require that data is aligned on word boundaries, so linkers generally round each $L_i$ up to a multiple of the most stringent alignment the architectures requires. 

![[Pasted image 20251101211927.png]]


## Multiple Segment Types
In all but the simplest object formats, multiple types of segments exist, necessitating that the linker groups corresponding segments from all input modules. This requires a **two-level storage allocation strategy**.

Standard segments frequently managed and coalesced by linkers include executable code `.text`, read/write data, and bss `.bss`.

### Pass 1 - Sizing
Linker reads all input modules and calculates the total size for each segment type. Each module $M_i$ has text size $T_i$, data size $D_i$, BSS size $B_i.$Linker allocates spaces for each of $T_i$, $D_i$ and $B_i$ as though segment were separately allocated at zero.
### Pass 2 - Addressing
Final segment addresses are assigned sequentially: the data segments follow the text segments, and the BSS segment follows both the text and data segments. 

Simply add total size(s) of the modules logically preceding the module to allocate space for e.g., $T_{total}$ added to address assigned for each of the data segments(according to pass 1 from logical zero sizing) to compute location.

### Segment and Page Alignment
Linkers must round segment sizes up to satisfy alignment constraints required by the architecture, which often means rounding to word boundaries. In ELF, the section header field `sh_addralign` specifies alignment constraints, if any, requires the section's starting virtual address `sh_addr` to be congruent to 0 modulo this alignment value. Segments can start at any offset in file, but virtual starting address **must**equal the file offset modulo the alignment constraint. 

Many $*nix$ systems use the trick to save file space that involves starting data immediately after `.text` in object file, and mapping that page into virtual memory twice, once **RO** for the `.text`, and once **COW** for the `.data`. In that case, data addresses logically start exactly one page beyond the end of the text, so rather than rounding up, the data addresses start exactly $pgsize$ beyond the end of the RO text. 

![[Pasted image 20251102022754.png]]
![[Pasted image 20251102022939.png]]


### Common Blocks and other Special Segments
Standard storage allocation works well for most program parts, must handle specialised segments often with less trivial logic. 

**Common blocks** such as those with history in Fortran (block of storage that enables different subprograms to share data without using arguments) (or equivalently, unallocated C external variables) are designed to use **overlaid storage** rather than being concatenated sequentially.
1. If a common block is declared with varying sizes across multiple input sizes, the linker reserves space based on the largest declared size encountered
2. Linkers traditionally handle uninitialised global variables and common blocks by assigning them space in the **BSS segment** of the output file. 

Other special segments include:
- **Initialisation and termination**
	- `.init` and `.fini` sections hold executable instructions run at process startup and termination, respectively. C++ requires these for handling global initialisers and finalisers. Related sections include arrays of function pointers `.{name}_array` that exact startup and shutdown code. These array sections often have `SHF_ALLOC` and `SHF_WRITE` set.
		- The reason for `SHF_WRITE` set is to accommodate dynamic relocations. 
			- What often happens is that part of a loadable RW data segment is overlaid with a read only segment called `GNU_RELRO` such that it is a subsegment of the RW data segment; will be marked as read-only when done with dynamic relocations by a call to `mprotect`.
- **Read only data**
	- Sections like `.rodata` and `.rodata1` contain read only-data intended to contribute to a non-writable segment. `SHF_ALLOC` only.
- **Thread local storage**
	- If supported, sections like `.tdata`(initialised) and `.tbss` (uninitialised) hold data that is replicated for each execution thread. 
- **Section Groups**
	- Sections of type `SHT_GROUP` define a set of related sections that must be treated as a unit during linking, often appearing only in relocatable objects `ET_REL`. 

## Linker Control Scripts
Provide detailed control over the arrangement of the output file and the assignment of addresses, became necessary to provide finer-grained control over the arrangement of data both in target address space and in output file (often the case for embedded systems). 

The GNU linker `ld` accepts a configuration file `.ld` to permit control over how sections from object files are combined and mapped into the final executable or binary image. Control alignment, placement, define where data is loaded from, how sections are ordered. There are MEMORY blocks which define memory regions and where they start, and SECTIONS which declare where code and data, and other potentially loadable segments are to appear in memory. The ENTRY(symbol) linker script command is used to set the program entry point, predicated on the value of the symbol. Gets much more complex, provides refined control over linker output. 
## UNIX a.out Storage Allocation
Not much more complex than previous examples; the set of segments is known in advance. Each input file has `text`, `data` and `bss` segments and perhaps common blocks disguised as external symbols.

The linker collects the sizes of the text, data and bss from each of the input files via the header. Simply allocates the storage and places in output file and assigns addresses according to the a.out format being used, described in chapter 4 e.g., header considered part of text segment for QMAGIC and mapping program at start of second virtual page, cluster headers together, cluster text together, start data according after text according to format specification, and deal with bss accordingly.

## ARM7TDMI and ELF Storage Allocation
ELF storage allocation considerably more complex than predecessor a.out objects.

Of course, ELF has a unique nature in that loadable sections are emitted as segments which will actually reside in memory. ELF maintain a dual nature, and are organised on disk into an ELF header, program header table(optional for linking view), section header table(optional for execution view), and the actual section/segment contents. When the ELF is loaded, it creates more mapped regions consisting of file-backed pages described by program headers. 

To coalesce input sections from ELF objects such that a coalesced ELF object can be produced a number of steps are taken.
![[Pasted image 20251027173124.png]]


1. Collect all sections from all inputs
	Linker pulls together all input sections by type: `.text`, `.rodata`, `.data`, `.bss`, `.got`, `.plt` etc
2. Classify sections by runtime properties
	Determine `SHF_ALLOC`
	Determine access flags (RWX)
	Determine alignment requirement `sh_addralign` and size
	Determine whether loaded from file or can be zeroed in memory e.g., `.bss`
3. Form permission groups
	This is the initial grouping via *page permissions* according to headers:
	- Read+Execute - usually `.text`
	- Read Only - `.rodata`
	- Read + Write - `.data`, `.rel/a`, etc
	- Read + Write but zero initialised `.bss`
		- Avoid creating pages that are both W and X for security purposes. 
4. Order sections within each group
	The linker picks a conventional ordering of the sections of each permission group such that they can be arranged logically.
	- Text segment: ELF header & program headers, then `.interp`, `.init`, `.text`, `.rodata` etc
	- Data segment: remember mapped twice, at end of text too, so starts exactly one page after last text page, `.data`, small data sections.
	- BSS: right after data in memory. 
Thus all sections are now in segments, at the discretion of the linker, whilst trying to directly reflect the hardware execution environment. 
During the above process, the linker also notes which symbols will be resolved at run time from shared libraries and creates .interp, .got, .plt, and symbol table sections to support run-time linking.

ELF objects are not loaded anywhere near address zero, instead loaded in middle of address space, so the stack can grow down from below the text segment, and the heap can grow up from the end of the data. 








