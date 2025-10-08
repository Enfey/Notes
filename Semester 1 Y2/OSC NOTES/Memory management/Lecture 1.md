# KEY POINTS TO REMEMBER
## Logical addresses vs physical addresses
- ==Logical addresses==, also called ==virtual addresses== are ==generated== by ==CPU== during ==program execution==, ==used within process== and are ==independent of physical hardware==. Each ==logical address== ==maps to a physicall address== ==via some offset== to a base address. Managed by MMU.
- ==Physical address== is that ==actual location in RAM,== and ==logical addresses== are ==translated into== ==physical addresses by MMU== using paging, or segmentation.

## Memory hierarchy
Computers have ==memory Hierarchies== 
- ==Registers, L1, L2, L3 cache==
- ==Main memory==
- ==RAM==
'==Higher memory==' is ==constructed== in ==different ways== to ==lower memory==, and is ==faster, more expensive==, and ==volatile==, whereas lower memory is less efficient, but cheaper and persistent.

The ==OS== provides a ==memory abstraction==, in that ==memory== can be ==seen== as one ==linear array== of ==bytes==/words. 
(==word== is ==fixed-size unit== of data that a p==rocessor can move== ==between memory== and the ==processor==, ==mapped to the width== of the ==data bus== and can be moved from memory to register vice versa in a single operation )

## OS responsibilities
- ==Allocate==/==deallocate== memory ==when requested== b==y processes==
- ==Keep track== of ==used== and ==unused memor==y
- ==Distribute memory== ==between processes== and ==simulate== an '==infinitely==' ==large memory space==
- ==Control access== when ==multi-programming is applied==
- ==Transparently move data== from ==memory== to ==disk== and vice/versa

## Approaches to memory historically
==Modern consumer electronics== often r==equire less complex memory management approaches==
Many early ideas, interestinhly, underpin modern memory management approaches such as **relocation**.

## Contiguous/fixed memory management models
**Definition**
==Allocate memory== as as ==single, continous blocks== of memory with ==no holes or gaps==.

### Contiguous approaches 
##### Mono-programming
==One single partition== for ==user processes== ie ==no multi==-==programming==/==multiple processes== ==dwelling in memory.== 
- ==Fixed region== of ==memory allocated== to OS/Kernel
- ==Remaining memory== is ==reserved entirely== for ==single process,== which is ==always in the same location==
- ==No protection== between ==user processes==, ==assumption== is ==improper memory cleanup== - leaves data vulnerable in memory, maybe ==introduces attack vector== etc.
- ==Process== has ==direct access to memory==, has no issues with resource contention
- ==Overlays enable the programmer to use more memory than available==
	- **Overlays** ==allow== a program to ==run even== if it’s larger than available memory by dividing the process into smaller parts called overlays
	- Keeping a root module in memory to manage which overlay is loaded
	- Swapping only the needed part of the program at a time
	- Requires careful programming, but allows more efficient memory usage
![[Pasted image 20250114210235.png]]
###### Disadvantages: 
- ==Direct access== to ==physical memory== -> ==may have access== to ==OS memory== ==without proper memory protection==
- ==OS== is a process ==dwelling in memory== -> have ==2 processes anyway==
- ==Multi-programming== is ==expected on modern machines==
- ==Low resource utilisation==, especially when waiting for I/O perations
Common in ==**embedded systems**== and **modern consumer electronics**

==Simulating multi-programming==
==Improves resource utilisation== by ==running multiple processes== in ==memory==
Can ==simulate swapping==: ==save current process image to disk== and ==load new one==( time consuming)
Can ==simulate threads==:: use ==threads within the same process== to ==overlap computation and I/O==, without needing to swap entire processes.

Example of resource wastage in Mono-programming
Disk rotation: Disk spinning at 7200 rpm takes 4.2ms to rotate half a track
CPU speed: CPU running at 3.2GHz can execute 3.2×10⁹ per second
During 4.2ms of I/O wait time, CPU could execute 13.44 million instructions




### Multi programming/Multiple processes
==Multiple processes== are ==loaded== into ==memory== and ==executed== '==simultaneously==', ==switching== between ==processes== ==during I/O waits,== ==improving== ==instruction throughput==. 
A ==process== ==spends== ==p percent== of its ==time== ==waiting== for ==I/O==
==CPU utilisation== is ==calculated== as ==1 - time all processes waiting for I/O== e.g., p = 0.9 then CPU utilisation = 1-p = 1-0.9 = 0.1
If ==n processes== are ==in memory==, the ==probabilit==y that ==all are waiting== for I/O ==simultaneously== is ==p^n==
CPU ==utilisation== is given by **==1-p^n==****
Formula ==assumes n processes== ==share the CPU==, as ==n increases== the ==probability of all processes waiting for I/O decreases, improving CPU utilisation.== 
As p increases e.g., 80% I/O, more processes needed to achieve reasonable CPU utilisation
![[Pasted image 20250114212603.png]]
At p=0.8p = 0.8p=0.8 (80% I/O wait) with 4 processes, CPU utilization is 1−0.84=0.59041 - 0.8^4 = 0.59041−0.84=0.5904 (59%).
Assume that:
==Computer has 1MB of memory==
==OS takes up 200KB==, leaving for ==four 200KB processes==
Then:
If ==have I/O wait time of 80%==, ==achieve just under 60% CPU utilisation,== add ==another 1 MB== allows us to ==run another 5 processes==, reaching 87% CPU utilisation (1-0.8^9).
==Assumption of multi-programming is that it can improve CPU utilisation .==
More complex models could be built using queueing theory, but we can still use this simplistic model to make approximate predictions

##### Multi-programming with fixed partitions
Seen above, ==Multi-programming== improves ==resource utilisation==
==This implementation== uses ==fixed== ==equal sized== ==contiguous partitions==
- ==Any process== can take ==any large enough partition==
- ==Allocation of processes== to ==partitions is trivial==
- ==Very little overhead== and ==simple implementation==
- ==OS== simply ==keeps track== of ==which partitions are being used== and ==which ones are free==
- ![[Pasted image 20250114214110.png]]
-
#### Disadvantages
- ==Low memory utilisation==
- ==Excessive internal fragementation== - a type of ==memory inefficiency== that ==occurs== when ==memory is distributed in fixed-sized blocks== and the ==memory allocated is larger== than the ==memory demanded==
- Requires the use of overlays
##### Multi-programming with non-equal sized contiguous partitions
==Use fixed== ==non-equal sized contiguous partitions.==
==Reduce internal fragmentation== 
==Must allocate carefully==
==NOT DYNAMIC, NOT ALLOCATED DEPENDING ON EXACT PROCESS REQUIREMENTS==
==Leads to extenral fragmentation== - ==occurs== when ==memory== is ==divided== into ==small blocks==, ==as allocated and deallocated==, ==small gaps== of ==unusable== but ==free== ==memory== ==accumulate==, and are often ==too small== to ==meet needs== of ==new processes==, even if total amount of free memory exceeds the demanded memory.
![[Pasted image 20250114214119.png]]
![[Pasted image 20250114224521.png]]

### Fixed partition allocation methods
==One private queue per partitison==
- ==Each assigns process== to ==smallest partition== would ==fit in==
- Reduces internal fragmentation
- Can reduce memory utilisation and result in starvation
	- e.g., could have lots of small jobs in one queue for large partitions
A single shared queue for all partitions can allocate small processes to large partitions but results in increased internal fragmentation

FINISH THIS

## Non-Contiguous memory management
**Definition**
Allocate memory smaller blocks and stored in non-adjacent memory locations,

