## Address translation implementation
1. ==Extract== the ==page number== from the ==logical address==
2. ==Use== the ==page number== as an ==index== to ==retrieve== the ==frame number== in the ==page table==
3. ==Substitute== the ==page number== with the ==frame number==, and ==place this== on the ==memory bus== to ==access this frame.==
Hardware implementation
1. ==MMU== ==intercepts logical addresses== to be read/written/executed
2. ==MMU uses a page table as above==
3. ==Resulting physical address== is put on ==memory bus.==
. ![[Pasted image 20250115213136.png]]
## Virtual memory and page faults
==The resident set== refers to ==pages in memory,== ie that is ==pages== that ==currently have== ==memory allocated to them in RAM.== 
A ==page fault== is ==generated== if the ==processor attempts== to ==access== a ==page==/frame that is ==not currently in memory==/has been s==wapped out.== 
- Pague fault r==esults in an interrupt==, ==transitioning the process to the blocked state==
- An ==I/O operation is kickstarted== to ==retrieve the page== from ==secondary storage== into ==RAM== (swapping)
- A ==context switch== (may) t==ake place==
- ==Another interrupt== ==signals the I/O operation is complete.==
### Virtual memory
==Memory management technique== to ==greatly improve== r==esource utilisation== by ==temporarily== ==storing pages== in ==secondary memory==. 
1. ==Improves CPU utilisation== 
	- ==Individual processes== ==take up== ==less memory== (only partially loaded) -> ==more processes in memory at once==
	- ==More efficient use of memory== 
		- ==No external fragmenation==
		- ==Reduces internal fragmenation== - ==by allocating memory in fixed sized blocks via pages==, which are ==often quite smaller==, and ==smaller than the size of a processes segments==, leads to there being ==fewer unused bytes in each block.== 
1. ==The logical address space can be larger than the physical address space, 64 bit machine -> 2^64 logical addresses theoretically==
### Page tables revisited
==ENTRY CONSISTS OF:==
- ==Present/absent bit set== if the ==page/frame== is i==n memory==
- The ==modified bit== is ==set== is the ==page/frame== has been ==modified==
	- Only ==modified pages== ==have to be written back to disk when evicted.==
- The ==referenced bit== that is ==set if the page is in use==
- ==Protection and sharing bits: RWX==
==![[Pasted image 20250115215559.png]]==
==LOGICAL ADDRESS = PAGE NUMBER + PAGE OFFSET==
### Page table size?
For a ==16 bit== machine, assume that 10 bits used for offset (2^10)
6 bits can be used to number pages
2^6 = 64 pages can be maintained
For a 32 bit machine, ==assuming pages of size 4KB, 20 bits can be used to number the pages, 12 bits used for offset (mapped to the page size, to address its memory).== 
==Ie 2^20 pages can be maintained, approx 1 million.==
==Leads to 4MB page table at 4 bytes per page table entry.== 

==How do we deal with the increasing size of page tables, and where do we store them?== 
- Their ==size prevents them== from being ==stored in registers==
- I==ncreasing the page size== ==reduces== the ==page table size,== but ==increases fragmentation==
Speed: ==address translation== happens at ==every memory reference,== so ==has to be fast==
- Accessing ==main memory results in memory stalls==
- How can we maintain ==manageable size and acceptable speed==


## Multi-level page tables
==Solution to the above issues.== ==Organises virtual address space== into ==hierarchical page tables.== The ==entries of level 1 page table== are ==pointers to a level 2 page table== and so on and so forth. T==he entries of the last level page table== ==store actual frame information==. ==Only the outermost page table resides in main memory==, and ==other page tables== will n==ot be brought to main memory unless required,== and are stored in virtual memory due to their size. 
==Page number is now divided into:==
- I==ndex into the root page table==
- ==An index into the second level page table containing the page to frame mapping(assuming 2 levels)==

![[Pasted image 20250115221432.png]]
![[Pasted image 20250115221541.png]]
==Assume that a fetch from main memory takes T nano seconds:==
	==With a single level page table, access is 2 x T==
		==With two page table levels, access is 3 x T==
With 2 levels, every memory reference already becomes 3 times slower, with memory access already forming bottleneck under normal circumstances - and this is under assumption that second page table level is in main memory. 
Multi-level page tables contain up to 5 levels in practice, linux.
==How do we maintain the speed of address translations.== 
## Translation look aside buffers
==Crucial hardware component== used to o==ptimise virtual address== ==translation==. ==Typically part== of ==MMU in processors==. ==Specialised cache== that ==exploits temporal localit==y - storing recent translations of virtual addresses to frame numbers. ==Can be searched in parallel==. ==Locality states that processes== make a l==arge number of references to a small number of pages.== 
![[Pasted image 20250115222222.png]]
### Example access speed
- Assume a single-level page tale
- Assume a ==20ns associative TLB lookup==
- Assume a 100ns memory access time
	- Access time for TLB hit = ==20 + 100==
	- ==Access time for TLB miss = 20 + 100 + 100 ??==
- ==Performance evaluation==
	- ==Access time for 80% hit rate==
		- ==120 x 0.8 + 220 x 0.2 = 140ns==
	- ==Access time for 98% hit rate==
		- ==120 x 0.98 + 220 x 0.02 = 122ns==

![[Pasted image 20250115222728.png]]
![[Pasted image 20250115222740.png]]
OFFSET IS MAPPED TO PAGE SIZE
