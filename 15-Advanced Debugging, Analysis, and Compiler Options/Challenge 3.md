## Challenge 3
Based on your challenge, here's how to accomplish each task in gdb with a program compiled using gcc -g (debug symbols required!)

### 1. Start GDB
```bash
gdb main.exe
```

### 2. List Source Code
```bash
list                   # Shows 10 lines around current position
list 20                # Shows lines around line 20
list main              # Shows function 'main'
list 1,50              # Shows lines 1 through 50
```

