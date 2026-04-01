## Challenge String Functions
You need to write a C program that takes two command-line arguments: a single character and a filename. 
The program must read the file line by line using fgets() or getline() and print only those lines that contain the specified character, using puts() for output. 
You can assume no line exceeds 256 characters, and lines are terminated by newline characters. If a line contains the character anywhere within it, the entire line should be printed.

### Example
#### File contents (test.out):
```text
hello, how are you
jason
nope
ok
what
lest
Aaaaa
```

#### Invocation:
```text
./a.out j test.out
```

#### Expected Output:
```text
jason
```

## Solution
```c
#include <stdio.h>
#include <string.h>

int main(int argc, char *argv[])
{
    char line[256]; // 256 characters as specified
    char targetChar;
    FILE *file;

    // Check command-line arguments
    if (argc != 3)
    {
        printf("Usage: %s <character> <filename>\n", argv[0]);
        return 1;
    }

    // Get the target character (first argument is a string, take first char)
    targetChar = argv[1][0];

    // Open the file
    file = fopen(argv[2], "r");

    if (file == NULL)
    {
        printf("Error: Could not open file '%s'\n", argv[2]);
        return 1;
    }

    // Read each line and check if it contains the target character
    while (fgets(line, sizeof(line), file) != NULL )
    {
        // Check if the line contains the target character
        if (strchr(line, targetChar) != NULL)
        {
            fputs(line, stdout); // Print the line
        }
    }

    fclose(file);

    return 0;
}
```

#### Note: How to pass command - Line Arguments in Code::Blocks
##### One argument
```text
Project → Set program's arguments...
┌─────────────────────────────────┐
│ Program arguments:              │
│ [ example.txt    ]              │
│                                 │
│      [ OK ]   [ Cancel ]        │
└─────────────────────────────────┘
```

##### Multiple arguments
if you need multiple arguments separate them with spaces 

```text
Project → Set program's arguments...
┌─────────────────────────────────┐
│ Program arguments:              │
│ [ j example.txt    ]            │
│                                 │
│      [ OK ]   [ Cancel ]        │
└─────────────────────────────────┘
```

