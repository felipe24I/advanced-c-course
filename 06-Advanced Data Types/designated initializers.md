## Designated initializers
Designated initializers allow you to initialize specific elements of an array, structure, or union by name, rather than relying on position. 
This feature was introduced in C99 and makes initialization more readable, maintainable, and less error-prone.

## Structure Initialization
### 1. Basic Structure Designated Initializers
```c
#include <stdio.h>
#include <string.h>

typedef struct {
    int id;
    char name[50];
    float salary;
    char department[30];
} Employee;

int main() {
    // Traditional positional initialization (order matters)
    Employee emp1 = {101, "Alice", 75000.0, "Engineering"};
    
    // Designated initializer (order doesn't matter)
    Employee emp2 = {
        .name = "Bob",
        .id = 102,
        .department = "Sales",
        .salary = 65000.0
    };
    
    // Mixed: designated and positional (C99)
    Employee emp3 = {
        103,                    // .id = 103
        .name = "Charlie",
        .salary = 80000.0       // department uninitialized
    };
    
    printf("Employee 1: %d, %s, %.2f, %s\n", 
           emp1.id, emp1.name, emp1.salary, emp1.department);
    printf("Employee 2: %d, %s, %.2f, %s\n", 
           emp2.id, emp2.name, emp2.salary, emp2.department);
    printf("Employee 3: %d, %s, %.2f, %s\n", 
           emp3.id, emp3.name, emp3.salary, emp3.department);
    
    return 0;
}
```

### 2. Partial Initialization
```c
#include <stdio.h>

typedef struct {
    int x;
    int y;
    int z;
} Point3D;

typedef struct {
    char name[50];
    int age;
    float score;
    char grade;
} Student;

int main() {
    // Only initialize some fields (others get zero/default)
    Point3D p1 = {.x = 10, .z = 30};
    // p1.y is automatically 0
    
    Student s1 = {
        .name = "Alice",
        .score = 95.5
        // age = 0, grade = '\0'
    };
    
    printf("Point: x=%d, y=%d, z=%d\n", p1.x, p1.y, p1.z);
    printf("Student: %s, age=%d, score=%.1f, grade='%c'\n",
           s1.name, s1.age, s1.score, s1.grade);
    
    return 0;
}
```

### 3. Nested Structure Initialization
```c
#include <stdio.h>

typedef struct {
    int x;
    int y;
} Point;

typedef struct {
    Point top_left;
    Point bottom_right;
    char color[20];
    int border_width;
} Rectangle;

typedef struct {
    char name[50];
    Point position;
    Rectangle bounds;
} Widget;

int main() {
    // Nested designated initializers
    Rectangle rect1 = {
        .top_left = {.x = 0, .y = 0},
        .bottom_right = {.x = 100, .y = 100},
        .color = "blue",
        .border_width = 2
    };
    
    // More complex nesting
    Widget widget = {
        .name = "Button",
        .position = {.x = 10, .y = 20},
        .bounds = {
            .top_left = {.x = 0, .y = 0},
            .bottom_right = {.x = 200, .y = 50},
            .color = "gray",
            .border_width = 1
        }
    };
    
    printf("Rectangle: (%d,%d) to (%d,%d), %s, border=%d\n",
           rect1.top_left.x, rect1.top_left.y,
           rect1.bottom_right.x, rect1.bottom_right.y,
           rect1.color, rect1.border_width);
    
    printf("Widget: %s at (%d,%d)\n",
           widget.name, widget.position.x, widget.position.y);
    
    return 0;
}
```

## Array Initialization
### 1. Array Element Designators
```c
#include <stdio.h>

int main() {
    // Traditional initialization
    int arr1[5] = {10, 20, 30, 40, 50};
    
    // Designated initializers - specify indices
    int arr2[5] = {[0] = 10, [2] = 30, [4] = 50};
    // arr2[1] and arr2[3] are 0
    
    // Out of order indices
    int arr3[10] = {[5] = 100, [1] = 200, [8] = 300};
    
    // Mixing positional and designated
    int arr4[] = {10, [3] = 40, 50, [1] = 20};
    // Results in: [0]=10, [1]=20, [2]=0, [3]=40, [4]=50
    
    printf("arr2: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", arr2[i]);
    }
    printf("\n");
    
    printf("arr3: ");
    for (int i = 0; i < 10; i++) {
        printf("%d ", arr3[i]);
    }
    printf("\n");
    
    printf("arr4: ");
    for (int i = 0; i < 5; i++) {
        printf("%d ", arr4[i]);
    }
    printf("\n");
    
    return 0;
}
```

### 2. Array Range Initialization (GCC Extension)
```c
#include <stdio.h>

int main() {
    // GNU extension: initialize a range of indices
    int arr1[20] = {
        [0 ... 4] = 10,    // indices 0-4 set to 10
        [5 ... 9] = 20,    // indices 5-9 set to 20
        [10 ... 14] = 30,  // indices 10-14 set to 30
        [15 ... 19] = 40   // indices 15-19 set to 40
    };
    
    // Combine single and range
    int arr2[10] = {
        [0] = 100,
        [2 ... 5] = 200,
        [7 ... 9] = 300
    };
    
    printf("arr1: ");
    for (int i = 0; i < 20; i++) {
        printf("%d ", arr1[i]);
    }
    printf("\n");
    
    printf("arr2: ");
    for (int i = 0; i < 10; i++) {
        printf("%d ", arr2[i]);
    }
    printf("\n");
    
    return 0;
}
```

### 3. Multi-dimensional Array Designators
```c
#include <stdio.h>

int main() {
    // 2D array designated initialization
    int matrix[4][4] = {
        [0][0] = 1,
        [0][3] = 2,
        [3][0] = 3,
        [3][3] = 4,
        [1][1] = 5,
        [2][2] = 6
    };
    
    // Row-by-row initialization
    int matrix2[3][3] = {
        [0] = {[0] = 1, [2] = 2},
        [1] = {[1] = 3},
        [2] = {[0] = 4, [1] = 5, [2] = 6}
    };
    
    printf("Matrix:\n");
    for (int i = 0; i < 4; i++) {
        for (int j = 0; j < 4; j++) {
            printf("%2d ", matrix[i][j]);
        }
        printf("\n");
    }
    
    printf("\nMatrix2:\n");
    for (int i = 0; i < 3; i++) {
        for (int j = 0; j < 3; j++) {
            printf("%2d ", matrix2[i][j]);
        }
        printf("\n");
    }
    
    return 0;
}
```

## Union Initialization
```
#include <stdio.h>
#include <string.h>

typedef union {
    int int_value;
    float float_value;
    char string_value[20];
} Data;

int main() {
    // Designated initializer for union (C99)
    Data d1 = {.int_value = 42};
    Data d2 = {.float_value = 3.14159f};
    Data d3 = {.string_value = "Hello"};
    
    printf("d1: int = %d\n", d1.int_value);
    printf("d2: float = %.5f\n", d2.float_value);
    printf("d3: string = %s\n", d3.string_value);
    
    // Without designated initializer, only first member is initialized
    Data d4 = {100};  // Initializes int_value (first member)
    printf("d4: int = %d\n", d4.int_value);
    
    return 0;
}
```

## Practical Example
### 1. Configuration Structure
```c
#include <stdio.h>
#include <stdbool.h>

typedef struct {
    char hostname[100];
    int port;
    bool ssl_enabled;
    int timeout_seconds;
    int retry_count;
    char username[50];
    char password[50];
    bool debug_mode;
} ConnectionConfig;

// Default configuration
ConnectionConfig default_config = {
    .hostname = "localhost",
    .port = 8080,
    .ssl_enabled = false,
    .timeout_seconds = 30,
    .retry_count = 3,
    .debug_mode = false
};

// Override specific settings
ConnectionConfig production_config = {
    .hostname = "api.example.com",
    .port = 443,
    .ssl_enabled = true,
    .timeout_seconds = 60,
    .retry_count = 5
    // username and password remain empty/zero
};

int main() {
    // Create custom config with designated initializer
    ConnectionConfig custom = {
        .hostname = "test.server.com",
        .port = 9000,
        .debug_mode = true
        // Other fields get default values (0, false, empty)
    };
    
    printf("Default config:\n");
    printf("  Host: %s:%d, SSL=%d, timeout=%d, retries=%d\n",
           default_config.hostname, default_config.port,
           default_config.ssl_enabled, default_config.timeout_seconds,
           default_config.retry_count);
    
    printf("\nProduction config:\n");
    printf("  Host: %s:%d, SSL=%d, timeout=%d, retries=%d\n",
           production_config.hostname, production_config.port,
           production_config.ssl_enabled, production_config.timeout_seconds,
           production_config.retry_count);
    
    printf("\nCustom config:\n");
    printf("  Host: %s:%d, debug=%d\n",
           custom.hostname, custom.port, custom.debug_mode);
    
    return 0;
}
```
