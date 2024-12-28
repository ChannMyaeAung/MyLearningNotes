#### Turtle Myrtle - Exercise 3



If we don't use the pointer to our `struct`

```C
#include <stdio.h>

typedef struct{
    const char *name;
    const char *species;
    int age;
} turtle;

void happy_birthday(turtle t){
    t.age = t.age + 1;
    printf("Happy Birthday %s! You are now %i years old!\n", t.name, t.age);
}

int main(){
    turtle myrtle = {"Myrtle", "Leatherback sea turtle", 99};
    happy_birthday(myrtle);
    printf("%s's age is now %i\n", myrtle.name, myrtle.age);
    return 0;
}
```



The `myrtle struct` will not be updated but its clone will.

```bash
chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ make all
Compiling the main file...
gcc -c main.c 
Compiling the final turtle exe file...
gcc main.o functions.o -o turtle

chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ ./turtle
Happy Birthday Myrtle! You are now 100 years old!
Myrtle's age is now 99
```



Now if we use pointers: 

Note: We'll also be implementing makefile for this.

main.c

```C
#include <stdio.h>
#include "header.h"

int main(void){
    turtle myrtle = {"Myrtle", "Leatherback sea turtle", 99};
    happy_birthday(&myrtle);
    printf("%s's age is now %i\n", myrtle.name, myrtle.age);
    return 0;
}
```

header.h

```C
typedef struct{
    const char *name;
    const char *species;
    int age;
} turtle;

void happy_birthday(turtle *t)
```

functions.c

```C
#include <stdio.h>
#include "header.h"

void happy_birthday(turtle *t){
    (*t).age = (*t).age + 1;
    printf("Happy Birthday %s! You are now %i years old!\n", (*t).name, (*t).age);
}
```



Makefile

```make
all: turtle

turtle: main.o functions.o
	@echo "Compiling the final turtle exe file..."
	gcc main.o functions.o -o turtle

main.o: main.c
	@echo "Compiling the main file..."
	gcc -c main.c

functions.o: functions.c
	@echo "Compiling the functions file..."
	gcc -c functions.c

clean:
	@echo "Removing everything except the source files..."
	@rm main.o functions.o turtle
```



```bash
chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ make all
Compiling the main file...
gcc -c main.c 
Compiling the functions file...
gcc -c functions.c 
Compiling the final turtle exe file...
gcc main.o functions.o -o turtle

chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ ./turtle
Happy Birthday Myrtle! You are now 100 years old!
Myrtle's age is now 100

chan@CMA:~/C_Programming/HFC/chapter_5/myrtle$ 

```

Now the function is updated.

By passing a pointer to the `struct`, we allowed the function to update the original data rather than taking a local copy.