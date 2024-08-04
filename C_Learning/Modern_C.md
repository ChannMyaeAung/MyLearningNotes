# Modern C

- C is an imperative programming language.

- C is a compiled programming language.
  - The name of the compiler and its command-line arguments depend a lot on the `platform` on which we will be running our program
  - There is a simple reason for this: the target binary code is **platform dependent**: that is, its form and details depend on the computer on which we want to run it.
  - A PC has different needs than a phone, and our refrigerator doesn't speak the same language as our set-top box.
  - That's one of the reasons for C to exist.
  - C provides a level of abstraction for all the different machine-specific languages usually referred to as **assembler**.
  
  ---
  
  

### size_t

`size_t` is an unsigned data type defined in the C standard library. It is used to represent the size of objects in bytes and is the type returned by the `sizeof` operator. The exact type of `size_t` can vary between different platforms, but it is typically defined as an unsigned integer type that is capable of representing the size of the largest possible object.

**Key Points**

- **Unsigned**: `size_t` is always non-negative.
- **Platform-dependent**: The actual type of `size_t` can vary depending on the platform and compiler, but it is usually an alias for `unsigned int` or `unsigned long`.



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
    x *= (2.0 - prod); // Heron approximation
}
```

- `for(;;)` is equivalent to `while(true)`.
- In this example, we use a standard macro `fabs`, which comes with the `tgmath.` header. 
- It calculates the absolute value of a `double`.



### `Strtod` function

In C, the `strtod` function is used to convert a string to a double-precision floating-point number (`double`). It is part of the standard library and is declared in the `<stdlib.h>` header file.

#### Function Prototype

```C
double strtod(const char *nptr, char **endptr);
```

#### Parameters

- `nptr`: A pointer to the null-terminated string to be converted.
- `endptr`: A pointer to a pointer to character. This parameter is optional and can be `NULL`. If it is not `NULL`, `strtod` sets `*endptr` to point to the character after the last character used in the conversion.

#### Return Value

- The function returns the converted value as a `double`.
- If no valid conversion could be performed, it returns `0.0`.
- If the value is out of range for a `double`, it returns `HUGE_VAL`, `-HUGE_VAL`, or `0.0` and sets `errno` to `ERANGE`.

#### Example Usage

```C
#include <stdio.h>

#include <stdlib.h>

int main() {

  const char *str = "123.45abc";

  char *endptr;

  double value;

  value = strtod(str, &endptr);

  printf("Converted value: %f\n", value);

  printf("Remaining string: %s\n", endptr);

  return 0;

}
```

#### Explanation

- The string `"123.45abc"` is converted to the double value `123.45`.
- `endptr` points to the remaining part of the string `"abc"` after the numeric conversion.

This function is useful for parsing strings that contain numeric values, especially when dealing with user input or reading data from files.



### Expressing Computations

- The type `size_t` represents values in the range `[0, SIZE_MAX]`.
- The value of `SIZE_MAX` is quite large. Depending on the platform, it is one of 
  - 2<sup>16</sup> - 1 = 65535
  - 2<sup>32</sup> - 1 = 4294967295
  - 2<sup>64</sup> - 1 = 18446744073709551615
- The concept of "numbers that cannot be negative" to which we referred for `size_t` corresponds to what C calls **unsigned integer types**.
- Symbols and combinations like + and != are called **operators**, and the things to which they are apply to are called **operands**.
- So, in something like a + b, + is the **operator** and a and b are its **operands**.

#### The result of unsigned / and % is always smaller than the operands thus Unsigned / and % can't overflow.

##### Theory:

1. Division (`/`): For any unsigned integers `a` and `b` where `b > 0`, the result of `a / b` is always less than or equal to `a`.
2. Modulus (`%`): For any unsigned integers `a` and `b` where `b > 0`, the result of `a % b` is always less than `b`.

`practice.c`

```C
int main()
{
    uint64_t a = 100;
    uint64_t b = 7;

    uint64_t div_result = a / b;
    uint64_t mod_result = a % b;

    printf("a = %lu, b = %lu\n", a, b);
    printf("a / b = %lu\n", div_result);
    printf("a %% b = %lu\n", mod_result);

    // Check the theory
    if (div_result <= a)
    {
        printf("The result of a / b is always less than or equal to a.\n");
    }
    else
    {
        printf("The result of a / b is greater than a (unexpected).\n");
    }

    if (mod_result < b)
    {
        printf("The result of a %% b is always less than b.\n");
    }
    else
    {
        printf("The result of a %% b is greater than or equal to b (unexpected).\n");
    }

    return 0;
}
```

`Code Execution`:

```sh
chan@CMA:~/C_Programming/practice$ ./practice
a = 100, b = 7
a / b = 14
a % b = 2
The result of a / b is always less than or equal to a.
The result of a % b is always less than b.
```

- This output confirms that the results of the division and modulus operations for unsigned integers are always within the expected bounds, demonstrating that these operations cannot overflow.

#### Comparison operators return the value `false` or `true`

- `false` and `true` are nothing more than fancy names for `0` and `1`, respectively.
- So, they can be used in arithmetic or for array indexing.
- In the following code, `c` will always be `1` and `d` will be `1` if `a` and `b` are equal and `0` otherwise:

```C
size_t c = (a < b) + (a == b) + (a > b);
size_t d = (a <= b) + (a >= b) - 1;
```



