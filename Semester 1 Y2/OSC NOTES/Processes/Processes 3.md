## Threads
A ==process== consists of ==two fundamental units:==
1. **Resources** - 
	- ==Logical address space== containing the ==process image== (.==text, heap, stack, program data==), ==files==, ==I/O devices accessed== etc
2. **Execution trace** - 
	- I==nstructions== that get ==run on CPU,== a ==detailed record== of the ==sequence of instructions== that a ==computer executes== ==during its runtime==.
A ==process== can ==share its resources== between ==multiple execution traces==, therefore ==makes sense== to ==make abstraction for execution trace==, aka ==**threads**==. 

![[Pasted image 20250124161612.png]]

### Threads from OS perspective
- ==Every thread== has its ==own execution context== e.g., ==program counter==, ==stack==, ==registers==.
- ==All threads== have ==access== to ==process==' ==shared resources== 
	- e.g., ==one thread opens a file==, ==all threads== of ==same process== can ==access the file,== applies to ==heap memory==, global variables etc.
- ==Similar to processes==, ==threads have:==
	- ==States and transitions== - new, running, ready to run, blocked , terminated
	- ==Thread control block==(TCB)
	- ==Thread table of TCBs==
	- ==Thread ID== (TID)
                ![[Pasted image 20250124161850.png]]

- ==Threads== incur ==less overhead== to ==create==/==terminate==/==switch== (==address space remains same== for ==threads of same process==, so ==can remain== in ==same address locality)
- ==Some CPUs== have ==direct hardware support== for ==multi-threading==
	- Typically offer up to ==8 hardware threads per core==
- ==Inter-thread communication== is ==easier/faste==r than ==IPC== as ==threads share memory== by ==default,== 
- ==No protection boundaries== are ==required== in the ==address space==. 
- ==Co-ordinating access== to ==shared resources== is ==complex== however need things like locks, message passing, transactional memory.

### Why use threads
- ==Multiple related activities== ==apply== to the ==same resources/context==, ==these resources== should be ==accessible==/==shared==
- ==Processes== will ==often contain== ==multiple== ==blocking tasks:==
	- ==I/O operations==(==thread blocks==, ==interrupt== marks ==completio==n)
	- ==Memory access==: ==page faults== result in blocking, have to fetch/perform ==page replacement==
	- ==One thread== can be ==blocked== while ==others do useful work in parallel/concurrently
	- ==Still faster concurrently== due to sharing s==ame logical address space.==
==Anything with a UI thread== e.g., Main thread for UI, to keep responsive at all times, another thread to handle I/O operations

## OS implementations of threads 
### User threads -
- ==Thread management== (creating, destroying, scheduling, thread control block manipulation) is ==carried out== in ==**user space**== with the ==help== of a ==**user library.**== 
- The ==process== maintains a ==**thread table**== ==managed== by the ==**runtime system**== ==without== the ==**kernel's knowledge==**
- **Advantages:** 
	- ==Threads are in user space== (==no mode switches== required). 
	- ==Full control== over ==thread schedule==r. ==OS independent.==
- **Disadvantages**: 
	- ==Blocking system calls== ==suspend entire process==, as ==kerne==l is not involved in thread management - ==user threadd== are ==mapped onto== a ==single process== ==managed by the kernel.== 
	- ==No true parallelism== as a ==process== is ==scheduled== on a ==single CPU==.
	- ==Non-preemptive==, no mechanism such as interrupts.
![[Pasted image 20250124162618.png]]


### Kernel threads
- ==Kernel manages threads==, ==user application== accesses ==threading facilitie==s through ==API== and ==system calls.== 
- ==Thread table== ==maintained== by ==kernel== 
- If a ==thread blocks==, the ==kernel choose==s a ==thread== from ==same== or ==different process==. 
- **Advantages**: 
	- ==True parallelism==, ==not mapped== onto ==single process,== ==genuinely independent== and can be ==scheduled== to ==run on== ==separate CPUs==.
	- ==Preemptive==, no r==un-time system needed== in user space
- **Disadvantages**:
	- ==Frequent mode switches== take place, resulting in ==lower performance==. 
Windows and Linux apply this approach.
![[Pasted image 20250124162926.png]]
![[Pasted image 20250124162909.png]]
![[Pasted image 20250124162916.png]]

### Hybrid implementations
- ==User threads== are ==multiplexed== onto ==kernel threads==. ==Kernel sees== and ==schedules the kernel threads== (a limited number). 
- ==User application sees user threads== and ==creates these==(an ==unrestricted number==) on the kernel threads. 
![[Pasted image 20250124163129.png]]



### Thread libraries
- ==Thread libraries== provide an ==interface== for ==managing threads== e.g., ==creating, running, destroying, synchronising.==
- ==Thread libraries== can be ==implemented entirely== in ==user space==, or ==based on system calls== ==relying== on ==kernel== for ==thread implementations.== 
- ==POSIX PThreads==, Windows Threads, Java Threads.
- The ==PThread spec== can be ==implemented== as ==user== or ==kernel threads== funnily enough! It defines a set of functions and what they should do.
```c
#include <pthread.h>
#include <stdio.h>
#define THREADS 10

void* hello(void* arg) {
	printf("Hello from thread %d\n", *((int*)arg));
	return 0;
}

int main() {
	int args[THREADS] = { 0 };
	pthread_t threads[THREADS];

	for(int i = 0; i < THREADS; i++) {
		args[i] = i;
		if(pthread_create(threads + i, NULL, hello, args+i))
			printf("Creating thread %d failed\n", i); 
			return-1;
	}
	for(int i = 0; i < THREADS; i++) 
		pthread_join(threads[i], NULL); }
}
```