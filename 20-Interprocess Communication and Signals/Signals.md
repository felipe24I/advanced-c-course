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
