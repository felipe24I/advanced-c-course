## Challenge
This challenge consists of creating a simple client/server system using sockets in C. 
Client1 sends an integer to the server, the server receives it, subtracts 1, prints both values, and then sends the decremented value to Client2. 
The server must listen on a known port and handle each client connection sequentially.

### server.c
```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080

int main() {
    int server_fd, client1_fd, client2_fd;
    struct sockaddr_in server_addr;
    int number, result;

    server_fd = socket(AF_INET, SOCK_STREAM, 0);

    server_addr.sin_family = AF_INET;
    server_addr.sin_addr.s_addr = INADDR_ANY;
    server_addr.sin_port = htons(PORT);

    bind(server_fd, (struct sockaddr *)&server_addr, sizeof(server_addr));

    listen(server_fd, 2);

    printf("Server listening on port %d...\n", PORT);

    client1_fd = accept(server_fd, NULL, NULL);
    read(client1_fd, &number, sizeof(number));

    result = number - 1;

    printf("Received value: %d\n", number);
    printf("Sent value: %d\n", result);

    close(client1_fd);

    client2_fd = accept(server_fd, NULL, NULL);
    write(client2_fd, &result, sizeof(result));

    close(client2_fd);
    close(server_fd);

    return 0;
}
```

### client1.c
```c
#include <stdio.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080

int main() {
    int sock;
    struct sockaddr_in server_addr;
    int number;

    printf("Enter an integer: ");
    scanf("%d", &number);

    sock = socket(AF_INET, SOCK_STREAM, 0);

    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(PORT);
    server_addr.sin_addr.s_addr = inet_addr("127.0.0.1");

    connect(sock, (struct sockaddr *)&server_addr, sizeof(server_addr));

    write(sock, &number, sizeof(number));

    close(sock);

    return 0;
}
```

### client2.c
```c
#include <stdio.h>
#include <unistd.h>
#include <arpa/inet.h>

#define PORT 8080

int main() {
    int sock;
    struct sockaddr_in server_addr;
    int result;

    sock = socket(AF_INET, SOCK_STREAM, 0);

    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(PORT);
    server_addr.sin_addr.s_addr = inet_addr("127.0.0.1");

    connect(sock, (struct sockaddr *)&server_addr, sizeof(server_addr));

    read(sock, &result, sizeof(result));

    printf("Client2 received: %d\n", result);

    close(sock);

    return 0;
}
```

### Compile and run
```bash
gcc server.c -o server
gcc client1.c -o client1
gcc client2.c -o client2
```

Run in this order:

```bash
./server
./client1
./client2
```
