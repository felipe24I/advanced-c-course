## Stacks
- A stack is a constrained version of a linked list
- all insetions and deletions are only made at the top of the stack
- the last item to be put in to the stack is always the first ietm to be removed, (last-in, first-out) (LIFO)
- a stack is referenced via a pointer to the top element of the stack (the link member in the last node of the stack is set to NULL to indicate the bottom of the stack)

### Basic Operations
#### Push
inserts a new element and places it on top of the stack

#### pop
removes an element from the top of the stack

#### peek
looking at an element at the top without removing it

#### isEmpty
checking if the stack is empty

#### Illustration
<img width="812" height="501" alt="image" src="https://github.com/user-attachments/assets/dac94f63-f6b9-4aee-b15f-9da8e2e70b0f" />

### Applications
- stack support recursive function calls
- stacks are used to store data in memory
- the call stack is useful when debugging
- stacks are used by compilers in the process of evaluating expressions and generating
- stacks can be used when implementing page visited history in a web browser
- a stack could be used as an "undo" operation in a text editor
- a stack can be used to implement psot-fix notation in a computer language (order of operations and operands)
- used in many algorithms like Tower of Hanoi, tree traversals, stock span problem, histogram problem
- an application to reverse a string could use a stack

### Exercise
```c
#include <stdlib.h>
#include<stdio.h>

struct Node
{
    int data;
    struct Node* link;
};

struct Node* top = NULL;

void push(int data)
{
    struct Node* temp = malloc(sizeof(struct Node));

    if (temp != NULL)
    {
        temp->data = data;
        temp->link = top;
        top = temp;
    }
}

int isEmpty()
{
    return top == NULL;
}

void pop()
{
    struct Node* temp;
    if (top != NULL)
    {
        temp = top;
        top = top->link;
        temp->link = NULL;
        free(temp);
    }
}

void display()
{
    struct Node* temp;

    if(top != NULL)
    {
        temp = top;
        while (temp != NULL)
        {
            printf("%d:\n", temp->data);
            temp = temp->link;
        }
    }
}

int main()
{
    push(1);
    push(2);
    push(3);
    push(4);

    display();

    pop();
    pop();

    display();

    return 0;
}
```

### Stack Array Implementation
```c
#include <stdio.h>

#define MAXSIZE 5

struct stack
{
    int stk[MAXSIZE];
    int top;
};

typedef struct stack STACK;

STACK s;

void push(void);
int pop(void);
void display(void);

void main()
{
    int choice;
    int option = 1;
    s.top = -1;

    while (option)
    {
        printf("--------------------------------------------\n");
        printf(" 1 --> PUSH \n");
        printf(" 2 --> POP \n");
        printf(" 3 --> DISPLAY \n");
        printf(" 4 --> EXIT \n");
        printf("--------------------------------------------\n");

        printf("Enter your choice\n");
        scanf("%d", &choice);

        switch (choice)
        {
            case 1:
                push();
                break;
            case 2:
                pop();
                break;
            case 3:
                display();
                break;
            case 4:
                return;

        }

        fflush(stdin);
        printf("Do you want to continue (Type 0 or 1)?\n");
        scanf("%d", &option);
    }
}

void push()
{
    int num = 0;
    if (s.top == (MAXSIZE -1))
    {
        printf("Stack is Full\n");
        return;
    }
    else
    {
        printf("Enter the element to be pushed\n");
        scanf("%d", &num);
        s.top = s.top + 1;
        s.stk[s.top] = num;
    }
    return;

}

int pop ()
{
    int num = 0;
    if (s.top == -1)
    {
        printf("Stack is Empty\n");
        return (s.top);
    }
    else
    {
        num = s.stk[s.top];
        printf("poped element is = %d\n", s.stk[s.top]);
        s.top = s.top - 1;
    }
    return(num);
}

void display()
{
    int i = 0;
    if (s.top == -1)
    {
        printf("Stack is empty\n");
        return;
    }
    else
    {
        printf("\n The status of the stack is \n");
        for (i = s.top; i >= 0; i--)
        {
            printf("%d\n", s.stk[i]);
        }
    }
    printf("\n");
}
```
