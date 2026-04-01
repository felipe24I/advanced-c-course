## Challenge 2
You need to write a C program that reads a file, converts all uppercase letters to lowercase and all lowercase letters to uppercase, while leaving all other characters unchanged. 
The program should accept the filename either as a command-line argument or by prompting the user. 
Using fgetc and fputc, you must read from the original file, write the converted characters to a temporary file, and then rename the temporary file back to the original filename to overwrite it. 
Functions like isupper, tolower, and toupper should be used to detect and perform the letter case conversions.

### Example
#### Original file (test.txt) contents:
```text
hello, how are you
jason
nope
ok
what
lest
aaaaa
```

#### After running the program:
```text
HELLO, HOW ARE YOU
JASON
NOPE
OK
WHAT
LEST
AAAAA
```

## Solution
```c
#include <stdio.h>
#include <string.h>
#include <ctype.h>

int main()
{
    FILE *file = NULL;
    FILE *newFile = NULL;
    char filename[100];
    char ch;

    printf("Enter the name of the file: ");
    fgets(filename, sizeof(filename), stdin);

    // Remove newline from filename
    int len = strlen(filename);
    if (filename[len-1] == '\n');
        filename[len-1] = '\0';

    // Open original file for reading
    file = fopen(filename, "r");

    if (file == NULL)
    {
        printf("\nError, could not open file '%s'\n", filename);
        return 1;
    }

    // Create temporary file for writing
    newFile = fopen("temp.txt", "w");

    if (newFile == NULL)
    {
        printf("\nError: Could not create temporary file\n");
        fclose(file);
        return 1;
    }

    // Read each character, convert case, and write to temp file
    while ((ch = fgetc(file)) != EOF)
    {
        if (isupper(ch))
        {
           fputc(tolower(ch), newFile); // Uppercase → Lowercase
        }
        else if (islower(ch))
        {
            fputc(toupper(ch), newFile); // Lowercase → Uppercase
        }
        else
        {
            fputc(ch, newFile); // Keep other characters unchanged
        }
    }


    // Close both files
    fclose(file);
    fclose(newFile);

    // Remove original file
    if (remove(filename) != 0)
    {
        printf("Error: Could not remove original file\n");
        return 1;
    }

    // Rename temporary file to original filename
    if (rename("temp.txt", filename) != 0)
    {
        printf("Error: Could not rename temporary file\n");
        return 1;
    }

    printf("\nSuccess! Case conversion completed.\n");
    printf("File '%s' has been updated.\n", filename);

    return 0;
}
```
