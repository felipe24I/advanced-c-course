## Interprocess Communication (IPC)
**Interprocess Communication (IPC)** is the mechanism that allows different processes (programs running on a computer) to **communicate and share data with each other.**

### Simple explanation:
IPC lets processes:

- send and receive information
- synchronize their actions
- work together even if they run separately

### Common IPC methods:
- **Pipes (same process):** first process communicates with the second process, all flow of data in one direction only (half duplex)
- **Named pipes (different processes, FIFO):** the first process can communicate with the second process and vice versa at the same time (full duplex)
- **Message Queues:** processes will communicate with each other by posting a message and retrieving it out of a queue (full duplex)
- **Shared Memory:** communication between two or more processes is achieved through a shared piece of memory among all processes
- **Sockets:** mostly used to communicate over a network between client and server
- **Signals:** communication between multiple processes by way of signaling, a source process will send a signal (recognized by number) and the destination process will handle it accordingly

### Example:
A web browser process might use IPC to communicate with:

- A rendering engine (to display pages)
- A network process (to fetch data)
