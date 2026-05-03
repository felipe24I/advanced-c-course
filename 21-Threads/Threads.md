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

### Threads in C (POSIX Threads - pthread)
C does not have built-in thread support in the standard library, but you can use the **POSIX thread library** (pthread).

### Basic Example
```c
#include <stdio.h>
#include <pthread.h>

void* myThread(void* arg) {
    printf("Hello from thread!\n");
    return NULL;
}

int main() {
    pthread_t thread;

    pthread_create(&thread, NULL, myThread, NULL);
    pthread_join(thread, NULL);

    printf("Thread finished.\n");
    return 0;
}
```

### Explanation
- pthread_t → thread identifier
- pthread_create() → creates a new thread
- pthread_join() → waits for the thread to finish
- Thread function must return void*

### Multiple Threads Example
```c
#include <stdio.h>
#include <pthread.h>

void* printNumber(void* arg) {
    int num = *(int*)arg;
    printf("Thread %d\n", num);
    return NULL;
}

int main() {
    pthread_t threads[3];
    int ids[3] = {1, 2, 3};

    for(int i = 0; i < 3; i++) {
        pthread_create(&threads[i], NULL, printNumber, &ids[i]);
    }

    for(int i = 0; i < 3; i++) {
        pthread_join(threads[i], NULL);
    }

    return 0;
}
```

### Important Concepts
1. Race Condition: When multiple threads access shared data at the same time.
2. Mutex (Mutual Exclusion): Used to protect shared resources.

```c
pthread_mutex_t lock;

pthread_mutex_lock(&lock);
// critical section
pthread_mutex_unlock(&lock);
```

### Example with Mutex
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

    printf("Counter: %d\n", counter);
    return 0;
}
```

### Compilation
```bash
gcc program.c -o program -lpthread
```

### Key Takeaways
- Threads share memory → fast but risky
- Use pthread in C
- Always manage synchronization (mutex, etc.)
- Useful for concurrent and parallel programs

### Important note
- Each thread has its own stack so local variables are not shared amongst each thread
- Threads of the same process share global variables
