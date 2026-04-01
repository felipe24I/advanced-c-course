## Challenge 1
You need to write a C program that counts the number of words and characters from either a file or standard input, depending on command-line arguments. 
If one argument is given, it is treated as a filename to read from; if no arguments are given, the program reads from standard input. 
A word is defined as any contiguous sequence of non-whitespace characters, while a character is defined as any printable character or newline (spaces and tabs are not counted). 
The program must output the results in the exact format: "The number of words in the file are: X" and "The number of characters in the file are: Y"

### Example Input (from file)
#### File test.txt:
```text
hello, how are you
jason
nope
ok
what
lest
aaaaa
```

#### Invocation:
```text
./a.out test.txt
```

#### Expected Output:
```text
The number of words in the file are: 10
The number of characters in the file are: 39
```
### Example Input (from standard input)
#### Invocation:
```text
./a.out
what
nope
ok
```

#### User Input:
```text
what
nope
ok
```
#### Expected Output:
```text
The number of words in the file are: 3
The number of characters in the file are: 10
```

## Solution
```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>

int main()
{
    FILE *file = NULL;
    char filename[100];
    char ch;
    int countC = 0;
    int countW = 0;
    int inWord = 0;
    char choice;

    printf("Read from (f)ile or (k)eyboard?: ");
    scanf(" %c", &choice); // The space tells scanf() to skip any whitespace characters (including newlines) before reading the actual character.
    getchar(); // Consume the leftover newline

    if (choice == 'f' || choice == 'F')
    {
        printf("Enter filename: ");
        fgets(filename, sizeof(filename), stdin);

        // Remove newline from filename
        int len = strlen(filename);
        if (filename[len-1] == '\n')
            filename[len-1] = '\0';

        file = fopen(filename, "r");

        if (file == NULL)
        {
            printf("Error: Could not open file.\n");
            return 1;
        }

    }
    else
    {
        file = stdin;
        printf("Enter text (Ctrl+Z then Enter to end):\n");
    }

    while ((ch = getc(file)) != EOF)
    {
        // Only count non-space, non-newline, non-tab characters
        if (ch != ' ' && ch != '\n' && ch != '\t')
        {
            countC++;
        }

        // Check if character is a word separator (space, newline, tab)
        if (ch == ' ' || ch == '\n' || ch == '\t')
        {
            inWord = 0; // not inside a word anymore
        }
        else
        {
            if (inWord == 0) //we just entered a new word
            {
                inWord = 1;
                countW++; // count this new word
            }
        }
    }

    if (choice == 'f' || choice == 'F')
    {
        fclose(file);
    }

    printf("\n--------------------------------\n");
    printf("Number of words: %d\n", countW);
    printf("Number of caracters: %d\n", countC);
    printf("--------------------------------\n");

    return 0;
}
```

**Note:** This solution wasn't implemented with command line arguments
