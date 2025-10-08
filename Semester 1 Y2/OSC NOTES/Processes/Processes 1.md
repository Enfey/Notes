## Process
> A process is an **==abstraction==** of a ==**running instance**== of a program.

A **program** is ==passive==, simply a ==file== somewhere. A ==process== has ==control structures== ==associated with it==, ==may be active==, and ==may have resources assigned== to it. 
### PCB
The ==**kernel**== maintains a ***==process control block==***(**PCB**), ==containing== all the ==process-specific information==. ==PCBs== are ==recorded== in the ==process table== - an ==array of **PCB**s==. They are ==kernel data structure==s and are ==only accessible== in ==**kernel mode==.**

A PCB contains three types of attributes:
1. ==Process identification==(==PID==, ==Parent PID==, UID)
2. ==Process control information==(==process state of operation==, ==scheduling information== etc)
3. ==Process state information==(==registers==, program counter, program status word, ==memory management info==, file handles etc)

Each process has a ==***unique process identifier***==(**PID**) used to associate it with its **PCB** - this is ==typically== the ==corresponding array index==. 



### In memory
A ==process memory image== contains the ==program code==(text), a ==data segment==, ==stack== and ==heap==. ==Every process== has its ==own logical address space==, in which the ==[[stack]] and [[heap]]== are placed at ==opposite sides== to ==allow them== to ==grow==.

### Process states
- ==**New**== - ==process== which has ==just== been ==created,== ==has PCB== and is waiting to be admitted and ==may not be in memory yet==
- ==**Ready**== - ==waiting for CPU== to become available
- **==Running==** - a ==running process== is currently ==having its instructions executed== by the CPU
- ==**Blocked**== - a ==blocked process== ==cannot continue execution== e.g., waiting for I/O
- ==**Terminated**== - process ==no longer executable==, the P==CB may be temporarily preserved==

### Process state transitions
1. ==**New -> Ready**== - ==admit== the ==process to queue== and commit to execution
2. **==Running->Blocked==** - ==process== is ==waiting for input== or ==carrying out a sys call==
3. ==**Ready->Running**== - ==process selected== by ==**process scheduler**==
4. ==**Blocked -> Ready**== - ==event happens== e.g., I/O operation has finished
5. ==**Running->Ready**== - ==process surrenders the CPU== e.g., ==due to an interrupt==/voluntarily gives up CPU time
6. ==**Running->Terminated**== - ==process finished== e.g., program ended/==exception encountered.==
==**Interrupts== and ==system calls==** drive these transitions. 

## Multi programming
- ==Modern computers== are ==**multi-programming== systems**/OS's. 
- A ==multi-programming system== is a ==single processor system== that ==allows multiple processes== to ==execute== in an o==verlapping manner==, rather than sequentially.

==**Multi-programming**== is achieved by ==**interleaving**== the ==execution of processes==, ==dividing== the ==CPU time== into ==**time-slices==.** ==Control== is then ==exchanged between processes== via ==**context switching**==, ==preventing== the ==CPU from being idle== and giving the **i==llusion** of parallelism==

### Response time
The ==**response time**== of a process is the ==elapsed time== from its ==creation== ==before== a ==process gets access== to the ==CPU.== 
### Effective utilisation
==Effective utilisation== is the ==fraction of time== the ==CPU== is ==doing useful work== i.e., ==not context switching.== 

### Tradeoffs
==Short time slices== result in ==good response times== but ==low effective utilisation==.
- Assume both context switches and time slices take 1ms. Takes ==198ms (99x(1+1))== for the ==last of 100 processes== to s==tart running==. 50% of CPU time is doing useful work.
Long time slices result in ==poor response times== but ==better effective utilisation==.
- Assume context switches take 1ms and time slices are 100ms, then it will take ==9999ms (99x(100+1))== for the ==last of 100 processes== to s==tart running==. ==99% of CPU time== is doing ==useful work.==
- 
**N-1 x Time Slice time x Context switch time**

## Context switching 
> Process of ==storing state== of ==a process== so that it ==can be restored== and ==execution resumed== at a ==later point==

When a ==context switch== occurs , the system ==saves the state== of the ==old process== and ==loads the state== of the ==new process==(overhead).  

**To perform a context switch:**
1. ==Save process state==(PC, registers etc) to PCB
2. ==Update PCB state==(==running==->==ready/blocked/terminated==)
3. ==Record PID== in ==appropriate queue==(ready/blocked)
4. ==Run scheduler==, ==select new PID from ready queue==
5. ==Update PCB state==(==ready->running==)
6. ==Restore state from PCB==(==PC==, ==registers== etc)
7. ==Return control== to running the new process


## Process creation
- ==True system calls== ==wrapped== in ==OS libraries== with well ==defined interface.== 
- On ==unix-like systems==, *==*fork*==* is called to create a copy of a process. The ==underlying system call== to implement fork ==is clone== on Linux.
- ==System calls via exit== and ==abort==(implemented via tgkill()) ca be used ==explicitly notify OS== ==process== has been ==terminated.==
- ==Possible== to ==wait for a process== and ==recover info== about ==how it terimated==, ==glibc wrapper function ***waitpid()***== used to ==wait== for a ==specific child process== to ==change state== Typically ==called by a parent process== to ==wait for child process to finish== and ==retrieve the exit status== or other info.
- ==OS== must ==retain information== about ==terminated processes== to ==serve wait calls== (syscall used to implement waitpid())
- ==Fork== creates ==exact copy== of ==current process==, ==returns process identifier== of ==child process== to ==parent process==, and ==0== to ==all child processes==. 
- F==irst instruction carried out by child process== is ==first one== after f==ork() call.==
- ==Call fork()== to ==create exact copy==, i==nherits memory mappings==, ==file descriptors==, but has ==new PID.==
- ==Commonly call fork==, and ==in child process==, call ==one of exec family of functions via user-space wrapper like glibc== to ==replace the current process image== with a ==new process image==, retaining the ==same PID== but ==different heap==, ==stack==, ==machine code==. 

```c
#include <stdio.h>
#include <unistd.h>
#include <sys/wait.h> 
int main() { 
	pid_t const pid = fork(); 
	if(pid < 0){ 
		printf("Fork failure\n"); 
		return -1; 
	} else if(pid == 0) { 
		printf("Child process\n"); 
		execl("/bin/ls", "ls", "-l", 0);
	} else { 
	int status = 0; 
	waitpid(pid, &status, 0); 
	printf("Child %d returned %d\n", pid, status); } 
}
```


### Further information about fork
based on exam q's

