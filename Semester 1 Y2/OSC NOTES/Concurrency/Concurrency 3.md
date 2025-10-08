## Semaphore
A ==semaphore== is an ==abstraction== for ==providing synchronisation.== They have a ==natural number capacity,== which ==changes== ==during== ==their use.== 
==A trivial semaphore== is implemented as a ==plain variable== that is changed..
A ==semaphore== ==controls acces==s to a ==pool of resources==, via the ==semaphores counter==

Two functions are used to manipulate semaphores:
1. `wait()` - ==which decrements the capacity== - if this ==new value is negative==, the ==thread executing wait== is ==blocked==(==added to the semaphore's queue==)
2. `post()` - ==increments the capacity== - ==any thread can post==

### Semaphore implementation
```c
typedef struct {
	int count;
	struct process * list;
} semaphore;

void wait (semaphore* S){
	lock(&mutex);
	S->count--;
	if(S->count < 0);{
		//add process to S list
		block();
	}
	unlock(&mutex);
}

void post (semaphore* S){
	lock(&mutex);
	S->count++;
	if(S-> count <= 0){
		//remove process p from list
		wakeup(P);
	}
	unlock(&mutex);
}
```
Post and wait must be atomic, guarantee it happens, and that value not changed by another thread. 

LOCK MUTEX
DECREMENT/INCREMENT COUNT
IF COUNT IS NEGATIVE BLOCK PROCESS, ADD TO LIST
IF COUNT IS GREATER THAN OR EQUAL TO 0, WAKE UP PROCESS WHICH POSTED
UNLOCK MUTEX


There are different types of semaphores:

### Binary semaphores
These are ==semaphores== which have ==restricted value== of ==either 0== or ==1==.  This can ==essentially== be ==used== as a ==lock==. This ==differs from mutex's== as a ==mutex== can o==nly be released== by the ==thread that acquired it==. Whereas any thread can call signal/post and use the critical section. There is ==no ownership==.

### Counting semaphores
The ==more general type== of ==semaphore== with ==integer capacity==. Used to ==manage access== to ==resource pool.==

### Advantages of semaphores
- ==More flexible==; can be used to implement a binary lock, or can be counting, which represents number of resources in a given resource pool.

### Disadvantages of semaphores
- ==Non-exclusive ownership==


### Producer/Consumer problem
==Producer(s)== and ==consumer(s)== share a ==buffer== of ==values== e.g., a printer queue. The ==buffer== can be ==bounded== or ==unbounded==. There can be ==any number of producers and consumers==. A ==producer attempts to add items== and ==blocks== if the ==buffer is full==. A ==consumer attempts to remove items== and ==blocks if the buffer is empty.==

The ==simplest version== has ==one producer==, o==ne consumer==, and a buffer of unbounded size.
It uses ==2 binary semaphores==: ==sync== - ==synchronises access to the buffer==, ==initialised to 1.==
==delay_consume==r ==ensures== the ==consumer blocks== when there are  ==no items available.==


```c
void * consumer(void * p){
	sem_wait(&delay_consumer);
	while(1){
		sem_wait(&sync);
		items--;
		printf("%d\n", items);
		sem_post(&sync);
		if (items == 0)
			sem_wait(&delay_consumer)
	}
}

void * producer(void *p){
	while(1){
		sem_wait(&sync);
		items++;
		printf("%d\n", items);
		if (items == 1)
			sem_post(&delay_consumer);
		sem_post(&sync);
	}
}
```
Delay consumer used as binary semaphore used to enter loop for consumer. Sync used as a lock for both thread function to control access to shared variable items. 

Still has race condition: if scheduled in certain order, consumer may check items == 0 and fail to block, because posts to sync, giving item chance to be incremented before consumer checks it. 

To fix, use temporary variable, copies the value of items inside the critical section, decrements delay_consumer to make it consistent.

#### CORRECT SOLUTION.
```c
void * consumer (void * p) {
	sem_wait(&delay_consumer);
	while(1){
		sem_wait(&sync);
		items--;
		temp = items;
		printf("%d\n", items);
		sem_post(&sync);
		if (temp == 0){
			sem_wait(&delay_consumer);
		}
	}

void * producer (void * p){
	while(1){
		sem_wait(&sync);
		items++;
		printf("%d\n", items);
		if (items == 1){
			sem_post(&delay_consumer)
		}
		sem_post(&sync);
	}
}
```

saves 'items' value prior to posting.


#### Multiple producers, Multiple consumers, Bounded buffer
==Different variant of problem==, with ==n consumers==, ==m consumers,== and a ==fixed buffer size N==. Solution based on ==3 semaphores==:
- ==sync:== used to enforce mutual exclusion for the buffer
- ==empty==: keeps track of the number of empty buffers, initialised to N.
- ==full==: keeps track of number of full buffers, initialised to 0.
==Empty and full== are ==counting semaphores== and ==represent resources.==![[Pasted image 20250123050114.png]]
WHILE LOOP IN BOTH, WAIT ON EMPTY IN PRODUCER, WAIT ON FULL IN CONSUMER, 
## Dining philosophers problem
Defined as
- Five philosophers are on round table
- Each one has a plate of spaghetti
- Spaghetti too slippery, philosopher needs 2 forks to be able to eat
- When hungry, philosopher tries to acquire forks on left and right.
Reflects general problem of sharing limited resources between a number of processes.

### Naive solution
 ```c
#define N 5
sem_t forks[N]; 

void* philosopher(void* id) {
	int i = *((int*) id);
	int left = (i + N - 1) % N; //e.g., 0-> 4, 1->0, 2->1
	int right = i % N; //simply i itself
	while(1) { 
		printf("%d is thinking\n", i);
		printf("%d is hungry\n", i); 
		sem_wait(&forks[left]); 
		sem_wait(&forks[right]); 
		printf("%d is eating\n", i); 
		sem_post(&forks[left]); 
		sem_post(&forks[right]); 
	} 
}
 ```
Deadlocks if all philosophers pick up their left fork, as the semaphores are equal all to 1 in the array. But the program will hang at sem_wait(&forks[right]) because all the semaphores are already held, and will never become available because no calls to post can be made. 

Deadlock can be prevented by:
- Putting forks down (sem_post) and waiting a random time.
- Putting one additional fork on the table
- One global mutex set by a philosopher thread when they want to eat, but only one can eat at a time now, and does not result in maximum parallelism. 

### Global mutex/semaphore solution. 
 ```c
pthread_t mutex;

void* philosopher(void* id){
	int i = *((int*) id);
	int left = (i + N - 1) % N;
	int right = i % N;

	printf("%d is thinking\n", i);
	printf("%d is hungry\n", i); 
	pthread_mutex_lock(&mutex);
	sem_wait(&forks[left]); 
	sem_wait(&forks[right]); 
	printf("%d is eating\n", i); 
	sem_post(&forks[left]); 
	sem_post(&forks[right]); 
	pthread_mutex_unlock(&mutex);
}
 ```
Guarantees philosophers cannot simultaneously wait on the fork to their left. 

Using an eating semaphore instead of a global mutex, with a value of N / 2, so setting to 2 would work above, as this is the largest number that can eat at once without potentially running into deadlock, as otherwise requesting more resources than technically exist, and will run into deadlock. See below. 
![[Pasted image 20250123230823.png]]
1. 
2. Half the philosophers can eat at once, if each philosopher holds only 1 fork initially. Number of philosophers = 2 X N. Resources required to eat = 2. N/2 = Number of philosophers able to eat at once, so N. 
3. Only adding one introduces an imbalance, e.g., 3 philosophers, 2 x 3 + 1 = 7, each requires 2 forks to eat, 7 % 2, leaves 1 fork leftover, so only 3, or N. 


## Maximum parallelism solution
Uses:
- `State[N]` one state variable for every philosopher(THINKING, HUNGRY, EATING)
- `phil[N]` one semaphore per philosopher rather than per fork
	- Philosopher blocks if one of their neighbours are eating
	- The neighbours wake up the philosopher
- `sync` one semaphore/mutex to enforce mutual exclusion of the critical section while updating states. 

```c
#define N 5
#define THINKING 1
#define HUNGRY 2
#define EATING 3

int state[N] = {THINKING, THINKING, THINKING, THINKING,
THINKING};
sem_t phil[N]; //initialised to 0
pthread_mutex_t sync;

void* philosopher(void* id){
	int i = *(int *) id);
	while(1){
		printf("%d is thinking\n", i);
		take_forks(i);
		printf("%d is eating\n", i);
		put_back_forks(i);
	}
}
//checks if philosopher i can start eating
void test_free(int i){ 
	int left = (i + N - 1) % N;
	int right = (i + 1) % N;
	if (state[i] == HUNGRY && state[left] != EATING &&
	state[right] != EATING) {
		state[i] = EATING;
		sem_post(&phil[i]);
	}
}

void take_forks(int i) {
	p_thread_mutex_lock(&mutex);
	state[i] = HUNGRY; 
	test(i);
	sem_post(&sync); 
	sem_wait(&phil[i]); //if cannot eat immediately, block here until one of neighbours finishes eating, used to acquire forks
}

void put_forks (int i ) {
	int left = (i + N - 1) % N;
	int right = (i + 1) % N;
	p_thread_mutex_lock(&mutex);
	state[i] = THINKING; 
	test(left); //check if left can eat, if can, wakeup
	test(right); //check if right can eat, if can, wakeup
	pthread_mutex_unlock(&mutex);
}
```

If N = 5, only 2 can be eating at same time. 
`state[0] = HUNGRY` → `test_free(0)`
`state[2] = HUNGRY` → `test_free(2)`
but, 1, 3, 4 cannot eat because one of their neighbours is eating. 
So N / Resources = number of threads able to use those resources at any given time.


## Readers Writers problem
- Reading data can happen **in parallel** without problems
- Writing data needs **mutual exclusion** to avoid race conditions
- Aim to observations to synchronise access to data
- Different solutions exist:
	- Solution 1: Naive solution with limited parallelism
	- Solution 2: readers receive priority, no reader is kept waiting unless a writer already has access. Issue: writers may starve
	- Solution 3: Writing is performed as soon as possible. Issue: readers may starve. 

### Solution 1: Naive solution

```c
void* reader(void* arg){
	while(1){
		pthread_mutex_lock(&mutex);
		printf("reading, counter now %d\n", counter);
		pthread_mutex_unlock(&mutex);
	}
}

void* writer(void* arg){
	while(1){
		pthread_mutex_lock(&mutex);
		printf("writing, counter now %d\n", counter++);
		pthread_mutex_unlock(&mutex);
	}
}
```
Prevents parallel reading

### Solution 2: Readers first
Allows parallel reading, a correct implementation of solution 2 requires:
- `iReadCount` - integer tracking number of readers
	- If `iReadCount` > 0: writers are blocked (sem_wait(&rwSync))
	- If `iReadCount` == 0: writers are released (sem_post(&rwSync))
	- If already writing, readers must wait
- `sync` mutex for mutual exclusion of iReadCount
- `rwSync` a semaphore that synchronises the readers and writers, set by the first/last reader. Multiple readers can access resource, but only one writer. 

```c
void* reader(void* arg){
	while(1){
		pthread_mutex_lock(&sync);
		iReadCount++;
		if (iReadCount == 1){
			sem_wait(&rwSync) // -1, becomes 0
		}
		pthread_mutex_unlock(&sync);
		
		pthread_mutex_lock(&sync);
		printf("reading record\n");
		iReadCount--;
		if (iReadCount == 0){
			sem_post(&rwSync); //+1, can be consumed by
			// writer when no readers left. 
		}
		pthread_mutex_unlock(&sync);
	}
}

void* writer(void* arg){
	while(1){
		sem_wait(&rwSync);
		printf("writing\n");
		sem_post(&rwSync);
	}
}
```
With lots of readers, writers may starve as readers never post to rwSync.

## Solution 3: prioritising writers
Uses:
- `iReadCount` and `iWriteCount` to keep track of number of readers/writers
- Mutexes `mRead` and `mWrite` to synchronise reader and writer critical section
- Semaphore `sReadTry` to stop readers when there is a writer waiting. (binary?)
- Semaphore `sResource` to synchronise resource for reading/writing (binary)

```c
void* reader(void* arg){
	while(1){
		sem_wait(&sReadTry);
		pthread_mutex_lock(&mRead);
		iReadCount++;
		if(iReadCount == 1){
			sem_wait(&sResource)
		}
		pthread_mutex_unlock(&mRead);
		sem_wait(&sReadTry); //makes sure that after 
		//critical section execution, that a writer isn't 
		//writing
		
		printf("reading\n");

		pthead_mutex_lock(&mRead);
		iReadCount--;
		if (iReadCount == 0 ){
			sem_post(&sResource);
		}
		pthread_mutex_unlock(&mRead);
	}
}

void* writer(void* arg){
	while(1){
		pthread_mutex_lock(&mWrite);
		iWriteCount++;
		if (iWriteCount == 1){
			sem_wait(&sReadTry);
		}
		pthread_mutex_unlock(&mWrite);

		sem_wait(&sResource);
		printf("writing\n");
		sem_post(&sResource);

		pthread_mutex_lock(&mWrite);
		iWriteCount--;
		if(iWriteCount == 0){
			sem_post(&sReadTry);
		}
		pthread_mutex_lock(&mWrite);
	}
}

```

Reader thread execution:
- Tries to enter critical section, waiting on sReadTry
- locks mReadMutex
- Increments reader count
- If 1st reader, acquire sResource to block writers
- Unlock the mutex
- perform read
- Lock the mutex
- Decrement iReadCount
- If last reader, release sResource
Writer thread execution
- Locks mWrite
- If first writer, waits on sReadTry to block additional readers
- Unlocks mWrite
- Waits on resource, ensures only 1 writer can write at a time
- Posts to resource, ensuring next writer, or reader
- Locks m write
- Decrements writers
- If last writer, posts to sReadTry to let readers enter critical section.
### The issue with lock based concurrency primitives
- ==**Deadlocks**==
- ==**Starvation**==
- ==**Priority inversion**==
	- Defined as a ==scenario== in ==process scheduling== where a ==high-priority task== is ==indirectly superseded== by a ==lower priority== task, effectively ==inverting the assigned priorities== and violating the priority model that high priority tasks can  only be pre-empted for higher priority tasks. ==Occurs== when there is a r==esource contention== with a ==low-priority task==.
	- Formulation:
		- ==Two tasks, H and L, high and low priority.==
		- ==Each require exclusive use== of ==shared resource R.==
		- ==H attempts to acquire R== after L ==has acquired it,== ==H becomes blocked== until L relinquishes resource. 
		- In a ==well designed system==, involves ==L relinquishing R quickly==
		- But is ==possible== that ==third task M of medium priority becomes runnable during L's use of R.== 
		- Makes M higher priority than L, so L pre-empted, making H wait long time for L to relinquish, as it now has to wait for M.
- **Lack of compositionality** - c==annot compose small correct== ==concurrent programs to form large concurrent programs==. ==Can lead to deadlocks, and other issues.==
- Could ==try to grab locks== and ==back off== if ==cannot be acquired== - risks **==livelock==**
	- ==Situation== where ==threads== are ==continously active==, but ==unable== to ==make progress==. If o==ne thread== ==fails== to ==acquire a lock,== ==backs off and tries again==. B==ut if other thread== ==keeps responding== in ==same way==, ==neither ever acquires the lock.== ==Not stuck waiting,== ==stuck actively trying== ==to avoiding blocking behaviour== that could lead to deadlocks.
- **Lock convoying** - ==threads== end ==up queueing== ==behind== a t==hread holding a lock==, ==linearising behaviour==, can take a ==long time to clear,== ==inefficient use of resources==, especially when large number of threads waiting. 
- **Access to shared state is implicit** - ==hard to tell== ==who's modifying what==, and ==if suitable locks== are ==held at the time==, especially with semaphores which are less easy to reason about due to the constraint that any thread can post to a semaphore if it visible to the thread.


### Alternative approaches
#### Messaging passing
==Alternative concurrency model==, allowing ==processes== to ==communicate== and ==co-ordinate== ==without== ==directly sharing memory== or ==locks== to access shared state. 

==We avoid sharing memory==, and ==instead have channels== c, d,... ==as the mediums== through ==which messages== are ==sent between processes.== ==Act as buffers== where ==messages==(data structure sent from one process to another) are ==temporarily stored== until ==receiving process== reads them
##### Synchronous message passing
There are ==two operations== on ==channel c==:
1. ==c(x)== ==sends message x== o==ver the channel==
2. ==¬c(x)== ==receives a value== ==over the channel== ==into x(variable x==).
==Communication== is synchronous here - ==a send== ==must wait== for a ==corresponding receive==, and vice versa. Phone calls.

Thread1:                                 Thread 2
c(message)                             ...
block                                      ...
block                                      ¬c(x)
...                                            ... x=data
==Thread 1 cannot proceed== ==until Thread 2== r==eceives the message==. ==Blocks waiting for available receiver.==

###### Pros
- ==Flow of data is explicit== - ==no exposure== to ==race conditions== as ==both threads== are ==synchronised== at the ==communication point,== meeting at the channel, and only proceeding once the other has performed the respective action.
- ==Easier to reason about== due to ==clear communication pattern,== leading to ==easier debugging.==

###### Cons
- ==Can implement mutexes== and ==semaphores via this model== still, leading to ==deadlocks, starvation== etc. 
- ==Latency== - can ==only perform work== ==once== the ==necessary communication== has been ==carried out== ie, sent and received. Not the case in asynchronous. 

##### Asynchronous message passing
==Alternative== where ==senders do not block== waiting for an available receiver. ==Often use== what is known as an ==Actor based model.==

To specify a simple counter:
```java
class Counter extends Actor {
	var counter = 0;

	def receive = {
		case Zero => counter = 0
		case Inc => counter = counter + 1;
	}
}
```
To increment a Counter object:
```
mycounter ! Inc //send the message, don't wait for receiver
mycounter ! Inc //send another, place in queue.
```
==Sender proceeds immediately== ==without waiting== for ==receiver.== ==Receiver retrieves message== at ==its convenience==. Usually ==use a message== ==queue==, to ==buffer messages== sent by sender, which ==can be dequeued by== ==receiver== when ready to process. In case of bounded buffer, sender may have to wait until some space is available, otherwise message discarded, or other messages overwritten. 

###### Pros
==Flow of data== is ==explicit== - less exposure to race conditions and other difficult debugging. 
==Decoupling== of ==sender and receiver== - ==sender== does ==not need to know== ==when== or ==how== the ==receiver== will ==process message==, and receiver does not need to be ready immediately.
==Better resource utilisation== - sender threads c==an continue to work== ==without blocking for receiver,== reducing idle time. 
Higher degree of concurrency - ==more than one sender== ==can send== to a ==given receiver==, due to the fact that s sender does not have to wait for r receiver to be available. Fire and forget. 
###### Cons
==Message loss== - due to ==bounded buffer==/queue
==Latency== - ==delay between== when ==message is sent,== vs when ==actually processed==
==Can implement mutexes== using asynchronous message passing - ==deadlocks, starvation etc. all still possible==


#### Transactional memory
==Alternative concurrency control== mechanism that ==allows multiple== ==threads== to ==execute== ==critical sections== ==without need== for ==explicit locks==. ==Allows== ==threads== to ==execute== '==transactions=='; ==atomic operations== which e==ither commit successfully==, or ==abort.== 

1. A ==thread begins== a ==transaction== where it ==reads or writes shared memory locations==. This can be comprised of multiple operations. 
2. ==Execute transaction==: ==read/write to shared memory,== ==updates invisible== to ==other thread==s until ==update committed==.
3. ==Conflict detection==: ==detects== if ==two transactions conflict== e.g., read and write to same memory location
4. ==Commit or abort:== transaction commits, making changes visible to other threads, or there is conflict, one or more transactions are aborted and restarted.

DATABASE ANALOGY BIT: Realistic transactions can be complex, and ==call rollback,== instead of commit to fail the whole transaction. 

Transactional memory pseudocode
```
do { 
	begin_transaction(); 
	modify_shared_data(); 
	commit(); 
}	 
while(!transaction_succeeds()...);
```
==No locks, and no risk of deadlock==
==A form of livelock== ==still possible== ==after rollbacks/aborts.==
==Starvation possible== - ==smaller transactions== may ==dominate longer ones==
==Unfamiliar programming model may be harder to use.== 






