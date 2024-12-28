#### Exercise 2 - Chapter 9 (`exec` functions)

`example.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){
    printf("PID of example.c = %d\n", getpid());
    
    // arguments for the new program
    char *args[] = {"./hello", "Hello", "C", "Programming", NULL};
    
    // Replace the current process image with a new one
    if(execv("./hello", args) == -1){
        // if execv fails, print this error.
        perror("execv failed");
    }
    printf("Back to example.c");
    return 0;
}

```

`hello.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[]){
    printf("We are in Hello.c\n");
    printf("PID of hello.c = %d\n", getpid());
    
    // printing the argument counts.
    printf("argc = %d\n", argc);
    
    // Print the arguments passed to this program.
    for(int i = 0; i < argc; i++){
        printf("argv[%d] = %s\n",i, argv[i]);
    }
    return 0;
}
```

Makefile

```make
CC = clang
CFLAGS = -Wall -Wextra -g

example: example.c
	$(CC) $(CFLAGS) example.c -o example
	
hello: hello.c
	$(CC) $(CFLAGS) hello.c -o hello
```

Code execution:

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/example
$ make example
make: 'example' is up to date.

chan@CMA:~/C_Programming/HFC/chapter_9/example
$ make hello
clang -Wall -Wextra -g hello.c -o hello

chan@CMA:~/C_Programming/HFC/chapter_9/example
$ ./example
PID of example.c = 31661
We are in Hello.c
PID of hello.c = 31661
argc = 4
argv[0] = ./hello
argv[1] = Hello
argv[2] = C
argv[3] = Programming

```

- argc - argument count
- argv - argument vector

#### Exercise 4 - Chapter 9 

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>

int main(){
    if(execl("/sbin/ifconfig", "/sbin/ifconfig", NULL) == -1){
        if(execlp("ipconfig", "ipconfig", NULL) == -1){
            fprintf("stderr", "Cannot run ipconfig: %s\n", strerr(errno));
            return -1;
        }
    }
}
```

 Code Breakdown:

- `#include <unistd.h>` - We need this for the `exec()` functions.
- `#include <errno.h>` - We need this for the `errno` variable.
- `#include <string.h>` - This will let us display errors with `strerror()`.
- `execl("/sbin/ifconfig", "/sbin/ifconfig", NULL) == -1` - We use `execl()` because we have the path to the program file.
- If `execl()` returns -1, it failed, so we should probably look for `ipconfig`.
- `execlp()` will let us find the `ipconfig`  command on the path.
- The `strerror()` function will display any problems.

Code Execution:

```sh
chan@CMA:~/C_Programming/test
$ make all
make: Nothing to be done for 'all'.

chan@CMA:~/C_Programming/test
$ ./final

We are in hello.c
PID of hello.c: 8147
strcmp(str1, str2): -15
strcmp(str1,str3): -33
s1: Xbcdef
s2: abcdef
enp2s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.105  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::19f0:82d1:7ed0:8796  prefixlen 64  scopeid 0x20<link>
        ether e4:a8:df:ba:4d:a0  txqueuelen 1000  (Ethernet)
        RX packets 31195  bytes 22229071 (22.2 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 33688  bytes 31468061 (31.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 97624  bytes 11603767 (11.6 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 97624  bytes 11603767 (11.6 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlo1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.124  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::fb0b:eac9:3edd:121a  prefixlen 64  scopeid 0x20<link>
        ether 38:d5:7a:65:22:fb  txqueuelen 1000  (Ethernet)
        RX packets 3580  bytes 1009135 (1.0 MB)
        RX errors 0  dropped 407  overruns 0  frame 0
        TX packets 1761  bytes 184536 (184.5 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


```

---

#### Exercise 5 - Chapter 9 (Mixed Messages Solution)

`main.c`

```C
int main(){
    char *my_env[] = {"FOOD=coffee", NULL};
    
    if(execle("./coffee", "./coffee", "donuts", NULL, my_env) == -1){
        fprintf(stderr, "Can't run process 0: %s\n", strerror(errno));
        return 1;
    }
 	return 0;   
}
```

`coffee.c`

```C
int main(int argc, char *argv[]){
    char *w = getenv("EXTRA"); // Try to get the value of the environment variable "EXTRA".
    if(!w) // if "EXTRA" is not set,
        w = getenv("FOOD"); // then try to get the value of the environment variable "FOOD"
    if(!w) // if neither "EXTRA" nor "FOOD" is set,
        w = argv[argc - 1]; // use the last command-line argument as the value for w.
    
    char *c = getenv("EXTRA"); // Try to get the value of the environment variable "EXTRA".
    if(!c) // If "EXTRA" is not set,
        c = argv[argc - 1]; // use the last command-line argument as the value for c.
    printf("%s with %s\n", c, w);
    return 0; // Return 0 to indicate successful execution.
}
```

Code Execution for the main program above:

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/coffee
$ ./main
donuts with coffee

```

for this code in `main.c`: (2nd)

```C
char *my_env[] = {"FOOD=donuts", NULL};

    if (execle("./coffee", "coffee", NULL, my_env) == -1)
    {
        fprintf(stderr, "Can't run process 3: %s\n", strerror(errno));
        return 1;
    }
```

Code Execution will be: (2nd)

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/coffee
$ ./main
coffee with donuts
```

for this code in `main.c`: (3rd)

```C
	if (execl("./coffee", "coffee", NULL) == -1)
    {
        fprintf(stderr, "Can't run process 3: %s\n", strerror(errno));
        return 1;
    }
```

Code Execution will be: (3rd)

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/coffee
$ ./main
coffee with coffee
```

for this code in `main.c`: (4th)

```C
char *my_env[] = {"FOOD=donuts", NULL};	
if (execle("./coffee", "./coffee", "cream", NULL, my_env) == -1)
    {
        fprintf(stderr, "Can't run process 3: %s\n", strerror(errno));
        return 1;
    }
```

Code Execution will be: (4th)

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/coffee
$ ./main
cream with donuts

```

---
