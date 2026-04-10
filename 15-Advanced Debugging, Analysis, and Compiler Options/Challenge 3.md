## Challenge 3
Based on your challenge, here's how to accomplish each task in gdb with a program compiled using gcc -g (debug symbols required!)

### 1. Start GDB
```bash
gdb main.exe
```

<img width="1173" height="421" alt="image" src="https://github.com/user-attachments/assets/45776b64-497a-4c4d-8bdc-d895ed84fa82" />

### 2. List Source Code
```bash
list                   # Shows 10 lines around current position
list 20                # Shows lines around line 20
list main              # Shows function 'main'
list 1,50              # Shows lines 1 through 50
```

<img width="527" height="285" alt="image" src="https://github.com/user-attachments/assets/99298e10-b661-45ec-926b-16671a3b5c90" />

### 3. Set Breakpoints
```bash
break main             # Stop at start of main function
break 25               # Stop at line 25
break main.c:15        # Stop at line 15 in specific file
break sum              # Stop at sum function
```

<img width="608" height="247" alt="image" src="https://github.com/user-attachments/assets/bb78060a-8377-4123-85a0-62cfb722f264" />

### 4. Run the Program
```bash
run                    # Start program (stops at breakpoints)
run arg1 arg2          # Run with command-line arguments
```

<img width="1442" height="188" alt="image" src="https://github.com/user-attachments/assets/e6e3a39e-602d-45d4-b6de-5fdc235530ec" />

### 5. Print Variable Values
```bash
print x                # Show value of variable x
print &x               # Show memory address of x
print *ptr             # Show what pointer points to
print array[0]         # Show array element
print/x var            # Show in hexadecimal
print/t var            # Show in binary
print/d var            # Show in decimal
```

<img width="315" height="267" alt="image" src="https://github.com/user-attachments/assets/6754e6eb-4b64-49e1-a25a-1907789f8cba" />

### 6. Show the Call Stack (Backtrace)
```c
backtrace              # Full call stack
bt                     # Shorthand
bt full                # Shows local variables too
where                  # Same as backtrace
```

<img width="601" height="311" alt="image" src="https://github.com/user-attachments/assets/9d90ffe6-44a2-4f24-bca3-ea39e70acd69" />

A backtrace (also called a stack trace, stack backtrace, or call stack trace) is a report of the active stack frames at a certain point during a program's execution. In simpler terms, it shows the sequence of function calls that led to the current point in the program.

### 7. Continue Execution
```c
continue               # Resume until next breakpoint
c                      # Shorthand
```

<img width="845" height="282" alt="image" src="https://github.com/user-attachments/assets/d720a582-8cf8-4df1-a62e-689c708f2367" />

### 8. Delete Breakpoints
```bash
info breakpoints       # List all breakpoints (see their numbers)
delete 1               # Delete breakpoint #1
delete                 # Delete ALL breakpoints (asks confirmation)
clear main             # Clear breakpoint at main
clear 25               # Clear breakpoint at line 25
```

<img width="946" height="333" alt="image" src="https://github.com/user-attachments/assets/e064c788-cf1d-45f0-bc40-be23cb1ebaaa" />

### 9. Step Through Code
```bash
step                   # Step INTO functions
next                   # Step OVER functions
finish                 # Run until current function returns
```

<img width="1158" height="260" alt="image" src="https://github.com/user-attachments/assets/ed0b217e-fe53-4ee2-b734-6c9d44f082c6" />

### 10. Get Help
```bash
help                   # List all command categories
help breakpoints       # Help about breakpoints
help print             # Help about print command
help running           # Help about running programs
help all               # Show all commands
```

<img width="923" height="561" alt="image" src="https://github.com/user-attachments/assets/08593266-a000-4142-b613-a22b73d9bd2a" />
