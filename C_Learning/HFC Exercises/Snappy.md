#### Chapter 5 - Exercises

snappy.c

```C
#include <stdio.h>
#include "functions.h"

int main(){
  struct fish snappy = {"Snappy", "Piranha", 69, 4};
    
    catalog(snappy);
    label(snappy);
    
  return 0;
}
```



functions.c

```C
#include <stdio.h>
#include "functions.h"

void catalog(struct fish f){
    printf("%s is a %s with %i teeth. He is %i.\n", f.name, f.species, f.teeth, f.age);
}

void label(struct fish f){
    printf("Name: %s\nSpecies: %s\nAge: %i years old, %i teeth\n", f.name, f.species, f.teeth, f.age);
}
```



functions.h

```C
struct fish{
    const char *name;
    const char *species;
    int teeth;
    int age;
};

void catalog(struct fish f);

void label(struct fish f);
```



```make
all: final

final: snappy.o functions.o
	@echo "Compiling final executable file..."
	gcc snappy.o functions.o -o snappy

snappy.o: snappy.c
	@echo "Compiling the snappy file..."
	gcc -c snappy.c

functions.o: functions.c
	@echo "Compiling the functions file..."
	gcc -c functions.c

clean:
	@echo "Removing everything except the source files..."
	@rm snappy.o functions.o snappy
```



```bash
chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ make clean
Removing everything except the source files...

chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ ls
functions.c  functions.h  Makefile  snappy.c

chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ make all
Compiling the snappy file...
gcc -c snappy.c 
Compiling the functions file...
gcc -c functions.c 
Compiling final executable file...
gcc snappy.o functions.o -o snappy 

chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ ./snappy
Snappy is a Piranha with 69 teeth. He is 4.
Name: Snappy
Species: Piranha
Age: 69 years old, 4 teeth
```



##### Nest `Struct`

functions.h

```C
struct preferences{
    const char *food;
    float exercise_hours;
}

struct fish{
    const char *name;
    const char *species;
    int teeth;
    int age;
    struct preferences care;
};

void catalog(struct fish f);

void label(struct fish f);

void info(struct fish f);
```



snappy.c

```C
#include <stdio.h>
#include "functions.h"

int main(){
    
    struct fish snappy = {"Snappy", "Piranha", 69, 4, {"Meat", 7,5}};
    
    catalog(snappy);
    label(snappy);
    info(snappy);
    
    return 0;
}
```





functions.c

```C
#include <stdio.h>
#include "functions.h"

void catalog(struct fish f)
{
    printf("%s is a %s with %i teeth. He is %i.\n", f.name, f.species, f.teeth, f.age);
}

void label(struct fish f)
{
    printf("Name: %s\nSpecies: %s\nAge: %i years old, %i teeth\n", f.name, f.species, f.teeth, f.age);
}

void info(struct fish f){
    printf("Snappy likes to eat %s.\n", f.care.food);
    printf("Snappy likes to exercise for %.2f hours.\n", f.care.exercise_hours);
}
```



```bash
chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ make all
Compiling the snappy file...
gcc -c snappy.c 
Compiling final executable file...
gcc snappy.o functions.o -o snappy 

chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ ./snappy
Snappy is a Piranha with 69 teeth. He is 4.
Name: Snappy
Species: Piranha
Age: 69 years old, 4 teeth
Snappy likes to eat Meat.
Snappy likes to exercise for 7.50 hours.

chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1$ 

```

---

