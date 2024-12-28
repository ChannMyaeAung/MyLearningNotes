#### Safe Cracker - Exercise 4

```C
#include <stdio.h>

typedef struct{
    const char *description;
    float value;
} swag;

typedef struct{
    swag *swag;
    const char *sequence;
} combination;

typedef struct{
    combination numbers;
    const char *make;
} safe;

int main(void){
    swag gold = {"GOLD", 1000000.0};
    combination numbers = {&gold, "6502"};
    safe s = {numbers, "RAMACON250"};
    
    printf("%s\n", s.numbers.swag->description);
    return 0;
}
```



```bash
chan@CMA:~/C_Programming/HFC/chapter_5/safe_cracker
$ make main
cc     main.c   -o main

chan@CMA:~/C_Programming/HFC/chapter_5/safe_cracker
$ ./main
GOLD

```



#### String Length Exercise from CS50

```C
#include <stdio.h>
#include <string.h>

int name_length(char *name);

int main(void){
    char name[25];
    printf("Enter your name: \n");
    fgets(name, sizeof(name), stdin);
    name[strcspn(name, "\n")] = '\0';
    printf("Your name is %d letters long.\n", name_length(name));
    return 0;
}

int name_length(char *name){
    int i = 0;
    while(name[i] != '\0'){
        i++;
    }
    return i;
}
```

Code Execution:

```bash
chan@CMA:~/C_Programming/practice
$ make practice
cc     practice.c   -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
Enter your name: 
Chan Myae Aung
Your name is 14 letters long.
```

