## Static library
.lib on Windows) is a collection of object files that are linked directly into an executable at compile time. Once linked, the library code becomes part of the final executable.

### Key Characteristics
* Linking time: During compilation (not at runtime)
* File size: Increases executable size
* Distribution: Library code is copied into the executable
* Updates: Requires recompiling the entire program
* No dependencies: Executable is standalone

### How to Create and Use Static Libraries in CodeBlocks
#### Part 1: Creating the Static Library Project
##### Step 1: Create a New Static Library Project
1. Open CodeBlocks
2. Go to File → New → Project
3. Select Static Library from the project types
4. Click Go
5. Name your project (e.g., myLib)
6. Choose a location to save it
7. Click Finish

##### Step 2: Write Your Library Code
write your code in the main.c source that was cretaed automatically

```c
#include <stdio.h>

void fun(void)
{
    printf("fun() called from a static library");
}
```

Create a header file (e.g., myLib.h):

```c
#ifndef MYLIB_H_INCLUDED
#define MYLIB_H_INCLUDED

void fun(void);

#endif // MYLIB_H_INCLUDED
```

##### Step 3: Build the Static Library
1. Click Build → Build (or press Ctrl+F9)
2. CodeBlocks will create a file named libMyMathLibrary.a (on Windows/Linux) or MyMathLibrary.lib (on Windows)
3. Note where this file is saved (usually in the bin/Debug or bin/Release folder of your library project)

#### Part 2: Using the Static Library in Another Project
##### Step 1: Create a New Console Application Project
1. File → New → Project
2. Select Console Application
3. Click Go
4. Choose C as the language
5. Name your project (e.g., TestingLibrary)
6. Choose a location
7. Click Finish

##### Step 2: Copy the Library Files
Copy these files from your library project to your test project folder:
* myLib.h (the header file)
* libMylib.a (the static library file)

##### Step 3: Configure Compiler Settings (IMPORTANT!)
This is the critical part you asked about:

A. Add the Library Search Directory
1. Go to Settings → Compiler
2. Click on Global compiler settings tab (or project-specific)
3. Select Search directories tab
4. Click Add
5. Browse to the folder containing your library file (.a or .lib)
6. Click OK

<img width="926" height="346" alt="image" src="https://github.com/user-attachments/assets/b90b4925-da3f-41a2-a57d-0c5fb64a5a59" />

![Search Directory: Points to where libMyLib.a is located]

B. Add the Linker Library
1. Stay in Settings → Compiler
2. Click on Linker settings tab
3. In Link libraries section, click Add
4. Browse to and select your library file (libmyLib.a)
5. Click OK

<img width="1072" height="356" alt="image" src="https://github.com/user-attachments/assets/9f6e64c1-4bf5-4eb5-9842-d749f05e126f" />

##### Step 4: Write Your Test Program
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

##### Step 5: Build and Run
1. Click Build → Build and Run (or press F9)
2. You should see the output showing all operations work correctly
