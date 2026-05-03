## Binary Trees
- a tree is a nonlinear, two-dimensional data structure with special properties
- trees whose nodes contain a maximum of two links are called binary trees (none, one, or both of which may be NULL)
- the root node is the first node in a tree
- each link in the root node refers to a child (the left child is the first node in the left subtree, the right child is the first node in the right subtree)
- the children of a node are called siblings
- a node with no children is called a leaf node

### Illustration
<img width="932" height="507" alt="image" src="https://github.com/user-attachments/assets/e8a3fc46-d97d-45ad-afc2-9adfe40d5857" />

### Applications
- often used in search, game logic, autocomplete tasks, and graphics
- the most common use of a binary tree is a binary search tree (used in many search applications where data is constantly entering/leaving, map and set objects in many libraries)
- binary space partition algorithm (used in almost every 3D video game to determine what objects need to be rendered)
- used in many high-bandwidth routers for storing router-tables
- different forms of binary trees are used by compilers to parse expressions
- data compression algorithms
- database problems like indexing

### Binary search trees
- a binary search tree is a linked structure that incorporates the binary search algorithm (ordered data structure, allows for fast lookup, addition and removal of items)
- a fundamental data structure used to construct more abstract data structures such as sets, multisets, and associative arrays
- a binary search tree has the following properties (values in any left subtree are less than the value in its parent node, values in any right subtree are greater than the value in its parent node)

### Basic Operations
- Insert – Adds an element to the tree / creates a tree.
- Search – Finds an element in the tree.

### Tree Traversals (because trees are non-linear)
#### 1. Inorder Traversal
- Output order – Nodes in non‑decreasing (ascending) order.
- Steps:
1. Traverse left subtree (inorder)
2. Process the current node’s value
3. Traverse right subtree (inorder)
- Key point – A node is not processed until its entire left subtree has been processed.

#### 2. Preorder Traversal
- Main use – Creating a copy of the tree.
- Steps:
1. Process the current node’s value
2. Traverse left subtree (preorder)}
3. Traverse right subtree (preorder)
- Key point – Node is processed as soon as it is visited.

#### 3. Postorder Traversal
- Main use – Deleting the tree.
- Steps:
1. Traverse left subtree (postorder)
2. Traverse right subtree (postorder)
3. Process the current node’s value
- Key point – Node is processed only after both its left and right children have been processed.

<img width="645" height="560" alt="image" src="https://github.com/user-attachments/assets/33060559-bf8e-4ffc-bd1a-cb223e23c858" />

### Implementation
```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

/* self-referential structure */
struct treeNode {
    int data;                         /* node value */
    struct treeNode *leftPtr;         /* pointer to left subtree */
    struct treeNode *rightPtr;        /* pointer to right subtree */
};

typedef struct treeNode TreeNode;
typedef TreeNode *TreeNodePtr;

/* function prototypes */
void insertNode(TreeNodePtr *treePtr, int value);
void inOrder(TreeNodePtr treePtr);
void preOrder(TreeNodePtr treePtr);
void postOrder(TreeNodePtr treePtr);

int main(void) {
    int i = 0;                        /* counter to loop from 1-10 */
    int item = 0;                     /* variable to hold random values */
    TreeNodePtr rootPtr = NULL;       /* tree initially empty */

    srand(time(NULL));
    printf("The numbers being placed in the tree are:\n");

    /* insert 10 random numbers into the tree */
    for (i = 1; i <= 10; i++) {
        item = rand() % 15;
        printf("%3d", item);
        insertNode(&rootPtr, item);
    }

    /* traverse the tree preOrder */
    printf("\n\nThe preOrder traversal is:\n");
    preOrder(rootPtr);

    /* traverse the tree inOrder */
    printf("\n\nThe inOrder traversal is:\n");
    inOrder(rootPtr);

    /* traverse the tree postOrder */
    printf("\n\nThe postOrder traversal is:\n");
    postOrder(rootPtr);

    printf("\n");
    return 0;
}

/* insert a node into the BST */
void insertNode(TreeNodePtr *treePtr, int value) {
    if (*treePtr == NULL) {
        *treePtr = malloc(sizeof(TreeNode));
        if (*treePtr != NULL) {
            (*treePtr)->data = value;
            (*treePtr)->leftPtr = NULL;
            (*treePtr)->rightPtr = NULL;
        } else {
            printf("%d not inserted. No memory available.\n", value);
        }
    } else {
        /* data to insert is less than data in current node */
        if (value < (*treePtr)->data) {
            insertNode(&((*treePtr)->leftPtr), value);
        }
        /* data to insert is greater than data in current node */
        else if (value > (*treePtr)->data) {
            insertNode(&((*treePtr)->rightPtr), value);
        } else {
            /* duplicate data value ignored */
            printf(" dup");
        }
    }
}

/* inorder traversal (left → node → right) */
void inOrder(TreeNodePtr treePtr) {
    if (treePtr != NULL) {
        inOrder(treePtr->leftPtr);
        printf("%3d", treePtr->data);
        inOrder(treePtr->rightPtr);
    }
}

/* preorder traversal (node → left → right) */
void preOrder(TreeNodePtr treePtr) {
    if (treePtr != NULL) {
        printf("%3d", treePtr->data);
        preOrder(treePtr->leftPtr);
        preOrder(treePtr->rightPtr);
    }
}

/* postorder traversal (left → right → node) */
void postOrder(TreeNodePtr treePtr) {
    if (treePtr != NULL) {
        postOrder(treePtr->leftPtr);
        postOrder(treePtr->rightPtr);
        printf("%3d", treePtr->data);
    }
}
```
