## Challenge 1 (Signal handling)
The problem consists of building a C program that generates random multiplication questions and tracks 
the user’s score while handling asynchronous events using signals. The program must detect when the user presses Ctrl + C (SIGINT) 
to display the final score and exit gracefully. Additionally, it must enforce a time limit: if the user does not answer a question within 5 seconds, 
a timer should trigger a SIGALRM signal, causing the program to print a “TIME’S UP!” message and then terminate by raising SIGINT. 

```c
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>
#include <unistd.h>
#include <time.h>

int total_questions = 0;
int correct_answers = 0;

void handle_sigint(int sig)
{
    printf("\nFinal score: %d correct out of %d questions\n",
           correct_answers, total_questions);
    exit(0);
}

void handle_sigalrm(int sig)
{
    printf("\nTIME'S UP!\n");

    // Raise SIGINT to show final score and exit
    raise(SIGINT);
}

int main(void)
{
    int a, b, answer, correct;

    srand(time(NULL));

    // Handle Ctrl + C
    signal(SIGINT, handle_sigint);

    // Handle alarm signal
    signal(SIGALRM, handle_sigalrm);

    while (1)
    {
        a = rand() % 10 + 1;
        b = rand() % 10 + 1;
        correct = a * b;

        printf("\nWhat is %d x %d? ", a, b);
        fflush(stdout);

        // Start 5-second timer
        alarm(5);

        if (scanf("%d", &answer) != 1)
        {
            printf("Invalid input.\n");
            while (getchar() != '\n');
            continue;
        }

        // Cancel alarm because user answered in time
        alarm(0);

        total_questions++;

        if (answer == correct)
        {
            printf("Correct!\n");
            correct_answers++;
        }
        else
        {
            printf("Incorrect. The answer was %d\n", correct);
        }
    }

    return 0;
}
```
