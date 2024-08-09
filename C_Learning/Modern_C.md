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



### & and && vs | and ||

In C, `&` and `&&` as well as `|` and `||` serve different purposes:

#### `&` vs `&&`

- **`&` (Bitwise AND)**: This operator performs a bitwise AND operation on two integers. Each bit in the result is set to `1` if the corresponding bits of both operands are `1`, otherwise it is set to `0`.

  ```C
  int a = 5; // 0101 in binary
  
  int b = 3; // 0011 in binary
  
  int result = a & b; // result is 1 (0001 in binary)
  ```

  

- **`&&` (Logical AND)**: This operator performs a logical AND operation. It evaluates to `true` (non-zero) if both operands are true (non-zero), otherwise it evaluates to `false` (zero). It also short-circuits, meaning if the first operand is false, the second operand is not evaluated.

  ```C
  int a = 5;
  
  int b = 3;
  
  if (a && b) {
  
    // This block will execute because both a and b are non-zero
  
  }
  ```

  

#### `|` vs `||`

- **`|` (Bitwise OR)**: This operator performs a bitwise OR operation on two integers. Each bit in the result is set to `1` if at least one of the corresponding bits of the operands is `1`.

  ```C
  int a = 5; // 0101 in binary
  
  int b = 3; // 0011 in binary
  
  int result = a | b; // result is 7 (0111 in binary)
  ```

  

- **`||` (Logical OR)**: This operator performs a logical OR operation. It evaluates to `true` (non-zero) if at least one of the operands is true (non-zero), otherwise it evaluates to `false` (zero). It also short-circuits, meaning if the first operand is true, the second operand is not evaluated.

  ```C
  int a = 5;
  
  int b = 0;
  
  if (a || b) {
  
    // This block will execute because a is non-zero
  
  }
  ```

  

#### Summary

- **`&`**: Bitwise AND
- **`&&`**: Logical AND (short-circuits)
- **`|`**: Bitwise OR
- **`||`**: Logical OR (short-circuits)



- The state of the program execution is determined by:
  - The executable
  - The current point of execution
  - The data
  - Outside intervention, such as IO from the user

### Bitwise Operators

Special operators used in bit level programming.

- &  = AND
- | = OR
- ^ = XOR
- <<  left shift
- `>>` Right shift

```C
int x = 6; //  00000110 in binary
int y = 12; // 00001100 in binary
int z = 0;   
z = x & y; //  00000100 in binary           
printf("AND = %d\n",z)
```

| 0    | 0    | 0    | 0    | 0    | 1    | 1    | 0    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 0    | 0    | 0    | 1    | 1    | 0    | 0    |
| 0    | 0    | 0    | 0    | 0    | 1    | 0    | 0    |

```sh
chan@CMA:~/C_Programming/test$ ./final
AND = 4
```

Since we have learned `AND` and `OR`, we will skip to the XOR.

`XOR`

The XOR operation compares each bit of its operands and returns `1` if the bits are different, and `0` if they are the same.

```C
#include <stdio.h>

int main()
{
    int x = 6;  // 6  = 00000110
    int y = 12; // 12 = 00001100
    int z = 0;  //      00001010

    z = x ^ y;
    printf("XOR = %d\n", z);
    return 0;
}
```

| 0    | 0    | 0    | 0    | 0    | 1    | 1    | 0    |
| ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
| 0    | 0    | 0    | 0    | 1    | 1    | 0    | 0    |
| 0    | 0    | 0    | 0    | 1    | 0    | 1    | 0    |

```sh
chan@CMA:~/C_Programming/test$ ./final
XOR = 10
```



`<<` Left shift

It shifts the bits of its left-hand operand to the left by the number of positions specified by its right-hand operand. Each left shift operation effectively **multiplies** the number by 2.

#### Syntax 

```
result = value << number_of_positions;
```



```C
int main()
{
    int x = 6;  // 6  = 00000110
    int y = 12; // 12 = 00001100
    int z = 0;  //      

    z = x << 1;
    printf("Left Shift = %d\n", z);
    return 0;
}
```

6 = 0000 0110, 6 << 1;

**Shifting the above value (`6`) one spot to the left, and then adding zero at the end.**

0000 1100, it became 12.

| 0                | 0    | 0    | 0    | 0    | 1    | 1    | 0                   |
| ---------------- | ---- | ---- | ---- | ---- | ---- | ---- | ------------------- |
| shift this value |      |      |      |      |      |      | add zero at the end |
| 0                | 0    | 0    | 0    | 1    | 1    | 0    | 0                   |

```sh
chan@CMA:~/C_Programming/test$ ./final
Left Shift = 12
```

- We can shift twice,  means we are just moving one more space to the left.

Shifting the value (`6`) one more spot to the left, and then adding zero at the end.

12 = 0000 1100, 6 << 2;

0001 1000, it became 24.

```C
int main()
{
    int x = 6;  // 6 = 00000110
    int y = 12; // 12= 00001100
    int z = 0;  //  4 =00000100

    z = x << 2;
    printf("Left Shift = %d\n", z);
    return 0;
}
```

```sh
chan@CMA:~/C_Programming/test$ ./final
Left Shift = 24
```



`>>` Right shift

It shifts the bits of its left-hand operand to the right by the number of positions specified by its right-hand operand. Each left shift operation effectively **divides** the number by 2.

```C
int main()
{
    int x = 6;  // 6 = 00000110
    int y = 12; // 12= 00001100
    int z = 0;  //  4 =00000100

    z = x >> 1;
    printf("Right Shift = %d\n", z);
    return 0;
}
```

6 = 0000 0110

**Shifting the above value (`6`) one spot to the right, and then adding zero at the beginning.**

0000 0011, it became 3.

| 0                         | 0    | 0    | 0    | 0    | 1    | 1    | 0                |
| ------------------------- | ---- | ---- | ---- | ---- | ---- | ---- | ---------------- |
| add zero at the beginning |      |      |      |      |      |      | shift this value |
| 0                         | 0    | 0    | 0    | 0    | 0    | 1    | 1                |

```sh
chan@CMA:~/C_Programming/test$ ./final
Right Shift = 3
```



If we shift right again, 

```C
z = x >> 2;
```

0000 0011,

we will shift these bits one more spot to the right, and then truncate the value at the end.

0000 0001, it became 1

| 0                                                            | 0    | 0    | 0    | 0    | 0    | 1    | 1                           |
| ------------------------------------------------------------ | ---- | ---- | ---- | ---- | ---- | ---- | --------------------------- |
| add one more zero at the beginning since we are right shifting two times. |      |      |      |      |      |      | shift this value (truncate) |
| 0 (this is extra zero added)                                 | 0    | 0    | 0    | 0    | 0    | 0    | 1                           |

```sh
chan@CMA:~/C_Programming/test$ ./final
Right Shift = 1
```



### Types

A type is an additional property that C associates with values. E.g, `size_t`, `double`, `bool`.

1. **All values have a type that is statically determined.**
   - In C, every value has a specific type that is determined at compile time, not at runtime. 
   - This is known as static typing. 
   - The type of a value dictates how much memory is allocated for it and how it is interpreted by the compiler.

2. **Possible operations on a value are determined by its type.**
   - The type of a value dictates what operations can be performed on it. 
   - For example, arithmetic operations can be performed on integers and floating-point numbers, but not on characters or pointers (without specific casting or context).

```C
int x = 5;
int y = 10;
int sum = x + y;  // Valid: Addition operation on integers

char ch = 'A';
// int invalid = ch + "Hello";  // Invalid: Addition operation between char and string
```

- `int sum = x + y;` is valid because addition is a defined operation for integers.
- `int invalid = ch + "Hello";` is invalid because addition is not a defined operation between a `char` and a string literal.

3. **A value's type determines the results of all operations.**
   - The type of a value not only determines what operations can be performed on it but also how those operations are executed and what the results will be. 
   - Different types can yield different results for the same operation due to differences in how they are represented and processed in memory.

```C
int main()
{
    int a = 5;
    int b = 2;
    float c = 5.0;
    float d = 2.0;

    int int_div = a / b;     // Integer division
    float float_div = c / d; // floating-point division

    printf("%d\n", int_div);
    printf("%f\n", float_div);
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/test$ ./final
2
2.500000
```



- `int int_div = a / b;` results in `2` because integer division truncates the decimal part.

- `float float_div = c / d;` results in `2.500000` because floating-point division retains the decimal part.

4. **A type's binary representation determines the results of all operations**

   - In C, the way a type is represented in binary (its binary representation) directly affects the results of operations performed on values of that type. 

   - This is because the CPU and the compiler interpret and manipulate data based on its binary form.

   - ```C
     int a = 5;       // Binary: 00000000 00000000 00000000 00000101
     int b = 2;       // Binary: 00000000 00000000 00000000 00000010
     int int_result = a / b;  // Integer division
     
     float x = 5.0;   // Binary (IEEE 754): 01000001 01000000 00000000 00000000
     float y = 2.0;   // Binary (IEEE 754): 01000000 00000000 00000000 00000000
     float float_result = x / y;  // Floating-point division
     ```

   - **Integer Division**: The binary representation of integers is straightforward, and integer division truncates the decimal part, resulting in `2` for `int_result`.

   - **Floating-Point Division**: The binary representation of floating-point numbers follows the IEEE 754 standard, which includes a sign bit, exponent, and mantissa. Floating-point division retains the decimal part, resulting in `2.5` for `float_result`.

   

5. **A type's binary representation is observable.**

   - The binary representation of a type can be observed and manipulated directly, often for debugging or low-level programming purposes. 
   - This can be done using techniques like type casting, bitwise operations, or by examining memory directly.

   `Example`

   ```C
   void print_binary(int n)
   {
       for (int i = 31; i >= 0; i--)
       {
           int bit = (n >> i) & 1;
           printf("%d", bit);
           if (i % 8 == 0 && i != 0)
           {
               // Add a space after every 8 bits, except at the end
               printf(" ");
           }
       }
       printf("\n");
   }
   
   int main()
   {
       int a = 5;
       printf("Binary representation of %d: ", a);
       print_binary(a);
   
       float b = 5.0;
       int *p = (int *)&b; // Type punning to observe binary representation
       printf("Binary representation of %f: ", b);
       print_binary(*p);
       return 0;
   }
   ```

   `Output`

   ```sh
   chan@CMA:~/C_Programming/test$ ./final
   Binary representation of 5: 00000000 00000000 00000000 00000101
   Binary representation of 5.000000: 01000000 10100000 00000000 00000000
   ```

   - **Integer Binary Representation**: The `print_binary` function prints the binary representation of an integer.
   - **Floating-Point Binary Representation**: By type punning (casting the address of a float to an int pointer), you can observe the binary representation of a floating-point number.

6. **Type Punning**

   - **Type punning**: Type punning is a technique in C and C++ where a variable of one type is accessed as if it were a different type. 
   - This is often done using pointers or unions to reinterpret the underlying binary representation of the data. 
   - Type punning can be useful for low-level programming tasks, such as interpreting the binary representation of floating-point numbers or manipulating hardware registers.

   `Explanation`

   1. Type Punning with Pointers:

      ```C
      float b - 5.0;
      int *p = (int *)&b;
      ```

      - Here, the address of the `float` variable `b` is  cast to an `int` pointer. This means that `p` now points to the same memory location as `b` but it is treated as if it points to an `int`.

      

   2. Dereferencing the Pointer:

      ```C
      print_binary(*p);
      // Refer to the above function in No.5
      ```

      - Dereferencing `p` (`*p`) accesses the binary representation of `b` as if it were an `int`. This allows us to pass the binary representation of the `float` to the `print_binary` function, which expects an `int`.

#### Why Use Type Punning?

- **Inspecting Binary Representation**: It allows you to inspect the binary representation of different data types, which can be useful for debugging or understanding how data is stored in memory.
- **Low-Level Programming**: It is often used in systems programming, embedded programming, and other low-level tasks where direct memory manipulation is required.

#### Caution

Type punning can lead to undefined behavior if not used carefully, especially when dealing with strict aliasing rules in C. It is important to ensure that the memory layout of the types involved is compatible and that the operation is supported by the compiler and platform.

7. 
