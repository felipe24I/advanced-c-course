## Challenge String Functions
You need to write a C program that takes two command-line arguments: a single character and a filename. 
The program must read the file line by line using fgets() or getline() and print only those lines that contain the specified character, using puts() for output. 
You can assume no line exceeds 256 characters, and lines are terminated by newline characters. If a line contains the character anywhere within it, the entire line should be printed.

### Example
#### File contents (test.out):
```text
hello, how are you
jason
nope
ok
what
lest
Aaaaa
```
