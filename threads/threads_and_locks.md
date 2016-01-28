title: Threads and Locks 
output: basic.html
controls: true

--

# threads and locks
--

### processes

"A process can be thought of as an instance of a program in execution." - CCI

Each process is allocated separate resources (CPU time, address space)

Inter-process communication is limited (pipes, files, sockets)

--

### thread

A thread exists within a process.

Each thread has its own stack and registers, but shares the heap with other threads in the same process.

--
### shared memory

Threads within the same process share the same memory space.

This can create conflict when two threads modify a resource at the same time.

--
### locks

Resources can be protected with locks. Only one thread at a time can hold the lock.

--
### synchronized keyword

In Java, methods, classes (via static methods), or arbitrary blocks of code can be marked synchronized.

Only one thread at a time may access a synchronized resource. Other threads will block.

--
### locks (aka monitors)

In Java, an entire object can be locked.

Only one thread at a time may hold the lock. Other threads will block.

-- 
### potential problems: deadlock

Thread A is waiting to access a resource held by thread B.

Thread B is waiting to access a resource held by thread A.

Both threads wait forever.

Example: meeting in a hallway

--
### causes of deadlock

In order for deadlock to occur, several conditions must be met.

1. Mutual Exclusion: There is limited access to a resource
2. Hold and Wait: A process holding a resource can request additional resources, without relinquishing its current resources.
3. No Preemption: One process cannot forcibly remove another process' resource.
4. Circular Wait: Two or more processes form a circular chain where each process is waiting on another resource in the chain.

--
### deadlock prevention

"Most deadlock prevention algorithms focus on avoiding Circular Wait." - CCI

Example: Dining Philosophers

--
### livelock

Both threads are doing something, but neither is making progress.

Livelock can occur after we try to prevent deadlock.

Let's revisit our examples.

-- 
### livelock prevention

We could try backing off from requesting resources when congestion is detected (like TCP AIMD). This may introduce race conditions.

We could introduce a "lock hierarchy". Threads must request resource A before resource B.

-- 
### other approaches to shared state

Minimize the use of mutable shared state. Functional languages attempt this.

--
### thanks!
