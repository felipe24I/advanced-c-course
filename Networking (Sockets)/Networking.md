## Netwroking
**Networking in C** means writing programs that communicate over a network using sockets.

The basic idea is:

- **Server:** creates a socket → binds it to an IP/port → listens → accepts clients → receives/sends data.
- **Client:** creates a socket → connects to server IP/port → sends/receives data.

Main functions:

```c
socket()   // creates a communication endpoint
bind()     // assigns IP address and port to server socket
listen()   // puts server socket in listening mode
accept()   // accepts a client connection
connect()  // client connects to server
send()     // sends data
recv()     // receives data
close()    // closes socket
```

Common headers:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>
```

Simple TCP server flow:

```c
int server_fd = socket(AF_INET, SOCK_STREAM, 0);
bind(server_fd, ...);
listen(server_fd, 5);

int client_fd = accept(server_fd, ...);

recv(client_fd, buffer, sizeof(buffer), 0);
send(client_fd, "Hello client", 12, 0);

close(client_fd);
close(server_fd);
```

Simple TCP client flow:

```c
int sock = socket(AF_INET, SOCK_STREAM, 0);

connect(sock, ...);

send(sock, "Hello server", 12, 0);
recv(sock, buffer, sizeof(buffer), 0);

close(sock);
```

In short: **sockets are like virtual cables** that allow two programs to communicate, either on the same computer or across the internet.

### 1. TCP Protocol
TCP (Transmission Control Protocol) is a connection-oriented protocol.

#### Key characteristics:
- Reliable (guarantees delivery)
- Ordered (data arrives in correct order)
- Error-checked
- Uses acknowledgments (ACKs)
- Uses retransmission if data is lost

#### How it works (simplified):
1. Connection setup (3-way handshake)
- Client → SYN
- Server → SYN-ACK
- Client → ACK
2. Data transfer
3. Connection termination

Used in:
- Web (HTTP/HTTPS)
- Email
- File transfer (FTP)

### 2. Client/Server Model
This is the basic communication architecture in networking.

#### Roles:
#### Client:
- Initiates communication
- Requests services

#### Server:
- Waits for requests
- Responds to clients

#### Example:
- Browser → client
- Web server → server

Interaction:

```text
Client ----request----> Server
Client <---response---- Server
```

### 3. Types of Servers
#### Iterative Server
- Handles one client at a time
- Simple but slow

```text
Client1 → served
Client2 → waits
```

#### Concurrent Server
- Handles multiple clients simultaneously

**Ways to implement:**

- Processes (fork())
- Threads (pthread)
- I/O multiplexing (select(), poll())

```text
Client1 → handled
Client2 → handled (same time)
```

#### Event-Driven Server
- Uses non-blocking I/O
- Efficient for high scalability

### 4. Sockets
A socket is an endpoint for communication.

Think of it as: “A file descriptor for network communication”

#### Types:
- SOCK_STREAM → TCP
- SOCK_DGRAM → UDP

#### Socket structure (IPv4):
```c
struct sockaddr_in {
    short sin_family;        // AF_INET
    unsigned short sin_port; // port
    struct in_addr sin_addr; // IP
};
```

### 5. Steps to Use Sockets (TCP Communication)
#### SERVER SIDE
#### Step-by-step:
```c
1. socket()
2. bind()
3. listen()
4. accept()
5. recv() / send()
6. close()
```

#### Flow:
```text
Create socket
     ↓
Bind IP + port
     ↓
Listen for connections
     ↓
Accept client
     ↓
Communicate
     ↓
Close
```

#### CLIENT SIDE
#### Step-by-step:
```c
1. socket()
2. connect()
3. send() / recv()
4. close()
```

#### Flow:
```text
Create socket
     ↓
Connect to server
     ↓
Communicate
     ↓
Close
```

### 6. Simple Example (Conceptual)
#### Serve
```c
socket → bind → listen → accept → recv/send → close
```

#### Client:
```c
socket → connect → send/recv → close
```

### Key Idea to Remember
#### TCP + Sockets = Reliable communication between processes
- TCP → ensures data correctness
- Socket → provides the interface in C

### Ultra Short Summary
- **TCP:** reliable, connection-oriented protocol
- **Client/Server:** client requests, server responds
- **Servers:** iterative (1 client), concurrent (many clients)
- **Socket:** communication endpoint
- Steps:
1. Server → socket, bind, listen, accept, send/recv
2. Client → socket, connect, send/recv
