## Challenge 1
Write a program that creates, assigns, and accesses double pointers in C/C++. Follow these steps:

1. Create a normal integer variable (non-pointer) and assign it a random value.
2. Create a single integer pointer variable.
3. Create a double integer pointer variable.
4. Assign the address of the normal integer variable (step 1) to the single pointer (step 2).
5. Assign the address of the single pointer (step 2) to the double pointer variable (step 3).

Then, display the following output using all possible syntax:

- All possible ways to find the **value** of the normal integer variable (step 1)
- All possible ways to find the **address** of the normal integer variable (step 1)
- All possible ways to find the **value** of the single pointer variable (step 2)
- All possible ways to find the **address** of the single pointer variable (step 2)
- All possible ways to print the **double pointer value** and **address** (step 3)

### Example Output
```text
Value of num is: 123
Value of num using singlePointer is: 123
Value of num using doublePointer is: 123

Address of num is: 0xffffcbcc
Address of num using singlePointer is: 0xffffcbcc
Address of num using doublePointer is: 0xffffcbcc

Value of Pointer singlePointer is: 0xffffcbcc
Value of Pointer singlePointer using doublePointer is: 0xffffcbcc

Address of Pointer singlePointer is: 0xffffcbcc
Address of Pointer singlePointer using doublePointer is: 0xffffcbcc

Value of Pointer doublePointer is: 0xffffcbcc
Address of Pointer doublePointer is: 0xffffcb8
```

**Note:** Actual memory addresses will vary on each run. The values shown above are just examples.
