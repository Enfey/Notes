## Multi-level Feedback Queues
- An ==approach== to ==priority queues== that ==allows== ==process priority== to ==change dynamically== based on behaviour over time
- ==Jobs== can ==move between queues==
	- ==Prioritise I/O== and ==interactive processes==
	- ==Move== to ==higher priority== queue to ==prevent starvation== and ==avoid inversion== of control/priority inversion (they're the same thing.
- ==Can use== ==different scheduling== algorithms for ==each queue==, e.g., round robin, FCFS, SJF.
- ==Defining== the ==characteristics== of ==this algorithm== includes:
	- ==Number of queues==
	- ==Migration policy== between queues, 
	- ==Initial access== to the ==queues== ==and why==
	- ==Scheduling algorithms== ==used== for the ==individual queues==. 
- Highly configurable.

Consider 3 processes running on single CPU machine, processes p1 and p2 have initial priority 1, p3 has initial priority 2, scheduler concludes that p3 is being starved of CPU time, temporarily has priority promoted to prevent starvation, computation continues as before. 
![[Pasted image 20250124170424.png]]

### Advantages
- ==**Fairness**== - via ==demotion== of ==longer running process== to ==low priority queues== and ==giving shorter tasks== ==higher priority==, ==ensures balance== between ==response time== and ==throughput==
- ==**Prevents starvation**== - ==has mechanisms== to ==prevent==, e.g., ==periodically escalating priority== of ==long-waiting==/long running ==processes== which have ==yet to have first execution==
### Disadvantages
- ==**Overhead**== - ==overhead of moving processes== ==between priorities dynamically==, can take time depending on implementation 
- ==**Priority inversion**== - if L holds a resource R that H wants, but L is to run first, H is kept waiting, it is possible that a process M becomes runnable during L's use of R, pre-empting H via the medium term scheduler, and inverting the priorities.
- ==**Unpredictable**== - ==due to priority escalation== and potential for priority inversion, ==in comparison== to ==something== like ==FCFS== which is entirely ==deterministic==. 



### Multi-level feedback queues in windows 7
- An ==interactive system==, using a ==preemptive scheduler== with ==dynamic priority levels.== 
- Two priority classes with ==16 different priority levels== exist.
	- **Real time** - ==processes/threads== have a ==fixed priority level==
	- **Variable** - ==processes/threads== can have their ==priorities boosted temporarily.==
	- Round robin used within the queues. 
- ==Priorities based== on ==process base priority==(==0-15==) and ==thread base priority==(==+/-2 relative to process priority==). 
- A ==**thread's priority** **dynamically changes==** during execution ==between its base priority== and the ==maximum priority== ==within== its ==class==.
	- ==Interactive I/O bound processes== receive a ==larger boost==
	- Boosting priorities ==prevents starvation== and ==priority inversion== and ensures interactivity.
![[Pasted image 20250124170643.png]]



### Linux scheduling
- ==Been multiple iterations== of ==scheduling== over ==different versions== of ==Linux== to make efficient use of multiple processors/cores
- Linux ==distinguishes== between ==two types of tasks== for scheduling:
	1. ==Real time tasks==, divided into:
		- ==Real time FIFO tasks==
		- ==Real time Round Robin tasks==
	2. ==Time sharing tasks==:  which use a ==preemptive approach== which are ==similar to **variable**== in ==windows==
- ==Real time FIFO== have the ==highest priority==, ==scheduled== using ==FCFS==, ==pre-empted only== if a ==higher priority job== shows up.
- ==Real time round robin== are ==pre-emptable== by ==clock interrupts== and have a ==time slice== ==associated== with them.
- ==Neither== can ==guarantee== ==hard deadlines==

#### Completely fair scheduler:
**Imagine Hypothetical ideal scenario:**
- ==CPU allows N== current tasks to be ==run simulataneously==, each receiving 1==/N CPU power==
- ==5 tasks==, each gets ==20%== available ==computational power.==
-  Obviously ==cant run== arbitrary ==num of tasks in parallel== in ==this way== -  can we ==approximate this ideal?==
**Deciding how to divide up CPU time**:
- ==Choose target latency== - ==amount of time== before ==every task== gets ==access to CPU==. Bounds how far we will drift from being fair.
- To ==hit target latency==, for ==N tasks==, ==each task== is ==allowed to run== for ==1/N== of the ==target latency==
- E.g., target latency of 1ms, 5 tasks, each run for 0.2ms.
- To ==avoid excessive== ==context switching== ==N== is ==large== also ==choose minimum granularity==:
	- ==Minimum amount of time== allow a task to ==run on CPU== ==before being considered== for ==replacement==
**Approximating fairness**
- ==Record a virtual time== ==each task== has ==had on CPU== and ==order tasks== in ==ascending order== of ==virtual time used==, via red-black tree
- ==Task== with ==lowest virtual time== on CPU, ==considered to be treated== ==least fairly== and ==chosen next to run on CPU==
- ==Afte==r has ==had 1/N== of ==target latency== in ==virtual time==, ==replace== it with ==next task== with ==lowest virtual== run ==time==. 

**Accounting for priorities**
- ==Weighting scheme== used to take ==different priorities==, ==assume weight== is literally the ==task priority== (simplification)
- ==Recorded virtual time== on CPU ==is real time== on ==CPU scaled by the weight==. ==After 100 ms== of actual computation time:
	- A p==riority 1 process== ==considered== to have ==used 100ms== of ==virtual time==
	- A priority 2 process considered to have used 200ms of virtual time
- Target latency based on virtual run time as below. 

 Assume three tasks, T1, T2, T3, priorities 1, 2, 3. Target latency of 300ms.  T(v, r) indicates task T has had v units of virtual run time, r units of real time. State after 100ms of virtual time is:
 - CPU: T1(0,0), queue: T2(0,0),T3(0,0) 
 - CPU: T2(0,0), queue: T3(0,0),T1(100,100) 
 - CPU: T3(0,0), queue: T1(100,100),T2(100,50) 
 - CPU: T1(100,100), queue T2(100,50),T3(100,33) 
 - CPU: T2(100,50), queue T3(100,33),T1(200,200)
 - 


**Enhancements**
To ==avoid potential pathological behaviours==
- ==New tasks== have ==virtual run time== ==set== to ==current minimum== ==virtual run time==, would be ==unfairly advantaged== if ==set to zero,== would be able to run until reach current minimum freely.
- ==Blocked tasks== have ==virtual run time== set to ==greater of==:
	- ==Current minimum virtual runtime== ==minus small offset== to ==ensure== it gets to ==run==
	- Its ==old== ==virtual run time==

## Multi-processor scheduling:
- ==Single processor machine== involves ==deciding== which ==thread== to ==run next==
- ==Scheduling decisions== on a ==multi-core machine== involve ==which thread to run when==, and ==which thread== to ==run where.== 

### Shared queues
- A ==single== or ==multi-level queue== ==shared== between ==all processor== ==components.==
- **Advantages**: 
	- ==Automatic load balancing between executors.== Unlike private, which if overloaded on one CPU, leading to unfair distribution of work. 
- **Disadvantages**:
	- ==Contention for jobs==, ==simultaneous access==; ==need synchronisation mechanism== to avoid data corruption.
	- ==Does not take advantage== of ==current state== of ==CPU's cache== becomes ==invalid== when ==moving== to ==different CPU==  increasing processing time via memory access due to cache misses. ==TLBs become invalid==, must r==ecompute virtual== to ==physical address==.

### Private queues
- ==Each CPU== maintains a ==private queue(s)==. 
**Advantages**: 
	- Can ==often reuse== ==existing CPU state== such as cache and TLB
	- ==Contention for queue== is ==minimised==
**Disadvantages**: 
- ==less load balancing==, but to ==mitigate== can ==migrate tasks== ==between CPUs==, ==depending== on ==migration policy.==


## Related vs unrelated threads
- **Related**: ==Multiple threads== that ==communicate== with ==one another== and ==ideally run together== e.g., search algorithm
- **Unrelated**: ==process threads== that are ==**independent==,** possibly ==started== by ==**different users**== running *==*different programs**==

### Scheduling related threads:
- ==Threads== ==belong== to the ==same process== and are ==co-operating== e.g., ==exchange messages== or s==hare information.== 
- Process A has thread A0 and A1, A0 and A1 cooperate.
- Process B has thread B0 and B1, B0 and B1 cooperate
- The scheduler selects A0 and B1 to run first, then A1 and B0, and A0 and A1, and B0 and B1, ran on different CPUs
- Try to send messages to other threads, which are in ready state, and can deal with them once running. 
- ![[Pasted image 20250124185357.png]]

==Aim is to get **collaborating threads**,== ==running==, as ==much as possible==, at **==same time**== across ==**multiple CPUs**==

#### Space sharing scheduling
- ==**N related threads**==, typically from single process, a==llocated to **N dedicated CPUs**== when enough CPUs avaialble
- ==**M related threads==,** typically ==from another process==, ==kept waiting until== **==M CPUs**== are ==available==
- At a==ny point in time== ==available CPUs== are ==partitioned== into ==blocks== of ==related threads.==
- ==As threads complete==, their ==dedicated CPUs== are ==returned== to the ==collection== of ==available CPUs==
- The ==CPUs== are ==not multiprogrammed== to ==keep== ==related threads== ==running== at the ==same time==, this means ==blocking calls== r==esult== in ==idle CPUs.==
Keeps related threads running, lack of multiprogramming avoids context switching overhead, but leads to wasted CPU cycles


#### Gang scheduling
- ==Scheduler groups== ==related threads== into ==**gangs**== to ==run simultaneously== on ==different CPUs.==
- This is ==**pre-emptive** algorithm== with ==time slices== ==synchronised== ==across all CPUs==
- ==Blocking threads== result in i==dle CPUs==
	- If a ==thread blocks== the ==rest of time slice== will be ==unused due to time slice sync across all CPUs.==
