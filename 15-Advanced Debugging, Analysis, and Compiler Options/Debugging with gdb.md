## GDB (GNU debugger)
GDB (GNU Debugger) is a powerful command-line debugger for C/C++ and other languages that allows you to:
* Run programs step by step
* Set breakpoints to pause execution
* Inspect and modify variables
* Examine the call stack
* Debug crashes (core dumps)

## Compiling for GDB
```bash
# Must compile with debug symbols (-g flag)
gcc -g program.c -o program

# With optimizations disabled (easier debugging)
gcc -g -O0 program.c -o program

# With extra debug info
gcc -g3 program.c -o program

# Without stripping symbols
gcc -g program.c -o program
# Don't use strip on debug binaries
```

## Starting GDB
```bash
# Start GDB with a program
gdb ./program

# Start with command-line arguments
gdb --args ./program arg1 arg2 arg3

# Debug a core dump
gdb ./program core

# Attach to running process by PID
gdb -p 1234

# Run GDB and execute commands
gdb -ex "run" -ex "bt" ./program
```

### Example 1
```c
#include <stdio.h>

int main (void) {
    const int data[5] = {1, 2, 3, 4, 5};
    int i = 0, sum = 0;

    for (i = 0; i >= 0; ++i)   // ERROR: condition is always true
        sum += data[i];

    printf ("sum = %i\n", sum);

    return 0;
}
```

**The problem:** The loop condition i >= 0 is always true because i starts at 0 and only increases (++i).
So the loop runs forever, incrementing i past the array bounds (0, 1, 2, 3, 4, 5, 6, …), until it tries to access memory outside data[], causing a segmentation fault.

#### Compiling for Debugging
We compile with the -g flag to include debug symbols.
This lets GDB show line numbers and variable values.

```bash
gcc -g main.c -o a.exe
```

#### Starting GDB
```bash
gdb a.exe
```

GDB loads the program and shows its startup message.

#### output
```text
GNU gdb (GDB) (Cygwin 15.2-1) 15.2
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-pc-cygwin".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from a.exe...
(gdb)
```

#### Running the Program Inside GDB
later we need to write run

```bash
(gdb) run
```

and the gdb inform us the error
#### Output:
```text
Starting program: /cygdrive/c/Users/ASUS/Documents/Intermediate_C/15-Compiler_options_and_debugging/Debugging_with_gdb/a.exe
[New Thread 15392.0x47e4]
[New Thread 15392.0x34f4]
[New Thread 15392.0x18a8]

Thread 1 "a" received signal SIGSEGV, Segmentation fault.
0x00000001004010cc in main () at main.c:8
8               sum += data[i];
```

#### list 9
```bash
(gdb) list 9
```

* list (or l) shows source code around a given line number.
* list 9 shows line 9 plus several lines before and after (usually 10 lines total).
* This helps you see the context of the crash: the for loop and the line where the segmentation fault occurred.

#### Output
```text
4           const int data[5] = {1, 2, 3, 4, 5};
5           int i = 0, sum = 0;
6
7           for (i = 0; i >= 0; ++i)
8               sum += data[i];
9
10          printf ("sum = %i\n", sum);
11
12          return 0;
13      }
```

#### print sum
```bash
(gdb) print sum
```

* print sum (or p sum) shows the current value of the variable sum at the moment of the crash.

#### Output
```text
$1 = -1328399838
```
* $1 is GDB's value history number (first value printed)
* -1328399838 is the actual value

#### print i
```bash
(gdb) print i
```

#### Output
```text
$2 = 789780
```

* GDB automatically assigns numbers to every value you print, so you can refer to them later.
* $2 = second value you printed
* 789780 is the actual value

#### print data
```bash
(gdb) print data
```

#### Output
```text
$3 = {1, 2, 3, 4, 5}
```

#### print data[0]
```bash
(gdb) print data[0]
```

#### Output
```text
$4 = 1
```

#### quit
```bash
(gbd) quit
```

quit exits GDB and returns you to the normal command prompt.

#### Output
```text
A debugging session is active.

        Inferior 1 [process 15392] will be killed.

Quit anyway? (y or n)
```

#### set var i = 5
```bash
(gdb) set var i = 5
```

This command changes the value of i while the program is paused (after the crash).

After setting i = 5, you can

```bash
(gdb) print i
```

#### Output
```text
$1 = 5
```

#### set var i = i * 2
```bash
(gdb) set var i = i * 2
```

```bash
(gdb) print i
```

#### Output
```text
$2 = 10
```

#### set var i = $1+20
```bash
(gdb) set var i = $1+20
```

```bash
(gdb) print i
```

#### Output
```text
$3 = 25
```

#### print main::i
```bash
(gdb) print main::i
```

Print the variable i from the function main.

#### Output
```text
$4 = 25
```

#### set var main::i = 0
```bash
(gdb) set var main::i = 0
```

This sets the variable i inside main to 0

#### set var x = 9 (x isnt in the program)
```bash
(gdb) set var x = 9
```

#### Output
```text
No symbol "x" in current context.
```

This means GDB cannot find any variable named x that is accessible right now.

#### p /x i
```bash
(gdb) p /x i
```

This prints the variable i in hexadecimal (base 16) format.
* p = short for print
* /x = format specifier for hexadecimal
* i = the variable name

#### Output
```text
$5 = 0x0
```

#### list 10, 15
```bash
(gdb) list 10, 15
```

This shows lines 10 through 15 of your source code.

#### Output
```text
10          printf ("sum = %i\n", sum);
11
12          return 0;
13      }
```

### Example 2:
```c
#include <stdio.h>

int foo(void)
{
    int x = 0;
    return x * x;
}

int main (void) 
{
    const int data[5] = {1, 2, 3, 4, 5};
    int i = 0, sum = 0;

    for (i = 0; i >= 0; ++i)
        sum += data[i];

    printf ("sum = %i\n", sum);

    return 0;
}
```

Now i added a function called foo

#### list foo
```bash
(gdb) list foo
```

This tells GDB to show the source code for the function named foo.

#### Output
```text
1       #include <stdio.h>
2
3       int foo(void)
4       {
5           int x = 0;
6           return x * x;
7       }
8
9       int main (void)
10      {
```

#### info source
```bash
(gdb) info source
```

This displays information about the source file you're currently debugging.

#### Output
```text
Current source file is main.c
Compilation directory is /cygdrive/c/Users/ASUS/Documents/Intermediate_C/15-Compiler_options_and_debugging/Debugging_with_gdb
Located in /cygdrive/c/Users/ASUS/Documents/Intermediate_C/15-Compiler_options_and_debugging/Debugging_with_gdb/main.c
Contains 20 lines.
Source language is c.
Producer is GNU C17 13.4.0 -mtune=generic -march=x86-64 -g.
Compiled with DWARF 5 debugging format.
Does not include preprocessor macro info.
```

#### list +
```bash
(gbd) list +
```

This shows the next set of lines after your previous list command.

#### Output
```text
11          const int data[5] = {1, 2, 3, 4, 5};
12          int i = 0, sum = 0;
13
14          for (i = 0; i >= 0; ++i)
15              sum += data[i];
16
17          printf ("sum = %i\n", sum);
18
19          return 0;
20      }
```

```bash
(gdb) list 1      # Shows lines 1-10
(gdb) list +        # Shows lines 11-20
(gdb) list +        # Shows lines 21-30
(gdb) list +        # Shows lines 31-40
```

#### list -
```bash
(gdb) list -
```

Show previous 10 lines

#### Output
```text
1       #include <stdio.h>
2
3       int foo(void)
4       {
5           int x = 0;
6           return x * x;
7       }
8
9       int main (void)
10      {
```

## Inserting Breakpoints
### Breakpoint 
A breakpoint pauses program execution at a specific line or function, allowing you to inspect variables, step through code, and understand what's happening.

### Setting Breakpoints in Your Program
#### 1. Break at a line number
```bash
(gdb) break 12
```

You set a breakpoint at line 12 - the line that declares and initializes i and sum

#### output
```text
Breakpoint 1 at 0x1004010cb: file main.c, line 12.
```

0x1004010cb is the location in memory where your breakpoint was set - specifically, the address of line 12 in your compiled program.

#### 2. Break at a function
```bash
(gdb) break main
```

Pauses when main() starts

#### output
```text
Breakpoint 2 at 0x1004010a8: file main.c, line 11.
```

#### 3. Break at a specific line in a function
```bash
(gdb) break main.c:15
```

This means: "line 15 inside function main" (absolute line number in the file).

#### Output
```text
Breakpoint 3 at 0x1004010e2: file main.c, line 15.
```

### Running With Breakpoints
```bash
(gdb) run
```

#### Output
```text
Starting program: /cygdrive/c/Users/ASUS/Documents/Intermediate_C/15-Compiler_options_and_debugging/Debugging_with_gdb/a.exe
[New Thread 11744.0x4b64]
[New Thread 11744.0x20b4]
[New Thread 11744.0x2b7c]

Thread 1 "a" hit Breakpoint 2, main () at main.c:11
11          const int data[5] = {1, 2, 3, 4, 5};
```

The program stops at breakpoint 2 since that is the first line it reaches

#### c (continue)
```bash
(gdb) c
```

This tells GDB to resume running the program from where it stopped until:
* The next breakpoint is hit
* The program crashes
* The program finishes normally
* You interrupt with Ctrl+C

#### Output
```text
Continuing.

Thread 1 "a" hit Breakpoint 1, main () at main.c:12
12          int i = 0, sum = 0;
```

gdb stopped because he reached the next breakpoint

#### s (step)
```bash
(gdb) s
```

* This executes one line of code and then stops again. Unlike continue which runs until the next breakpoint, step moves line by line.
* Steps into functions, you want to see inside called functions

#### Output
```text
14          for (i = 0; i >= 0; ++i)
```

gdb stopped at line 14 because is the next code line, maybe 13 has no code or an instruction

#### next (n)
```bash
(gdb) n
```

* This executes one line of code but steps over function calls (executes them without entering)
* Steps over functions, You trust the function works correctly

#### Output
```text
Thread 1 "a" hit Breakpoint 3, main () at main.c:15
15              sum += data[i];
```

#### info locals
```bash
(gdb) info locals
```

This shows all local variables in the current function and their current values.

#### info break
```
(gdb) info break
```

This shows all breakpoints you've set, their status, and how many times they've been hit.

#### clear
```bash
(gdb) clear
```

Without arguments, clear removes the breakpoint at the current line (where execution is paused).

#### example: clear foo
```bash
(gdb) clear foo
```

This removes all breakpoints located at the function foo.

#### bt - Backtrace
```bash
(gdb) bt
```

Shows the call stack - all functions that led to the current point.

#### info args - Show Function Arguments
```bash
(gdb) info args
```

Shows all arguments passed to the current function.

#### Example output

If you had a function with arguments:

```c
int add(int a, int b) {
    return a + b;
}
```

```bash
(gdb) break add
(gdb) run
(gdb) info args
a = 5
b = 10
```

#### help - GDB's Built-in Documentation
```bash
(gdb) help
```

Shows all command categories available in GDB.

#### Output example:
```text
List of classes of commands:

aliases -- Aliases of other commands
breakpoints -- Making program stop at certain points
data -- Examining data
files -- Specifying and examining files
internals -- Maintenance commands
obscure -- Obscure features
running -- Running the program
stack -- Examining the stack
status -- Status inquiries
support -- Support facilities
tracepoints -- Tracing of program execution
user-defined -- User-defined commands

Type "help" followed by a class name for a list of commands in that class.
Type "help all" for the list of all commands.
Type "help command" for brief information on a single command.
Type "apropos word" to search for commands matching "word".
```

#### More specific help:
```bash
(gdb) help breakpoints
# Shows all breakpoint-related commands

(gdb) help break
# Shows detailed help for the 'break' command

(gdb) help run
# Shows how to use 'run'

(gdb) help info
# Shows all 'info' subcommands

(gdb) help stack
# Shows backtrace, frame, etc. commands
```

## Call stack
The call stack is a data structure that a program uses to keep track of function calls. It operates on the Last In, First Out (LIFO) principle, meaning the last function called is the first one to finish and be removed.

### How it works
1. When a function is called, a new frame (containing arguments, local variables, and the return address) is pushed onto the stack.
2. If that function calls another function, the new function's frame is pushed on top.
3. When a function returns, its frame is popped off the stack, and execution resumes where the previous function left off.

### Simple example (C)
```c
#include <stdio.h>

void functionC() {
    printf("Entering function C\n");
    printf("Exiting function C\n");
}

void functionB() {
    printf("Entering function B\n");
    functionC();  // Call C from B
    printf("Exiting function B\n");
}

void functionA() {
    printf("Entering function A\n");
    functionB();  // Call B from A
    printf("Exiting function A\n");
}

int main() {
    printf("Starting main\n");
    functionA();  // Call A from main
    printf("Ending main\n");
    return 0;
}
```
### Output:
```text
Starting main
Entering function A
Entering function B
Entering function C
Exiting function C
Exiting function B
Exiting function A
Ending main
```

### What happens on the call stack:
1. main() is pushed
2. functionA() is pushed on top
3. functionB() is pushed on top
4. functionC() is pushed on top
5. functionC() finishes → popped
6. functionB() finishes → popped
7. functionA() finishes → popped
8. main() finishes → popped

### Stack overflow example:
```c
#include <stdio.h>

void recursive() {
    int arr[1000];  // Takes stack space
    printf(".");
    recursive();    // Calls itself forever
}

int main() {
    recursive();  // Will crash with stack overflow
    return 0;
}
```

This will eventually crash because the stack keeps growing with no returns to pop frames off.
