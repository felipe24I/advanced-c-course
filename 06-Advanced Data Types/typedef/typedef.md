## typedef
typedef is a keyword that creates an alias or alternative name for existing data types. 
It doesn't create new types; it simply provides a new name for an existing type, improving code readability and maintainability.

### Basic Syntax
```c
typedef existing_type new_type_name;

// Examples
typedef int Integer;
typedef float RealNumber;
typedef char Character;
```

### Simple Type Aliases
```c
#include <stdio.h>

// Basic type aliases
typedef int Integer;
typedef unsigned int UInt;
typedef long long BigInt;
typedef float Float32;
typedef double Float64;

int main() {
    Integer count = 42;           // Actually int
    UInt positive = 100;          // Actually unsigned int
    BigInt large = 9999999999LL;  // Actually long long
    Float32 pi = 3.14159f;        // Actually float
    
    printf("count: %d\n", count);
    printf("positive: %u\n", positive);
    printf("large: %lld\n", large);
    printf("pi: %f\n", pi);
    
    return 0;
}
```

## Typedef with Structures
### 1. Simple Structures
```c
#include <stdio.h>
#include <string.h>

// Without typedef (need 'struct' keyword everywhere)
struct Point {
    int x;
    int y;
};

// With typedef (cleaner syntax)
typedef struct {
    int x;
    int y;
} Point2D;

// Named structure with typedef
typedef struct Student {
    int id;
    char name[50];
    float grade;
} Student;

int main() {
    // Without typedef
    struct Point p1 = {10, 20};
    
    // With typedef
    Point2D p2 = {30, 40};
    Student s1 = {123, "Alice", 95.5};
    
    printf("Point: (%d, %d)\n", p1.x, p1.y);
    printf("Point2D: (%d, %d)\n", p2.x, p2.y);
    printf("Student: %d, %s, %.2f\n", s1.id, s1.name, s1.grade);
    
    return 0;
}
```

### 2. Self-Referential Structures (Linked Lists)
```c
#include <stdio.h>
#include <stdlib.h>

// Self-referential structure with typedef
typedef struct Node {
    int data;
    struct Node *next;  // Need 'struct Node' here (typedef not yet complete)
} Node;

// Or separate declaration
typedef struct LinkedListNode LinkedListNode;
struct LinkedListNode {
    int data;
    LinkedListNode *next;  // Can use typedef name now
};

// Example: Linked list with typedef
typedef struct ListNode {
    int value;
    struct ListNode *next;
} ListNode;

void print_list(ListNode *head) {
    while (head) {
        printf("%d -> ", head->value);
        head = head->next;
    }
    printf("NULL\n");
}

int main() {
    // Create a simple linked list
    ListNode *head = malloc(sizeof(ListNode));
    head->value = 10;
    head->next = malloc(sizeof(ListNode));
    head->next->value = 20;
    head->next->next = malloc(sizeof(ListNode));
    head->next->next->value = 30;
    head->next->next->next = NULL;
    
    print_list(head);
    
    // Clean up
    while (head) {
        ListNode *temp = head;
        head = head->next;
        free(temp);
    }
    
    return 0;
}
```

### Typedef with Pointers
```c
#include <stdio.h>
#include <stdlib.h>

// Pointer type aliases
typedef int* IntPtr;
typedef char* String;
typedef void* Pointer;

// Function pointer typedefs
typedef int (*CompareFunc)(int, int);
typedef void (*CallbackFunc)(void*);

// Array pointer typedef
typedef int (*ArrayPtr)[10];  // Pointer to array of 10 ints

int compare_int(int a, int b) {
    return a - b;
}

void process_data(void *data) {
    int *value = (int*)data;
    printf("Processing: %d\n", *value);
}

int main() {
    // Using pointer typedefs
    IntPtr p = malloc(sizeof(int));
    *p = 42;
    
    String name = "Hello, World!";  // char*
    
    printf("Value: %d\n", *p);
    printf("String: %s\n", name);
    
    // Using function pointer typedefs
    CompareFunc cmp = compare_int;
    int result = cmp(10, 5);
    printf("Comparison: %d\n", result);
    
    CallbackFunc cb = process_data;
    int data = 100;
    cb(&data);
    
    free(p);
    
    return 0;
}
```

### Typedef with Arrays
```c
#include <stdio.h>

// Array type aliases
typedef int IntArray10[10];
typedef float Matrix3x3[3][3];
typedef char String100[100];

// More complex array typedefs
typedef int (*IntArrayPtr)[5];  // Pointer to array of 5 ints

int main() {
    // Using array typedefs
    IntArray10 numbers = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    Matrix3x3 identity = {
        {1, 0, 0},
        {0, 1, 0},
        {0, 0, 1}
    };
    String100 message = "Hello";
    
    // Print array
    printf("Numbers: ");
    for (int i = 0; i < 10; i++) {
        printf("%d ", numbers[i]);
    }
    printf("\n");
    
    // Print matrix
    printf("Identity matrix:\n");
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%d ", identity[i][j]);
        }
        printf("\n");
    }
    
    printf("Message: %s\n", message);
    
    return 0;
}
```

### Typedef with Enums
```c
#include <stdio.h>

// Enum with typedef
typedef enum {
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY,
    SUNDAY
} Weekday;

// Enum with explicit values
typedef enum {
    STATUS_OK = 0,
    STATUS_ERROR = 1,
    STATUS_PENDING = 2,
    STATUS_CANCELLED = 3
} StatusCode;

// Enum for flags
typedef enum {
    FLAG_READ = 0x01,
    FLAG_WRITE = 0x02,
    FLAG_EXEC = 0x04,
    FLAG_HIDDEN = 0x08
} FileFlags;

int main() {
    Weekday today = WEDNESDAY;
    StatusCode result = STATUS_OK;
    FileFlags flags = FLAG_READ | FLAG_WRITE;
    
    printf("Today is: %d\n", today);  // 2 (Wednesday)
    printf("Result: %d\n", result);   // 0 (OK)
    printf("Flags: 0x%02X\n", flags); // 0x03 (READ|WRITE)
    
    if (flags & FLAG_READ) {
        printf("Read permission granted\n");
    }
    
    return 0;
}
```

### Typedef with Function Pointers
```c
#include <stdio.h>
#include <stdlib.h>

// Function pointer typedefs
typedef int (*Operation)(int, int);
typedef void (*Printer)(const char*);
typedef int (*Comparator)(const void*, const void*);

// Operation functions
int add(int a, int b) { return a + b; }
int subtract(int a, int b) { return a - b; }
int multiply(int a, int b) { return a * b; }
int divide(int a, int b) { return b != 0 ? a / b : 0; }

// Calculator using function pointer
int calculate(Operation op, int a, int b) {
    return op(a, b);
}

// Function returning function pointer
Operation get_operation(char op) {
    switch (op) {
        case '+': return add;
        case '-': return subtract;
        case '*': return multiply;
        case '/': return divide;
        default: return NULL;
    }
}

// Array of function pointers
typedef struct {
    char symbol;
    Operation func;
} OperationMap;

OperationMap operations[] = {
    {'+', add},
    {'-', subtract},
    {'*', multiply},
    {'/', divide}
};

int main() {
    // Direct use
    printf("10 + 5 = %d\n", calculate(add, 10, 5));
    printf("10 * 5 = %d\n", calculate(multiply, 10, 5));
    
    // Dynamic operation
    Operation op = get_operation('+');
    printf("20 + 30 = %d\n", op(20, 30));
    
    // Using operation map
    for (int i = 0; i < 4; i++) {
        printf("%d %c %d = %d\n", 
               15, operations[i].symbol, 3,
               operations[i].func(15, 3));
    }
    
    return 0;
}
```
