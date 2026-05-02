## Abstract Data Types
Is a type whose behavior is defined by a set of value and a set of operations

The definition of ADT only mentions what operations are to be performed
- Not how these operations will be implemented
- Does not specify hpw data will be organized in memory or what algorithms will be used for implementing the operations

It is called "abstract" because it gives an implementation-independent view
- the process of providing only the essentials and hiding the details is known as abstraction
- a contract for a type of data structure

Examples of abstract data types include
- lists, stacks, trees, etc

### Creating a new Abstract Data type
Suposse you want to define a new data type

 - You have to provide a way to store the data (usually by designing a structure
 - You have to provide ways od manipulating the data (adding, deleting, etc)

### Three-step process (from more abstract to more concrete)
#### 1. Provide an abstract description of the type's properties and of the operations you can perform on the type
- not tied to any particular implementation
- referred to as an abstract data type (ADT)

#### 2. Develop a programming interface that implements the ADT
- Indicate how to store the data
- describe a set of functions that perform the desired operations

#### 3. Write code to implement the interface

### Abstraction example
- For a list abstract data type, the data is a number of items
- A list should be able to hold a sequence of items in some kind of order
- The list type should support operations such as adding an item to the list

here are some useful operations

- initializing a list to empty
- adding an item to the end of a list
- determining whether the list is empty
- determining whether the list is full
- determining how many items are in the list

The above is an abstract definition of a list data type
- a data object capable of holding a sequence of items and to which you can apply any of the above
- does not state what kind of items can be stored in the list
- does not specify how to store the items or what algorithms to use

 
  
