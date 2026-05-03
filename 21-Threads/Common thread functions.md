## Common thread functions
### pthread_detach
pthread_detach() tells the system that you will not use pthread_join() for that thread.

When the thread finishes, its resources are released automatically.

```c
pthread_t t;

pthread_create(&t, NULL, task, NULL);
pthread_detach(t);
```

Important: a detached thread **cannot be joined** later.

### Stack management
Each thread has its own **stack**, used for local variables and function calls.

You can control stack size using **pthread_attr_t.**

```c
pthread_attr_t attr;
pthread_t t;

pthread_attr_init(&attr);
pthread_attr_setstacksize(&attr, 1024 * 1024); // 1 MB stack

pthread_create(&t, &attr, task, NULL);

pthread_attr_destroy(&attr);
pthread_join(t, NULL);
```

Useful when a thread needs many local variables or deep function calls.

### pthread_equal
Used to compare two thread IDs.

Do **not** compare thread IDs using ==.

```c
if (pthread_equal(t1, t2)) {
    printf("Same thread\n");
} else {
    printf("Different threads\n");
}
```

You can get the current thread ID with:

```c
pthread_t self = pthread_self();
```

### pthread_once
Used to run initialization code only once, even if many threads try to call it.

Very useful for global setup.

```c
#include <stdio.h>
#include <pthread.h>

pthread_once_t once_control = PTHREAD_ONCE_INIT;

void init() {
    printf("Initialization only once\n");
}

void* task(void* arg) {
    pthread_once(&once_control, init);
    printf("Thread running\n");
    return NULL;
}

int main() {
    pthread_t t1, t2;

    pthread_create(&t1, NULL, task, NULL);
    pthread_create(&t2, NULL, task, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    return 0;
}
```

Output idea:

```text
Initialization only once
Thread running
Thread running
```

#### Quick summary
- pthread_detach() → thread cleans itself, no join.
- Stack management → configure each thread’s stack size.
- pthread_equal() → safely compares thread IDs.
- pthread_once() → guarantees one-time initialization.
