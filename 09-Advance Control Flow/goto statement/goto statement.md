## goto statement
The **goto** statement is a control flow construct that unconditionally transfers execution to another point in the program identified by a label

```c
// C/C++ syntax
goto label_name;
// ... code ...
label_name:
    // code to execute after goto
```

### How It Works
```c
#include <stdio.h>

int main() {
    int num = 0;
    
    printf("Start\n");
    
    if (num == 0) {
        goto error_handler;
    }
    
    printf("This won't execute if num is 0\n");
    
error_handler:
    printf("Error occurred!\n");
    
    return 0;
}
```

### Example 1:  Error Handling
```c
FILE *file = fopen("data.txt", "r");
if (!file) goto error;

char *buffer = malloc(1024);
if (!buffer) goto cleanup_file;

// Process data...

cleanup_buffer:
    free(buffer);
cleanup_file:
    fclose(file);
    return;

error:
    printf("Error occurred\n");
    return;
```

### Example 2: Breaking Out of Nested Loops
```c
for (int i = 0; i < 10; i++) {
    for (int j = 0; j < 10; j++) {
        if (matrix[i][j] == target) {
            found = 1;
            goto found_target;  // Exit both loops
        }
    }
}
found_target:
    printf("Found at [%d][%d]\n", i, j);
```

