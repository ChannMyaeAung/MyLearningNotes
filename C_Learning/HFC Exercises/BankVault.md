### Chapter 7 - Exercise 1

Encrypt function encrypt the contents of a string. Checksum function check if the contents of a string have been modified.

**Detailed explanations are in `HFCTheories.md`.**

encrypt.h

```C
void encrypt(char *message);
```

encrypt.c

```C
#include "encrypt.h"

void encrypt(char *message){
    while(*message){
        *message = *message ^ 31;
        message++;
    }
}
```

checksum.h

```C
int checksum(char *message);
```

checksum.c

```C
#include "checksum.h"
int checksum(char *message){
    int c = 0;
    while(*message){
        c += c ^ (int)(*message);
        message++;
    }
    return c;
}
```

main.c

```C
#include <stdio.h>
#include "encrypt.h"
#include "checksum.h"

int main(){
    char s[] = "Speak friend and enter";
    char t[] = "abc";
    encrypt(s);
    printf("Encrypted to '%s'\n",s);
    printf("Checksum is %i\n", checksum(s));
    // If we call encrypt() again, it will decrypt it.
    encrypt(s);
    printf("Decrypted back to '%s'\n", s);
    printf("Checksum is %i\n", checksum(s));
    
    encrypt(t);
    printf("Encrypted to '%s'\n", t);
    printf("Checksum is %i\n", checksum(t));
    encrypt(t);
    printf("Decrypted back to '%s'\n", t);
    printf("Checksum is %i\n", checksum(t));
    return 0;
}
```

Makefile

```makefile
all: final

final: encrypt.o checksum.o main.o
	@echo "Compiling the final exe file..."
	gcc encrypt.o checksum.o main.o -o final

main.o: main.c
	@echo "Compiling the main file..."
	gcc -c main.c

encrypt.o: encrypt.c 
	@echo "Compiling the encrypt file..."
	gcc -c encrypt.c 

checksum.o: checksum.c 
	@echo "Compiling the checksum file..."
	gcc -c checksum.c 

clean: 
	@echo "Removing everything except the source files..."
	@rm encrypt.o checksum.o final

```



Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_8/exercise_1
$ make all
Compiling the main file...
gcc -c main.c
Compiling the final exe file...
gcc encrypt.o checksum.o main.o -o final

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_1
$ ./final
Encrypted to 'Loz~t?ymvzq{?~q{?zqkzm'
Checksum is 89561741
Decrypted back to 'Speak friend and enter'
Checksum is 89548156
Encrypted to '~}|'
Checksum is 382
Decrypted back to 'abc'
Checksum is 107

```

 #### Exercise - 2

Working with archive. 

encrypt.h

```C
void encrypt(char *message);
```

checksum.h

```C
int checksum(char *message);
```



bank_vault.c

```C
#include <stdio.h>
#include <string.h>
#include "encrypt.h"
#include "checksum.h"

int main(void)
{
    char t[] = "abc";

    encrypt(t);
    printf("Encrypted to '%s'\n", t);
    printf("Checksum is '%d'\n", checksum(t));
    encrypt(t);
    printf("Decrypted back to '%s'\n", t);
    printf("Checksum is '%d'\n", checksum(t));
    return 0;
}
```

checksum.c

```C
#include <stdio.h>
#include "checksum.h"

int checksum(char *message)
{
    int c = 0;
    while (*message)
    {
        c += c ^ (int)(*message);
        message++;
    }
    return c;
}
```

encrypt.c

```C
#include <stdio.h>
#include "encrypt.h"

void encrypt(char *message)
{
    while (*message)
    {
        *message = *message ^ 31;
        message++;
    }
}
```

Makefile

```makefile
all: bank_vault

bank_vault: bank_vault.o libhfsecurity.a 
	gcc bank_vault.c -I. -L. -lhfsecurity -o bank_vault
	
bank_vault.o: bank_vault.c
	gcc -c bank_vault.c -I. -o bank_vault.o

encrypt.o: encrypt.c 
	gcc -c encrypt.c -o encrypt.o

checksum.o: checksum.c 
	gcc -c checksum.c -o checksum.o


libhfsecurity.a: encrypt.o checksum.o
	ar -rcs libhfsecurity.a encrypt.o checksum.o
```

`-I.`, `-L.` - because the header files and the archive are in the current directory. Otherwise, we would have to specify the pathname like `-I../header_files/`.

`-lhfsecurity` - We need `-lhfsecurity` because the archive is called `libhfsecurity.a`.

Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_8/exercise_2
$ make all
gcc -c bank_vault.c -I. -I../header_files -o bank_vault.o
gcc bank_vault.c -I. -L. -lhfsecurity -o bank_vault

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_2
$ ./bank_vault
Encrypted to '~}|'
Checksum is '382'
Decrypted back to 'abc'
Checksum is '107'

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_2$ 


```

