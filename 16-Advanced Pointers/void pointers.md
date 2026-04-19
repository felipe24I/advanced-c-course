## Void pointers (void*)
A void pointer is a generic pointer that can point to any data type, but cannot be dereferenced directly.

### Key Characteristics:
#### 1. Can Point to Any Type
```c
int x = 42;
float y = 3.14;
char c = 'A';

void *vp;

vp = &x;  // points to int
vp = &y;  // now points to float
vp = &c;  // now points to char
```

#### 2. Cannot Dereference Directly
```c
int x = 42;
void *vp = &x;

printf("%d", *vp);  // ERROR! Cannot dereference void*
printf("%d", *(int*)vp);  // OK - cast first
```

#### 3. Must Cast Before Use
```c
int x = 42;
void *vp = &x;

int *ip = (int*)vp;        // Cast to int pointer
printf("%d", *ip);         // 42

// Or in one line:
printf("%d", *(int*)vp);   // 42
```

### Common Use Cases:
#### 1. Generic Functions (like qsort)
```c
#include <stdio.h>

// Generic swap function - works with ANY type
void swap(void *a, void *b, size_t size) {
    char temp[size];
    memcpy(temp, a, size);
    memcpy(a, b, size);
    memcpy(b, temp, size);
}

int main() {
    int x = 10, y = 20;
    swap(&x, &y, sizeof(int));
    printf("x=%d, y=%d\n", x, y);  // x=20, y=10
    
    double p = 1.1, q = 2.2;
    swap(&p, &q, sizeof(double));
    printf("p=%.1f, q=%.1f\n", p, q);  // p=2.2, q=1.1
}
```

#### 2. Function Parameters (callback functions)
```c
void printValue(void *data, int type) {
    if (type == 0) {
        printf("Int: %d\n", *(int*)data);
    } else if (type == 1) {
        printf("Float: %.2f\n", *(float*)data);
    }
}
```

#### 3. Dynamic Memory Allocation
```c
void *ptr = malloc(100);  // Returns void*
int *int_ptr = (int*)ptr;  // Cast to specific type
```

#### Another example
```c
#include <stdio.h>

int main() {
    int aiData[3] = {10, 200, 300};
    
    void *pvData = &aiData[1];
    
    // Cast to char* for arithmetic (char is 1 byte)
    char *pcData = (char*)pvData;
    pcData += sizeof(int);
    pvData = pcData;
    
    printf("%d", *(int*)pvData);  // 300
    
    return 0;
}
```

#### Note:
A void pointer can be de-refenced when it is cast to concrete data type

#### Note:
What will be the output of the below program?

```c
    #include <stdio.h>
    
    int main()
    {
        int *p = NULL;
        void *vp = NULL;
        if (vp == p);
            printf("equal\n”);
        return 0;
    }
```

R// equal

#### Note: 
local variables are stored in an area called **stack**
