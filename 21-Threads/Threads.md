## Threads
threads are a way to run multiple parts of a program concurrently within the same process. 
Unlike processes, threads share the same memory space, which makes communication faster but also introduces synchronization challenges.

### What is a Thread?
A **thread** is the smallest unit of execution inside a process. A single program can have multiple threads running tasks simultaneously.

- **Process →** has its own memory
- **Threads →** share memory of the process

### Why use Threads?
- Improve performance (parallelism)
- Handle multiple tasks at once (e.g., UI + background work)
- Better CPU utilization
