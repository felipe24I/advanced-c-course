## Queues
- a  queue is similar to a checkout line in a grocery store (the first person in line is serviced first, other customers enter the line only at the end and wait to be serviced)
- queue elements are removed only from the head of the queue
- queue elements are inserted only at the tail of the queue
- a queue is referred to as a first-in, first-out (FIFO)
- The difference between stacks and queues is in removing (in a stack we remove the item that was most recently added, in a queue, we remove the item that was least recently added)

### Two main operations in a queue
- enqueue: will insert an element into the back of the queue
- dequeue: will remove an element from the front of the queue

Other operations

- IsEmpty - check if queue is empty
- IsFull - check if queue is full
- peek - get the value of the front of queue without removing it
- poll or offer (same as dequeue and enqueue)

<img width="727" height="400" alt="image" src="https://github.com/user-attachments/assets/d9c8569c-f6b8-4273-9db7-f6409b146933" />

### Queue Applications
- some computers have only a single processor, so only one user at a time may be serviced
- queues are also used to support print spooling
- Information packets also wait in queues in computer networks
- basically, when a resource is shared among multiple consumers, queues are often utilized

### Advantages
- speed
- flexibility

### Implementation
a queue can be implemented either through an array or a linked list

#### Implementation as linked list
```c
#include <stdio.h>
#include <stdlib.h>

// =============================
// Node structure
// =============================
struct QNode
{
    int key;
    struct QNode* next;
};

// =============================
// Queue structure
// =============================
struct Queue
{
    struct QNode *front, *rear;
};

// =============================
// Create a new node
// =============================
struct QNode* newNode(int k)
{
    struct QNode* temp = malloc(sizeof(struct QNode));

    if (temp == NULL)
    {
        printf("Memory allocation failed\n");
        exit(1);
    }

    temp->key = k;
    temp->next = NULL;
    return temp;
}

// =============================
// Create queue
// =============================
struct Queue* createQueue()
{
    struct Queue* q = malloc(sizeof(struct Queue));

    if (q == NULL)
    {
        printf("Memory allocation failed\n");
        exit(1);
    }

    q->front = q->rear = NULL;
    return q;
}

// =============================
// Enqueue (insert at rear)
// =============================
void enQueue(struct Queue* q, int k)
{
    struct QNode* temp = newNode(k);

    if (q->rear == NULL)
    {
        q->front = q->rear = temp;
        return;
    }

    q->rear->next = temp;
    q->rear = temp;
}

// =============================
// Dequeue (remove from front)
// =============================
int deQueue(struct Queue* q)
{
    if (q->front == NULL)
    {
        printf("Queue is empty\n");
        return -1; // error value
    }

    struct QNode* temp = q->front;
    int value = temp->key;

    q->front = q->front->next;

    if (q->front == NULL)
        q->rear = NULL;

    free(temp);
    return value;
}

// =============================
// Display queue
// =============================
void display(struct Queue* q)
{
    struct QNode* temp = q->front;

    if (temp == NULL)
    {
        printf("Queue is empty\n");
        return;
    }

    while (temp != NULL)
    {
        printf("%d -> ", temp->key);
        temp = temp->next;
    }
    printf("NULL\n");
}

// =============================
// Main
// =============================
int main()
{
    struct Queue* q = createQueue();

    enQueue(q, 1);
    enQueue(q, 2);

    printf("Dequeued: %d\n", deQueue(q));
    printf("Dequeued: %d\n", deQueue(q));

    enQueue(q, 3);
    enQueue(q, 4);
    enQueue(q, 5);

    display(q);

    printf("Dequeued: %d\n", deQueue(q));

    display(q);

    return 0;
}
```

#### Implementation as a array
```c
#include <stdio.h>

#define MAX 50

void enqueue();
void dequeue();
void display();

int queue_array[MAX];
int rear = -1;
int front = -1;

int main()
{
    int choice;
    int option;

    while (1)
    {
        printf("\n1. Insert element to queue\n");
        printf("2. Delete element from queue\n");
        printf("3. Display all elements of queue\n");
        printf("4. Quit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
            case 1:
                enqueue();
                break;
            case 2:
                dequeue();
                break;
            case 3:
                display();
                break;
            case 4:
                return 0;
            default:
                printf("Wrong choice\n");
        }

        printf("Do you want to continue (Type 0 or 1)? ");
        scanf("%d", &option);

        if (option == 0)
            break;
    }

    return 0;
}

void enqueue() {
    int add_item;

    if (rear == MAX - 1)
        printf("Queue Overflow\n");
    else {
        if (front == -1)
            front = 0;

        printf("Insert the element in queue: ");
        scanf("%d", &add_item);

        rear = rear + 1;
        queue_array[rear] = add_item;
    }
}

void dequeue() {

    if (front == -1 || front > rear) {
        printf("Queue Underflow\n");
        return;
    }
    else {
        printf("Element deleted from queue is: %d\n", queue_array[front]);
        front = front + 1;
    }
}

void display()
{
    int i;

    if (front == -1) {
        printf("Queue is empty\n");
        return;
    }

    printf("Queue is:\n");
    for (i = front; i <= rear; i++) {
        printf("%d ", queue_array[i]);
    }
    printf("\n");
}
```
