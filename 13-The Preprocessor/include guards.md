## Include guards
Include guards prevent a header file from being included multiple times in the same translation unit, which would cause compilation errors due to multiple definitions.

* Prevents multiple definitions of the same variable / function / macro
* The standard C header files uses the **#ifndef** technique to avoid multiple inclusions

## Purpose
In C, each .c file is compiled **separately** into an "object file" (.o or .obj).
* When person.c compiles, it reads person.h once.
* When main.c compiles, it reads person.h once.

Because they are different files, there is no conflict. The compiler doesn't "remember" what happened in person.c while it is working on main.c.

### When does "Redefinition" actually happen?
Redefinition happens when **one single .c file** ends up seeing the same struct or function twice. This usually happens with **nested includes:**

Imagine this scenario **WITHOUT** #ifndef:
1. person.h defines struct Person
2. utilities.h has #include "person.h"
3. main.c has:

```c
#include "person.h"    // First time: struct Person is defined.
#include "utilities.h" // Second time: it pulls person.h AGAIN.
```

**In this case, main.c sees** struct Person **twice**.
The compiler screams "Redefinition error!" because it thinks you are trying to create two different things with the same name in the same file.

### Include Guards
With your #ifndef PERSON_H guards:
* In **main.c**, the first #include "person.h" defines the macro.
* If you added another #include that points back to person.h, the #ifndef would see the macro exists and **skip** the code.

**Final note:** include guards ensures that the contents of a header file cannot be included more than once into a source file 

```c
// Without include guards - BAD!
// myheader.h
typedef struct {
    int x;
    int y;
} Point;

// main.c
#include "myheader.h"
#include "myheader.h"  // ERROR: Point redefined!
```

### The Basic Pattern
```c
#ifndef HEADER_NAME_H
#define HEADER_NAME_H

// Header content goes here

#endif /* HEADER_NAME_H */
```

### Simple Example
```c
// ========== person.h ==========
#ifndef PERSON_H
#define PERSON_H

typedef struct {
    char name[50];
    int age;
} Person;

void person_init(Person *p, const char *name, int age);
void person_print(const Person *p);

#endif /* PERSON_H */

// ========== person.c ==========
#include "person.h"
#include <stdio.h>
#include <string.h>

void person_init(Person *p, const char *name, int age) {
    strcpy(p->name, name);
    p->age = age;
}

void person_print(const Person *p) {
    printf("%s is %d years old\n", p->name, p->age);
}

// ========== main.c ==========
#include "person.h"
#include "person.h"  // Second inclusion is safe!
#include <stdio.h>

int main() {
    Person p;
    person_init(&p, "Alice", 30);
    person_print(&p);
    return 0;
}
```

## How Include Guards Work
```c
// First inclusion:
#ifndef PERSON_H      // Is PERSON_H defined? NO
#define PERSON_H      // Define it NOW
// ... header content ...  // This gets processed
#endif

// Second inclusion:
#ifndef PERSON_H      // Is PERSON_H defined? YES (from first inclusion)
// ... header content ...  // This is SKIPPED!
#endif
```

### Complete Example with Multiple Headers
```c
// ========== config.h ==========
#ifndef CONFIG_H
#define CONFIG_H

#define MAX_NAME 100
#define MAX_BUFFER 1024
#define VERSION "1.0"

#endif /* CONFIG_H */

// ========== utils.h ==========
#ifndef UTILS_H
#define UTILS_H

#include "config.h"  // Safe even if included multiple times

typedef struct {
    char name[MAX_NAME];
    int value;
} Data;

void process(Data *d);

#endif /* UTILS_H */

// ========== database.h ==========
#ifndef DATABASE_H
#define DATABASE_H

#include "config.h"  // Also includes config.h
#include "utils.h"   // Also includes utils.h

void db_init(void);
void db_save(Data *d);

#endif /* DATABASE_H */

// ========== main.c ==========
#include "database.h"  // Includes config.h and utils.h
#include "utils.h"     // Safe - utils.h guard prevents reprocessing
#include "config.h"    // Safe - config.h guard prevents reprocessing

int main() {
    Data d = {"Test", 42};
    process(&d);
    db_init();
    db_save(&d);
    return 0;
}
```

### Naming Conventions
```c
// Common naming patterns:

// 1. Simple: FILENAME_H
#ifndef PERSON_H
#define PERSON_H
#endif

// 2. With prefix: PROJECT_FILENAME_H
#ifndef MYAPP_PERSON_H
#define MYAPP_PERSON_H
#endif

// 3. Full path: PROJECT_PATH_FILENAME_H
#ifndef MYAPP_UTILS_STRING_H
#define MYAPP_UTILS_STRING_H
#endif

// 4. Based on module: MODULE_FILENAME_H
#ifndef DATABASE_CONNECTION_H
#define DATABASE_CONNECTION_H
#endif

// 5. Random suffix (less common): FILENAME_H_1234
#ifndef PERSON_H_9F3E4A
#define PERSON_H_9F3E4A
#endif
```
