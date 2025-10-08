## Managing free memory space (dynamic contiguous memory allocation schemes)

BITS BYTES KILOBYTES MEGABYTES GIGABYTES CONVERSIONS WITH POWERS
==Bit = 1 or 0==
==BYTE = 8 bits or 2^3==
==KILOBYTE = 1024 bytes or 2^10==
==MEGABYTES = 1024 KILOBYTES OR 2^20==
==GIGABYTES = 1024 MEGABYTES OR 2^30==

e.g., 4 KB = 2^12 = 4096
2n units natural for addressing memory
### Bitmap
==Simplest data structure== that can be used
Lets say:
- ==Memory is split== into ==blocks== of ==4KB== size (==2^12 bytes==)(==4096==)
- A ==bit map== is ==set up== so that if ==each bit is zero==, the ==memory== is ==free==, and ==1== if the ==memory is used etc==
- ==32 MB of memory = 32 x 2^20==
- ==2^20 / 2^12== = ==32 x 2^8== bitmap entries = ==8192==
==The size== of the ==bitmap== will ==depend== on the ==size of the memory== and the s==ize of the allocation unit.==

To find a ==hole== of say ==128KB==, do ==128==/==4=== = 32, must find ==32 adjacent bits== ==all set to zero, typically a long operation== , especially with smaller blocks.
==Tradeoff between size of bitmap== and ==size of blocks exists==, the s==ize of bitmaps can become prohibitive== for ==small blocks== and can ==make searching the bitmap slower==, but ==larger blocks may increase internal fragmentation==.
Rarely use bitmaps then


## Linked list
==Enables== to ==deal== with a ==variable number== of ==free== and ==used partitions==
- ==Linked list== contains ==number of entries==, ==mapped== to ==each memory block==
- ==Each entry== contains ==data items== e.g., ==start of memory block==, size, ==free/allocated flag==
- Also ==contains a pointer== to ==next in chain==
- ==Allocation== of ==processes== to ==unused blocks== becomes ==non-trivial==

### First fit
==Scans== the ==list== until a ==sufficiently large gap== is ==found==
- ==If== the ==space== is the ==exact size== the ==all== the ==space== is ==allocated==
- If the ==block== is ==larger== than ==requested size==, the ==block== is ==split== into ==2 parts==
	- ==First part==
		- ==Allocated to the process==
		- ==Marked as use==d
		- ==Its== ==size== is ==set== to the ==size requested==
	- ==Second part==
		- ==Represents remaining memory== after allocation
		- ==Marked as free==
		- Its ==size is leftover memory size==, creating ==new entry for leftover portion==
==May increase fragmentation,== ==small free partitions left between allocated partitions.== 
### Next fit
==Continues search== for ==free memory== ==from== ==where== it ==last left off==, if ==end of list is reached==, it ==wraps around== to the ==beginning.== 
- ==Gives even chance== to ==all memory== to ==get allocated== (first fit concentrates on start of list)
- ==Simulations shown== that ==next fit performs== w==orse on average== than ==first fit==, though allocator may be more efficient?
- Essentially a modified first fit, yet performs worse ie 

## Best fit
==Searches== for ==smallest block== that is ==large enough== to ==satisfy== ==allocation request==
- ==Scans all memory blocks==
- ==Allocate smallest hole== ==able== to ==satisfy memory demand==
- If ==block is larger== than ==needed,== ==split like in first fit.==
- ==Slower than first fit==, ==results in external fragmentation== due to ==small leftover holes.==
- ==O(N)==

### Worst fit
==Allocates memory== from ==largest available hole== in memory
- ==Scans all memory blocks==
- ==Allocates largest hole to satisfy memory demand==
- ==If block is larger than needed, split like in first fit==
- ==The left over part will still be large== (potentially more useful, ==aims to combat fragmentation caused== by best fit), ==not good in practice==
- ==O(N)==

### Quick fit
- ==Enhancement to traditional memory allocators==, ==optimising allocation speed== for specific use cases. 
- ==Maintains multiple lists==, ==each dedicated== to ==specific block size==, e.g., 64 bytes. 
- ==When a block== of a ==particular size is freed==, it is ==added to the corresponding list==. If ==exact size not available==, may ==use fallback algorithm.== 
- When ==process requests memory==, ==checks list corresponding to requested memory size==, and ==returns free memory== to list based on its size. 
- ==Ideal for frequent memory requests of simular size==, ==extremely efficient== to ==find required size hole== ==using== this ==algorithm==. But can lead to many tiny holes like best fit. 



#### Coalescing
==Takes place== when ==two adjacent entries== in the ==linked list== become ==free==, ==forming single larger== block to ==reduce fragmentation==. When b==lock deallocated==, ==freed block== has ==pointer checked t==o s==ee if adjacent to other free blocks==, if exist, ==converged to single free block==. ==Improves allocation efficiency==, esp with algorithms like first fit, ==reduces external fragmentation==. ==Coalescing can be immediate,== or ==deferred== e.g., until memory allocation request that cannot be satisfied. Adds additional overhead. ==Separate links are deleted O(n), and a single link is inserted==

#### Compacting
==Used to reduce external fragmentation==, ==rearranges allocated== and ==freed memory blocks residing in memory== to ==two contiguous blocks==. 
- ==All memory scanned==
- ==Allocated memory blocks== ==shifted toward one end of memory==, ==typically freezing process image== during this process, r==equires support for run-time relocation==
- ==Creates contiguous== ==free block== at ==end of memory==, ==algorithms like first fit== or next fit ==become more efficient==
- ==Complex to implement==, and ==time-consuming== depending on size of memory unit

## Overview so far
- ==Mono programming== is easy but does result in low resource utilisation
- ==Fixed partition facilitiates== multi-programming but results in internal fragmentation
- ==Dynamic partitioning and segmentation factiliate multi-==programming and ==reduce internal fragmentation significantly,== but ==result in external fragmentation== but ==allocation methods==, ==coalsecing and compacting alleviate.==
- Can we design a memory management scheme that resolves the shortcomings of contiguous memory schemes?
## Paging
==Paging uses fixed partitioning== and ==code-relocation== to devise a new ==non-contiguous memory management scheme.==
- ==Memory split== into much ==smaller fixed-size blocks==, called ==frames==
- ==Process is allocated== to ==one or more blocks== (e.g., 11KB process takes up three 4KB blocks)
- ==Blocks== do n==ot have to be stored contigious== in physical memory
- ==Process perceives them to be contiguous==
Benefits of non-contiguous schemes include:
- ==Internal fragmentation== is ==reduced== to few ==last blocks only== - if process does not fully utilise a page, remaining space is wasted.
- ==No external fragmentation==
 ![[Pasted image 20250115043102.png]]
==A page== is a ==small block of contiguous memory== in the l==ogical address space,== as seen by the process. ==Pages and frames commonly have the same size==, which is a power of 2, ==ranging from 512== bytes to ==1GB.== These ==pages== are then ==mapped== onto ==frames.==

Often u==sed in conjunction with segmentation.== 
Page table is a data structure used to store the mapping between virtual pages and physical frames

==Memory is linear array of bytes==, ==N address lines used== to specify 2^n addresses, ==each memory cell is a byte,== ==can address up to 64 KB== for ==16== bit machine(/1024). If split into 16 pages, then block size = 2^16/2^4 = 4KB

Address translation
==Translates virtual address== pages to ==physical frames.== 
==A logical address== is ==relative== to the ==start of the program==, or the ==start of the segment==. The l==eft most n bits== ==represent the page number,== the ==rightmost m bits== r==epresent the offset== within the page. In ==4 KB block==, ==4 bits== used for the ==page number==(16 pages), and ==12== bits used for the to ==represent== the ==offset== ==within the page==, which is the specific byte wuthin the page.

The ==page table== holds the ==page to frame mapping==. The ==page number== is used as an ==index to page table== that ==lists the number== of the associated frame.
- frameNumber=f(pageNumber)
==Offset within page== ==remains the same,== ==page number is simply replaced with frame number==. USED TO LOCATE EXACT BYTE WITHIN A PAGE, SPECIFIYING RELATIVE POSITION WITHIN A PAGE OR FRAME.

==Multiple base registers== will be ==required== for a ==process== with t==his== ==scheme==, ==one for each page==, ==set of base registers== ==stored in page table== and ==mapped to each page==. ==Every process has its own page table and set of base registers.==???
OS keeps track of free frames.

### Principles of paging
==Logical address space== is ==divided== into ==equal sized pages==
==Physical address space== is ==divided== into ==equal sized frames==
==Non-contiguous== ==memory management scheme== (although pages themselves are contiguous).
==A page table== contains multiple “==relocation registers==” to ==map the pages on to frames==
The ==offset== ==within the page== and ==frame== ==remains the same==
Reduced internal fragmentation ??
No external fragmentation??
==Principle of locality== - : ==code and data references are usually clustered==
==Not all pages have to be loaded in memory at the same time ⇒ virtual memory==
==Loading all pages/process is wasteful, desired blocks can be loaded on demand instead==


## Practice Q
![[Pasted image 20250115211921.png]]
8GB = 8192 MB 
Half free memory = 4096 MB
To represent each block in bitmap, need to do 8192/1 = 8182 Bits
To represent each block in linked list, need 4096 nodes. 32 + 16 + 32 = 80 bits, 80 x 4096 = 327680 bits
327680 / 8 = 40960 bytes = 40KB roughly

![[Pasted image 20250115212413.png]]
2^64 / 2^12 = 2^52 max number of frames
IDK
Virtual address space size = Number of frames (2^52) x frame size(2^12) = 2^64
Number of pages = Virtual address space / page size
2^64 / 2^12 = 2^52 entries
17/5 = 4.25 = 5 pages
4-3 = 1 KB

![[Pasted image 20250124151351.png]]
