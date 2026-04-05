## Preprocessor operators
### Backslash (\)
allows for the continuation of a macro to the next line when the macro is too long for a single line

```c
#define min(x,y) \
((x)<(y) ? (x) : (y))
```

### defined()
is used in constant expressions to determine if an identifier is defined using #define

```c
#if defined (WINDOWS) || defined (WINDOWSINT)
# define BOOT_DRIVE "C:/"
#else
# define BOOT_DRIVE "D:/"
#endif

#include <stdio.h>

int main(void) {
    printf("Here is the boot drive path: %s\n", BOOT_DRIVE);
    return 0;
}
```

The above will display as output "C:/" if either WINDOWS or WINDOWSNT is defined, output will be "D:/" otherwise

## The # Operator
Converts a macro parameter into a string literal by adding double quotes.

```c
#define str(x) #x

int main()
{
  printf(str(testing)); // "testing"
  return 0; 
}
```

```c
#include <stdio.h>

#define HELLO(x) printf("Hello, " #x "\n");

int main(void) {
    HELLO(John);   // Prints: Hello, John
    HELLO(Alice);  // Prints: Hello, Alice
    HELLO(Bob);    // Prints: Hello, Bob
    
    return 0;
}
```

**Note:** When HELLO(John) appears in a program, It expands to:

```c
printf("Hello, " "John" "\n");
```

Because strings separated by whitespace are concatenated during preprocessing, the preceding statement is equivalent to:

```c
printf("Hello, John\n");
```

The # operator must be used in a macro with arguments because the operand of # refers to an argument of the macro.



## The ## Operator**
is used in macro definitions to join two tokens together

```c
#define TOKENCONCAT(x,y) x##y // TOKENCONCAT(O, K) is replaced by OK in the program
```

**Macro Definition**

```c
#define concatenate(x, y) x ## y
```

**How It Works:** When you declare:

```c
int xy = 10;
```

And then call:

```c
printf("%d", concatenate(x, y));
```
The compiler expands this to:

```c
printf("%d", xy);
```

Which will display 10 to standard output.

**Complete Example Program**

```c
#include <stdio.h>

#define concatenate(x, y) x ## y

int main(void) {
    int xy = 10;                    // Regular variable declaration
    int ab = 25;                    // Another example
    
    // concatenate(x, y) becomes xy
    printf("%d\n", concatenate(x, y));  // Prints: 10
    
    // concatenate(a, b) becomes ab  
    printf("%d\n", concatenate(a, b));  // Prints: 25
    
    return 0;
}
```

**key point:** The ## operator (token-pasting operator) concatenates two tokens into a single token. In this case, x and y are pasted together to form xy, which is then recognized as the variable name.

### Example 1: Token Pasting (##) with Prefix
```c
#define make_function(name) int my_ ## name (int foo) {}
make_function(bar)
```

Expands to:

```c
int my_bar(int foo) {}
```

This defines a function called my_bar()
  
### Example 2: Stringizing (#) with puts()
```c
#define eat(what) puts("I'm eating " #what " today.")
eat(fruit)
```

The macro-processor turns this into:

```c
puts("I'm eating " "fruit" " today.")
```

Which the C parser interprets as a single string constant (The C parser here refers to the compiler's parser, not the preprocessor):

```c
puts("I'm eating fruit today.")
```

### Complete Runnable Example
```c
#include <stdio.h>

// Creates a function with "my_" prefix
#define make_function(name) int my_ ## name (int foo) {}

// Creates a string output about eating
#define eat(what) puts("I'm eating " #what " today.")

// Define functions using the macro
make_function(bar)      // Creates: int my_bar(int foo) {}
make_function(apple)    // Creates: int my_apple(int foo) {}
make_function(pizza)    // Creates: int my_pizza(int foo) {}

int main(void) {
    // Test the eat() macro
    eat(fruit);      // Output: I'm eating fruit today.
    eat(vegetables); // Output: I'm eating vegetables today.
    eat(meat);       // Output: I'm eating meat today.
    
    // The functions were defined but not used in this example
    // They could be called like: my_bar(5);
    
    return 0;
}
```
