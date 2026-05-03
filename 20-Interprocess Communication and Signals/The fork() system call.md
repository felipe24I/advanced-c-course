## The fork() system call
fork() is used to **create a new process.** It duplicates the current process, creating a **child process.**

### Basic idea
When you call fork():

- The original process = **parent**
- A new process = **child**
- Both run the same code **after the fork**

### Syntax
```c
#include <unistd.h>

pid_t fork(void);
```

### Return values
This is the most important part:

- 0:	Running in the child
- > 0: Running in the parent (returns child PID)
- -1:	Error

### Example
```c
#include <stdio.h>
#include <unistd.h>

int main() {
    pid_t pid = fork();

    if (pid == 0) {
        // Child process
        printf("I am the child\n");
    } else if (pid > 0) {
        // Parent process
        printf("I am the parent, child PID = %d\n", pid);
    } else {
        printf("Fork failed\n");
    }

    return 0;
}
```

### What happens in memory?
- The child gets a **copy of the parent’s memory**
- Variables are duplicated (not shared by default)

```c
int x = 5;
fork();
x++;
```

Both processes will have their own x

### Important behavior
After fork(), both processes run **independently:**

```c
printf("Hello\n");
fork();
printf("World\n");
```

Output:

```text
Hello
World
World
```

### Why is fork() important?
It is the base for:

- Process creation
- Running programs (exec() after fork)
- Servers (handling multiple clients)
