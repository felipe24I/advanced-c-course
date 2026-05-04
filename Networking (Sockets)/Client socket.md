### Steps to create a client socket
#### 1. Create the socket
- socket()

#### 2. Define server address (IP + port)
- Use struct sockaddr_in

#### 3. Connect to the server
- connect()

#### 4. Send data
- send() or write()

#### 5. Receive response
- recv() or read()

#### 6. Close socket
- close()

### Full TCP Client Example (C)
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080
#define BUFFER_SIZE 1024

int main() {
    int sock;
    struct sockaddr_in server_addr;
    char buffer[BUFFER_SIZE] = {0};
    char *message = "Hello from client";

    // 1. Create socket
    sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock < 0) {
        perror("socket failed");
        exit(EXIT_FAILURE);
    }

    // 2. Define server address
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(PORT);

    // Convert IP address from text to binary
    if (inet_pton(AF_INET, "127.0.0.1", &server_addr.sin_addr) <= 0) {
        perror("Invalid address");
        close(sock);
        exit(EXIT_FAILURE);
    }

    // 3. Connect to server
    if (connect(sock, (struct sockaddr *)&server_addr, sizeof(server_addr)) < 0) {
        perror("Connection failed");
        close(sock);
        exit(EXIT_FAILURE);
    }

    printf("Connected to server!\n");

    // 4. Send data
    send(sock, message, strlen(message), 0);
    printf("Message sent\n");

    // 5. Receive response
    read(sock, buffer, BUFFER_SIZE);
    printf("Server: %s\n", buffer);

    // 6. Close socket
    close(sock);

    return 0;
}
```

#### Compile and Run
```bash
gcc client.c -o client
./client
```

#### Key Concepts (important)
- connect() → initiates connection to server
- inet_pton() → converts IP string → binary
- Client does NOT use bind(), listen(), or accept()
- Communication is full-duplex (send & receive)

#### Mental Flow
```text
socket → connect → send/receive → close
```

#### How to test (important)
1. Run your **server first**
2. Then run the **client**

```bash
./server
./client
```

## Illustration
<img width="392" height="453" alt="image" src="https://github.com/user-attachments/assets/506bf075-3fac-4268-a5ad-0f8db5ea3c0e" />
