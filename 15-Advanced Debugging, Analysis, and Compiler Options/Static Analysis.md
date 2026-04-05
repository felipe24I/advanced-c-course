## Static Analysis
Static analysis is the process of examining source code without executing it to find potential bugs, security vulnerabilities, coding standard violations, and other issues. 
It's like having an automated code reviewer that never gets tired.

```bash
# Simple static analysis with GCC warnings
gcc -Wall -Wextra -Wconversion program.c

# More thorough analysis
gcc -Wall -Wextra -Wpedantic -Wshadow -Wformat=2 program.c
```

### Coverity
Coverity (now part of Synopsys) is a static application security testing (SAST) tool that finds critical software defects such as null pointer dereferences, memory leaks, and concurrency issues without executing the code. 
It is widely used in industries like automotive, finance, and telecommunications for safety and security compliance

### CodeSonar
CodeSonar is a Static Application Security Testing (SAST) tool developed by CodeSecure (formerly GrammaTech). It is designed to find deep, complex defects in software that could lead to system crashes, memory corruption, data races, or security vulnerabilities . 
The tool is specifically built for "zero-tolerance" environments such as automotive, avionics, medical devices, and industrial control systems 

Unlike simple linters that check formatting, CodeSonar performs whole-program, interprocedural analysis using a technique called "abstract execution." 
This allows it to understand complex interactions across different functions and files, identifying issues that other tools often miss 
