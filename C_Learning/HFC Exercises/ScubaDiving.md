#### Scuba Diver Exercise - Exercise 2

header.h

```C
typedef struct{
    float tank_capacity;
    int tank_psi;
    const char *suit_material;
} equipment;

typedef struct scuba{
    const char *name;
    equipment kit;
} diver;

//Here we'll just use "diver" although we give the struct name "scuba".

void badge(diver d);
```



functions.c

```C
#include <stdio.h>
#include "header.h"

void badge(diver d){
    printf("Name: %s Tank: %2.2f(%i) Suit: %s\n", d.name, d.kit.tank_capacity, d.kit.tank_psi, d.kit.suit_material);
}
```



main.c

```C
#include <stdio.h>
#include "header.h"

int main(){
    diver randy = {"Randy", {5.5, 3500, "Neoprene"}};
    badge(randy);
    return 0;
}
```



Makefile

```makefile
all: diver

diver: main.o functions.o
	@echo "Compiling the final exe diver file..."
	gcc main.o functions.o -o diver
	
main.o: main.c
	@echo "Compiling main file..."
	gcc -c main.c

functions.o: functions.c
	@echo "Compiling functions file..."
	gcc -c functions.c

clean:
	@echo "Removing everything except the source files..."
	@rm main.o functions.o diver
```



Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_5/scubadiver_puzzle
$ make all
Compiling main file...
gcc -c main.c 
Compiling the final exe diver file...
gcc main.o functions.o -o diver 

chan@CMA:~/C_Programming/HFC/chapter_5/scubadiver_puzzle
$ ./diver
Name: Randy Tank: 5.50(3500) Suit: Neoprene

```

