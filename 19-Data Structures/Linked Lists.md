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

### Illustration
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
