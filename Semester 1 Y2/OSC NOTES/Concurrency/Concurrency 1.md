## Concurrency
- ==Threads== and ==processes== execute ==**concurrently**== or in ==**parallel**== and can ==share resources== e.g., devices, memory, variables, data structures.
- A ==thread== is ==free== to be ==interrupted== at ==any time==, ==either by timer== or ==due to I/O== action. ==Process state== ==saved== in respective ==PCB==.
- ==Due to this== ==outcome== may ==become unpredictable== as ==sharing data== can ==lead to inconsistencies==  can be ==interrupted part way== through doing something. 
- ==Outcome of execution== may ==depend== in the ==order== in which ==code== gets to ==run on the CPU.==
```c
#include <stdio.h>
#include <pthread.h>

int counter = 0;
void* calc(void* param) {
	int const iterations = 50000000;

	for(int i = 0; i < iterations; i++)
		counter++;

	return 0;
}

int main() {

	pthread_t tid1 = 0, tid2 = 0;

	pthread_create(&tid1, NULL, calc, 0);
	pthread_create(&tid2, NULL, calc, 0);
	pthread_join(tid1,NULL);
	pthread_join(tid2,NULL);

	printf("The value of counter is: %d\n", counter);
}
```
2 threads are created, both executing the calc function, each thread performing 50000000 increments on shared variable counter.

The ==counter++== is ==not== an ==atomic operation==; ==consists== of ==multiple steps:==
1. ==load value== of ==counter from memory== and s==tore in register==
2. ==add one to value in register==
3. ==store the value of the register== in ==counter memory location.== 

==Both threads== will ==therefore attempt== to ==increment counter== at the ==same time,== given that it is shared, this ==gives rise== to a ==race== 
==condition.== 

### Race condition
- ==Code== has a ==race condition== if its ==behaviour== is ==dependent== on the ==timing== of ==when== ==computation== is ==performed.== 
- A ==race condition== ==typically occurs== when ==multiple processes==/==threads== ==access shared resources== and the r==esult is dependent== on the ==order== in ==which instructions== are ==interleaved.== 

### Concurrency within the OS
- ==Kernels== are ==pre-emptive==, and ==kernel code== can be ==interrupted== at ==any point==. 
- The ==kernel maintains== ==data structures== such as ==process tables== and ==open list files==, and these ==data structures== may be ==accessed concurrently==. 
- The ==OS must make sure interactions== ==within== the ==OS== ==do not results== in ==race conditions.== 

### Critical section
==Software abstraction== to ==prevent race conditions==.
- A ==section of code== where ==shared resources== are ==accessed== or ==modified==.

How to enforce ==mutual exclusion==? 
- Could ==provide direct support== for ==critical sections== in ==OS== and ==compiler.== 
- ==OS== and ==compiler== ==provide== mechanisms like ==locks/mutexes== (mutual exclusion) object which are used to ==lock a critical section==, so that==only one thread can access it.== 
- ==Once== a ==thread finishes== its work here, it ==releases the lock==. A mutex ==can be implemented in various ways:== 
	- peterson's algorithm (software based), hardware based test_and_set(), swap_and_compare(), or OS blocking processes waiting for the lock. Mutexes and other concurrency primitive ==introduce deadlocks.==
### Critical section requirements
==An implementation== of ==critical sections== should ==satisfy== the ==following requirements==
- ==**Mutual exclusion:**== ==Only one thread== or ==process== can ==enter== the ==critical section== ==at a time== to ==ensure shared data== is ==not accessed simultaneously==.
- **==Progress:==**
	- ==Threads in the 'remaining code==' do n==ot influence access to critical section== 
	- ==Deciding== ==which== of the ==competing processes== ==gets access== ==cannot be postponed indefinitely==
- **Fairness/bounded waiting**: ==fairly distributed== ==waiting times,== needs to be an ==upper bound== on ==amount of time== ==thread== can be ==made to wait.==
These ==requirements must be satisfied,== ==independent== of the ==order== in which ==computations== are ==executed.== 

### Deadlocks
- A ==set of threads== is ==deadlocked== if ==each thread== in the set is ==waiting== for an ==event== that ==only== the ==other thread== ==in the set can cause.==
- ==Waiting for a resource== h==eld by another thread== ==that is also deadlocked== (==whihc cannot run== and ==hence cannot release the resource==). 
- Can happen between any number of threads, and for any number of resources. 

==Four conditions== must hold for deadlocks to occur: Known as Coffman's Conditions.
- ==Mutual exclusion:== ==A resource== can be ==assigned== to ==at most== ==one process== at a ==time==. 
- ==Hold and wait condition==: a ==resource== can be ==held whilst requesting== ==new resources==
- ==No preemption==: ==resources== ==cannot be== ==forcefully== ==taken away== from a process, must be released voluntarily
- Circular wait: ==closed chain== of ==processes exists==, where ==each process== ==holds== ==at least one resource== ==needed== by the ==next process== in the chain.
==No deadlocks can occur if one of the conditions is not satisfied==

