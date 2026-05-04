## Networking in C: Sockets API
### 1. Sockets API
The Sockets API is the set of C functions used to create network programs.

A socket is like a file descriptor used for communication.

```c
int sockfd;
sockfd = socket(AF_INET, SOCK_STREAM, 0);
```

Common headers:

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/socket.h>
#include <arpa/inet.h>
#include <netinet/in.h>
```

### 2. Ports
A **port** identifies a specific service or program on a machine.

**Example:**

```text
IP address: 192.168.1.10
Port: 80
```

**Together:**

```text
192.168.1.10:80
```

**Common ports:**

```text
80   HTTP
443  HTTPS
22   SSH
25   SMTP
53   DNS
```

### 3. Port and Service Functions
These functions help translate between **service names** and **port numbers.**

**Example functions:**

```c
getservbyname()
getservbyport()
```

**Example:**

```c
struct servent *service;

service = getservbyname("http", "tcp");

printf("Port: %d\n", ntohs(service->s_port));
```

getservbyname() finds the port number for a service like "http".

### 4. IP Address Functions
Used to convert IP addresses between text and binary.

#### inet_addr()
```c
in_addr_t addr;

addr = inet_addr("127.0.0.1");
```

Converts string IP to binary form.

#### inet_ntoa()
```c
printf("%s\n", inet_ntoa(address.sin_addr));
```

Converts binary IP to string.

#### inet_pton()
```c
inet_pton(AF_INET, "127.0.0.1", &server.sin_addr);
```

Better modern function.

#### inet_ntop()
```c
inet_ntop(AF_INET, &server.sin_addr, buffer, sizeof(buffer));
```

Converts binary IP to string.

### 5. Main Structures
#### struct sockaddr_in
Used for IPv4 addresses.

```c
struct sockaddr_in server;

server.sin_family = AF_INET;
server.sin_port = htons(8080);
server.sin_addr.s_addr = INADDR_ANY;
```

important fields:

```c
sin_family   // Address family, usually AF_INET
sin_port     // Port number
sin_addr     // IP address
```

#### struct sockaddr
Generic socket address structure.

```c
(struct sockaddr *)&server
```

Many socket functions expect struct sockaddr *, so we cast sockaddr_in.

#### struct hostent
Used for host information.

```c
struct hostent *host;
```

Can store hostname, IP addresses, etc.

#### struct servent
Used for host information.

```c
struct hostent *host;
```

Stores service name, port, and protocol.

### 6. Steps in Using Sockets
#### Server Steps
```text
socket()
bind()
listen()
accept()
recv() / send()
close()
```

Example flow:

```c
int server_fd = socket(AF_INET, SOCK_STREAM, 0);

bind(server_fd, (struct sockaddr *)&server_addr, sizeof(server_addr));

listen(server_fd, 5);

int client_fd = accept(server_fd, NULL, NULL);

recv(client_fd, buffer, sizeof(buffer), 0);

send(client_fd, "Hello", 5, 0);

close(client_fd);
close(server_fd);
```

#### Client Steps
```text
socket()
connect()
send() / recv()
close()
```

Example flow:

```c
int sockfd = socket(AF_INET, SOCK_STREAM, 0);

connect(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr));

send(sockfd, "Hello server", 12, 0);

recv(sockfd, buffer, sizeof(buffer), 0);

close(sockfd);
```

### 7. Sockets vs FILE I/O
Sockets and files are similar because both use file descriptors.

Example:

```c
read(sockfd, buffer, sizeof(buffer));
write(sockfd, message, strlen(message));
```

But sockets are different because they communicate over a network.

#### FILE I/O
- Reads/writes files
- Uses read, write, close
- Usually local
- Data is stored

#### Socket I/O
- Sends/receives network data
- Uses send, recv, close
- Usually remote
- Data is transmitted

### 8. Socket Functions
#### socket()
Creates a socket.

```c
int socket(int domain, int type, int protocol);
```

Example:

```c
int sockfd = socket(AF_INET, SOCK_STREAM, 0);
```

Meaning:

```text
AF_INET      IPv4
SOCK_STREAM TCP
0           default protocol
```

#### connect()
Used by the **client** to connect to a server.

```c
int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

Example:

```c
connect(sockfd, (struct sockaddr *)&server_addr, sizeof(server_addr));
```

#### bind()
Assigns an IP address and port to a socket.

Used mostly by servers.

```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

Example:

```c
bind(server_fd, (struct sockaddr *)&server_addr, sizeof(server_addr));
```

#### listen()
Makes the socket wait for incoming TCP connections.

```c
int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
```

Example:

```c
bind(server_fd, (struct sockaddr *)&server_addr, sizeof(server_addr));
```

The backlog is the waiting queue size.

#### accept()
Accepts a client connection.

```c
int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
```

Example:

```c
int client_fd = accept(server_fd, NULL, NULL);
```

Important: accept() returns a **new socket** for communicating with that client.

#### recv()
Receives data from a connected socket.

```c
ssize_t recv(int sockfd, void *buf, size_t len, int flags);n);
```

Example:

```c
int bytes = recv(client_fd, buffer, sizeof(buffer), 0);
```

Used mainly with TCP.

#### recvfrom()
Receives data and also gets the sender address.

```c
ssize_t recvfrom(
    int sockfd,
    void *buf,
    size_t len,
    int flags,
    struct sockaddr *src_addr,
    socklen_t *addrlen
);
```

Used mainly with TCP.

#### send()
Sends data through a connected socket.

```c
ssize_t send(int sockfd, const void *buf, size_t len, int flags);
```

Example:

```c
send(client_fd, "Hello", 5, 0);
```

Used mainly with TCP.

#### sendto()
Sends data to a specific address.

```c
ssize_t sendto(
    int sockfd,
    const void *buf,
    size_t len,
    int flags,
    const struct sockaddr *dest_addr,
    socklen_t addrlen
);
```

Usually used with UDP.

#### read()
Reads data from a socket or file descriptor.

```c
ssize_t read(int fd, void *buf, size_t count);
```

Example:

```c
read(sockfd, buffer, sizeof(buffer));
```

Similar to recv() but without socket-specific flags.

#### write()
Writes data to a socket or file descriptor.

```c
ssize_t write(int fd, const void *buf, size_t count);
```

Example:

```c
write(sockfd, "Hello", 5);
```

Similar to send() but without socket-specific flags.

#### close()
Closes the socket completely.

```c
int close(int fd);
```

Example:

```c
close(sockfd);
```

After this, the socket cannot send or receive.

#### shutdown()
Closes only part of the communication.

```c
int shutdown(int sockfd, int how);
```

Options:

```c
SHUT_RD    // Stop receiving
SHUT_WR    // Stop sending
SHUT_RDWR  // Stop both
```

Example:

```c
shutdown(sockfd, SHUT_WR);
```

This means: “I will not send more data, but I can still receive.”

### 9. Important Difference
#### close() vs shutdown()
```text
close()    → destroys the socket descriptor
shutdown() → disables send/receive direction
```

Example:

```c
shutdown(sockfd, SHUT_WR);
```

Useful when the client wants to tell the server:

```text
I finished sending data.
```

But still wants to receive the response.

### 10. Short Exam Summary
The Sockets API allows C programs to communicate over networks. A server creates a socket with socket(), assigns an IP and port with bind(), waits with listen(), 
accepts clients with accept(), exchanges data with send() and recv(), and finishes with close(). 
A client creates a socket, connects with connect(), sends and receives data, then closes the socket. 
TCP uses SOCK_STREAM, while UDP uses SOCK_DGRAM. Functions like inet_pton() and inet_ntop() convert IP addresses, 
while getservbyname() and getservbyport() work with service names and ports.
