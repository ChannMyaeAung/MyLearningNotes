### Fetching HTTPS Pages 

To fetch HTTPs pages,

- We need to use a library that supports SSL/TL because HTTPS is HTTP over TLS(Transport Layer Security).
- One commonly used library for this in C is OpenSSL.
- Here is how we can modify our code to use OpenSSL for fetching HTTPS pages.



First we need to install the OpenSSL library if it is not already installed.

```sh
sudo apt-get install libssl-dev
```



#### Exercises (Fetching HTTPS Pages)

`main.c`

```C
...
#include <openssl/ssl.h>
#include <openssl/err.h>
int main(int argc, char *argv[])
{
    (void)argc;

    // Initialize SSL context
    SSL_CTX *ctx = init_ssl_context();

    // Open socket and connect to server
    int d_sock = open_socket("en.wikipedia.org", "443");

    // Connect SSL over the socket
    SSL *ssl = connect_ssl(d_sock, ctx);

    // Send request
    char buf[255];
    snprintf(buf, sizeof(buf), "GET /wiki/O'Reilly_Media HTTP/1.1\r\n");
    ssl_say(ssl, buf);
    ssl_say(ssl, "Host: en.wikipedia.org\r\n");
    ssl_say(ssl, "Connection: close\r\n\r\n");

    // Receive response
    char rec[256];
    //Read the response
    int bytesRcvd = ssl_read_in(ssl, rec, sizeof(rec) - 1);
    
    // Keep reading if there's a response
    while (bytesRcvd > 0)
    {
        rec[bytesRcvd] = '\0'; // Null-terminate the received data
        printf("%s", rec); // Print the received data
        bytesRcvd = ssl_read_in(ssl, rec, sizeof(rec) - 1);
    }
    // Clean up and close connections
    close_ssl(ssl, ctx, d_sock);
    return 0;
}
```

### Explanation of `main.c`

1. **Initialize SSL Context:**

   ```C
   SSL_CTX *ctx = init_ssl_context();
   ```

   - This initializes the SSL context, which is required for creating SSL connections.

2. **Open Socket and Connect to Server:**

   ```C
   int d_sock = open_socket("en.wikipedia.org", "443");
   ```

   - This creates a socket and connects it to the specified server (`en.wikipedia.org`) on port 443 (HTTPS).

   - Port 443 is the standard port for HTTPS (HTTP Secure) communications. 

   - HTTPS is the secure version of HTTP, which is the protocol used for transmitting web pages. 

   - It uses SSL/TLS to encrypt the data exchanged between the client and server, ensuring that the data cannot be intercepted or tampered with by third parties.

   - When connecting to "en.wikipedia.org" on port 443, you are establishing a secure connection using SSL/TLS. This is necessary for securely fetching content from websites that support HTTPS, ensuring that the data received and sent is encrypted for privacy and security.

     

3. **Connect SSL over the Socket:**

   ```C
   SSL *ssl = connect_ssl(d_sock, ctx);
   ```

   - This wraps the socket with SSL, establishing an encrypted connection.

4. **Send HTTP Request:**

   ```C
   snprintf(buf, sizeof(buf), "GET /wiki/O'Reilly_Media HTTP/1.1\r\n");
   ssl_say(ssl, buf); // Send the request line
   ssl_say(ssl, "Host: en.wikipedia.org\r\n"); // Send the Host header
   ssl_say(ssl, "Connection: close\r\n\r\n"); // Send the Connection header and end of headers
   ```

   - These lines build and send the HTTP request to the server.

   - HTTP Request Format: we need to ensure that our HTTP request is properly formatted. 

   - HTTP/1.1 requests should end with an additional `CRLF (\r\n)` after the headers to indicate the end of the request headers as in `ssl_say(ssl, "Connection: close\r\n\r\n");`

     

5. **Receive and Print Response:**

   ```C
   char rec[256];
   int bytesRcvd;
   while ((bytesRcvd = ssl_read_in(ssl, rec, sizeof(rec) - 1)) > 0) // Read the response
   {
       rec[bytesRcvd] = '\0'; // Null-terminate the received data
       printf("%s", rec); // Print the received data
   }
   ```

   - This loop reads data from the server and prints it until there's no more data.

6. **Error Handling:**

   ```C
   if (bytesRcvd < 0) // Check for errors
   {
       ERR_print_errors_fp(stderr); // Print any SSL errors
   }
   ```

   - If there was an error reading from the server, print the error details.

7. **Clean Up and Close Connections:**

   ```C
   close_ssl(ssl, ctx, d_sock);
   ```

   - This function cleans up and closes the SSL connection, socket, and SSL context.



`functions.c`

```C
#define _XOPEN_SOURCE 700
#include <openssl/ssl.h>
#include <openssl/err.h>

int open_socket(char *host, char *port)
{
    struct addrinfo *res;
    struct addrinfo hints;
    memset(&hints, 0, sizeof(hints));
    hints.ai_family = PF_UNSPEC;
    hints.ai_socktype = SOCK_STREAM;
    if (getaddrinfo(host, port, &hints, &res) == -1)
    {
        error("Can't resolve the address");
    }

    int sock_d = socket(res->ai_family, res->ai_socktype, res->ai_protocol);

    if (sock_d == -1)
    {
        error("Can't open socket");
    }

    if (connect(sock_d, res->ai_addr, res->ai_addrlen) == -1)
    {
        error("Can't connect to socket");
    }

    freeaddrinfo(res);
    return sock_d;
}

SSL_CTX *init_ssl_context()
{
    // Load human-readable error strings for libssl
    SSL_load_error_strings();
    
    // Initialize the OpenSSL library
    SSL_library_init();
    
    // Load all available encryption algorithms
    OpenSSL_add_all_algorithms();
	
    // Create a new SSL context using TLS
    SSL_CTX *ctx = SSL_CTX_new(TLS_client_method());
    if (ctx == NULL)
    {
        // Print any errors if context creation fails
        ERR_print_errors_fp(stderr);
        // Exit the program with failure
        exit(EXIT_FAILURE);
    }
    
    // Return the created SSL context
    return ctx;
}

SSL *connect_ssl(int socket, SSL_CTX *ctx)
{
    // Create a new SSL structure for a connection 
    SSL *ssl = SSL_new(ctx);
    // Bind the SSL object to the socket file descriptor
    SSL_set_fd(ssl, socket);
	
    // Perform the SSL/TLS handshake with the server
    if (SSL_connect(ssl) == -1)
    {
        // Print any errors if the handshake fails
        ERR_print_errors_fp(stderr);
        exit(EXIT_FAILURE);
    }
    
    // Return the SSL structure
    return ssl;
}

void close_ssl(SSL *ssl, SSL_CTX *ctx, int socket)
{
    // Shut down the SSL connection
    SSL_shutdown(ssl);
    // Free the SSL structure
    SSL_free(ssl);
    // Close the socket file descriptor
    close(socket);
    // Free the SSL context
    SSL_CTX_free(ctx);
}

int ssl_say(SSL *ssl, char *s)
{
    // Write data to the SSL connection
    int result = SSL_write(ssl, s, strlen(s));
    // Check if the write operation failed
    if (result == -1)
    {
        // Print any SSL errors
        ERR_print_errors_fp(stderr);
    }
    // Return the result of the write operation
    return result;
}

int ssl_read_in(SSL *ssl, char *buf, int len)
{
    // Read data from the SSL connection
    int received = SSL_read(ssl, buf, len - 1);
    
    // Check if data was received
    if (received > 0)
    {
        // Null-terminate the received data
        buf[received] = '\0';
    }
    // Return the number of bytes received
    return received;
}

```

### Detailed Explanations

#### What does `SSL_CTX` stand for?

- `SSL_CTX` is a structure used in OpenSSL to hold all SSL/TLS settings. 
- The [`SSL_CTX`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) structure is part of the OpenSSL library, which is widely used for secure communication over networks. 
- It stands for "SSL Context," where SSL refers to Secure Sockets Layer (and by extension, its successor, Transport Layer Security or TLS). 
- This context is used to manage settings and certificates across multiple SSL/TLS connections. 
- It includes options for configuring the SSL/TLS protocols, choosing which certificates and keys to use, and setting up other security-related options.

#### Notes to remember

In C, the presence of an asterisk (`*`) in a function declaration indicates that the function returns a pointer, while its absence means the function returns a value directly. Here's a breakdown of the difference:

- `SSL *connect_ssl()`: This function declaration indicates that `connect_ssl` returns a pointer to an `SSL` object. The asterisk (`*`) signifies that the return type is a pointer. This is common when the function allocates an object in memory and returns a reference to it, allowing the function to return complex data structures or instances of structures/classes.
- `SSL connect_ssl()`: This declaration indicates that `connect_ssl` returns an `SSL` object directly, not a pointer. This means the function returns a copy of the object itself. Returning complex objects like this can be less efficient than returning pointers due to the overhead of copying the object's data. However, it can be used for simple data types or when returning a static or global object that does not need to be dynamically allocated.

In summary, the key difference is in how the function returns its result: either as a direct copy (`SSL connect_ssl()`) or as a reference/pointer to the object (`SSL *connect_ssl()`). The choice between these two approaches depends on the specific requirements of the function, including efficiency considerations and whether the object needs to be modified by the caller.

##### Why SSL Context is necessary?

An SSL context `SSL_CTX *` is necessary for several reasons related to setting up and managing SSL/TLS connections securely. Here's why it's needed:

1. **Initialization and Configuration**: The SSL context acts as a factory for creating SSL/TLS connections. It holds all the configuration options, methods, and certificates required for the SSL/TLS handshake. When we create an SSL object with `SSL_new(ctx)`, it inherits the settings from the context. This allows us to configure multiple SSL connections similarly without setting options individually for each connection.
2. **Certificate Management**: The SSL context is used to load and manage certificates and private keys. These are essential for the SSL/TLS handshake process, where the server (and optionally the client) must present a certificate to prove its identity. The context provides a centralized way to manage these credentials.
3. **Protocol Options**: SSL/TLS has evolved over time, with newer versions offering better security. The context allows us to specify which versions of the SSL/TLS protocols our application supports or should use, enabling us to enforce stronger security policies.
4. **Performance Optimization**: Creating an SSL context can be resource-intensive, as it involves loading cryptographic algorithms, processing certificates, and more. By using a single context for multiple connections, we can amortize the cost of these operations across all connections, improving performance.
5. **Error Handling**: The context also plays a role in error handling. For example, when an SSL/TLS error occurs, functions like [`ERR_print_errors_fp`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) can use the context to obtain detailed error messages, helping in diagnosing and resolving issues.

In summary, the SSL context ([`SSL_CTX *`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html)) in OpenSSL is a critical structure that encapsulates global configuration for SSL/TLS operations. It simplifies managing multiple connections, enhances security by centralizing certificate and protocol management, and improves performance by reusing expensive cryptographic operations.

#### `init_ssl_context`

1. Load Error Strings:
   - `SSL_load_error_strings()` loads human-readable error messages for SSL functions.
2. Initialize SSL Library:
   - `SSL_library_init()` initializes the SSL library.
3. Add All Algorithms:
   - `OpenSSL_add_all_algorithms()` loads all available encryption algorithms.
4. Create SSL Context:
   - `SSL_CTX_new(TLS_client_method())` creates a new SSL context using the TLS client method.
5. Error Handling:
   - If the context creation fails, `ERR_print_errors_fp(stderr)` prints the errors and exits the program.

#### `connect_ssl`

1. Create SSL Structure:
   - `SSL_new(ctx)` creates a new SSL structure for the connection.
2. Set File Descriptor:
   - `SSL_set_fd(ssl, socket)` binds the SSL structure to the socket file descriptor.
3. Perform Handshake:
   - `SSL_connect(ssl)` performs the SSL/TLS handshake with the server.
4. Error Handling:
   - If the handshake fails, `ERR_print_errors_fp(stderr)` prints the errors and exits the program.

#### `close_ssl`

1. Shutdown SSL Connection:
   - `SSL_shutdown(ssl)` shuts down the SSL connection.
2. Free SSL Structure:
   - `SSL_free(ssl)` frees the SSL structure.
3. Close Socket:
   - `close(socket)` closes the socket file descriptor.
4. Free SSL Context:
   - `SSL_CTX_free(ctx)` frees the SSL context.

#### `ssl_say`

1. Write Data:
   - `SSL_write(ssl, s, strlen(s))` writes data to the SSL connection.
2. Error Handling:
   - If the write operation fails, `ERR_print_errors_fp(stderr)` prints the SSL errors.

#### `ssl_read_in`

1. Read Data:
   - `SSL_read(ssl, buf, len - 1)` reads data from the SSL connection. The -1 ensures there is space for a null terminator to be added, making the buffer a valid C string.
2. Null-Terminate Data:
   - If data is received, it is null-terminated to ensure it is a valid C string.

`MakeFile`

```makefile
CC = clang
CFLAGS = -Wall -Wextra -g -D_XOPEN_SOURCE=700
LIBS = -lssl -lcrypto

all: final

final: main.o hello.o
	@echo "Linking and producing the final application"
	$(CC) $(CFLAGS) main.o hello.o -o final $(LIBS)

main.o: main.c 
	@echo "Compiling the main file"
	$(CC) $(CFLAGS) -c main.c 

hello.o: hello.c 
	@echo "Compiling the hello file"
	$(CC) $(CFLAGS)  -c hello.c 

clean:
	@echo "Removing everything except the source files"
	@rm main.o hello.o final
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/test$ ./final "O'Reilly_Media"
HTTP/1.1 200 OK
date: Thu, 18 Jul 2024 14:28:52 GMT
server: mw-web.codfw.main-f564bd77d-7dwv6
x-content-type-options: nosniff
content-language: en
origin-trial: AonOP4SwCrqpb0nhZbg554z9iJimP3DxUDB8V4yu9fyyepauGKD0NXqTknWi4gnuDfMG6hNb7TDUDTsl0mDw9gIAAABmeyJvcmlnaW4iOiJodHRwczovL3dpa2lwZWRpYS5vcmc6NDQzIiwiZmVhdHVyZSI6IlRvcExldmVsVHBjZCIsImV4cGlyeSI6MTczNTM0Mzk5OSwiaXNTdWJkb21haW4iOnRydWV9
accept-ch: 
vary: Accept-Encoding,Cookie,Authorization
last-modified: Thu, 04 Jul 2024 14:28:52 GMT
content-type: text/html; charset=UTF-8
age: 0
x-cache: cp5017 miss, cp5017 miss
x-cache-status: miss
server-timing: cache;desc="miss", host;desc="cp5017"
strict-transport-security: max-age=106384710; includeSubDomains; preload
report-to: { "group": "wm_nel", "max_age": 604800, "endpoints": [{ "url": "https://intake-logging.wikimedia.org/v1/events?stream=w3c.reportingapi.network_error&schema_uri=/w3c/reportingapi/network_error/1.0.0" }] }
...
```

---

