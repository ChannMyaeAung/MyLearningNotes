### Chapter 11 - Sockets and Networking

#### What is a socket?

- A socket is an endpoint for sending and receiving data across a network.
- It's a fundamental concept in network programming that provides a way for software to communicate over a network (e.g., Internet or a local network).
- Sockets allow for establishing a connection between two machines or processes to exchange data.
- They are used by various protocols (TCP, UDP, etc.) to facilitate communication between applications running on different hosts.
- In essence, a socket binds an IP address and a port number together to create a unique identifier for the connection, enabling the transmission of data between endpoints.



#### The Internet knock-knock server

- C is used to write most of the low-level networking code on the Internet.
- Most networked applications need two separate programs: a **server** and a **client**.
- Other than telling us it's running, the server won't display anything else on the screen.
- If we open a second console, we'll be able to connect to the server using a client program called **telnet**.
- Telnet takes two parameters: the **address** of the server and the **port** the server is running on.
- If we are running telnet on the same machine as the server, we can use **127.0.0.1** for the address like this:

```sh
telnet 127.0.0.1 30000
```

- 30000 is the number of the network port.



#### Knock-knock server overview

- The server will be able to talk to several clients at once.
- The client and the sever will have a structured conversation called a **protocol**.
- There are different protocols used on the Internet. Some of them are low-level protocols, like the **internet protocol(IP)** which are used to control how many 1s and 0s are sent around the Internet. Other protocols are high-level protocols, like the **hypertext transfer protocol(HTTP)** which controls how web browsers talk to web severs.
- The joke server is going  to use a custom high-level protocol called the **Internet knock-knock protocol(IKKP)**.
- A **protocol** always has a strict set of rules. As long as the client and the server both follow those rules, everything is fine.
- But if one of them breaks the rules, the conversation usually stops pretty abruptly.



#### BLAB: how severs talk to the Internet

- When C programs need to talk to the outside world, they use **data streams** to read and write bytes.
- If we're going to write a program talk to the network, we need a new kind of data stream called a **socket**.

```C
#include <sys/socket.h> // We'll need this header.

int listener_d = socket(PF_INET, SOCK_STREM, 0); // Create a new IPv4 TCP socket

if(listener_d == -1) // Check if socket creation failed
    error("Can't open socket!");

```

- `int listener_d`: Declares a variable `listener_d` of type `int`. This variable will store the file descriptor of the socket.
- `socket(PF_INET, SOCK_STREAM, 0)`: This function call creates a new socket.
  - `PF_INET`: Specifies the protocol family to be used, which is IPv4 in this case. `PF_INET` stands for "Protocol Family Internet".
  - `SOCK_STREAM`: Specifies the type of socket to be created. `SOCK_STREAM` indicates that this will be a stream socket, which is typically used for TCP (Transmission Control Protocol) communication.
  - `0`: Specifies the protocol to be used. When `0` is passed, it means the default protocol for the specified socket type will be used, which is TCP for `SOCK_STREAM`.

If the socket is successfully created, `socket()` returns a file descriptor (an integer) that can be used to refer to the socket in subsequent operations. If it fails, it returns `-1`.



- Before a server can use a socket to talk to a client program, it needs to go through four stages that we can remember with the acronym **BLAB** : **Bind, Listen, Accept, Begin**.
- Bind to a port. Listen. Accept a connection. Begin talking.

#### 1. Bind to a port

- A computer might need to run several programs at once.
- It might be sending out web pages, posting email, and running a chat server all at the same time.
- To prevent the different conversations from getting confused, each server uses a different **port**.
- A port is just like a channel on a TV.
- Different ports are used for different network services, just like different channels are used for different content.

![](/home/chan/Pictures/Screenshots/Screenshot from 2024-07-09 23-04-46.png)

- When a server starts up, it needs to tell the OS which port it's going to use. This is called **binding the port**.
- The knock-knock server is going to use port 30000 and to bind it we'll need two things: the **socket descriptor** and a **socket name**.
- A socket name is just a `struct` that means "Internet port 30000".

```C
#include <arpa/inet.h> // We'll need this header for creating Internet addresses.

...
struct sockaddr_in name;
name.sin_family = PF_INET;
name.sin_port = (in_port_t)htons(30000);
name.sin_addr.s_addr = htonl(INADDR_ANY);
int c = bind (listener_d, (struct socketaddr *) &name, sizeof(name));
if(c == -1)
    error("Can't bind to socket");
```

- `struct sockaddr_in name;`: Declares a variable `name` of type `struct sockaddr_in`. This structure is used to specify the address to which the socket will be bound.
- `name.sin_family = PF_INET;`: Sets the address family to `PF_INET`, which stands for IPv4.
- `name.sin_port = (in_port_t)htons(30000);`: Sets the port number to `30000`. The `htons` function converts the port number to network byte order (big-endian), which is necessary for network communication.
- `name.sin_addr.s_addr = htonl(INADDR_ANY);`: Sets the IP address to `INADDR_ANY`, which means the socket will be bound to all available network interfaces on the machine. The `htonl` function converts the IP address to network byte order.

**Binding the Socket to the Address:**

```C
int c = bind(listener_d, (struct sockaddr *) &name, sizeof(name));
if(c == -1)
    error("Can't bind to socket");
```

- `int c = bind(listener_d, (struct sockaddr *) &name, sizeof(name));`: This function call binds the socket `listener_d`to the address specified by the `name`structure.
  - `listener_d`: The file descriptor of the socket to be bound.
  - `(struct sockaddr *) &name`: A pointer to the `sockaddr_in` structure cast to a pointer of type `struct sockaddr`.
  - `sizeof(name)`: The size of the `sockaddr_in` structure.

If the `bind` function is successful, it returns `0`. If it fails, it returns `-1`.


#### 2. Listen

- If our server becomes popular, we'll probably get lots of clients connecting to it at once.
- The `listen()` system call tells the OS how long we want the queue to be:

```C
if(listen(listener_d, 10) == -1){
    error("Can't listen");
}
```

- Calling `listen()` with a queue length of 10 means that up to 10 clients can try to connect to the server at once.
- They won't be immediately answered, but they'll be able to wait.
- The 11th and 12th clients will be told the server is too busy.

#### 3. Accept a connection

- Once we've bound a port and set up a listen queue, we then just have to wait.
- Servers spend most of their lives waiting for clients to contact them.
- The `accept()` system call waits until a client contacts the server and then it returns a **second socket descriptor** that we can use to hold a conversation on.

```C
struct sockaddr_storage client_addr;
unsigned int address_size = sizeof(client_addr);
int connect_d = accept(listener_d, (struct sockaddr *)&client_addr, &address_size);

if(connect_d == -1){
    error("Can't open secondary socket");
}
```



This new **connection descriptor** (connect_d) is the one the the server will use to...

#### 4. Begin talking



#### Some Essentials Networking Concepts

**`sockaddr_in` vs. `sockaddr_storage`**:

- `sockaddr_in`: Used specifically for IPv4 addresses. Contains fields for address family (`sin_family`), port (`sin_port`), and IP address (`sin_addr`).
- `sockaddr_storage`: A generic structure that is large enough to hold either IPv4 or IPv6 address structures. Used for protocol-independent code.

**Fields in `sockaddr_in`**:

- `sin_family`: Specifies the address family (e.g., `PF_INET` for IPv4).
- `sin_port`: The port number, which must be in network byte order.
- `sin_addr`: A structure containing a single field `s_addr` that holds the IP address in network byte order.

**Byte Order Conversion**:

- `htons` (Host to Network Short): Converts a 16-bit (short) value from host byte order to network byte order.
- `htonl` (Host to Network Long): Converts a 32-bit (long) value from host byte order to network byte order.
- Network byte order is big-endian, which ensures consistent interpretation of multi-byte values across different platforms.

**Type Casting with `in_port_t`**:

- `in_port_t` is a type defined for port numbers. Casting to `in_port_t` ensures the port number is of the correct type for network operations.

#### A socket's not our typical data stream

- Data streams have all been the same.
- Whether we're connected to files or Standard I/O, we've been able to use functions like `fprintf()` and `fscanf()` to talk to them.
- Sockets are a little different. 
- A socket is **two way**: it can be used for input and output which means it needs different functions to talk to it.
- If we want to output data on a socket, we can't use `fprintf()`. Instead, we use a function called `send()`.

```C
// This is the message we're going to send over the network.
char *msg = "Internet Knock-Knock Protocol Server\r\nVersion 1.0\r\nKnock! Knock!\r\n>";


if(send(connect_d, msg, strlen(msg),0) == -1){
    error("Send");
}

// connect_d = the socket descriptor
// msg = message
// strlen(msg) = message's length
// 0 = The final parameter is used for advanced options. This can be left as 0.
```

- It's important to always check the return value of system calls like `send()`. Network errors are really common and our servers will have to cope with them.

#### Which port should we use?

- There are lots of different servers available and we need to make sure we don't use a port number that's normally used for some other program.
- On Cygwin and most Unix-style machies, we'll find a file called `/etc/services/` that lists the ports used by most of the common servers.
- When choosing a port, make sure there isn't another application that already uses the same one.
- Port numbers can be between **0 and 65535** and we need to decide whether we want to use a low number(< 1024) or a high one.
- Port numbers that are lower than 1024 are usually only available to the superuser or administrator on most systems.
- This is because the low port numbers are reserved for well-known services, like web servers and mail servers.
- Most of the time, we'll probably want to use a port number greater than 1024.



#### Reading from the client

- In the same way the sockets have a special `send()` function to write data, they also have a `recv()` function to read data.

```C
<bytes read> = recv(<descriptor>, <buffer>, <bytes to read>, 0);
```

- If someone types in a line of text into a client and hits return, the `recv()` function stores the text into a character array like this:

![](/home/chan/Pictures/Screenshots/Screenshot from 2024-07-12 22-18-35.png)

- `recv()` will return the value 14, because there are 14 characters sent from the client.

Things to remember:

- The characters are not terminated with a `\0` character.
- When someone types text in telnet, the string always ends `\r\n`.
- The `recv()` will return the number of characters, or -1 if there's an error, or 0 if the client has closed the connection.
- We're not guaranteed to receive all the characters in a single call to `recv()` which means we might have to call `recv()` more than once.
- We might need to call `recv()` a few times to get all the characters.
- `recv()` can be tricky to use. It's best to wrap `recv()` in a function that stores a simple `\0-terminated` string in the array it's given.

```C
int read_in(int socket, char *buf, int len) // This reads all the characters until it reaches '\n'
{
    // Initialize a pointer 's' to point to the buffer 'buf'
    char *s = buf;
    
    // Store the original length of the buffer in 'slen'
    int slen = len;
    
    // recv reads data from the socket into the buffer 's' with max length 'slen'
    int c = recv(socket, s, slen, 0);
    // Keep reading until there're no more characters or reach '\n'
    while((c > 0) && (s[c-1] != '\n')){
        
        // Move the pointer 's' forward by 'c' characters
        s += c; 
        
        // Decrease the remaining lenght by 'c'
        slen -= c;
        
        // Read more data from the socket
        // Read more data into the remaining part of the buffer
        c = recv(socket, s, slen, 0);
    }
    if(c < 0)
        return c; // In case there's an error, return the error code 'c'
    else if (c == 0)
        buf[0] = '\0'; // Nothing read; send back an empty string (set the first character of the buffer to '\0' to indicate an empty string)
    else
        s[c - 1] = '\0'; // Replace the last character read ('\r' character) with a '\0' to terminate the string
    
    // Return the number of characters read (initial length - remaining length)
    return len - slen;
}
```

1. **Function Signature**:

   ```C
   int read_in(int socket, char *buf, int len)
   ```

   - The function `read_in` takes three arguments: a socket descriptor (`socket`), a buffer to store the read data (`buf`), and the length of the buffer (`len`).

2. **Pointer and Length Initialization**:

   ```C
   char *s = buf; 
   int slen = len;
   ```

   - `s` is a pointer initialized to the start of `buf`.
   - `slen` is initialized to the length of the buffer (`len`).

3. **Initial Data Read**:

   ```C
   int c = recv(socket, s, slen, 0);
   ```

   - `recv` reads data from the socket into the buffer `s` up to `slen` bytes. The return value `c` is **the number of bytes read** or **a negative value** if an error occurs.

4. **Loop to Continue Reading Until Newline**:

   ```C
   while ((c > 0) && (s[c-1] != '\n')) {
       s += c; 
       slen -= c;
       c = recv(socket, s, slen, 0);
   }
   ```

   - The loop continues reading as long as there are more characters (`c > 0`) and the last character read is not a newline (`s[c-1] != '\n`).
   - Inside the loop:
     - `s += c` moves the pointer `s` forward by `c` characters.
     - `slen -= c` decreases the remaining buffer length by `c`.
     - `recv` reads more data into the remaining part of the buffer.

5. **Error Handling and String Termination**:

   ```C
   if (c < 0)
       return c;
   else if (c == 0)
       buf[0] = '\0';
   else
       s[c - 1] = '\0';
   ```

   - If `c < 0`, an error occurred, and the function returns the error code `c`.
   - If `c == 0`, no data was read (EOF), so the buffer is set to an empty string.
   - Otherwise, the last character read is replaced with `\0` to terminate the string.

6. **Return Number of Characters Read**:

   ```C
   return len - slen;
   ```

   - The function returns the number of characters read, calculated as the initial length (`len`) minus the remaining length (`slen`).

This function ensures that the buffer contains a complete line of input terminated by `\0`, and handles cases where data is read in chunks, not all at once.



#### Writing a web client

- All protocols are structured conversations.
- Most web servers run on port 80.
- telnet doesn't support HTTPS.
- We can use a tool called `curl` instead of telnet.

GET https:///wiki/O'Reilly_Media HTTP/1.1

Host: en.wikipedia.org

```sh
curl -L "https://en.wikipedia.org/wiki/O'Reilly_Media"

```

```sh
chan@CMA:~$ curl -L "https://en.wikipedia.org/wiki/O'Reilly_Media"
<!DOCTYPE html>
<html class="client-nojs vector-feature-language-in-header-enabled vector-feature-language-in-main-page-header-disabled vector-feature-sticky-header-disabled vector-feature-page-tools-pinned-disabled vector-feature-toc-pinned-clientpref-1 vector-feature-main-menu-pinned-disabled vector-feature-limited-width-clientpref-1 vector-feature-limited-width-content-enabled vector-feature-custom-font-size-clientpref-1 vector-feature-appearance-enabled vector-feature-appearance-pinned-clientpref-1 vector-feature-night-mode-disabled skin-theme-clientpref-day vector-toc-available" lang="en" dir="ltr">
<head>
<meta charset="UTF-8">
<title>O'Reilly Media - Wikipedia</title>
<script>(function(){var className="client-js vector-feature-language-in-header-enabled vector-feature-language-in-main-page-header-disabled vector-feature-sticky-header-disabled vector-feature-page-tools-pinned-disabled vector-feature-toc-pinned-cli
```

- We'll get a whole bunch of script files of that page it seems.

#### Clients are in charge

- Clients and servers communicate using sockets.
- A sever spends most of its life waiting for a fresh connection from a client.
- Until a client connects, a server really can't do anything.
- Clients don't have that problem. 
- A client can connect and start talking to a server whenever it likes.

Below is the sequence for a client:

1. Connect to a remote port.
2. Begin talking.

Below is the sequence of servers (BLAB):

1. Bind a port
2. Listen
3. Accept a conversation
4. Begin talking

#### Remote ports and IP addresses

- When a server connects to a network, it just has to decide which port it's going to use.
- But clients need to know a little more: they need to know **the port of the remote server**, but they also need to know its **internet protocol(IP) address:**

```sh
208.201.239.100
```

- Addresses with 4 digits are in IP version 4 format. Most will eventually be replaced with longer version  6 addresses.
- Internet addresses are kind of hard to remember which is why most of the time human beings use **domain names**.
- A domain name is just an easier-to-remember piece of text like `www.oreilly.com`.
- Even though we prefer domain names, the actual packets of information that flow across the network only use  the numeric IP address.

#### Create a socket for an IP address

- Once our client knows the address and port number of the server, it can create a **client socket**.
- Client sockets and server sockets are created the same way.

```C
int s = socket(PF_INET, SOCK_STREAM, 0);
```

- The difference between client and server code is **what they do with sockets** once they're created.
- A server will **bind** the socket to a local port, but a client will **connect** the socket to a remote port.

```C
struct sockaddr_in si;
memset(&sii, 0, sizeof(si));
si.sin_family = PF_INET;
si.sin_addr.s_addr = inet_addr("208.201.239.100");
si.sin_port = htons(80);
// The above lines create a socket address for 208.201.239.100 on port 80.


// This line connects the socket to the remote port.
connect(s, (struct sockaddr *)&si, sizeof(si));
```

- The above code works only for **numeric IP addresses.**
- To connect a socket to a remote domain name, we'll need a function called `getaddrinfo()`.

#### `getaddrinfo()` gets addresses for domains

- The domain name system (DNS) is a huge address book.
- It's a way of converting a domain name like `www.oreilly.com` into the kinds of numeric IP addresses that computers need to address the packets of information they send across the network.

#### Create a socket for a domain name

- Most of the time, we'll want our client code to use the DNS system to create sockets.
- That way, our users won't have to look up the IP addresses themselves.
- To use DNS, we need to construct our client sockets in a slightly different way:

```C
#include <netdb.h>
// We need to include this header for the getaddrinfo() function.

...
    
// Pointer to a linked list of addrinfo struct
struct addrinfo *res;
// Hints structure to specify criteria for selecting socket addresses
struct addrinfo hints;

// Initialize the hints struct to zero
memset(&hints, 0, sizeof(hints));

// Allow IPv4 or IPv6 (PF_UNSPEC means unsepcified)
hints.ai_family = PF_UNSPEC;

// Specify a stream socket (TCP)
hints.ai_socktype = SOCK_STREAM;


// creates a name resource for port 80 on www.oreilly.com
// getaddrinfo() expects the port to be a string ("80")
getaddrinfo("www.oreilly.com", "80", &hints, &res);

// The function getaddrinfo() converts human-readable text strings (hostname and service name) into a dynamically allocated linked list of struct addrinfo structures, suitable for use in subsequent calls to socket() and connect(). 
// The hints argument specifies criteria for selecting the socket address, and res is a pointer to the resu

```

Here's a breakdown of what each part does:

1. **Header Inclusion:**

   ```C
   #include <netdb.h>
   ```

   This header is necessary because it contains the definition for the `getaddrinfo()` function and related structures.

2. **Variable Declarations:**

   ```C
   struct addrinfo *res;
   struct addrinfo hints;
   ```

   - `res`: A pointer to a linked list of `addrinfo` structures. This will store the results returned by `getaddrinfo()`.
   - `hints`: A structure that specifies criteria for selecting the socket addresses returned in the `res` list.

3. **Initializing the `hints` Structure:**

   ```C
   memset(&hints, 0, sizeof(hints));
   ```

   - This line sets all the fields of the `hints` structure to zero, ensuring no garbage values are present. 
   - `memset` stands for "memory set". It's a function in C and C++ used to fill a block of memory with a particular value.

   ```C
   void *memset(void *s, int c, size_t n);
   
   // s - pointer to the block of memory to fill.
   // c - value to be set. The value is passed as int, but the function filles the block of memory using the unsigned char conversion of this value.
   // n - Number of bytes to be set to the value.
   // memset returns a pointer to the memory area s.
   ```

   ```C
   int main(){
       char array[10];
       // Initialize all elements of array to 'A'
       memset(array, 'A', sizeof(array));
       return 0;
   }
   ```

   ```sh
   chan@CMA:~/C_Programming/practice
   $ ./practice
   AAAAAAAAAA
   ```

   - In this example, `memset` is used to fill the entire `array` with the character `A`.

4. **Setting `hints` Criteria:**

   ```C
   hints.ai_family = PF_UNSPEC;
   hints.ai_socktype = SOCK_STREAM;
   ```

   - `hints.ai_family = PF_UNSPEC`: This allows the function to return socket addresses that can be used with either IPv4 or IPv6.
   - `hints.ai_socktype = SOCK_STREAM`: This specifies that the socket type should be a stream socket (TCP).
   - `ai` stands for "address info".

5. **Calling `getaddrinfo()`:**

   ```C
   getaddrinfo("www.oreilly.com", "80", &hints, &res);
   ```

   This function call translates the hostname (`"www.oreilly.com"`) and the service name (`"80"`, which is the HTTP port) into a linked list of address structures. The `hints` argument provides criteria for the kinds of addresses the caller is interested in, and the `res` argument is set to point to the resulting list of address structures.

   - The hostname (`"www.oreilly.com"`) is the address we want to look up.
   - The service name (`"80"`) is the port number we want to connect to, specified as a string.
   - `&hints` is a pointer to the hints structure that provides criteria for selecting the returned addresses.
   - `&res` is a pointer to the linked list of results.

This setup is typically used in network programming to prepare for creating and connecting sockets using the returned address information.



- The `getaddrinfo()` constructs a new data structure on the **heap** called a naming resource.
- The naming resource represents a port on a server with a given domain name.
- Hidden away inside the naming resource is the IP address that the computer will need.
- Sometimes very large domains can have several IP addresses, but the code here will simply pick one of them.
- We can then use the naming resource to create a socket.

```C
int s = socket(res->ai_family, res->ai_socktype, res->ai_protocol);
```

- Finally we can connect to the remote socket.
- Because the naming resource was created on the heap, we'll need to tidy it away with a function called `freeaddrinfo()`.

```C
// This will connect to the remote socket.
connect(s, res->ai_addr, res->ai_addrlen);

// When we've connected, we can delete the address data with freeaddrinfo().
freeaddrinfo(res);

// res->ai_addr is the addr of the remote host and port
// res->ai_addrlen is the size of the address in memory.
```

- Once we have connected a socket to a remote port, we can read and write to it using the same `recv()` and `send()` functions we used for the server.
- `recv()` stands for "receive". It is a function used in socket programming to receive data from a socket. This function is commonly used in network applications to read incoming data from remote hosts over a TCP/IP network.
- The exercise is in `HFCExercises.md`.



### Questions & Answers

Q: Should I create sockets with IP addresses or domain names?

A: Most of the time, we'll want to use domain names. They're easier to remember and occasionally some servers will change their numeric addresses but keep the same domain names.



Q: So, do I even need to know how to connect to a numeric address?

A: Yes. If the server we're connecting to is not registered in the domain name system, such as machines on our home network, then we will need to know how to connect by IP.



Q: Can I use `getaddrinfo()` with a numeric address?

A: Yes, we can. But if we know that the address we are using is a numeric IP, the first version of the client socket code is simpler.



### Bullet Points - Chapter 11

- A protocol is a structured conversation.
- Servers connect to local ports.
- Clients connect to remote ports.
- Clients and Servers both use sockets to communicate.
- We write data to a socket with `send()`.
- We read data from a socket with `recv()`.
- HTTP is the protocol used on the Web.
- To fetch HTTPS pages, we need to use a library that supports SSL/TLS because HTTPS is HTTP over TLS (Transport Layer Security). One commonly used library for this in C is `OpenSSL`.
- Telnet is a simple network client.
- Servers `BLAB`: bind(), listen(), accept() and begin talking.
- DNS = Domain name system
- Use `fork()` to cope with several clients at once.
- Create sockets with the `socket()` function.
- `getaddrinfo()` finds addresses by domain.