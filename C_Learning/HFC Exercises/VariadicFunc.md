#### Variadic Functions

**Theories and Code Breakdown are in `HFCTheories.md`.**

functions.c

```C
#include <stdarg.h>
#include "practice.h"

void print_ints(int args,...){
    va_list ap;
    va_start(ap, args);
    int i;
    for(i = 0; i < args; i++){
        printf("arguments: %i\n", va_arg(ap, int));
    }
    va_end(ap);
}
```

practice.h

```C
void print_ints(int args, ...);
```

practice.c (main)

```C
#include "practice.h"

int main(){
    print_ints(3, 79, 101, 32);
    return 0;
}
```

Code Execution:

```bash
chan@CMA:~/C_Programming/practice
$ make all
Compiling practice file...
gcc -c practice.c 
Compiling functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc practice.o functions.o -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
arguments: 79
arguments: 101
arguments: 32

```



#### Exercise - 5

The guys in the Head First Lounge want to create a function that can return the total cost of a round of drinks:

functions.c

```C
#include <stdarg.h>
#include "practice.h"

double price(enum drink d){
    switch(d){
        case MUDSLIDE:
            return 6.79;
        case FUZZY_NAVEL:
            return 5.31;
        case MONKEY_GLAND:
            return 4.82;
        case ZOMBIE:
            return 5.89;
    }
    return 0;
}

double total(int args, ...){
    double total = 0;
    va_list ap;
    va_start(ap, args);
    int i;
    for(i = 0; i < args; i++){
        enum drink d = va_arg(ap, enum drink);
        total += price(d);
    }
    va_end(ap);
    return total;
}
```

practice.h

```C
enum drink{
    MUDSLIDE,
    FUZZY_NAVEL,
    MONKEY_GLAND,
    ZOMBIE
};

double price(enum drink d);

double total(int args, ...);
```

practice.c (main)

```C
#include "practice.h"

int main(){
    printf("Price is %.2f\n", total(2, MONKEY_GLAND, MUDSLIDE));
    printf("Price is %.2f\n", total(3, MONKEY_GLAND, MUDSLIDE, FUZZY_NAVEL));
    printf("Price is %2.f\n", total(1, ZOMBIE));
    return 0;
}
```

Code Execution

```bash
chan@CMA:~/C_Programming/practice
$ make all
Compiling practice file...
gcc -c practice.c 
Compiling the final exe file...
gcc practice.o functions.o -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
Price is 11.61
Price is 16.92
Price is 5.89

chan@CMA:~/C_Programming/practice
$ 

```

---