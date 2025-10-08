
## Code relocation and protection
==Relcoation== is the ==process== of **==adjusting addresses==** in a ==program== so it can **==execute correctly==** when ==loaded== at a **==memory address==** ==different== from the ==one== it was ==originally compiled== ==for==.

==Logical addresses== are ==memory addresses== s==een by processes,== ==relative== to the ==start of the program==. ==Assigned by the compiler==, they are ==independent== of ==physical memory addresses.==
### Static relocation at compile time
==Occurs== when ==program== is ==compiled== with ==assumption== it will ==always be loaded== into ==fixed memory address==. The ==compiler generates== ==absolute physical addresses== for the process, ==the program== can ==only run== if the ==OS loads== it into the ==pre-determined== ==physical location.==
- ==No runtime overhead== as ==addresses== are ==pre-determined,== but this also ==prevents runtime adjustments==
- ==Impractical== to ==always reside== in ==same memory location,== making it inflexible and limiting system resources

### Dynamic relocation at load time
- A ==programs logical addresses== are ==adjusted== at ==load time== to ==reflect== ==physical memory locations==, ==achieved== by ==adding an offset== to ==every logical address== ==assigned== by the compiler. 
- ==Compiler assumed== ==process== will ==run== at ==address 0==. The l==ogical address space== must be ==mapped onto physical address space== ==once location of load is known.== 
- This ==relocation== ==must be used in any OS== that ==allows processes== to ==run at changing locations== in physical memory. 
- The ==OS determines== the ==base address for process==, and ==loader adds== base address to ==all logical address== to ==calculate physical addresses==. ==Increases load time==, due to address calculation. 
- ==Flexible==, as ==programs== can be ==loaded== into memory locations. 
- ==Load time relocation== does ==not account== for ==processes being swapped in== and ==out of memory,== ==if so==, ==must be reloaded== into ==same physical relocatio==n. 
![[Pasted image 20250114223518.png]]
Due to lack of hardware support, and large overhead in terms of performance to do this with software, this relocation has little support for swapping.
### Dynamic relocation at runtime with hardware support
![[Pasted image 20250114223630.png]]
==Two special purpose registers== are ==maintained== in the ==CPU==, well, the ==MMU==, containing a ==base address== and ==bound==
- The ==base register== ==stores== the ==start address== of the ==partition==
- The ==bound register== holds the ==size of the partition==, and ==can also be used== to ==ensure that processes== ==cannot access memory== ==outside of allocated region==.
At load time:
- ==The base register== is ==added== to the ==logical address== to ==generate the physical address==
- ==The resulting address== is ==compared== against the ==bound register,== which ==triggers an interrupt== if it ==exceeds the bound of the partition==. 
## Protection
==Must be enforced once you can have multiple processes in memory at the same time.==

## Dynamic partitioning and swapping
- ==Contiguous memory scheme== which ==allocated memory== ==dynamically for processes== ==depending== on their ==exact memory demand,== ==eliminating internal fragmentation==, ==in comparison== to ==fixed partitioning schemes.== 
- A ==variable(dynamic) number== of ==partitions== ==reside== in ==memory== at ==any given time.== 
- ==Partitions== are ==created== during r==un-time== ==according== to ==process demands==, varying at runtime according to process need
- ==Need to track free space==. ==Potential need for compaction==. ==Leads to holes via swapping.== 
- Use segmentation to reduce external fragmentation
![[Pasted image 20250114225856.png]]
What does efficient dynamic partitioning require:
	==a method to keep track of free memory==, a == == memory when requested by process.
### Swapping
==Moves processes== between ==drive== and ==main mamoey==
==Reasons for swapping:==
- ==Some processe==s ==only run== ==occasionally==
- ==Total amount of memory required== ==exceeds available memory==
- ==Memory requirements== ==change during runtime== (stack/heap growth)
- We ==have more processes== ==than partitions==(assuming fixed partitions)
==With dynamic relocation at runtime with hardware support==, ==processes== can be ==loaded== into a ==different memory location==, ==base register== ==simply changes.== 

==External fragmentation== ==caused== by ==dynamic partitioning== however, as ==swappning a process out of memory== will ==create 'a hole'== and is ==time consuming==
==New processes== may ==not use== the ==entire hole==, leaving ==unused block==
==New processes== ==may be large== for a ==given partition== and t==he overhead== of ==memory compaction== to ==recover holes== can be ==prohibitive==


### Compaction
==Technique== used to ==address external fragmentation== in systems that use ==dynamic partitioning==. ==As processes== are ==loaded and terminated,== ==free memory== ==scattered across system in small==, ==non contiguous blocks,== ==not usable== for ==larger processes==. ==Goal== is to ==reorganise processes== in memory to consolidate 'holes'. 
==Time consuming== as must i==dentify fragments==, ==pause processes== and ==ensure memory contents== are ==untainted==, ==all active processes== are ==moved toward lower addresses==/beginning of memory. ==Updates things such as PCB, page tables, segment tables to reflect new locations of processes==, and updates references to physical memory locations by recalculating them. T==Then all the memory at the end is consoldiated==, and processes can resume. Not done frequently.
## Segmentation
==Dynamic partitioning== ==loads entire logical address space== into ==contiguous physical memory==, using a ==single base and bounds pair== per process, whilst r==esulting in external fragmentation==. ==Unused space== between ==stack and heap== ==takes up physical memory==.

==Segmentation== ==loads only== ==relevant sections== into memory. ==Divides memory== into ==segments based on logical divisions of program== which are allocated separately==.Splits the logical address space into== s==eparate contiguous segments==. ==Each segment is loaded separately== into (contiguous) memory ==resulting in each segment== have a ==different base and bound pair==. This ==pair is stored== in a ==segmentation table==, and ==part of the logical address== is ==used as an index into the segmentation table.==
==Segments== can have ==protection bits associated with them== (RWX). Read bit indiccates segment can be read, write indicates segment can be written to, X indicates sgement can be executed. 
==Base address== is ==position relative to start of segment.== 
Segments usually refer to code segment, data segment, stack segment etc.
OS remains responsible for finding sufficiently large contiguous range of physical memory for each segment. 