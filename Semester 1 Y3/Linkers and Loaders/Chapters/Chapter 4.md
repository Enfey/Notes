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
Linkers must round segment sizes up to satisfy alignment constraints required by the architecture, which often means rounding to word boundaries. 


## ARM7TDMI and ELF Storage Allocation





