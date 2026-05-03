## Thread Synchronization
**Synchronization** ensures that multiple threads **coordinate access to shared resources** (variables, files, memory).

Without it → unpredictable results.

Common tools:
- Mutex (lock/unlock)
- Semaphores
- Condition variables

### Race Condition
A **race condition** happens when:
- Two or more threads access shared data **at the same time**, and the result depends on execution order.

#### Example (Race Condition)
```c
#include <stdio.h>
#include <pthread.h>

int counter = 0;

void* increment(void* arg) {
    for(int i = 0; i < 100000; i++) {
        counter++;  // NOT safe
    }
    return NULL;
}
```

Problem: counter++ is not atomic, possible result ≠ expected value

The counter++ operation fails in multi-threaded environments because it is not atomic, 
meaning its three machine-level steps—reading the current value from memory, modifying it in a register, 
and writing it back—can be interrupted. If two threads execute this sequence simultaneously, 
they may both read the same initial value before either has a chance to update it. 
As a result, both threads calculate and write back the same incremented value, effectively overwriting each other's work and causing lost updates.

#### Fix (Mutex)
```c
pthread_mutex_t lock;

void* increment(void* arg) {
    for(int i = 0; i < 100000; i++) {
        pthread_mutex_lock(&lock);
        counter++;
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}
```

Now it is **thread-safe**

### Deadlock
A deadlock happens when: Two or more threads wait forever for each other’s resources.

#### Example (Deadlock)
```c
pthread_mutex_t A, B;

void* t1(void* arg) {
    pthread_mutex_lock(&A);
    pthread_mutex_lock(&B);
}

void* t2(void* arg) {
    pthread_mutex_lock(&B);
    pthread_mutex_lock(&A);
}
```

t1 waits for B, t2 waits for A → infinite wait

#### How to Avoid Deadlocks
- Always lock in the same order
- Use timeouts
- Keep critical sections small

### Thread-Safe Code
A program is thread-safe if: It works correctly even when multiple threads run simultaneously.

#### Ways to achieve thread safety:
- **Use mutex:** Protect shared variables
- **Avoid shared data:** Use local variables instead
- **Use atomic operations:** (e.g., stdatomic.h)

#### Thread-safe Example
```c
#include <stdio.h>
#include <pthread.h>

int counter = 0;
pthread_mutex_t lock;

void* increment(void* arg) {
    for(int i = 0; i < 100000; i++) {
        pthread_mutex_lock(&lock);
        counter++;
        pthread_mutex_unlock(&lock);
    }
    return NULL;
}
```
