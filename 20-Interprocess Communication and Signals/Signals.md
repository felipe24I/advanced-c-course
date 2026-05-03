## Signals
**Signals** are a type of **Interprocess Communication (IPC)** used to **notify a process that a specific event has occurred.**

### Simple idea:
A signal is like a **short alert or interruption** sent to a process.

It does **not send data**, only tells the process “something happened”.

### Common uses:
- Stop or terminate a process
- Pause/resume execution
- Handle errors (like illegal operations)
- Notify events (like child process finished)

### Examples of signals in Unix/Linux:
- SIGINT → Interrupt (e.g., pressing Ctrl + C)
- SIGKILL → Force kill a process
- SIGTERM → Request graceful termination
- SIGSTOP → Pause a process
- SIGCONT → Resume a paused process

<img width="682" height="303" alt="image" src="https://github.com/user-attachments/assets/1e78b89f-6c8e-41d5-8e8c-098dfe7cf43d" />

<img width="821" height="381" alt="image" src="https://github.com/user-attachments/assets/b41c02a1-0458-44ce-a081-d52cca12a86f" />

### How signals work:
1. A process (or OS) sends a signal
2. The target process receives it
3. The process:
- Executes a default action, or
- Runs a custom handler function

### Default actions
whenever a signal is raised (either programmatically or system generated signal), a default action is performed for some signals

- Term - the process will terminate
- Core - the process will terminate and produce a core dump file that traces the process state at the time of termination
- Ign - the process will ignore the signal
- Stop - the process will stop, like with a Ctrl-Z
- Cont - the process will continue from being stopped
  
### Simple example in C:
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    printf("Signal received: %d\n", sig);
}

int main() {
    signal(SIGINT, handler);  // Capture Ctrl+C

    while(1) {
        printf("Running...\n");
        sleep(1);
    }

    return 0;
}
```

When you press Ctrl + C, instead of stopping, it prints a message.

### Key idea for exam:
Signals are a lightweight IPC mechanism used to notify processes of events without exchanging data.

## Raising a signal
Raising a signal means sending a signal to a process, usually to notify it that an event occurred.

### Simple idea:
“Raise” = trigger or send a signal

A signal can be raised:

- By the same process (to itself)
- By another process
- By the operating system

### Common ways to raise a signal in C:
#### 1. Using raise() (to itself)
raise() send a signal to the current process

**Syntax:** 

```c
int raise (int sig)
```

- if this function is executed successfully, the signal specified in sig is generated
- if the call is unsuccessful, raise() returns a non-zero value
- **Note:** the function raise can only send a signal to the program that contains it, cannot send a signal to other processes

```c
#include <stdio.h>
#include <signal.h>

void handler(int sig) {
    printf("Signal received: %d\n", sig);
}

int main() {
    signal(SIGINT, handler);

    printf("Raising SIGINT...\n");
    raise(SIGINT);  // Send signal to itself

    return 0;
}
```

This sends a signal to the **same process.**

#### 2. Using kill() (to another process or itself)
kill() **sends a signal to a process (or group of processes)** using its PID (Process IDentifier).

- **Note:** **PID** It is a unique number assigned to every running process on a Linux, Unix, or Windows system.

**Syntax:**

```c
int kill(pid_t pid, int sig);
```

- pid → Process ID
- sig → Signal (e.g., SIGTERM, SIGKILL)

**How pid works:**

- pid > 0 → send to that specific process
- pid = 0 → send to all processes in the same group
- pid = -1 → send to all processes (with permission)
- pid < -1 → send to a process group

#### Example
```c
#include <signal.h>
#include <unistd.h>

int main() {
    kill(getpid(), SIGTERM);  // Send SIGTERM to itself
    return 0;
}
```

This requests the process to terminate gracefully.

**Common signals used with kill():**

- SIGTERM → polite termination
- SIGKILL → force termination (cannot be caught)
- SIGSTOP → pause
- SIGCONT → resume

**Important:**

- kill() returns 0 if successful
- Returns -1 if it fails (e.g., invalid PID or no permission)

#### 3. alarm() — Schedule a signal after time
alarm() schedules a **SIGALRM signal** to be sent to the process **after a number of seconds.**

**Syntax:** 

```c
unsigned int alarm(unsigned int seconds);
```

- seconds → time to wait
- Returns remaining time of a previous alarm (if any)

#### Example
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    printf("Alarm triggered!\n");
}

int main() {
    signal(SIGALRM, handler);

    alarm(3);  // After 3 seconds → SIGALRM

    while(1);  // Wait
    return 0;
}
```

After 3 seconds, the signal is raised automatically.


**Key behavior:**

- Only one alarm at a time per process
- Calling alarm() again replaces the previous one
- alarm(0) → cancels the alarm
  
## Handling a Signal using the signal function
Handling a signal means **defining what your program should do when a signal is received.**

### 1. What is signal()
The function signal() lets you **attach a handler function to a signal.**

So instead of the default behavior (like terminating), your program runs your own code.

### Syntax
```c
void (*signal(int sig, void (*handler)(int)))(int);
```

### Simplified:
```c
signal(signal_type, handler_function);
```

### 2. How it works (step by step)
1. You define a handler function
2. You register it with signal()
3. When the signal arrives → your handler executes

### 3. Basic example
```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    printf("Signal received: %d\n", sig);
}

int main() {
    signal(SIGINT, handler);  // handle Ctrl + C

    while(1) {
        printf("Running...\n");
        sleep(1);
    }

    return 0;
}
```

**What happens:**

- Press Ctrl + C
- Instead of terminating → it prints the message

### 4. Default vs Custom behavior

Each signal has a default action, for example:

- SIGINT → terminate program
- SIGTERM → terminate
- SIGKILL → force kill (cannot be handled)

With signal(), you override this behavior.

## Handling a signal using sigaction() in C
sigaction() is a more reliable and modern way to handle signals than signal().

### Basic idea
You create a struct sigaction, assign your handler, and register it for a signal.

```c
#include <stdio.h>
#include <signal.h>
#include <unistd.h>

void handler(int sig) {
    write(1, "Signal received\n", 16);
}

int main() {
    struct sigaction sa;

    sa.sa_handler = handler;   // function to execute
    sigemptyset(&sa.sa_mask);  // no extra signals blocked
    sa.sa_flags = 0;           // default behavior

    sigaction(SIGINT, &sa, NULL); // handle Ctrl + C

    while (1) {
        write(1, "Running...\n", 11);
        sleep(1);
    }

    return 0;
}
```

### What happens?
When you press Ctrl + C, the OS sends SIGINT. Instead of terminating, the program executes:

```c
void handler(int sig)
```

So the program prints:

```text
Signal received
```

and keeps running.

### Important fields
1. Stores the handler function
 
```c
sa.sa_handler
```

2. Signals to block while the handler is running.

```c
sa.sa_mask
```

3. Extra options, for example SA_RESTART.

```c
sa.sa_flags
```

### Example with SA_RESTART
```c
sa.sa_flags = SA_RESTART;
```

This helps restart some interrupted system calls automatically.
