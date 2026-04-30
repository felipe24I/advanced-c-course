## Dynamic Library
Is a collection of compiled code that is **linked to your program at runtime (or load time)** instead of being built directly into the executable.

### Key idea
Instead of copying all the code into your program (like with static libraries), a dynamic library is **loaded when the program runs**, saving memory and making updates easier.

### How it works
1. You write functions in a separate file (library).
2. Compile them into a dynamic library.
3. Your main program **references** that library.
4. At runtime, the OS loads the library into memory.

### Simple example
#### Library code (mathlib.c)
```c
int add(int a, int b) {
    return a + b;
}
```

<img width="1448" height="52" alt="image" src="https://github.com/user-attachments/assets/20e893aa-368e-405e-81b0-6536780add43" />

<img width="1765" height="32" alt="image" src="https://github.com/user-attachments/assets/b13c5881-8e95-4cca-8e9a-5617ed28a0ef" />
