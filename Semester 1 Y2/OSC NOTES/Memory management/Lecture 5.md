RECALL
VIRT MEM RELIES ON LOCALITIES THAT CONSTITUTE GROUPS OF PAGES WHICH ARE USED TOGETHER, IE EXPLOITING SPACIAL LOCALITY, PROCESSES MOVE BETWEEN THESE LOCALITIES and is exploited to reduce page faults.


## Inverted page table
==Typical page table sizes== are ==proportional== to the ==number of pages== in the ==virtual address space==. ==Significant for 64 bit systems==, number of pages explodes.
E.g., ==32 bit virtual address space== = ==2^32 addresses==
==Page size = 4KB = 2^12==
==Number of pages== = ==Virtual address space== / p==age size = 2^20==
Page table entries, mapped to system bits so would be 32 bits, so 2^20 x 4 bytes = 4MB page table size.

An ==inverted page table size== is ==proportional== to the ==size== of ==main memory==
- Contains ==one entry== for ==every frame,== n==ot for every page==
- ==Is indexed== by ==frame number==/hash code, ==not by page number==
- ==When referencing memory==, ==OS must search inverted page table for a corresponding entry==, could be slow
- ==Inverted page tables== use ==hash function== to ==transform page number== (==n bits==) and PID into an index in inverted page table. 

### Inverted page table entries
- ==Process identifier (PID) :== ==Process that owns this page==
- ==Page number==(logical address space) 
- ==Protection bits== (RWX)
- ==Link pointer==: f==ield points towards next entry==(==collision resolution?==) - if ==two or more VPN + PID== combinations hash to same index, ==IPT uses this at hashed index== to s==earch linearly through other potential matches== to find ==correct frame mapping.== 

### Lookup process
To find physical address corresponding to given virtual address
- ==Virtual page number==(==VPN==) and ==PID== are ==hashed== to ==search== for a ==matching entry== in the IPT
- ![[Pasted image 20250116000409.png]]
- ![[Pasted image 20250116012757.png]]
- WORKS MORE LIKE SECOND IMAGE APPARENTLY
==PID== is r==equired== as a ==single page table== is ==shared by all process,== ==unlike traditional page table==s where ==each proces==s ==maintains its own table.== ==Used to uniquely distinguish==, as ==just using VPN could result in collisions.== 

### Advantages
==Memory efficient== - significant reduction in memory usage for systems with large virtual address spaces e.g., 64 bit systems as IPT size is proportional to number of physical frames
==Global structure== - single IPT handles all processes, ==eliminating need for separate page table== for ==each process== and ==multi-level page tables.==

### Disadvantages
==Logical to physical address translation becomes slower==, though this c==an be optimised using things like TLB==
==Collisions== have to be ==handled== and will ==slow down address translation.==

## Paging decisions
==Two key decisions== have to be made u==sing virtual memory.== 
1. ==What pages are loaded and when -> page fetching policies==
2. ==What pages are removed from memory and when -> page replacement algorithms==
Pages are ==**Shuttled**== between primary and secondary memory.

### Page fetching
#### Demand paging
==Method== of ==virtual memory management==, which ==copies disk pages== into ==physical memory== ==only== when an ==attempt== is ==made== to ==access it== and the ==pag==e is ==not a member== of the ==resident set==. Essentially ==lazy loading== pages.
1. ==Attempt to access page==
2. ==If page is valid==, continue ==processing instruction as normal==
3. ==If page is invalid== then a ==page-fault trap occurs==
4. ==Save registers/process state==
5. ==Check is memory reference== is ==valid== to a ==location== in ==secondary memory==, if not, then terminate process for illegal memory access.
6. ==Issue disk I/O==
7. ==Context switch== (optional)
8. ==Interrupt for I/O completion==: analyse interrupt from disk, store process state/registers, update page table (page now in memory)
9. ==Context switch to original process.==
==First instruction immediately== ==causes page fault,== ==more page faults== will ==occur==, but ==stabilises== over time ==until moving to next locality==. 
The ==set of pages used at a given point in time is called its working set.==


#### Pre-paging
==Aims== to ==combat== ==large number of page faults== caused ==by initial process load== and ==locality switch== of ==demand paging==. ==Technique== that ==seeks to load== ==multiple pages== into ==memory== ==before== they are ==explicitly requested== by the process, ==done in anticipation of process' future memory access patterns==. ==Useful when restarting a process,== as ==all pages expected to be used==, so ==could bring everything into memory at once==, ==drastically reducing the page fault rate==. 
==Need to keep track== of ==which pages== m==ake up the working set:==
- ==Use an aging algorithm== e.g., ==reference bits== that are ==shifted== ==depending== on ==last access==.

==Avoiding uneccessary pages== and ==page replacement== is ==important!==
Let:
- ma denote the memory access time
- p denote the page fault rate -> 1 - p is the hit rate
- pft deonote the page fault time
Effective access time is given by:
	Ta = (1-p) x ma + (pft x p)
	Not considering TLBs
For a single level page table for example
- Memory access time of 100ns(10^-9)
- Two memory accesses required(200^-9)
- A page fault time of 8ms(10^-3, hard drives slow af)
	Ta = (1-p) x 200 +p x 8000000
	==Access time is essentially proportional to page fault rate, and so must be avoided at all costs==


### Page replacement algorithms
==OS== must ==choose== a ==page== to ==remove== when a ==new one== is ==loaded== and all are occupied. ==Choice made== by ==page replacement algorithms==, accounting for:
- ==When page was last used==/==expected== to be ==used again==
- ==Whether page== has ==been modified==(==only== ==modified pages== ==need to be written back to disk==, as contents differ, aka clean pages). 
==Decisions need to be made intelligently==

#### Optimal page replacement
In ==ideal/optimal world==
- ==Every page labelled== with ==number of instructions==/==length of time== ==before== it is ==used again.== Essentially k==nows the future access pattern==/needs to know it. 
- ==Page which will not be referenced for the longest time== is o==ptimal one to remove.==
Infeasible in practice, can be used to ==provide a lowerbound on number of page faults== in a ==controlled environment==, as always makes the optimal choice. Used for comparison with other algorithms and can be used for post-execution analysis. 

#### FIFO page replacement
==One of simplest page replacement algorithms== used in virtual memory management. ==Maintains a queue of pages in memory==, ==ordered by their arrival time==. When ==page loaded into memory==, ==added to end of qyeye==. When a ==page fault occurs,== ==FIFO selects front of queue==(oldest page) ==for replacement,== and ==evicts it==, ==loads the new page==, and ==adds it== to the ==back of the queue.==

- ==Easy to understand + implement==
- ==Low computational overhead to append and remove head and never traverse.==
- ==Performs poorly== -> ==heavily used pages== are ==just as likely== to ==be evicted== as ==lightly used pages==
![[Pasted image 20250116010700.png]]

##### Second chance fifo
==Improvement== to ==basic FIFO== algorithm. ==Introduces additional== ==second change== for ==pages== that have been ==accessed recently==. ==Each page== in ==queue== is associated with ==reference bit,== ==altered depending on access time==. ==Head of queue evicted== if ==not been referenced.== If ==reference bit is set==, ==page== is ==placed== at ==tail== of ==lis==t, and has ==reference bit reset==, giving it ==second chance==. ==Degrades to FIFO== if ==all pages were initially referenced==. ==Requires multiple scans through the queue==, ==leading to increased replacement time==. 
###### Clock replacement
==More efficient way== to ==implement Second chance FIFO==, by ==maintaining a circular list==/queue, and ==queue is managed== by a ==clock hand== that ==points== to ==next page== to be checked. ==When page accessed==, ==ref bit set to 1==, ==when page fault occurs==, ==clock hand(iterator)== which is ==pointing to last examined page in list==, ==inspects ref bit==. ==If r = 0,== ==new page put in place of page== 'hand' points to. ==Otherwise==, ==R bit cleared==, c==lock hand is incremented,== ==process repeated==. ==Removes computational overhead== of ==evicting head== and ==pushing to back of queue==, keeps in place, and uses a list. ==Slow for long lists.==


//LEC 6 BUT FLOWS NICELY INTO WHAT COVERED HERE>
#### Not recently used
==Referenced== and ==modified bits== are ==kept== in the ==page table== ==corresponding== to ==parcticular entry==. ==Referenced bits set to 0 at start==, and ==reset periodically== e.g., ==system clock interrupt, or searching list.==
==Four different page 'types' exist==
- ==Class 0: not referenced and not m==odified
- ==Class 1: not referenced and modified==
- ==Class 2: referenced and not modified==
- ==Class 3: referenced and modified==
==Referenced set when page accessed.== 
==Page table entries== ==inspected== upon every ==page fault==
- ==Find a page== from ==class 0== to ==be removed==
- ==If step 1 fails==, ==scan looking== for ==class 1== and ==set reference bit== to ==0== for ==each page visited==
- If ==step 2 fails,== start again from step 1 (==elements from class 2, have now become class 0 or 1.==)??
- ==NRU provides good performance== and is ==easy to understand and implement.== 
==Simple approximation of LRU strategy==




#### Least recently used
This ==greedy algorithm== seeks to ==evict the page== that has ==not been used== the ==longest==, ==using== a ==defined counter== ==rather than== an ==approximation==. ==Kernel tracks== when ==page== was l==ast used/referenced==, and e==very page table entry== contains a ==field for the counter==. ==Based on locality of reference.== 
==Costly implementation== since it ==requires a list of pages sorted in the order== in ==which they have been used==. ==Can be implemented== in ==hardware== using a ==counter== that is ==incremented== after ==each instruction==. 

![[Pasted image 20250123034634.png]]
![[Pasted image 20250123034651.png]]



### Working set
==Working s==et of a process ==refers to subset of pages== that a process ==actively uses== during a ==specific period of time==, ==dynamic set== which ==changes according to localit==y and ==whether page is dormant== or active. Depends on window of time t considered. 

Official definition:
==The working set W(t, k)== comprises the ==set referenced pages== in the last ==k, t.==
The working set is a function of time t, and k.
Where |
	 ==k = set of pages== ==used within== a ==pre-specified time interval== and/or most recently used pages. 
	 ==t = window of time considered.== 
	 
==W(t,k)|== is then ==variable in time.== Specifically:
	==1 ≤ |W(t,k)| ≤ min(k,N)==
	Where N is the number of pages of the process. 
	Bounds the size of the working set :
		==At least 1.==
		 ==Upper bound can be at most k==, or ==N if process has only referenced N pages total ie process entirely fits in memory and is extremely compact==.
![[Pasted image 20250123040520.png]]
At time t1, W(t1, 3) contains 3 most recently referenced pages. etc.
==Choosing right k value is paramount:==
- ==Too small, inaccurate, pages are missing==
- ==Too large: too many unused processes present==
- ==Infinite: all pages of process are in the working set==

==Working sets== can be used to ==guide the size of resident set==s, we ==monitor this set== and ==evict pages== from ==resident se==t that ==do not appear frequently in working set.==

==Costly to maintain== -> ==page fault frequency==(PFF) ==can be used as an approximation==, ==if PFF is increased==, ==may need to increase==. I==f PFF is very reduced==, we m==ay try to decrease.==
### Resident set
==Resident set== of a process ==refers to set of pages== that are ==currently loaded== in ==physical memory==. Includes ==all pages== that the ==operating system== ==has kept in memory==, ==regardless== of w==hether they were recently accessed== or not. 

#### Size of the resident set
- ==Small resident sets== enable to ==store more processes== in memory -> improved CPU utilisation
- ==Small resident sets== may ==result in more page faults==
- ==Large resident sets== may ==no longer reduce page fault== (diminishing returns)
==Exists tradeoff== between ==sizes of resident sets== and ==system utilisation==.

For ==variable sized resident sets==, ==replacement policies== can be:
- ==Local== - ==a page of the same process is replaced==
- ==Global== - ==a page can be taken away from a different process.==
==Variable sized sets== require ==careful evaluation of their size== ==when a local scope== is ==used==, based on working set or PFF.

#### Global
==Global replacement policies== can ==select frames== from the ==superset of all resident sets==, ie the ==global set,== they can be 'taken' from other processes. 
	==Frames are allocated dynamicall==y to ==processes based on demand.== 
	==Processes cannot control their own PFF==, the ==PFF of one process== is ==influenced by other processes.== 

#### Local
==Local replacement policies== can ==select frames== that are ==allocated to the current process==. ==Every process== has a ==fixed fraction of memory==, the ==locally oldest page== is ==not necessarily== the ==oldest global page.==

Page replacement algorithms explained before can use both policies. 
Windows uses a variable approach with local replacement. 

### Paging daemon
==Autonomous system process== which aims to ==prepare for page eviction== ==before the need arises==. ==Wakes up when free memory becomes low== and ==selects pages to evict==, ==via analysis of the working set==. ==Selects pages to evict using page replacement algorithms,== if ==too few frames are free==, and ==schedules disk writes for dirty pages that are to be evicted.==

==More efficient== to ==proactively keep a number of free pages== for ==future page faults==, o==therwise have to find a page to evict, and if modified, write it to the drive==, increasing page fault time. 

==Can be combined with buffering== (==free and modified lists==) => write the modified pages but keep them in main memory when possible, allows delaying of write operations.
### Thrashing
==Performance problem== that ==occurs when kernel== ==spends majority of its time== ==swapping pages== between ==RAM and disk== ==rather than== ==performing work.== ==Assume all pages== are i==n active use==, and n==ew page== ==needs to be loaded==, the ==evicted page== will ==have to be reloaded soon afterwards==, then thrashing occurs. 

==CPU utilisation too low==, so ==scheduler increases degree of multi-programming==, ==frames allocated to new processes==, and ==taken away from existing processes==.
- ==I/O requests subsequently queued up== as a consequence of page faults
==CPU spends more time managing memory==, ==scheduler increases degree of multi-programming etc etc==. 

Causes of thrashing include:
- ==degree of multi-programming is too high==,ie total demand exceeds supply
- ==An individual process== is ==allocated too few pages==
==Can be prevented== using ==good page replacement policies==, ==paging daemon==, ==reducing degree of multi-programming==(medium term scheduler - responsible for moving processes out of memory to prioritise others).
PFF can be used to determine whether system is thrashing. 

![[Pasted image 20250123043647.png]]
![[Pasted image 20250123043654.png]]
![[Pasted image 20250123043702.png]]
![[Pasted image 20250123043720.png]]
![[Pasted image 20250124232421.png]]
2^42 / 2^12 = 2^32
2^32 / 2^12 = 2^20
3 levels
15 + (3 x 90) + 90 = 375ns for miss
15 + 90 = 105 for hit

(0.98 x 105 ) + (375 x 0.02) = 110.4ns
