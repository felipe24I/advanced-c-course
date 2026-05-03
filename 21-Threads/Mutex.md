## What is a Mutex?
A mutex (mutual exclusion) is a synchronization tool that ensures: Only one thread at a time can access a critical section.

It prevents race conditions when threads share data.

### Basic Idea
- **Lock →** enter critical section
- **Unlock →** leave critical section

```text
Thread 1 → LOCK → use resource → UNLOCK  
Thread 2 → waits → LOCK → use resource → UNLOCK
```

### Basic Usage
#### 1. Declare and initialize
```c
pthread_mutex_t lock;
pthread_mutex_init(&lock, NULL);
```

#### 2. Lock and unlock
```c
pthread_mutex_lock(&lock);

// critical section

pthread_mutex_unlock(&lock);
```

### 3. Destroy
```c
pthread_mutex_destroy(&lock);
```

### Example (Prevent Race Condition)
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

int main() {
    pthread_t t1, t2;

    pthread_mutex_init(&lock, NULL);

    pthread_create(&t1, NULL, increment, NULL);
    pthread_create(&t2, NULL, increment, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&lock);

    printf("Counter: %d\n", counter);
    return 0;
}
```

### Important Functions
#### pthread_mutex_lock()
Blocks until the mutex is available.

### pthread_mutex_trylock()
Does not block.

```c
if (pthread_mutex_trylock(&lock) == 0) {
    // got the lock
    pthread_mutex_unlock(&lock);
}
```

### pthread_mutex_unlock()
Releases the mutex.

### Common mistakes
#### Forgetting to unlock
```c
pthread_mutex_lock(&lock);
// no unlock → deadlock
```

#### Locking twice (same thread)
```c
pthread_mutex_lock(&lock);
pthread_mutex_lock(&lock); // blocks forever
```

#### Not initializing
```c
pthread_mutex_t lock; // not initialized → undefined behavior
```

### Deadlock Example
```c
pthread_mutex_lock(&A);
pthread_mutex_lock(&B);

// another thread:
pthread_mutex_lock(&B);
pthread_mutex_lock(&A);
```

Circular wait → deadlock

### Good Practices
- Keep critical sections short
- Always unlock (even on errors)
- Use consistent lock order
- Initialize and destroy properly

### Quick Summary
- Mutex = mutual exclusion
- Prevents race conditions
- lock() → enter critical section
- unlock() → leave
- Misuse → deadlocks
