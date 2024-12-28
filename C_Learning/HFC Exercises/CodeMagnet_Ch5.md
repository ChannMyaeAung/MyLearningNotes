#### Code Magnets - Exercise 5

main.c

```C
#include <stdio.h>
#include "header.h"

int main(){
    fruit_order apples = {"apples", "England", .amount.count=144, COUNT};
    
    fruit_order strawberries = {"strawberries", "Spain", .amount.weight=17.6, POUNDS};
    
    fruit_order oj = {"orange juice", "U.S.A", .amount.volume=10.5, PINTS};
    
    display(apples);
    display(strawberries);
    display(oj);
    return 0;
}
```

header.h

```C
typedef enum{
    COUNT, POUNDS, PINTS
} unit_of_measure;

typedef union{
    short count;
    float weight;
    float volume;
}quantity;

typedef struct{
    const char *name;
    const char *country;
    quantity amount;
    unit_of_measure units;
} fruit_order;

void display(fruit_order order);
```

functions.c

```C
#include <stdio.h>
#include "header.h"

void display(fruit_order order){
    printf("This order contains ");
    if(order.units == PINTS){
        printf("%2.2f pints of %s\n", order.amount.volume, order.name);
    }else if(order.units == POUNDS){
        printf("%2.2f lbs of %s\n", order.amount.weight, order.name);
    }else{
        printf("%i %s\n", order.amount.count, order.name);
    }
}
```



Makefile

```makef
all: magnet

magnet: main.o functions.o
	@echo "Compiling the final exe file..."
	gcc main.o functions.o -o magnet

main.o: main.c 
	@echo "Compiling the main file..."
	gcc -c main.c 

functions.o: functions.c 
	@echo "Compiling the functions file..."
	gcc -c functions.c 

clean:
	@echo "Removing everything execept the source file..."
	@rm magnet main.o functions.o 
```



Code Execution

```bash
chan@CMA:~/C_Programming/HFC/chapter_5/codeMagnets
$ make all
Compiling the functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc main.o functions.o -o magnet

chan@CMA:~/C_Programming/HFC/chapter_5/codeMagnets
$ ./magnet
This order contains 144 apples
This order contains 17.60 lbs of strawberries
This order contains 10.50 pints of orange juice

```

---

