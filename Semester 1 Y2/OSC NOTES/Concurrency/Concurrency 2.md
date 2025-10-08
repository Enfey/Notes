## Peterson's Solution
==Software based== ==approach== for ==achieving== ==mutual exclusion== in ==concurrent programming==, which worked well on older machines. ==Two shared variables== are ==used:==
1. `turn` - ==variable== indicating ==which process's turn== it is to e==nter  critical section.== 
2. `bool flag[i]` - ==bool array== where ==process/thread i== ==sets its== ==entry== to ==`true`== if it ==wants to enter== ==critical section==. 
==Satisfies all critical section requirements.==

### Peterson's solution C pseudo-implementation
```c
	do { 
		flag[j] = true; // j wants to enter critical section 
		turn = i; // allow i to access first 
		while (flag[i] && turn == i); // whilst i wants to access  
		//critical section // and its i’s turn, apply busy waiting 
	// CRITICAL SECTION, e.g. counter++ 
	
		flag[j] = false;  
	} while (...); // remainder section
```
==`flag[j] = true`== indicates ==process j== ==wants enter== ==critical section..== ==Turn set to i== to ==allow i to access==, ==process j== ==busy waits== at ==the loop== when ==either process i== ==doesn't want to enter critical section==, or it is ==no longer process i's turn==.

busy waiting = process enters a loop to keep checking for some condition, without relinquishing control of CPU; loop does not perform any useful work but still uses cycles. Exits loop when condition is met. 

The code repeats for process i, except all i's and j's are swapped :D.



### Peterson's solution with regard to critical section requirements
#### Progress
==Threads== in ==remaining code== do ==not influence== ==access== to ==critical== ==sections.== 
==Which thread== ==makes progress== is ==decided== in ==finite time==:
1. ==No thread wants to enter critical section==
2. ==Only thread i wants to enter critical section== so `flag[j] && turn == j` is false and it may proceed
3. ==Only thread j wants to enter critical section== so `flag[i] && turn = i` is false and it may proceed.
4. ==Both threads want to enter the critical section==, then ==at least one of `flag[j]== && turn ==j` or `flag[i] && turn == i` is false, depending on turn, and somebdoy can make progress.
#### Fairness/bounded waiting
Fairly distributed waiting times. Processes cannot be made to wait indefinitely. 
	Even if thread i and thread j both want to enter their critical section:,
	 a process will never wait longer than one turn for entrance to the critical section.

#### Mutual exclusion
The ==variable== `turn` can have ==only== ==one== ==value== at a ==time==. Hence, ==at most==, ==one== of the ==loop conditions== is ==false==, and ==at most one process== can ==enter== its ==critical section.==


## Hardware approaches
`test_and_set()` and `compare_and_swap` are low-level operations ==formed of atomic==(=uninterruptable) ==instructions.==
==Allow a process==s to **check** and **update** a variable’s value in a single, uninterruptible step.
If called simultaneously, they will be executed sequentially. Can also. be used in conjunction with lock variables.

OS may use these hardware instructions to implement higher level mechanisms for mutual exclusion such as mutexes and semaphores. 

## Test and set implementation and implementation of mutual exclusion via test and set
```c
bool test_and_set(bool* bIsLocked) {
    bool rv = *bIsLocked;
    *bIsLocked = true;
    return rv;
   
}

//usage

...
do {
	while(test_and_set(&bIsLocked)); //lock in use, busy wait

	//critical section
	
	bIsLocked = false;
}  while(...)
```
- ==Atomically== ==checks== the ==value== of the ==lock variable==
- ==Sets== the ==lock variable== to ==true==
- ==Return== the ==previous value== of ==lock variable==, ==allows caller== to ==determine== whether ==lock== was ==already held.== 

### Compare and swap
```c
int compare_and_swap(int *iIsLocked, int expected, int new_value){
	int const old_value = *iIsLocked;
	if (old_value == expected)
		*iIsLocked = new_value;
	return old_value;
	
}
```
- ==Compares  contents== of a ==memory location== with a ==given value==
- ==If they match==, ==modify== the ==content== of that ==memory location== to a ==new given value.== 
- Returns ==old value no matter what==.

#### Compare and swap to implement mutual exclusion
```c
do {
	//while lock used, apply busy waiting
	while (compare_and_swap(&iIsLocked, 0, 1));
	//critical section
	iIsLocked = 0;
} while(...);
```
Repeatedly check if iIsLocked == 0, indicating lock is free, sets to 1 if acquired, busy waits otherwise. 
### Hardware vs software pros cons of implementing mutual exclusion 
#### Hardware
##### Pros
- ==Atomicity== - ==guaranteed by hardware==, ==preventing race== ==conditions==
- ==Speed== - ==hardware primitives== are ==very fast==, directly implemented at hardware level
- ==Easy to implement locks== via hardware, as hardware handles atomicity of operation
##### Cons
- ==Busy waiting,== wastes CPU cycles
- ==Requires CPU support== for ==atomic instructions== - not guaranteed architecturally.
#### Software
##### Pros
- ==No hardware dependency== - can work on systems that lack atomic instructions
- ==Many software algorithm==s are ==designed== to ==ensure fairness== and prevent starvation.
- Can ==avoid busy waiting==
##### Cons
- ==More complex== to implement and ==verify==
- ==May not scale well== in multiprocessor systems due to memory synchronisation overhead.
## Mutex
- A ==mutex is an abstraction==; it is a ==synchronisation primitive== used in ==concurrent programming== for ==providing mutual exclusion==.
- A ==mutex typically== ==behaves== as a ==binary lock==, meaning it is ==either locked or unlocked.==
- ==Only the thread== that ==acquires the mutex== should be allowed to ==unlock it==
A ==mutex provides an interface== with ==2 functions to implement==:
1. `acquire(&mutex)` - ==called== ==before== ==entering critical section==, ==returns when nobody else== is ==in the critical section==
2. `release(&mutex)` - ==called after== ==entering the critical section,== ==allows other threads== to ==acquire the mutex==, should only be called after a matching `acquire`
- ==How exactly this interface is provided is an implementation detail.==
	- Under naive assumptions, could use Peterson's algorithm. 
	- Could use atomic hardware operations and busy waiting.
	- The OS could block threads that are trying to acquire a mutex.

### Advantages of Mutex
- ==Exclusive ownership== 
- ==More obvious control flow than binary semaphore==

### Disadvantages of Mutex
- ==Improper use== e.g., failing to release mutex can ==result in deadlocks==
- ==Can only be used== to m==anage one resource== at a time, doesn't facilitate complex relationship between an amount of resources like a counting semaphore.
- S==impler to implement.== 
## Lock
- ==General term== for ==mechanisms== that ==ensure== only ==one thread== can ==access a resource at a time.==
- ==Multiple ways to implement locks==, such as via a mutex, which has restraint that it is binary, and only the thread that acquires it should be allowed to free it. 