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

