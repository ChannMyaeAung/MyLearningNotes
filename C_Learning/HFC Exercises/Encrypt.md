#### Message Hider

encrypt.c

```C
#include "encrypt.h"

void encrypt(char *message){
    char c;
    while(*message){
        *message = *message ^ 31;
        message++;
    }
}
```



encrypt.h

```C
void encrypt(char *message);
```



main.c

```C
#include <stdio.h>
#include "encrypt.h"

int main(){
    char msg[80];
    while(fgets(msg, 80, stdin)){
        encrypt(msg);
        printf("%s\n", msg);
    }
}
```



```C
chan@CMA:~/C_Programming/HFC/chapter_4/exercise_2$ 
gcc main.c encrypt.c -o main

// When we run the program, we can enter text and see the encrypted version.
chan@CMA:~/C_Programming/HFC/chapter_4/exercise_2$ 
./main
    
I am a secret message
V?~r?~?lz|mzk?rzll~xz
    
chan@CMA:~/C_Programming/HFC/chapter_4/exercise_2$ 
// We can even pass it the contents of the encrypt.h file to encrypt it.
./main < encrypt.h
ipv{?zq|mfok7|w~m?5rzll~xz6$
    
chan@CMA:~/C_Programming/HFC/chapter_4/exercise_2$ 

```

**Code Breakdown**

- In `main.c`, it includes the standard input/output library `stdio.h` and a custom header file `encrypt.h`. 
- Then inside the function, it initializes an array `msg` of 80 characters to store user input.
- It enters a loop where it continuously reads input form the user using `fgets()`, encrypts it using a function `encrypt()` defined in `encrypt.c` and prints the encrypted message using `printf()`.
- Moving on to `encrypt.c`, it defines the `encrypt()` function. This function takes a pointer to a character array `(char *message)` as its parameter. Inside this function, there's a loop that iterates over each character in the message. For each character, it performs a bitwise XOR(exclusive OR) operation with the value 31 `*message = *message ^ 31`. This is where the actual encryption happens.



Q: Why do we use `message` as a pointer variable?

A: Because we want to modify the original message passed to the `encrypt()` function. Using a pointer allows us to directly manipulate the memory where the message is stored.



Q: What is XOR(^)?

A: XOR(exclusive OR) is a bitwise operation in C. It returns true if the operands are different and false if they are the same. For example,

-  `0 ^ 0` results in `0`.
-  `0 ^ 1` results in `1`.
-  `1 ^ 0` results in `1`.
-  `1 ^ 1` results in `0`.



Q: Why do we use 31 in the XOR operation?

A: The choice of 31 for the XOR operation is somewhat arbitrary. It's a simple value that's commonly used in basic encryption algorithms. You could use any other value, but 31 was chosen here probably because it provides a decent level of obfuscation for such a simple algorithm.

