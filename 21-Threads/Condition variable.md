## Condition variable
A **condition variable** in C threads is used when one thread must **wait until another thread changes a condition.**

It is normally used together with a **mutex**

```c
#include <stdio.h>
#include <pthread.h>
#include <unistd.h>

pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;

int ready = 0;

void* worker(void* arg) {
    pthread_mutex_lock(&mutex);

    while (ready == 0) {
        printf("Worker waiting...\n");
        pthread_cond_wait(&cond, &mutex);
    }

    printf("Worker continues because ready = 1\n");

    pthread_mutex_unlock(&mutex);
    return NULL;
}

void* signaler(void* arg) {
    sleep(2);

    pthread_mutex_lock(&mutex);

    ready = 1;
    printf("Signaler changed ready to 1\n");

    pthread_cond_signal(&cond);

    pthread_mutex_unlock(&mutex);
    return NULL;
}

int main() {
    pthread_t t1, t2;

    pthread_create(&t1, NULL, worker, NULL);
    pthread_create(&t2, NULL, signaler, NULL);

    pthread_join(t1, NULL);
    pthread_join(t2, NULL);

    pthread_mutex_destroy(&mutex);
    pthread_cond_destroy(&cond);

    return 0;
}
```

### Important parts:
pthread_cond_wait(&cond, &mutex);

This does two things:
1. Releases the mutex.
2. Sleeps until another thread sends a signal.

When it wakes up, it locks the mutex again.

That is why we use:

```c
while (ready == 0)
```

and not only:

```c
if (ready == 0)
```

Because a thread can wake up even if the condition is not really ready yet.

### Main functions:
```c
pthread_cond_init()
pthread_cond_wait()
pthread_cond_signal()
pthread_cond_broadcast()
pthread_cond_destroy()
```

### 1. pthread_cond_init
```c
int pthread_cond_init(pthread_cond_t *cond, const pthread_condattr_t *attr);
```

#### What it does:
Initializes a condition variable.

#### Details:
- cond: the condition variable you will use
- attr: usually NULL (default behavior)

#### Key idea:
You must initialize it **before using it.**

#### Example:
```c
pthread_cond_t cond;
pthread_cond_init(&cond, NULL);
```

Alternative (static initialization):

```c
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;
```

The term **static initialization** in the context of pthread_cond_t cond = PTHREAD_COND_INITIALIZER; refers to compile-time allocation and setup, rather than runtime execution.

### 2. pthread_cond_wait
```c
int pthread_cond_wait(pthread_cond_t *cond, pthread_mutex_t *mutex);
```

#### What it does:
Makes a thread **sleep until a condition is signaled.**

#### What happens internally (VERY IMPORTANT)
When you call:

```c
pthread_cond_wait(&cond, &mutex);
```

It does 3 things atomically:
1. Unlocks the mutex
2. Puts the thread to sleep
3. When awakened → locks the mutex again

#### Why use a while loop?
```c
while (condition == false) {
    pthread_cond_wait(&cond, &mutex);
}
```

#### Reasons:
- **Spurious wakeups** (thread wakes up without signal)
- Multiple threads competing
- Condition may no longer be true

### 3. pthread_cond_signal
```c
int pthread_cond_signal(pthread_cond_t *cond);
```

#### What it does:
Wakes up **ONE** thread waiting on the condition.

#### Important:
- If no threads are waiting → signal is lost
- Must be used with a mutex

#### Example:
```c
pthread_mutex_lock(&mutex);

ready = 1;

pthread_cond_signal(&cond);

pthread_mutex_unlock(&mutex);
```

### 4. pthread_cond_broadcast
```c
int pthread_cond_broadcast(pthread_cond_t *cond);
```

#### What it does:
Wakes up **ALL waiting threads**

#### When to use:
- When multiple threads depend on the same condition
- Example: producer-consumer with many consumers

### 5. pthread_cond_destroy
```c
int pthread_cond_destroy(pthread_cond_t *cond);
```

#### What it does:
Releases resources used by the condition variable.

#### Important:
- No threads should be waiting when you destroy it
- Otherwise → undefined behavior

#### Example:
```c
pthread_cond_destroy(&cond);
```

### 6. pthread_cond_timedwait (VERY USEFUL)
```c
int pthread_cond_timedwait(
    pthread_cond_t *cond,
    pthread_mutex_t *mutex,
    const struct timespec *abstime
);
```

#### What it does:
Like pthread_cond_wait, but with a timeout

#### Behavior:
Waits until:
- Condition is signaled 
- OR time expires

#### Return values:
- 0 → success
- ETIMEDOUT → timeout occurred

#### Example:
```c
struct timespec ts;
clock_gettime(CLOCK_REALTIME, &ts);
ts.tv_sec += 5;

int res = pthread_cond_timedwait(&cond, &mutex, &ts);

if (res == ETIMEDOUT) {
    printf("Timeout!\n");
}
```

#### Complete Concept (IMPORTANT FOR EXAM)
A condition variable is ALWAYS used with:

- A shared variable (state)
- A mutex (protection)
- A condition (logic)

### Pattern (memorize this)
#### Waiting thread:
```c
pthread_mutex_lock(&mutex);

while (!condition) {
    pthread_cond_wait(&cond, &mutex);
}

pthread_mutex_unlock(&mutex);
```

#### Signaling thread:
```c
pthread_mutex_lock(&mutex);

condition = true;

pthread_cond_signal(&cond);

pthread_mutex_unlock(&mutex);
```

#### Common mistakes (EXAM traps)
- Forgetting mutex
- Using if instead of while
- Signaling without changing condition
- Destroying while threads still wait
- Not protecting shared variable

#### Real-world intuition
Think of it like:
- Mutex = door lock
- Condition variable = doorbell

Thread: “I’ll sleep until someone rings the bell AND the condition is true.”
