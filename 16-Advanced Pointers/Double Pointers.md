## Double Pointers
* **Definition:** When a pointer holds the address of another pointer, it is called a pointer-to-pointer or double pointer.
* **Basic Pointer Concept:** A pointer stores the address of a variable.

<img width="332" height="102" alt="image" src="https://github.com/user-attachments/assets/16bd617c-27fc-4aee-af92-f4dfd60c72b7" />

* **Double Pointer Concept:** The first pointer contains the address of the second pointer. The second pointer points to the location containing the actual value.

<img width="553" height="111" alt="image" src="https://github.com/user-attachments/assets/edbb7d9f-9a44-4c4b-b393-bc9f3bad829e" />

### Syntax:
```c
int **var; // a pointer to a pointer of type int
```

* **Accessing Value:** To access a value indirectly pointed to by a pointer-to-pointer, the asterisk operator (*) must be applied twice.

### Example Code:
```c
// Declaring a double pointer
int **ipp;

// Declaring some ints
int i = 5, j = 6, k = 7;

// Initializing some pointers
int *ipp1 = &i, *ipp2 = &j;

// Assigning our double pointer
ipp = &ipp1;
```

