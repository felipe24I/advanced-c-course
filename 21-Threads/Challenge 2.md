## Challenge 2 Using mutex
This challenge focuses on fixing the race condition from the previous exercise by ensuring data consistency when multiple threads access a shared global counter. 
The goal is to modify the program by introducing a mutex to protect the critical section, so that when a thread increments and prints the counter,
no other thread can modify it at the same time. Each thread must lock the mutex before accessing the shared variable and unlock it afterward, 
guaranteeing that both print statements within the same thread display the same counter value and preventing inconsistent results caused by concurrent access.

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>

#define NUM_THREADS 10

int counter = 0;
pthread_mutex_t mutex;

void* print_message(void* arg) {
    int number = *(int*)arg;

    pthread_mutex_lock(&mutex);

    counter++;

    printf("Message from thread number %d | Thread ID: %lu | Counter: %d\n",
           number, pthread_self(), counter);

    printf("Again from thread number %d | Thread ID: %lu | Counter: %d\n",
           number, pthread_self(), counter);

    pthread_mutex_unlock(&mutex);

    pthread_exit(NULL);
}

int main() {
    pthread_t threads[NUM_THREADS];
    int numbers[NUM_THREADS];

    pthread_mutex_init(&mutex, NULL);

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

    pthread_mutex_destroy(&mutex);

    printf("All threads finished. Final counter value: %d\n", counter);

    return 0;
}
```
