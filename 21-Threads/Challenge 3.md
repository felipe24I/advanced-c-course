## Challenge 3 using mutex + condition variable
This challenge consists of modifying the previous mutex program to also use condition variables, 
so that all threads with even numbers execute and print first, while threads with odd numbers wait. 
Since thread execution order is not guaranteed, the odd threads must sleep using pthread_cond_wait until a shared variable confirms that all even threads have finished. 
Once the last even thread finishes, it uses pthread_cond_broadcast to wake up all waiting odd threads. 
This ensures synchronization and controlled ordering between groups of threads.

```c
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h>
#include <unistd.h>

#define NUM_THREADS 10

int counter = 0;
int number_evens_finished = 0;

pthread_mutex_t mutex;
pthread_cond_t cond;

void* print_message(void* arg) {
    int number = *(int*)arg;

    pthread_mutex_lock(&mutex);

    if (number % 2 != 0) {
        while (number_evens_finished < NUM_THREADS / 2) {
            pthread_cond_wait(&cond, &mutex);
        }
    }

    counter++;

    printf("Thread number %d | Thread ID: %lu | Counter: %d\n",
           number, pthread_self(), counter);

    printf("Again thread number %d | Thread ID: %lu | Counter: %d\n",
           number, pthread_self(), counter);

    if (number % 2 == 0) {
        number_evens_finished++;

        if (number_evens_finished == NUM_THREADS / 2) {
            pthread_cond_broadcast(&cond);
        }
    }

    pthread_mutex_unlock(&mutex);

    pthread_exit(NULL);
}

int main() {
    pthread_t threads[NUM_THREADS];
    int numbers[NUM_THREADS];

    pthread_mutex_init(&mutex, NULL);
    pthread_cond_init(&cond, NULL);

    for (int i = 0; i < NUM_THREADS; i++) {
        numbers[i] = i + 1;

        if (pthread_create(&threads[i], NULL, print_message, &numbers[i]) != 0) {
            perror("Error creating thread");
            exit(1);
        }
    }

    sleep(1);

    for (int i = 0; i < NUM_THREADS; i++) {
        pthread_join(threads[i], NULL);
    }

    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond);

    printf("All threads finished. Final counter value: %d\n", counter);

    return 0;
}
```

Compile:

```bash
gcc main.c -o main -pthread
```

In this solution, odd threads wait until all even threads finish. When the last even thread finishes, it wakes all odd threads using pthread_cond_broadcast.
