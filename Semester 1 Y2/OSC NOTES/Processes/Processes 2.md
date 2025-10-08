## Scheduling

- ==OS responsible== for ==managing== and ==scheduling processes== - ==relies== on a component - ==the scheduler:==
	- ==decide== ==which process== to ==run next,== using a particular ==scheduling algorithm== to do so. 
	- ==OS decides== ==when to admit processes== to the system(==new->ready==), 
	- ==decides which process to run next==(==ready->run==), and d
	- ==decides when and which processes to interrupt==(==running->ready==). 
- ==Type of scheduling algorithm== is ==influenced== by ==type of OS==, e.g., batch vs real time. 

### Scheduling classification by time horizon:
- **Long term** - 
	- ==Controls== the ==degree== of ==multiprogramming== via ==deciding== which ==processes== to ==admit to the system== (ie new->ready)
	- Usually ==absent== in ==modern OS==. Invoked very infrequently.
- **Medium term** 
	- ==Controls swapping== and the ==degree of multi-programming via this==
- **Short term** -
	- ==Decides== which ==process== to ==run next==
	- ==Manages== the ==ready queue==
	- ==Invoked very frequently== and so ==must be fast==, often ==called== in ==response== to ==clock interrupts==, ==I/O interrupts==, or ==blocking system calls==.
Where do these fit in with state transitions?
### Scheduling classification by approach
**Non-preemptive:** 
	- ==Processes== are only ==interrupted voluntarily== e.g., ==I/O operation== or ==syscall via yield()==. 
**Preemptive**:
	- ==Processes== can be ==interrupted forcefully== or ==voluntarily==, ==typically driven== by ==interrupts== from a ==system clock==, 
	- ==Prevents CPU time monopolisation==, but requires ==additional context switches== which ==generate overhead==. Most modern OS use this..

### Performance assessment:
==Criteria can be conflicting== e.g., ==improving response time== may r==equire more context switches== and will ==increase turnaround time== and decrease throughput. 
#### User oriented criteria
- **Response time** - ==minimise time== between ==job creation== and its ==first execution==
- **Turnaround time** - ==minimise time== between ==job creation== and ==completion==
- **Predictability** - ==minimise== the ==variance== in ==processing times==

#### System oriented criteria
**Throughput** - ==maximise== number of ==jobs processes== ==per hour==
**Fairness** - ==is CPU time==/wait time ==equally distributed==, are ==some processes== being ==starved of resources==?

### Scheduling algorithms:
1. ==FCFS==(first come first serve)
2. ==Shortest job first==
3. ==Round robin==
4. ==Priority queues==

Performance measures used - 
**Average response time**: ==average== of ==time taken== for **==all==** ==processes== to ==start.== 
**Average turnaround time**: ==average== ==time taken== for **==all==** the ==processes== to ==finish.== 

#### FCFS:
- A ==**non-preemptive**== algorithm that ==operates== as a ==strict queueing mechanism== and ==schedules processes== in ==same order== that they were ==added to the queue==.
- **Advantages**: ==positional fairness== and ==easy to implement==
- **Disadvantages**: ==favours longer processes== over short ones d==ue to zero preemption==, and ==could compromise== ==resource utilisation==

![[Pasted image 20250124153126.png]]


#### Shortest job first:
- A ==**non-preemptive algorithm**== that ==starts processes== in ==order of ascending processing time== using a ==provided==/known ==estimate of processing==
- **Advantages**: ==Always result== in ==optimal turn around time==
- **Disadvantages**:
	- ==Resource starvation== may ==occur== as ==long processes== may never ==execute==, 
	- ==Fairness== and ==predictability== are ==compromised,== 
	- ==Processing times== have to be ==known beforehand,== may be ==difficult== to ==predict length== of ==next CPU burst== ==accuratel==y. 
![[Pasted image 20250124153638.png]]
Why does this give optimal turn around times?
- ==Assume 4 processes p1, p2, p3, p4==
- Assume ==processes run in order== and the ==ith process== requires ==Ti milliseconds==
- Let ==Si== be the ==turnaround time== of the ==ith process==

==s1 = T1== 
==s2 = T1 + T2== 
==s3= T1 + T2 + T3== 
==s4 = T1+T2+T3+T4==

==So avg turnaround time = 4T1 + 3T2 + 2T3+ 1T4 / 4==

==Where T1== is the ==lowest amount of time== in ==ms== to ==achieve first completion==, it ==appears the most== in the ==average calculation==, thus ==minimising the average turnaround time== by ==reducing the average wait time by frontloading the shortest jobs==.

#### Round robin
- ==**Pre-emptive**== version of ==FCFS== that ==forces context switches== at ==periodic intervals== or ==time slices==.
- ==Processes== ==run== in the ==order== they ==added to the queue==, processes  ==forcefully== interrupted by the ==timer==.
- **Advantages**: 
	- ==Improved response time==, and ==effective== for ==general purpose== ==time sharing systems.==
	- ==Reduces risk== of ==starvation== ==due to long processes== ==via== use of ==context switches.==
	- ==Fair to all processes== as ==every job== gets a ==time slice.==
- **Disadvantages**: 
	- ==Increased context switching== and thus overhead,
	- ==Favours CPU bound processes== which usually run long ==over I/O processes== (which do not usually run long).
	- ==Performance depends== ==heavily== on t==ime slice length==:
		- Assuming multiprogramming system with pre-emptive scheduling and a context switch time of 1ms:
		- The ==smaller the time slice length==, the ==better response time==, but ==high context switch overhead==, and ==low throughput==. 1ms.
		- ==The larger==, the ==increased throughput==, but ==behaves like FCFS== and ==leads to high response time==. 1000ms

![[Pasted image 20250124154838.png]]


#### Priority Queues
- A ==**preemptive algorithm**== that ==schedules processes== based on ==differing priority.== 
- A ==**round robin**== is ==used within== the ==same priority levels==. ==Process priority== saved in ==**PCB**.== 
- ==Implemented== in terms of ==multiple queues==, ==one for each priority level==. 
- Advantages: 
	- ==Can prioritise I/O bound jobs==
	- C==an optimise system performance== ==based on== ==importance== ==of tasks==
- Disadvantages: 
	- ==low priority processes== may ==suffer== from ==resource starvation== (when ==priorities are static== that is.)
	- Can ==increase overhead== due to ==priority elevation== and changes. 
![[Pasted image 20250124155334.png]]

![[Pasted image 20250124155349.png]]
Solution: Assuming time slice of 15ms
Sequence: A(15)->B(15)->A(15)->B(15)->A(15)->B(7)->A(15)->A(7)->C(14)->D(15)->D(1)


0+15+104+118  / 4
82+104+118+134 / 4












Average response time: (0+15+104+118) /4 (the ms it gets to initially run at)
Average turnaround time: (82+104+118+134) /4 (the ms it completes at)
