#### Piranha Puzzle - Exercise 1

piranha.h

```C
struct exercise{
    const char *description;
    float duration;
};

struct meal{
    const char *ingredients;
    float weight;
};

struct preferences{
    struct meal food;
    struct exercise exercise;
};

struct fish{
    const char *name;
    const char *species;
    int teeth;
    int age;
    struct preferences care;
};

void label(struct fish a);
```



functions.c

```C
#include <stdio.h>
#include "piranha.h"

void label(struct fish a){
    printf("Name:%s\nSpecies:%s\n%i years old, %i teeth", a.name, a.species, a.teeth, a.age);
    printf("Feed with %2.2f lbs of %s and allow to %s for %2.2f hours\n", a.care.food.weight, a.care.food.ingredients, a.care.exercise.description, a.care.exercise.duration);
}
```



piranha.c (main file)

```C
#include <stdio.h>
#include "piraha.h"

int main(void){
    struct fish snappy = {"Snappy", "Piranha", 69, 4, {"neat",0.2}, {"swim in the jacuzzi", 7.5}};
    label(snappy);
    return 0;
}
```



Makefile

```make
all: piranha

piranha: piranha.o functions.o
	@echo "Compiling the final piranha file..."
	gcc piranha.o functions.o -o piranha

piranha.o: piranha.c
	@echo "Compiling the piranha file..."
	gcc -c piranha.c

functions.o: functions.c
	@echo "Compiling the functions file..."
	gcc -c functions.c

clean: 
	@echo "Removing everything except the source files..."
	@rm piranha.o functions.o piranha
```



Execution

```bash
chan@CMA:~/C_Programming/HFC/chapter_5/piranha_puzzle
$ make clean
Removing everything except the source files...

chan@CMA:~/C_Programming/HFC/chapter_5/piranha_puzzle
$ ls
functions.c  Makefile  piranha.c  piranha.h

chan@CMA:~/C_Programming/HFC/chapter_5/piranha_puzzle
$ make all
Compiling the piranha file...
gcc -c piranha.c 
Compiling the functions file...
gcc -c functions.c 
Compiling the final piranha file...
gcc piranha.o functions.o -o piranha

chan@CMA:~/C_Programming/HFC/chapter_5/piranha_puzzle
$ ./piranha
Name: Snappy
Species: Piranha
69 years old, 4 teeth
Feed with 0.20 lbs of meat and allow to swim in the jacuzzi for 7.50 hours.

chan@CMA:~/C_Programming/HFC/chapter_5/piranha_puzzle$ 

```

---

