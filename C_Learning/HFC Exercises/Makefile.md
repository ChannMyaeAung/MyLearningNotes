#### Make Magnets Solution

```bash
oggswing: oggswing.c oggswing.h
	gcc oggswing.c -o oggswing

swing.ogg: whitennerdy.ogg oggswing
	oggswing whitennerdy.ogg swing.ogg
```



Here's a breakdown:

1. **Line 1: `oggswing: oggswing.c oggswing.h`**
   - This line specifies that the target `oggswing` depends on `oggswing.c` and `oggswing.h`. In other words, if any of these source files are modified, `oggswing` needs to be rebuilt.
2. **Line 2: `[TAB] gcc oggswing.c -o oggswing`**
   - This line contains a command to compile the `oggswing.c` source file into an executable named `oggswing` using the GCC compiler. The `[TAB]` represents a tab character, which is often used as indentation in Makefiles.
3. **Line 4: `swing.ogg: whitennerdy.ogg oggswing`**
   - This line specifies that the target `swing.ogg` depends on `whitennerdy.ogg` and `oggswing`. In other words, before `swing.ogg` can be created, `whitennerdy.ogg` and `oggswing` must already exist.
4. **Line 5: `[TAB] oggswing whitennerdy.ogg swing.ogg`**
   - This line contains a command to execute `oggswing`, passing `whitennerdy.ogg` as input and producing `swing.ogg` as output. It seems like `oggswing` is some sort of program or script that processes audio files.

In summary, this script automates the compilation of a C program (`oggswing`) and its usage to process audio files (`whitennerdy.ogg` and `swing.ogg`). It ensures that the C program is built if its source files are modified and then uses it to process audio files.



```bash
chan@CMA:~/C_Programming/test
$ code hello.h main.c hello.c

```

hello.h

```C
void hello();
const char *get_message();
```

hello.c

```C
#include <stdio.h>
#include "hello.h"

void hello(){
    printf("%s", get_message());
}

const char *get_message(){
    return "Hello World\n";
}
```

main.c

```C
#include "hello.h"

int main(){
    hello();
    return 0;
}
```



```bash
chan@CMA:~/C_Programming/test$ gcc -c main.c

chan@CMA:~/C_Programming/test$ gcc -c hello.c

chan@CMA:~/C_Programming/test$ ls
hello.c  hello.h  hello.o  main.c  main.o

chan@CMA:~/C_Programming/test
$ gcc main.o hello.o -o final

chan@CMA:~/C_Programming/test
$ ls
final  hello.c  hello.h  hello.o  main.c  main.o

chan@CMA:~/C_Programming/test$ ./final
Hello World

chan@CMA:~/C_Programming/test$ code Makefile
```

#### Makefile

```makefile
all: final

final: main.o hello.o
	@echo "Linking and producing the final application"
	gcc main.o hello.o -o final

main.o: main.c
	@echo "Compiling the main file"
	gcc -c main.c

hello.o: hello.c
	@echo "Compiling the hello file"
	gcc -c hello.c

clean:
	@echo "Removing everything except the source files"
	@rm final main.o hello.o
```



```bash
chan@CMA:~/C_Programming/test
$ ls
final  hello.c  hello.h  hello.o  main.c  main.o  Makefile

chan@CMA:~/C_Programming/test
$ make clean
Removing everything except the source files

chan@CMA:~/C_Programming/test
$ ls
hello.c  hello.h  main.c  Makefile

chan@CMA:~/C_Programming/test
$ make all
Compiling the main file
gcc -c main.c 
Compiling the hello file
gcc -c hello.c 
Linking and producing the final application
gcc main.o hello.o -o final

chan@CMA:~/C_Programming/test
$ ls
final  hello.c  hello.h  hello.o  main.c  main.o  Makefile

chan@CMA:~/C_Programming/test
$ ./final
Hello World
chan@CMA:~/C_Programming/test$ 

```



If there's a change in one of the files, I don't have to rerun all the recipes and recompile all the code with `Makefile`.

Let's say if I change Hello World from Hello My Princess.

hello.c

```C
const char *get_message(){
    return "Hello My Princess!\n";
}

```

Then I only have to run this:

```bash
chan@CMA:~/C_Programming/test
$ make clean
Removing everything except the source files

chan@CMA:~/C_Programming/test
$ make all
Compiling the main file
gcc -c main.c 
Compiling the hello file
gcc -c hello.c 
Linking and producing the final application
gcc main.o hello.o -o final

chan@CMA:~/C_Programming/test
$ ./final
Hello My Princess!
chan@CMA:~/C_Programming/test$ 

```

---