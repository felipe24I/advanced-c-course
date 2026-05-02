## Date and Time in C → <time.h>
 ```c
#include <time.h>
```

### Key concept (VERY IMPORTANT)
C represents time as:
- Number of seconds since Jan 1, 1970 (Unix Epoch)

### 1. time()
```c
time_t time(time_t *t);
```

Gets current time (in seconds since 1970)

#### Example
```c
#include <stdio.h>
#include <time.h>

int main() {
    time_t now = time(NULL);
    printf("%ld\n", now);
    return 0;
}
```

#### Output
```text
1700000000   (example)
```

### 2. ctime()
```c
char *ctime(const time_t *t);
```

Converts time to **readable string**

#### Example
```c
time_t now = time(NULL);
printf("%s", ctime(&now));
```

#### Output
```text
Mon Apr 29 21:30:00 2026
```

### 3. localtime()
```c
struct tm *localtime(const time_t *t);
```

Converts time → structured format

#### struct tm fields
```c
struct tm {
    int tm_sec;   // seconds (0–59)
    int tm_min;   // minutes (0–59)
    int tm_hour;  // hours (0–23)
    int tm_mday;  // day (1–31)
    int tm_mon;   // month (0–11)
    int tm_year;  // years since 1900
};
```

#### Example
```c
time_t now = time(NULL);
struct tm *t = localtime(&now);

printf("%d/%d/%d\n", t->tm_mday, t->tm_mon+1, t->tm_year+1900);
```

### 4. strftime() (VERY IMPORTANT)
```c
size_t strftime(char *str, size_t max, const char *format, const struct tm *timeptr);
```

Formats date/time nicely

#### Example
```c
char buffer[100];
strftime(buffer, sizeof(buffer), "%Y-%m-%d %H:%M:%S", t);
printf("%s\n", buffer);
```

#### Output
```text
2026-04-29 21:30:00
```

#### Common format codes
- **%Y:** Year
- **%m:** Month
- **%d:** Day
- **%H:** Hour
- **%M:** Minute
- **%S:** Second

### 5. difftime()
```c
double difftime(time_t t1, time_t t2);
```

Returns difference in seconds

#### Example
```c
time_t t1 = time(NULL);
// wait...
time_t t2 = time(NULL);

printf("%f seconds\n", difftime(t2, t1));
```

### 6. clock()
```c
clock_t clock(void);
```

Measures **CPU time used by program**

#### Example
```c
clock_t start = clock();

// code here

clock_t end = clock();

double time_spent = (double)(end - start) / CLOCKS_PER_SEC;
```

#### Easy flow (VERY IMPORTANT)
```text
time() → localtime() → strftime()
```

This is the typical pipeline

#### Full example
```c
#include <stdio.h>
#include <time.h>

int main() {
    time_t now = time(NULL);
    struct tm *t = localtime(&now);

    char buffer[100];
    strftime(buffer, sizeof(buffer), "%Y-%m-%d %H:%M:%S", t);

    printf("Current time: %s\n", buffer);

    return 0;
}
```

#### Common mistakes
- Forgetting tm_year + 1900
- Forgetting tm_mon + 1
- Using ctime() when formatting is needed

#### One sentence summary
C handles time as seconds since 1970, and you convert it using localtime() and strftime() to make it readable.
