### Steps to create a server socket
#### 1. Create the socket
- Use socket()
- Defines protocol (TCP/UDP)
  
#### 2. Bind the socket to an IP and port
- Use bind()
- Associates the socket with a specific port
  
#### 3. Listen for incoming connections (TCP only)
- Use listen()
- Marks socket as passive (server mode)
  
#### 4. Accept a client connection
- Use accept()
- Returns a new socket for communication
  
#### 5. Send/Receive data
- Use read()/write() or recv()/send()
  
#### 6.Close sockets
- Use close()

### Full TCP Server Example (C)
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int server_fd, client_fd;
    struct sockaddr_in address;
    int addrlen = sizeof(address);
    char buffer[BUFFER_SIZE] = {0};
    char *message = "Hello from server";

    // 1. Create socket
    server_fd = socket(AF_INET, SOCK_STREAM, 0);
    if (server_fd == 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }

    // 2. Configure address
    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY; // any IP
    address.sin_port = htons(PORT);

    // 3. Bind socket
    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        perror("bind failed");
        close(server_fd);
        exit(EXIT_FAILURE);
    }

    // 4. Listen
    if (listen(server_fd, 3) < 0) {
        perror("listen");
        close(server_fd);
        exit(EXIT_FAILURE);
    }

    printf("Server listening on port %d...\n", PORT);

    // 5. Accept client
    client_fd = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&addrlen);
    if (client_fd < 0) {
        perror("accept");
        close(server_fd);
        exit(EXIT_FAILURE);
    }

    printf("Client connected!\n");

    // 6. Receive data
    read(client_fd, buffer, BUFFER_SIZE);
    printf("Client: %s\n", buffer);

    // 7. Send response
    send(client_fd, message, strlen(message), 0);
    printf("Message sent\n");

    // 8. Close sockets
    close(client_fd);
    close(server_fd);

    return 0;
}
```

#### Compile and Run
```bash
gcc server.c -o server
./server
```

#### Key Concepts (important for exams)
- AF_INET → IPv4
- SOCK_STREAM → TCP
- INADDR_ANY → Accept connections from any IP
- htons() → Converts port to network byte order
- accept() → creates a new socket per client

#### Quick mental model
```text
socket → bind → listen → accept → read/write → close
```

## Illustration
<img width="392" height="453" alt="image" src="https://github.com/user-attachments/assets/d5815e26-16cc-4e57-9ec6-a2498efc84f1" />
