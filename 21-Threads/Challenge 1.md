## Challenge 1
The challenge consists of creating a multithreaded program in C using pthreads where 10 threads are created, each receiving a different argument. 
All threads execute the same function, which increments a shared global counter and prints a message along with its thread ID and the counter value twice. 
The main thread must wait for all child threads to finish using pthread_join. By running the program multiple times, the goal is to observe how the shared counter behaves, highlighting issues like race conditions due to the lack of synchronization.

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

#define NUM_THREADS 10

int counter = 0;   // global shared variable

void* print_message(void* arg) {
    int number = *(int*)arg;

    counter++;

    printf("Message from thread number %d | Thread ID: %lu | Counter: %d\n",
           number, pthread_self(), counter);

    printf("Again from thread number %d | Thread ID: %lu | Counter: %d\n",
           number, pthread_self(), counter);

    pthread_exit(NULL);
}

int main() {
    pthread_t threads[NUM_THREADS];
    int numbers[NUM_THREADS];

    for (int i = 0; i < NUM_THREADS; i++) {
        numbers[i] = i + 1;

        if (pthread_create(&threads[i], NULL, print_message, &numbers[i]) != 0) {
            perror("Error creating thread");
            exit(1);
        }
    }

    for (int i = 0; i < NUM_THREADS; i++) {
        pthread_join(threads[i], NULL);
    }

    printf("All threads finished. Final counter value: %d\n", counter);

    pthread_exit(NULL);
}
```

To compile:

```bash
gcc main.c -o main -pthread
```

To run:

```bash
./main
```

### What strange thing happens?
The counter is shared by all threads, so one thread may increment it, print once, and before it prints the second time, another thread can change the counter. 
Because of that, the two print statements inside the same thread may show different counter values. This happens because there is a race condition.
