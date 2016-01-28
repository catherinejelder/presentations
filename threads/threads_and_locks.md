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

--
### deadlock prevention

In order for deadlock to occur, several conditions must be met.

One is "Circular Wait": "Two or more processes form a circular chain where each process is waiting on another resource in the chain." - CCI

Most deadlock prevention algorithms focus on avoiding Circular Wait.

--
### livelock

Both threads are doing something, but neither is making progress.

Livelock can occur after we try to prevent deadlock.

-- 
### livelock prevention

Try backing off.

-- 
### other approaches to shared state

Minimize the use of mutable shared state. Functional languages attempt this.


--
### thanks!
