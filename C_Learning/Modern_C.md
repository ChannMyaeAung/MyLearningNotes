# Modern C

- C is an imperative programming language.
- C is a compiled programming language.
  - The name of the compiler and its command-line arguments depend a lot on the `platform` on which we will be running our program
  - There is a simple reason for this: the target binary code is **platform dependent**: that is, its form and details depend on the computer on which we want to run it.
  - A PC has different needs than a phone, and our refrigerator doesn't speak the same language as our set-top box.
  - That's one of the reasons for C to exist.
  - C provides a level of abstraction for all the different machine-specific languages usually referred to as **assembler**.

### First Example of a C program

##### `main.c`



```C
#include <stdio.h>
#include <stdlib.h>

int main(void){
    // Declarations
    double A[5] = {
        [0] = 9.0,
        [1] = 2.9,
        [4] = 3.E+25, // (3 * 10^25)
        [3] = .00007,
    }
    
    // Doing some work
    for(size_t i = 0; i < 5; ++i){
        printf("element %zu is %g, \tits square is %g\n", i, A[i], A[i] * A[i]);
    }
    return EXIT_SUCCESS;
}
```

##### Explanation 

- First, we declare an array of `A` of type `double` with **5 elements**.
- The array is initialized using designated initializers:
  - `A[0]` is set to `9.0`
  - `A[1]` is set to `2.9`
  - `A[4]` is set to `3.E+25` (which is `3 * 10^25`)
  - `A[3]` is set to `0.00007`
  - `A[2]` is implicitly initialized to `0.0` because it is not explicitly set.
- Then we have a **for loop** that iterates over the array `A` from index `0` to `4`.
- `size_t` is an unsigned data type used for array indexing and loop counting.
- Inside the loop, `printf` is used to print:
  - The index `i` of the current element.
  - The value of the current element `A[i]`.
  - The square of the current element `A[i] * A[i]`.
- Then, we return `EXIT_SUCCESS` from the main function, indicating that the program executed successfully.
- `EXIT_SUCCESS` is typically defined in `stdlib.h`.
- `%zu` - This format specifier is used in `printf` to print a value of type `size_t` which is an unsigned integer type used for array indexing and loop counting. It ensures that the correct format is used regardless of the platform (32-bit or 64-bit).
- `%g` - This format specifier is used to print floating-point numbers. It automatically chooses between `%f`(fixed-point notation) and `%e` (exponential notation) based on the value's magnitude and precision. It provides a more compact representation of the number.
- Functions (`main` and `printf`) and Constants such as `EXIT_SUCCESS`.

##### `Makefile`

```makefile
# Compiler and standard
CC = clang
STD = -std=c18
# Compiler flags
CFLAGS = -Wall -Wextra -g 
LIBS = -lpthread -lm -lssl -lcrypto
# Directories
LIBDIR = ./libs
OBJDIR = ./obj


all:practice

practice: $(OBJDIR)/practice.o $(LIBDIR)/libfunctions.a
	$(CC) $(STD) $(OBJDIR)/practice.o -L$(LIBDIR) -lfunctions -o practice $(LIBS)

$(OBJDIR)/practice.o: practice.c practice.h 
	$(CC) $(STD) $(CFLAGS) -c practice.c -o $(OBJDIR)/practice.o 

$(OBJDIR)/functions.o: functions.c practice.h
	$(CC) $(STD) $(CFLAGS) -c functions.c -o $(OBJDIR)/functions.o 

$(OBJDIR)/functions_2.o: functions_2.c practice.h 
	$(CC) $(STD) $(CFLAGS) -c functions_2.c -o $(OBJDIR)/functions_2.o

$(LIBDIR)/libfunctions.a: $(OBJDIR)/functions.o $(OBJDIR)/functions_2.o 
	ar -rcs $(LIBDIR)/libfunctions.a $(OBJDIR)/functions.o $(OBJDIR)/functions_2.o

clean:
	@echo "Removing everything except the source files..."
	rm -f $(OBJDIR)/*.o $(LIBDIR)/libfunctions.a practice

```



##### Code Execution

```sh
chan@CMA:~/C_Programming/practice$ ./practice
element 0 is 9, 	its square is 81
element 1 is 2.9, 	its square is 8.41
element 2 is 0, 	its square is 0
element 3 is 7e-05, 	its square is 4.9e-09
element 4 is 3e+25, 	its square is 9e+50

```

- `7e-05` - This is the value of `A[3]`, which is `0.00007`. This is a scientific notation, where `e` stands for "exponents". It means `7 * 10^(-5)`.
- Since the value `0.00007` squared is `0.00007 * 0.00007 = 0.0000000049`, we can see that the result is `4.9e-09` which is equivalent to `4.9 * 10^(-9)`.
- Similarly, `3e+25` stands for `3 * 10^(25)`, and its square value `9e+50` stands for `9 * 10^(50)`.
- We can see a pattern here. The value `e` will be equal to 10 to the power of its value after. For example, e-05 means 10 to the power of minus 5 since it is -05 and e+25 is 10 to the power of 25 since it is +25.



### Conditional Execution

```C
if(i > 25){
    j = i - 25;
}
```

- In this example, `i > 25` is called the **controlling expression**.
- the part in `{...}` is called the **dependent block**.



- The value 0 represents logical false.
- Any value different from 0 represents logical true.

- We can rewrite:

```C
if(i != 0){
    ...
}
```

as:

```C
if(i){
    ...
}
```

- The type `bool` specified in `stdbool.h` is what we should be using if we want to store truth values.
- Its values are `false` and `true`.
- Technically, `false` is just another name for 0 and `true` for 1.
- We can avoid redundancy by rewriting something like:

```C
bool b = ...;
if((b != false) == true){
    ...
}
```

as

```C
bool b = ...;
if(b){
    
}
```



```C
for(;;){
    double prod = a * x;
    if(fabs(1.0 - prod) < eps){
        break;
    }
    x *= (2.0 - prod);
}
```

