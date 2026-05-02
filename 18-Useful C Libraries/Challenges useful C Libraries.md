## Challenge #1 — 50 random numbers (-0.5 to 0.5)
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    srand(time(NULL));

    int n = 50;
    printf("%d\n", n);

    for (int i = 0; i < n; i++) {
        double r = ((double)rand() / RAND_MAX) - 0.5;
        printf("%f\n", r);
    }

    return 0;
}
```

### Key idea
```c
((double)rand() / RAND_MAX) → [0,1]
- 0.5 → [-0.5, 0.5]
```

## Challenge #2 — qsort with functions
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void fillarray(double arr[], int n) {
    for (int i = 0; i < n; i++) {
        arr[i] = ((double)rand() / RAND_MAX) * 10.0;
    }
}

void showarray(const double arr[], int n) {
    for (int i = 0; i < n; i++) {
        printf("%.4f ", arr[i]);
        if ((i + 1) % 6 == 0) printf("\n");
    }
    printf("\n");
}

int compare(const void *a, const void *b) {
    double x = *(double*)a;
    double y = *(double*)b;

    if (x < y) return -1;
    if (x > y) return 1;
    return 0;
}

int main() {
    srand(time(NULL));

    int n = 50;
    double arr[50];

    fillarray(arr, n);

    printf("Random list:\n");
    showarray(arr, n);

    qsort(arr, n, sizeof(double), compare);

    printf("\nSorted list:\n");
    showarray(arr, n);

    return 0;
}
```

### Key concepts
- qsort() needs comparator
- void* → must cast
- return <0, 0, >0

## Challenge #3 — Print current time (with error handling)
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    time_t now = time(NULL);

    if (now == -1) {
        fprintf(stderr, "Error getting time\n");
        exit(EXIT_FAILURE);
    }

    char *time_str = ctime(&now);

    if (time_str == NULL) {
        fprintf(stderr, "Error formatting time\n");
        exit(EXIT_FAILURE);
    }

    printf("Current time: %s", time_str);

    return 0;
}
```

### Key idea
- time() → get time
- ctime() → convert to string
- Always check errors

## Challenge #4 — Seconds since start of month
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    time_t now = time(NULL);

    if (now == -1) {
        fprintf(stderr, "Error getting time\n");
        exit(EXIT_FAILURE);
    }

    struct tm *current = localtime(&now);

    if (current == NULL) {
        fprintf(stderr, "Error converting time\n");
        exit(EXIT_FAILURE);
    }

    struct tm start = *current;

    // Set to beginning of month
    start.tm_mday = 1;
    start.tm_hour = 0;
    start.tm_min = 0;
    start.tm_sec = 0;

    time_t start_time = mktime(&start);

    if (start_time == -1) {
        fprintf(stderr, "Error computing start of month\n");
        exit(EXIT_FAILURE);
    }

    double seconds = difftime(now, start_time);

    printf("Seconds since start of month: %.0f\n", seconds);

    return 0;
}
```

### Key flow (VERY IMPORTANT)
```text
time() → localtime() → modify tm → mktime() → difftime()
```
