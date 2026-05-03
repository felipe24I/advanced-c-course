## pthreads API (POSIX Threads)
The **pthreads API** is a standard library used in C to create and manage threads on Unix/Linux systems.

It allows you to:

- Create threads
- Synchronize them
- Control their execution

You must include:

```c
#include <pthread.h>
```

### 1. Creating a Thread → pthread_create
#### Syntax
```c
int pthread_create(pthread_t *thread,
                   const pthread_attr_t *attr,
                   void *(*start_routine)(void *),
                   void *arg);
```

#### Meaning of parameters
- thread → stores thread ID
- attr → thread attributes (usually NULL)
- start_routine → function the thread will run
- arg → argument passed to the function

#### Example
```c
#include <stdio.h>
#include <pthread.h>

void* task(void* arg) {
    printf("Hello from thread\n");
    return NULL;
}

int main() {
    pthread_t t;

    pthread_create(&t, NULL, task, NULL);

    pthread_join(t, NULL);

    return 0;
}
```

### 2. Waiting for a Thread → pthread_join
#### Syntax
```c
int pthread_join(pthread_t thread, void **retval);
```

#### What it does
- Blocks the calling thread (usually main)
- Waits until the specified thread finishes
- Optionally retrieves the return value

#### Example with return value
```c
void* task(void* arg) {
    return (void*) 42;
}

int main() {
    pthread_t t;
    void* result;

    pthread_create(&t, NULL, task, NULL);
    pthread_join(t, &result);

    printf("Returned: %ld\n", (long)result);
}
```

### 3. Ending a Thread → pthread_exit
#### Syntax
```c
void pthread_exit(void *retval);
```

#### What it does
- Terminates the calling thread
- Sends a return value to pthread_join

#### Example
```c
void* task(void* arg) {
    printf("Thread running\n");
    pthread_exit((void*)100);
}
```

#### Important Note
If you use return inside the thread function:

```c
return value;
```

It is equivalent to:

```c
pthread_exit(value);
```

#### Complete Example (All Together)
```c
#include <stdio.h>
#include <pthread.h>

void* work(void* arg) {
    int x = *(int*)arg;
    printf("Thread received: %d\n", x);
    pthread_exit((void*)(x * 2));
}

int main() {
    pthread_t t;
    int value = 10;
    void* result;

    pthread_create(&t, NULL, work, &value);

    pthread_join(t, &result);

    printf("Result: %ld\n", (long)result);

    return 0;
}
```

#### Quick Summary
- pthread_create() → creates a new thread
- pthread_join() → waits for a thread to finish
- pthread_exit() → terminates a thread and returns a value

### Why use a struct?
pthread_create only allows **one argument** (void*), so if you want to pass multiple values, you wrap them in a struct.

#### Example: Struct + Thread
```c
#include <stdio.h>
#include <pthread.h>

typedef struct {
    int x;
    int y;
} Data;

void* multiply(void* arg) {
    Data* d = (Data*)arg;

    int* result = malloc(sizeof(int));
    *result = d->x * d->y;

    pthread_exit((void*)result);
}

int main() {
    pthread_t t;
    Data d = {3, 4};
    void* res;

    pthread_create(&t, NULL, multiply, &d);
    pthread_join(t, &res);

    printf("Result: %d\n", *(int*)res);

    free(res);
    return 0;
}
```

#### Explanation (simple)
- You create a struct (Data) → stores multiple values
- You pass it as (void*) to the thread
- Inside the thread → cast it back:

```c
Data* data = (Data*)arg;
```

- Access values with data->x, data->y

#### Quick Summary
- Use struct to pass multiple values
- Cast void* → struct* inside thread
- Use -> to access members
- Use malloc if returning data
