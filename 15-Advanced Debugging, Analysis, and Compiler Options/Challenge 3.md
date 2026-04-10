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


