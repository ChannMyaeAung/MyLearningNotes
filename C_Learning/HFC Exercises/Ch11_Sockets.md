### Chapter 11 - sockets and networking

#### Chapter 11 - Exercise 1

`main.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <sys/socket.h>
#include <unistd.h>
#include <arpa/inet.h>
#include "header.h"

int main(){
    
    // Array of advice strings to be sent to clients
    char *advice[] = {
        "Take smaller bites\r\n",
        "Go for the tight jeans. No they do NOT make you look fat.\r\n",
        "One word: inappropriate\r\n",
        "Just for today, be honest. Tell your boss what you *really* think\r\n",
        "You might want to rethink that haircut\r\n"};
    
    // Create a socket using the Internet address family (PF_INET) and TCP (SOCK_STREAM)
    int listener_d = socket(PF_INET, SOCK_STREAM, 0);
    if(listener_d == -1){
        error("Can't open socket!"); // Handle error if socket creation fails
    }
    
    // Set up the address structure
    struct sockaddr_in name;
    name.sin_family = PF_INET; // Use the internet address family
    
    // Set port number to 30000, converting to network byte order
    name.sin_port = (in_port_t)htons(30000);
    
    // Bind to any available network interface
    name.sin_addr.s_addr = htonl(INADDR_ANY);
    
    // bind the socket to the address and port number specified in the 'name' struct
    int c = bind(listener_d, (struct sockaddr *)&name, sizeof(name));
    if(c == -1){
        // Handle error if binding fails
        error("Can't bind to socket\n");
    }
    
    // Listen for incoming connections, with a backlog of 10 pending connections
    if(listen(listener_d, 10) == -1)){
     	error("Can't listen");   
    }
    puts("Waiting for connection"); // Print a message indicating the server is waiting for connections
    
    // Infinite loop to handle incoming connections
    while(1){
        struct sockaddr_storage client_addr;
    unsigned int address_size = sizeof(client_addr);
    int connect_d = accept(listener_d, (struct sockaddr *)&client_addr, &address_size);
    if(connect_d == -1){
        error("Can't open secondary socket!");
    }
    char *msg = advice[rand() % 5];
    
    if(send(connect_d, msg, strlen(msg), 0) == -1){
        error("send");
    }
    close(connect_d);
    }
    
    return 0;
}
```

- We should always check if socket, bind, listen, accept or send return -1.

`functions.c`

```C
void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(0);
}

```

`header.h`

```C
void error(char *msg);
```



Code Execution:



```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ make all
clang -Wall -Wextra -g -c main.c
clang -Wall -Wextra -g main.o functions.o -o test
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Waiting for connection
^C
```

On the second console

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Just for today, be honest. Tell your boss what you *really* think
Connection closed by foreign host.

chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Go for the tight jeans. No they do NOT make you look fat.
Connection closed by foreign host.

```



- We're using **127.0.0.1** as the IP address because the client is running on the same machine as the server.
- We could have connected to the server from anywhere on the network and we'd have gotten the same response.

If I start the server, then run the client one time, it works.

But then if I stop the server and restart it real quick, the client can't get a response anymore!

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Can't bind to socket: Address already in use

```

On the second console

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
telnet: Unable to connect to remote host: Connection refused

```

- When we bind a socket to a port, the OS will prevent anything else from rebinding to it for the next 30 seconds or so, and that includes the program that bound the port in the first place.
- To get around the problem, we need to set an option on the socket before we bind it:

The error message "Can't bind to socket: Address already in use" indicates that the port our server is trying to bind to is already occupied by another process. This is a common issue when a server process is terminated and immediately restarted. The underlying socket remains in a `TIME_WAIT` state for a period, preventing another process from binding to the same port.

To resolve this issue, you can set the `SO_REUSEADDR` socket option on our listener socket. This option tells the kernel that we're okay with reusing the local address (`port`), even if it's still in the `TIME_WAIT` state. Here's how we can modify our code to include this option:

1. Include the `setsockopt` function call right after creating our listener socket and before binding it to an address.
2. Use `SO_REUSEADDR` to allow the port to be reused.

Here's an example snippet showing how to set `SO_REUSEADDR`:

We have to insert it after creating the listener socket(`listener_d`) and before we bind it to an address.

```C
// We need an int variable to store the option. Setting it to 1 means "Yes, reuse  the port"
int reuse = 1;
if (setsockopt(listener_d, SOL_SOCKET, SO_REUSEADDR, (void *)&reuse, sizeof(int)) == -1) {
    error("Can't set the 'reuse address' option on the socket");
}
```

- If the server has responded to a client and then gets stopped and restarted, the call to the bind system call fails.
- If we didn't check for errors, the rest of the server code would run even though it couldn't use the server port.
- The above code makes the socket **reuse the port** when it's bound. That means now we can stop and restart the server and there will be no errors.

`SOL_SOCKET`is a constant used in socket programming to specify that the options being set or retrieved apply at the socket level, as opposed to a protocol level. 

- It's used with `setsockopt`and `getsockopt` functions to set or get options that are applicable to all types of sockets, regardless of the communication protocol chosen (e.g., TCP, UDP). 
- Essentially, `SOL_SOCKET` specifies that the options are to be interpreted by the socket layer itself, not by any specific protocol implementation.

`(void *)&reuse`

The reason for casting it to a different pointer type, such as `(char *)` or more appropriately `(void *)`, is primarily historical and due to the generic interface of `setsockopt`. The function is designed to accept a wide variety of option values, and thus it expects a pointer to `void` to accommodate this flexibility.

However, in modern C programming, casting to `(void *)` is the preferred approach when passing the address of `reuse` to `setsockopt`. This is because `(void *)` is a generic pointer type that can point to any type of data. Casting to `(char *)` works, but it's not semantically correct in this context since we're not working with character data. The correct and clear way to pass an integer option value to `setsockopt` is by using `(void *)&reuse`, as this accurately reflects that we're passing a pointer to an integer, treated generically.



#### Test Run

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Waiting for connection
^C

chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Waiting for connection
```

On the second console:

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Just for today, be honest. Tell your boss what you *really* think
Connection closed by foreign host.

chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Just for today, be honest. Tell your boss what you *really* think
Connection closed by foreign host.

```



#### Chapter 11 - Long Exercise

#includes are removed to save space.

`functions.c`

```C
void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(0);
}

int catch_signal(int sig, void (*handler)(int)){
    struct sigaction action;
    action.sa_handler = handler;
    sigemptyset(&action.sa_mask);
    action.sa_flags = 0;
    return sigaction(sig, &action, NULL);
}

int open_listener_socket(){
    int s = socket(PF_INET, SOCK_STREAM, 0);
    if(s == -1){
        error("Can't open socket");
    }
    return s;
}

void bind_to_port(int socket, int port){
    
    // Create a structure to hold the address
    struct sockaddr_in name;
    
    // Use the IPv4 protocol family
    name.sin_family = PF_INET;
    
    // Convert the port to network byte order
    name.sin_port = (in_port_t)htons(port);
    
    // Bind to any available network interface
    name.sin_addr.s_addr = htonl(INADDR_ANY);
    
    // Allow reuse of local addresses
    // Useful for avoiding "address already in use" error
    int reuse = 1;
    if(setsockopt(socket, SOL_SOCKET, SO_REUSEADDR, (void *)&reuse, sizeof(int)) == -1){
        error("Can't set the reuse option on the socket.");
    }
    
    // Bind the socket to the address and port
    int c = bind(socket, (struct sockaddr *)&name, sizeof(name));
    if(c == -1){
        error("Can't bind to socket");
    }
}

// Function to send a message to a socket
int say(int socket, char *s){
    // Send the message
    int result = send(socket, s, strlen(s), 0);
    
    // Don't call error() if there's a problem. We don't want to stop the server if there's just a problem with one client.
    if(result == -1){
        fprintf(stderr, "%s: %s\n", "Error talking to the client", strerror(errno));
    }
    return result;
}

// Function to read data from a socket
int read_in(int socket, char *buf, int len){
    // Pointer to the buffer
    char *s = buf;
    
    // Length of the buffer
    int slen = len;
    
    // Receive data from the socket
    int c = recv(socket, s, slen, 0);
    
    // Continue receiving until a newline character is found
    while((c > 0) && (s[c-1] != '\n')){
        s += c; // Move the buffer pointer
        slen -= c; // Decrease the remaining length
        
        // Receive more data
        c = recv(socket, s, slen, 0);
    }
    
    // Check if receiving failed
    if(c < 0){
        return c; // Return the error code
    }else if(c == 0){ // Check if the connection is closed
        buf[0] = '\0'; // Null-terminate the buffer
    }else{
        s[c-1] = '\0'; // Replace the newline character with null terminator.
    }
    
    // Return the number of bytes read
    return len - slen;
}

// Global variable for the listener socket descriptor
int listener_d;

// If someone hits CTRL+C when the server is running, this function will close the socket before the program ends.
void handle_shutdown(){
    
    // Closes the listener socket if it is open
    if(listener_d){
        close(listener_d);
    }
    
    // Print a message before exiting the program.
    fprintf(stderr, "Bye!\n");
    exit(0);
}
```

`main.c`

```C
// Main function, entry point of the program
int main(int argc, char *argv[]){
    
    // Set up signal handler for SIGINT (Ctrl+C)
    if(catch_signal(SIGINT, handle_shutdown) == -1){
        error("Can't set the interrupt handler.");
    }
    
    // Open a listening socket
    listener_d = open_listener_socket();
    
    // Bind the socket to port 30000
    bind_to_port(listener_d, 30000);
    
    // Start listening for incoming connections
    if(listen(listener_d, 10) == -1){
        error("Can't listen");
    }
    
    // Storage for client address
    struct sockaddr_storage client_addr;
    // Size of the address
    unsigned int address_size = sizeof(client_addr);
    puts("Waiting for connection");
    
    char buf[255]; // Buffer to store receive data
    
    // Infinite loop to accept and handle incoming connections
    while(1){
        // Accept a new connection
        int connect_d = accept(listener_d, (struct sockaddr*)&client_addr, &address_size);
        if(connect_d == -1){
            error("Can't open secondary socket");
        }
        
        // Send a welcome message to the client
        if(say(connect_d, "Internet Knock-Knock Protocol Server\r\nVersion 1.0\r\nKnock!Knock!\r\n>") != -1)
        {
            // Read client's response
            read_in(connect_d, buf, sizeof(buf));
            // Check if the client responded with "Who's there?"
            // Checking if the return value is not zero which means the strings aren't equal.
            // non-zero = true so it's like saying if(true) if strings aren't equal
            // zero = false which means strings are equal. So the condition becomes false and we proceed to else block.
            if(strncasecmp("Who's there?", buf, 12)){
                // If not, prompt the correct response
                say(connect_d, "You should say 'Who's there?!");
            }else{
                // If correct, proceed with the joke
                if(say(connect_d, "Oscar\r\n") != -1){
                    // Read client's next response
                    read_in(connect_d, buf, sizeof(buf));
                    // Check if the client responded with "Oscar who?"
                    if(strncasecmp("Oscar who?", buf, 10)){
                        // If not, prompt the correct response
                        say(connect_d, "You should say 'Oscar who?'!\r\n");
                    }else{
                        // Complete the joke
                        say(connect_d, "Oscar silly question, you get a silly answer\r\n");
                    }
                }
            }
            // Close the connection to the client
            close(connect_d);
        }
    }
    return 0;
}
```

Code Execution:

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Waiting for connection
^CBye!
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Waiting for connection


```

On the second console

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Internet Knock-Knock Protocol Server
Version1.0
Knock!Knock!
>Who's there?
Oscar
> Oscar who?
Oscar silly question, you get a silly answer
Connection closed by foreign host.

chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Internet Knock-Knock Protocol Server
Version1.0
Knock!Knock!
>Come in
You should say 'Who's there?'!Connection closed by foreign host.

```

- If we break the protocol and send back in invalid response like the second one "Come in", the server is able to validate the data we send and close the connection immediately.

Code Explanation (`main.c`)

**Signal Handling**:

```C
if (catch_signal(SIGINT, handle_shutdown) == -1)
{
    error("Can't set the interrupt handler");
}
```

- Sets up a signal handler for `SIGINT` (interrupt signal, typically sent when the user presses Ctrl+C). If it fails, it calls the `error` function.

**Open Listening Socket**:

```C
listener_d = open_listener_socket();
```

- Opens a socket for listening to incoming connections.

**Bind to Port**:

```C
bind_to_port(listener_d, 30000);
```

- Binds the opened socket to port 30000.

**Start Listening**:

```C
if (listen(listener_d, 10) == -1)
{
    error("Can't listen");
}
```

- Starts listening on the bound socket with a backlog of 10 connections.

**Client Address Storage**:

```C
struct sockaddr_storage client_addr;
unsigned int address_size = sizeof(client_addr);
puts("Waiting for connection");
char buf[255];
```

- Prepares a structure to store client address information, gets the size of this structure, prints a waiting message, and allocates a buffer for reading data.

**Infinite Loop for Accepting Connections**:

```C
while (1)
{
```

- Starts an infinite loop to continuously accept and handle incoming connections.

**Accept Connection**:

```C
int connect_d = accept(listener_d, (struct sockaddr *)&client_addr, &address_size);
if (connect_d == -1)
{
    error("Can't open secondary socket");
}
```

- Accepts a new connection. If it fails, calls the `error` function.

**Send Welcome Message**:

```C
if (say(connect_d, "Internet Knock-Knock Protocol Server\r\nVersion1.0\r\nKnock!Knock!\r\n>") != -1)
{
```

- Sends a welcome message to the connected client. If it succeeds, proceeds to the next steps.

**Read Client's Response**:

```C
read_in(connect_d, buf, sizeof(buf));
if (strncasecmp("Who's there?", buf, 12))
{
    say(connect_d, "You should say 'Who's there?'!");
}
```

- Reads the client's response into `buf`. If the response is not "Who's there?" (case-insensitive comparison), sends a prompt for the correct response.

**Proceed with the Joke**:

```C
else
{
    if (say(connect_d, "Oscar\r\n> ") != -1)
    {
        read_in(connect_d, buf, sizeof(buf));
        if (strncasecmp("Oscar who?", buf, 10))
        {
            say(connect_d, "You should say 'Oscar who?'!\r\n");
        }
        else
        {
            say(connect_d, "Oscar silly question, you get a silly answer\r\n");
        }
    }
}
```

- If the client responds correctly with "Who's there?", proceeds to the next part of the joke. If the client responds correctly with "Oscar who?", completes the joke.

**Close Connection**:

```C
close(connect_d);
```

- Closes the connection to the client after handling the interaction.

Right now, our servers can only talk to one person at a time.

first console:

```sh
chan@CMA:~/C_Programming/test$ ./final
Waiting for connection


```

second console is accessed by one user.

```sh
chan@CMA:~/C_Programming/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Internet Knock-Knock Protocol Server
Version 1.0
Knock!Knock!
>

```

If someone else tries to get through to the server, he can't; it's busy with the first user:

```sh
chan@CMA:~/C_Programming/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.

```

- The server is still busy talking to the first guy.
- The main server socket will keep the client waiting until the server calls the `accept()` system call again.

But

#### We can fork() a process for each client

- When the clients connect to the server, they start to have conversation on a separate, newly created socket.
- That means the main server socket is free to go and find another client.
- When a client connects, we can `fork()` a separate child process to deal with the conversation between the server and the client.
- While the client is talking to the child process, the server's parent process can go connect to the next client.



#### The parent and child use different sockets

- One thing to bear in mind is that the parent server process will only need to use the main listener socket.
- That's because the main listener socket is the one that's used to `accept()` new connections.
- On the other hand, the child process will only ever need to deal with the secondary socket that gets created by the `accept()` call. 
- That means once the parent has forked the child, the parent can close the secondary socket and the child can close the main listener socket.

```C
// After forking the child, the parent can close this socket
close(connect_d);

// Once the child gets created, it can close this socket.
close(listener_d);
```

Q: If I create a new process for each client, what happens if hundreds of clients connect? Will my machine create hundreds of processes?

A: Yes. If we think our server will get a lot of clients, we need to control how many processes we create. The child can signal us when it's finished with a client and we can use that to maintain a count of current child processes.



#### Modified Code for our Exercise (`main.c`)

```C
while(1){
    int connect_d = accpet(listener_d, (struct sockaddr*)&client_addr, &address_size);
    if(connect_d == -1){
        error("Can't open secondary socket");
    }
    
    // This creates the child process and we know that if the fork() call returns 0, we must be in the child.
    if(!fork()){
        
        // In the child, we need to close the main listener socket.
        // The child will use only the connect_d socket to talk to the client
        close(listener_d);
        if(say(connect_d, "Internet Knock-Knock Protocol Server\r\nVersion 1.0\r\nKnock! Knock!\r\n>") != -1){
            read_in(connect_d, buf, sizeof(buf));
            if(strncasecmp("Who's there?", buf, 12)){
                say(connect_d, "You should say 'Who's there?!");
            }else{
                if(say(connect_d, "Oscar\r\n> ") != -1){
                    read_in(connect_d, buf, sizeof(buf));
                    if(strncasecmp("Oscar who?", buf, 10)){
                        say(connect_d, "You should say 'Oscar who?'!\r\n");
                    }else{
                        say(connect_d, "Oscar silly question, you get a silly answer\r\n");
                    }
                }
            }
        }
        // Once the conversation is over, the child can close the socket to the client.
        close(connect_d);
        
        // Once the child has finished talking, it should exit. That will prevent it from falling into the main server loop.
        exit(0);
    }
    close(connect_d);
}
return 0;
```



Code Execution:

```sh
chan@CMA:~/C_Programming/test$ ps -ef
...
chan       10928    3699  0 22:40 pts/0    00:00:00 ./final
chan       10931    9619  0 22:40 pts/1    00:00:00 telnet 127.0.0.1 30000
chan       10932   10928  0 22:40 pts/0    00:00:00 ./final

```

```sh
  10928 pts/0    00:00:00 final
  10931 pts/1    00:00:00 telnet
  10932 pts/0    00:00:00 final
  11640 ?        00:00:00 chrome
  11659 ?        00:00:00 kworker/10:0H-kblockd
  11660 ?        00:00:00 kworker/u32:0-events_power_efficient
  11746 pts/2    00:00:00 telnet
  11747 pts/0    00:00:00 final

```



- There are now two processes for the server: one for the parent and one for the child.
- That means we can connect, even while the first client is still talking to the server.

Now running our program

first console:

```sh
chan@CMA:~/C_Programming/test
$ make all
Compiling the main file
clang -Wall -Wextra -g -c main.c 
Linking and producing the final application
clang -Wall -Wextra -g main.o hello.o -o final

chan@CMA:~/C_Programming/test
$ ./final
Waiting for connection

```



second console:

```sh
chan@CMA:~/C_Programming/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Internet Knock-Knock Protocol Server
Version 1.0
Knock!Knock!
>Who's there?
Oscar

```



third console:

```sh
chan@CMA:~/C_Programming/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Internet Knock-Knock Protocol Server
Version 1.0
Knock!Knock!
>Who's there?

```



#### Chapter 11 - Last Exercise

`functions.c`

```C
#include <string.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netdb.h>
#include <unistd.h>

// Error handling function to print an error message and exit
void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(0);
}

// Function tto set up a signal handler for a given signal
int catch_signal(int sig, void (*handler)(int)){
    // Declare a sigaction struct to specify the signal handling action
    struct sigaction action;
    // Set the handler function for the signal
    action.sa_handler = handler;
    // Initialize the signal mask to empty
    sigemptyset(&action.sa_mask);
    // No special flags
    action.sa_flags = 0;
    // Set the action for the specified signal
    return sigaction(sig, &action, NULL);
}

// Function to open a listener socket
int open_listener_socket(){
    // Create a socket using the IPv4 protocol family and stream socket type
    int s = socket(PF_INET, SOCK_STREAM, 0);
    
    // Check if socket creation failed.
    if(s == -1){
        error("Can't open socket");
    }
    // return the socket descriptor
    return s;
}

// Function to open a socket and connect to a specified host and port
int open_socket(char *host, char *port){
    struct addrinfo *res;
    struct addrinfo hints;
    
    // zero out the hints struct
    memset(&hints, 0, sizeof(hints));
    
    // Specify the address family as unspecified (both IPv4 and IPv6)
    hints.ai_family = PF_UNSPEC;
    
    // Specify the socket type as a stream socket (TCP)
    hints.ai_socktype = SOCK_STREAM;
    
    // Get address information for the specified host and port
    if(getaddrinfo(host, port, &hints, &res) == -1){
        error("Can't resolve the address.");
    }
    
    // Create a socket with the obtained address information
    int d_sock = socket(res->ai_family, res->ai_socktype, res->ai_protocol);
    
    // Check if socket creation was successful
    if(d_sock == -1){
        error("Can't open socket");
    }
    
    // Connect the socket to the address obtained
    int c = connect(d_sock, res->ai_addr, res->ai_addrlen);
    
    // Free the address information allocated by getaddrinfo()
    freeaddrinfo(res);
    if(c == -1){
        error("Can't connect to socket");
    }
    
    // Return the connected socket descriptor
    return d_sock;
}

// Function to bind a socket to a specified port
void bind_to_port(int socket, int port){
    // Create a structure to hold the address
    struct sockaddr_in name;
    // Use the IPv4 protocol family
    name.sin_family = PF_INET;
    // Convert the port to network byte order
    name.sin_port = (in_port_t)htons(port);
    // Bind to any available network interface
    name.sin_addr.s_addr = htonl(INADDR_ANY);
    
    // Allow reuse of local addresses
    // Useful for avoiding "Address already in use" error
    int reuse = 1;
    if(setsockopt(socket, SOL_SOCKET, SO_REUSEADDR, (void *)&reuse, sizeof(int)) == -1){
        error("Can't set the reuse option on the socket.");
    }
    
    // Bind the socket to the address and port
    int c = bind(socket, (struct sockaddr *)&name, sizeof(name));
    if(c == -1){
        error("Can't bind to socket");
    }
}

// Function to send a message through a socket
int say(int socket, char *s){
    // Send the message
    int result = send(socket, s, strlen(s), 0);
    if(result == -1){
        fprintf(stderr, "%s: %s\n", "Error talking to the client", strerror(errno));
    }
    // Return the result of send
    return result;
}

// Function to read a message from a socket
int read_in(int socket, char *buf, int len){
    // Initialize pointers and length for receiving data
    char *s = buf;
    int slen = len;
    // Receive data from the socket
    int c = recv(socket, s, slen, 0);
    // Loop until we receive a newline character or encounter an error
    while((c > 0) && (s[c-1] != '\n')){
        s += c;
        slen -= c;
        c = recv(socket, s, slen, 0);
    }
    
    // Check if there was an error receiving data
    if(c < 0){
        return c;
    }else if(c == 0){
        buf[0] = '\0';
    }else{
        s[c-1] = '\0';
    }
    
    // Return the number of bytes read
    return len - slen;
}

// Global variable to hold the listener socket descriptor
int listener_d;

// Function to handle shutdown signals
void handle_shutdown(){
    // Check if the listener socket is open
    if(listener_d){
        // if so, close the listener socket
        close(listener_d);
    }
    // print a shutdown message
    fprintf(stderr, "Bye!\n");
    // exit the program with status 0.
    exit(0);
}
```

`main.c`

```C
int main(int argc, char *argv[]){
    (void argc); // Explicitly mark argc as unused to avoid compiler warnings
    
    // Declare a variable to hold the socket descriptor
    int d_sock;
    
    // Open a socket and connect to the specified host and port
    d_sock = open_socket("en.wikipedia.org", "80");
    
    // Format the GET request string and store it in buf
    sprintf(buf, "GET /wiki/O'Reilly_Media HTTP/1.1\r\n");
    
    // Send the GET request to the server
    say(d_sock, buf);
    
    // Send the Host header to the server
    say(d_sock, "Host: en.wikipedia.org\r\n");
    
    // Buffer to hold the received data
    char rec[256];
    
    // Receive data from the server
    int bytesRcvd = recv(d_sock, rec, 255, 0);
    
    // Loop to receive all data from the server
    while(bytesRcvd){
        // Check if receiving data failed
        if(bytesRcvd == -1){
            error("Can't read from the server");
        }
        
        // Null-terminate the received data
        rec[bytesRcvd] = '\0';
        
        // Print the received data
        printf("%s", rec);
        
        // Receive more data
        bytesRcvd = recv(d_sock, rec, 255, 0);
    }
    // Close the socket
    close(d_sock);
    return 0;
}
```



Code Execution:

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
HTTP/1.1 301 Moved Permanently
content-length: 0
location: https://en.wikipedia.org/wiki/O'Reilly_Media
server: HAProxy
x-cache: cp5022 int
x-cache-status: int-tls
connection: close


```

Since, the page has been permanently moved to another location and http has been changed and upgraded to https, our fetch was successful but we need to use another approach for fetching `https` sites.

