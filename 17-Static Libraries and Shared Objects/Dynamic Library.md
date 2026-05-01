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
#### 1. Create the dynamic library project
In Code::Blocks, you created a console project and wrote:

```c
#include <stdio.h>
#include "myLib.h"

void fun(void)
{
    printf("fun() called from a dynamic library\n");
}
```

This file actually acts as your library source, even though it's named main.c (not ideal naming, but it works).

#### 2. Create the header file
You created myLib.h:

```c
#ifndef MYLIB_H_INCLUDED
#define MYLIB_H_INCLUDED

void fun(void);

#endif // MYLIB_H_INCLUDED
```

This allows other programs to know the function prototype

#### 3. Compile the dynamic library
In CMD, you ran:

```bash
gcc -g -fPIC main.c -shared -o lib_mylib.dll
```

<img width="1448" height="52" alt="image" src="https://github.com/user-attachments/assets/20e893aa-368e-405e-81b0-6536780add43" />


#### Correction:
- fPIC is not needed on Windows
- But the command still works

This step creates:

```text
lib_mylib.dll
```

#### 4. Make the DLL available
You mentioned using:

```bash
export PATH=/cygdrive/c/users/jfedin/Downloads/myDynamicLibrary:$PATH
```

<img width="855" height="58" alt="image" src="https://github.com/user-attachments/assets/379f6df4-3717-46ec-a95e-46d747b0e6e2" />


That works (in Git Bash)

Alternative (simpler):

Copy lib_mylib.dll into the same folder as main.exe

Windows CMD equivalent would be:

```bash
set PATH=C:\Users\jfedin\Downloads\myDynamicLibrary;%PATH%
```

#### 5. Create a second project (client program)
You created another console app:

```c
#include <stdio.h>
#include <stdlib.h>
#include "myLib.h"

int main()
{
    fun();
    return 0;
}
```

This program uses the function from the DLL

#### 6. Compile the client program (object file)

```bash
gcc -I ../myDynamicLibrary -c main.c -o main.o
```

-I tells GCC where to find myLib.h

<img width="1765" height="32" alt="image" src="https://github.com/user-attachments/assets/b13c5881-8e95-4cca-8e9a-5617ed28a0ef" />


### 7. Link with the dynamic library
You wrote:

```bash
gcc -o main.exe main.o -L..]/myDynamicLibrary -llib_mylib
```

This links your program with the library

<img width="1882" height="45" alt="image" src="https://github.com/user-attachments/assets/38c96f34-9197-4f52-9226-52b89248fbcc" />


#### 8. Run the program
In CMD:

```bash
main.exe
```

Output:

```text
fun() called from a dynamic library
```

<img width="1311" height="52" alt="image" src="https://github.com/user-attachments/assets/a2d26797-5757-4abd-a388-8f2978e76d4d" />
