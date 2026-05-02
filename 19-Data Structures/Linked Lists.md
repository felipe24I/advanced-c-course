## Linked Lists
Is an abstract data structure
- it can be used to store a lot of different kinds of data
- You have to create your own
- It is a linear ata structure

It is a sequence of data structures which are connected together via links/nodes
- each link contains data items (elements)

There are different types of linked lists
- **single linked list:** can only be parsed one way (forward)
- **doubly linked list:** previous and next pointers, can be traversed forward and backward

Linked lists are dynamic
- the length of a list can increase or decrease as necessary

A linked list can be used when the number of data elements to be represented in the data structure is unpredictable

A node/link can contain data of any type including other struct objects

linked lists can be maintained in sorted order by inserting each new elements at the proper point in the list

### Linked lists and pointers
Linked list heavily use pointers in its implementation
- unerstanding pointers is crucial to understanding how linked lists work
- You must alse be familiar with dynamic memory allocation and structures

Linked lists work an array that can grow and shrink as needed, form any point in the array

Linked lists are accessed via a pointer to the first node of the list

The link pointer in the last node of a list is set to NULL to mark the end of the list

### Links / Nodes
Each node contains the following
- a piece of data
- the first node/link is often referred to as the head node/link
- each node/link also contains a connection to anothyer node/link, this is referred to as the next node/link, the last node/link points to NULL

In order to traverse the list, you follow the pointer from each node/link to the next

Prepending to a list is very fast

Inserting into a sorted list is very fast

### Ilustration
<img width="755" height="381" alt="image" src="https://github.com/user-attachments/assets/142637c7-1de6-43e7-90bf-662c7b5bb4d2" />

### linked lists vs arrays
arrays are a fixed sixe

- must know the upper limit on the number of elements in advance

an array can be declared to contain more elements than the number of data items expected

- this can waste memory

linked lists can provide better memory utilization than arrays

- using dynamic memory allocation that grow and shrink at execution time
- however, extra memory space for a pointer is required with each element of the list (more overhead)
- increases the risk of memory leaks and segment faults

Insertion and deletion in a sroted array can be time consuming
- all the elements following the inserted or deleted element must be shifted appropriately

The elements of an array are stored contiguously in memory
- allows immediate access to any array element

Linked list elements are not stored at a contiguous location
- elements are linked using pointers
- have to access elements sequentially starting from the first node/link

### Exercise
```c
#include <stdio.h>
#include <stdlib.h>

/* self-referential structure */
typedef struct node
{
    char data;
    struct node *nextPtr;
} node_t;

typedef node_t *ListNodePtr;

/* prototypes */
void insert( ListNodePtr *head, char value);
void insertAtEnd( ListNodePtr *head, char value);
void insertAtBegining(ListNodePtr *head, char val);
char delete(ListNodePtr *head, char value);
void deleteAtBeginning( ListNodePtr *head);
int isEmpty( ListNodePtr head);
void printList( ListNodePtr currentPtr);

int main(void)
{
    ListNodePtr head = NULL; /* initially there are no nodes */
    int choice = 0; /* user's choice */
    char item = '\0'; /* char entered by user */

    /* display the menu */
    printf( "Enter your choice:\n"
           " 1 to insert an element into the list in alphabetical order. \n"
           " 2 to insert an element at the end of the list. \n"
           " 3 to insert an element at the beginning of the list. \n"
           " 4 to delete an element from the list. \n"
           " 5 to delete an element from the begining of the list. \n"
           " 6 to end. \n");
    printf(":: ");
    scanf("%d", &choice);

    /* loop while user does not choose 3 */
    while (choice != 6)
    {
        switch (choice)
        {
            case 1:
                printf( "Enter a character: ");
                scanf( "\n%c", &item);
                insert( &head, item); /* insert ietm in list */
                printList(head);
                break;
            case 2:
                printf( "Enter a character: ");
                scanf( "\n%c", &item);
                insertAtEnd( &head, item); /* insert ietm in list */
                printList(head);
                break;
            case 3:
                printf( "Enter a character: ");
                scanf( "\n%c", &item);
                insertAtBegining( &head, item); /* insert ietm in list */
                printList(head);
                break;
            case 4: /* delete an element */
                /* if list is not empty */
                if ( !isEmpty( head))
                {
                    printf("Enter character to be deleted: ");
                    scanf("\n%c", &item);
                    /* if character is found, remove it*/
                    if ( delete( &head, item)) /* remove item */
                    {
                        printf( "%c deleted.\n", item);
                        printList(head);
                    }
                    else
                    {
                        printf("%c not found.\n\n", item);
                    }
                }
                else
                {
                    printf( "List is empty.\n\n");
                }
                break;
            case 5: /* delete an element at the begining */
                /* if list is not empty */
                if ( !isEmpty( head))
                {
                    /* if character is found, remove it*/
                    deleteAtBeginning( &head);
                    printf( "%c deleted.\n", item);
                    printList(head);
                }
                else
                {
                    printf( "List is empty.\n\n");
                }
                break;
            default:
                printf("Enter your choice:\n"
                       " 1 to insert an element into the list.\n"
                       " 2 to delete an elemnt from the list"
                       " 3 to end.\n");
                break;
        } /* end switch */

        printf( "? ");
        scanf("%d", &choice);
    } /* end while */

    printf( "End of run.\n");

    return 0;

}

void insertAtBegining(ListNodePtr *head, char val)
{
    ListNodePtr new_node = malloc(sizeof(node_t));
    new_node->data = val;
    new_node->nextPtr = *head;
    *head = new_node;
}

void insertAtEnd(ListNodePtr *head, char val)
{
    ListNodePtr current = *head;

    if(current != NULL)
    {
        while (current->nextPtr != NULL)
        {
            current = current->nextPtr;
        }

        /* now we can add a new variable */
        current->nextPtr = malloc(sizeof(node_t));
        current->nextPtr->data = val;
        current->nextPtr->nextPtr = NULL;
    }
    else
    {
        current = malloc(sizeof(node_t));
        current->data = val;
        current->nextPtr = NULL;
        *head = current;
    }
}

void insert( ListNodePtr *head, char value)
{
    ListNodePtr newPtr; /* pointer to new node */
    ListNodePtr previousPtr; /* pointer to previous node in list*/
    ListNodePtr currentPtr; /* pointer to current node in list */

    newPtr = malloc(sizeof(node_t)); /*create node*/

    if ( newPtr != NULL) /* is space available */
    {
        newPtr->data = value; /* place value in node */
        newPtr->nextPtr = NULL; /* node does not link to another node */

        previousPtr = NULL;
        currentPtr = *head;

        /* loop to find the correct location in the list */
        while ( currentPtr != NULL && value > currentPtr->data)
        {
            previousPtr = currentPtr; /* walk to ... */
            currentPtr = currentPtr->nextPtr; /* ...next node */
        } /* end while */

        /* insert new node at beginning of list */
        if (previousPtr == NULL)
        {
            newPtr->nextPtr = *head;
            *head = newPtr;
        }
        else /* Insert new node between previousPtr and currentPtr */
        {
            previousPtr->nextPtr = newPtr;
            newPtr->nextPtr = currentPtr;
        }
    }
    else
    {
        printf("%c not inserted. No memory available.\n", value);
    }

}

void deleteAtBeginning(ListNodePtr *head)
{
    ListNodePtr tempPtr = NULL; /* temporary node pointer */

    if(head == NULL)
    {
        return;
    }
    else
    {
        tempPtr = *head; /* hold onto node being removed */
        *head = ( *head)->nextPtr; /* de-thread the node */
        free(tempPtr); /* free the de-threaded node */
    }
}

/* Delete a list element */
char delete (ListNodePtr *head, char value)
{
    ListNodePtr previousPtr; /* pointer to previous node in list*/
    ListNodePtr currentPtr; /* pointer to current node in list */
    ListNodePtr tempPtr; /* temporary node pointer */

    /* delete first node */
    if ( value == (*head)->data)
    {
        tempPtr = *head; /* hold onto node being removed */
        *head = (*head)->nextPtr; /*de-threas the node */
        free(tempPtr); /* free the de-threaded node */
        return value;
    }
    else
    {
        previousPtr = *head;
        currentPtr = (*head)->nextPtr;

        /* loop to find the correct location in the list */
        while (currentPtr != NULL && currentPtr->data != value)
        {
            previousPtr = currentPtr;
            currentPtr = currentPtr->nextPtr;
        }

        /* delete node at currentPtr */
        if ( currentPtr != NULL)
        {
            tempPtr = currentPtr;
            previousPtr->nextPtr = currentPtr->nextPtr;
            free(tempPtr);
            return value;
        }
    }

    return '\0';
}

void printList( ListNodePtr currentPtr)
{
    /* if list is empty */
    if ( currentPtr == NULL)
    {
        printf("List is empty.\n\n");
    }
    else
    {
        printf("The list is:\n");

        /*while not the end of the list */
        while (currentPtr != NULL)
        {
            printf("%c --> ", currentPtr->data);
            currentPtr = currentPtr->nextPtr;
        }

        printf("NULL\n\n");
    }
}

/* Return 1 if the list is empty, 0 otherwise */
int isEmpty(ListNodePtr head)
{
    return head == NULL;
}
```
