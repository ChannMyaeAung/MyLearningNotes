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
      float b = 5.0;
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



### Basic Types

- All basic values in  C are numbers, but there are different kinds of numbers.
- As a principal, distinction, we have two different classes of numbers, each with two subclasses: **unsigned integers**, **signed integers**, **real floating-point numbers**, and **complex floating point numbers**.

| Class          | Class       | Systematic Name      | Other name          | Rank |
| -------------- | ----------- | -------------------- | ------------------- | ---- |
|                |             | **Bool**             | **bool**            | 0    |
|                |             | **unsigned char**    |                     | 1    |
| Integers       | Signed      | **unsigned short**   |                     | 2    |
|                |             | unsigned int         | unsigned            | 3    |
|                |             | unsigned long        |                     | 4    |
|                |             | unsigned long long   |                     | 5    |
|                | [Un] signed | **char**             |                     | 1    |
|                |             | **signed char**      |                     | 1    |
|                | Signed      | **signed short**     | **short**           | 2    |
|                |             | signed int           | signed or int       | 3    |
|                |             | signed long          | long                | 4    |
|                |             | signed long long     | long long           | 5    |
|                |             | float                |                     |      |
|                |             | double               |                     |      |
| Floating point |             | long double          |                     |      |
|                |             | float _Complex       | float complex       |      |
|                |             | double _Complex      | double complex      |      |
|                |             | long double _Complex | long double complex |      |

- Types in bold don't allow for arithmetic, they are promoted before doing arithmetic. (Bool(bool), unsigned char, unsigned short, char, signed char, signed short (short)).
- Type `char` is special since it can be unsigned or signed, depending on the platform.
- As we can see from the table, there are six types that we can't use directly for arithmetic, the so-called **narrow types** because they have a smaller range or size compared to other types. 
- They are promoted to one of the wider types before they are considered in an arithmetic expression. This promotion is part of the type conversion rules in C, often referred to as "integer promotion" and "usual arithmetic conversions".
- Nowadays, on any realistic platform, this promotion will be a **signed int** of the same value as the narrow type, regardless of whether the narrow type was signed.
- Before arithmetic, narrow integer types are promoted to **signed int**.

#### Promotion Rules

1. **Integer Promotion**:
   - All narrow integer types (`char`, `signed char`, `unsigned char`, `short`, `signed short`, `unsigned short`) are promoted to `int` if they can fit within the range of an `int`.
   - If they cannot fit within the range of an `int`, they are promoted to `unsigned int`.
2. **Usual Arithmetic Conversions**:
   - After integer promotion, if the operands are of different types, further conversions are applied to bring both operands to a common type.
   - For example, if one operand is `int` and the other is `float`, the `int` is converted to `float`.

#### Example

```C
int main(){
    char a = 5;
    short b = 10;
    int result = a + b;
    printf("Result: %d\n", result);
    return 0;
}
```

In this example:

- `a` is of type `char`.
- `b` is of type `short`.

Before performing the addition `a + b`:

- `a` is promoted to `int`.
- `b` is promoted to `int`.

The addition is then performed using the `int` type, and the result is stored in `result`, which is also of type `int`.

#### Output

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Result: 15
```



#### Unpromoted Types

The 12 remaining, unpromoted, types split nicely into the four classes.

"Unpromoted" types refer to those types that do not undergo promotion (such as integer promotion) when used in expressions. They can be used in their native form without being promoted to a larger type like `int`.

The remaining 12 types that are referred to as "unpromoted" in the context are:

1. **Floating-Point Types**:
   - **`float`**: A single-precision floating-point type.
   - **`double`**: A double-precision floating-point type.
   - **`long double`**: An extended-precision floating-point type.
2. **Complex Types** (from the C99 standard):
   - **`float complex`**: A complex number type based on `float`.
   - **`double complex`**: A complex number type based on `double`.
   - **`long double complex`**: A complex number type based on `long double`.
3. **Unsigned Integer Types**:
   - **`unsigned int`**: An unsigned integer type, typically the same size as `int`.
   - **`unsigned long`**: An unsigned long integer type, typically larger than `unsigned int`.
   - **`unsigned long long`**: An unsigned long long integer type, typically larger than `unsigned long`.
4. **Signed Integer Types**:
   - **`int`**: A standard integer type.
   - **`long`**: A long integer type, typically larger than `int`.
   - **`long long`**: A long long integer type, typically larger than `long`.

As we can see, each of the four classes of base types has three distinct unpromoted types.

```
signed char < short < int < long < long long
```

```
bool < unsigned char < unsigned short < unsigned < unsigned long < unsigned long long 
```

- For any arithmetic or comparison, the narrow unsigned types are promoted to `signed int` and not to `unsigned int`, as this diagram might suggest.
- The comparison of the ranges of `signed` and `unsigned` types is more difficult.
- An unsigned type can never include the negative values of a signed type.
- Use `size_t` for sizes, cardinalities, or ordinal numbers.
- Unsigned types are the most convenient types, since they are the only types that have an arithmetic that is defined consistently with mathematical properties: the modulo operation.
- They can't raise signals on overflow and can be optimized best.
- Use `unsigned` for small quantities that can't be negative.
- If our program really needs values that may be both positive and negative but don't have fractions, use a signed type.
  - Use `signed` for small quantities that bear a sign.
- Use `ptrdiff_t` for large differences that bear a sign.
- If we want to do fractional computation with a value such as 0.5 or 3.77189E+89, use floating-point types.
- Use `double` for floating-point calculations.
- Use `double complex` for complex calculations.
- The C standard defines a lot of other types, among them other arithmetic types that model special use cases.

| Type        | Header   | Context of definition     | Meaning                                    |
| ----------- | -------- | ------------------------- | ------------------------------------------ |
| `size_t`    | stddef.h |                           | type for "sizes" and cardinalities         |
| `ptrdiff_t` | stddef.h |                           | type for size differences                  |
| `uintmax_t` | stdint.h |                           | maximum width unsigned, preprocessor       |
| `intmax_t`  | stdint.h |                           | maximum width signed integer, preprocessor |
| `time_t`    | time.h   | time(0), difftime(t1, t0) | calendar time in seconds since epoch       |
| `clock_t`   | time.h   | clock()                   | processor time                             |

- The two types `time_t` and `clock_t` are used to handle times.
- They are semantic types because the precision of the time computation can be different from platform to platform.
- The way to have a time in seconds that can be used in arithmetic is the function `difftime`: it computes the difference of two timestamps.
- `clock_t` values present the platform's model of processor clock cycles, so the unit of time is usually much less than a second.
- `CLOCKS_PER_SEC` can be used to convert such values to seconds.

#### Code Example:

##### `ptrdiff_t`

`ptrdiff_t` is a signed integer type used to represent the difference between two pointers.

```C
#include <stdio.h>
#include <stddef.h>

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int *ptr1 = &arr[1]; // Points to 20
    int *ptr2 = &arr[4]; // Points to 50

    ptrdiff_t diff = ptr2 - ptr1; // Difference in elements

    printf("Difference between pointers: %td\n", diff); // Output: 3

    return 0;
}
```

##### `uintmax_t` and `intmax_t`

`uintmax_t` and `intmax_t` are the largest unsigned and signed integer types, respectively.

```C
#include <stdio.h>
#include <stdint.h>

int main() {
    uintmax_t max_unsigned = UINTMAX_MAX;
    intmax_t max_signed = INTMAX_MAX;
    intmax_t min_signed = INTMAX_MIN;

    printf("Maximum unsigned integer: %ju\n", max_unsigned);
    printf("Maximum signed integer: %jd\n", max_signed);
    printf("Minimum signed integer: %jd\n", min_signed);

    return 0;
}
```

```sh
Maximum unsigned integer: 18446744073709551615
Maximum signed integer: 9223372036854775807
Minimum signed integer: -9223372036854775808
```



##### `time_t`

time_t is used to represent calendar time.

```C
#include <stdio.h>
#include <time.h>

int main() {
    time_t current_time;
    time(&current_time);

    printf("Current time: %s", ctime(&current_time));

    return 0;
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Current time: Wed Aug 14 22:36:14 2024
```

- **`time_t current_time;`**: Declares a variable `current_time`of type `time_t` to store the current calendar time.
- **`time(&current_time)`**: Calls the `time` function to get the current calendar time and stores it in `current_time`.
- **`printf("Current time: %s", ctime(&current_time));`**: Converts the `current_time` to a human-readable string using `ctime` and prints it. `ctime` returns a string representing the local time based on the `current_time`.

#### `clock_t` and `CLOCKS_PER_SEC`

`clock_t` is used to represent processor time, and `CLOCKS_PER_SEC` is the number of clock ticks per second.

```C
#include <stdio.h>
#include <time.h>

int main() {
    clock_t start = clock();

    // Simulate some work with a sleep
    volatile int sum = 0;
    for (volatile int i = 0; i < 100000000; ++i){
        sum += i;
    }

    clock_t end = clock();
    double cpu_time_used = ((double) (end - start)) / CLOCKS_PER_SEC;

    printf("CPU time used: %f seconds\n", cpu_time_used);
    
    printf("Sum: %d\n", sum);

    return 0;
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
CPU time used: 0.000599 seconds
Sum: 1783293664

```

- **`clock_t start = clock();`**: This line records the current processor time at the start of the program.
- **Simulated Work**: The `for` loop with a volatile integer simulates some work by running a loop 1,000,000 times. The `volatile` keyword prevents the compiler from optimizing away the loop.
- **`clock_t end = clock();`**: This line records the processor time at the end of the simulated work.
- `double cpu_time_used = ((double) (end - start)) / CLOCKS_PER_SEC`: This line calculates the CPU time used by subtracting the start time from the end time and dividing by CLOCKS_PER_SEC to convert the clock ticks to seconds.

#### Summary

- **Narrow types** (`char`, `signed char`, `unsigned char`, `short`, `signed short`, `unsigned short`) are promoted to `int` or `unsigned int` before being used in arithmetic expressions.
- This promotion ensures that arithmetic operations are performed using types that can handle a wider range of values, reducing the risk of overflow and other issues.
- **Unpromoted types** are those that do not undergo such promotion and retain their native type.
- The "four classes" divide these unpromoted types into categories: floating-point, complex, unsigned integers, and signed integers. Each class contains exactly three types, providing a structured classification of these types in C.



### Specifying Values

Numerical constants (literals) can be specified in several ways:

1. **Decimal integer constant**: The most natural choice for most of us. (e.g 123).
1. **Octal integer constant**: This is specified by a sequence of digits, the first being 0 and the following between 0 and 7. For example, 077 has the value of 63. This type of specification merely has historical value and is rarely used nowadays. Only one octal literal is commonly used: 0 itself.
1. **Hexadecimal integer constant**: E.g 0xFFFF
1. **Decimal floating-point constant**: Quite familiar as the version that has a decimal point. But there is also the "scientific" notation with an exponent. In the general form, `mEe` is interpreted as m . 10<sup>e</sup>.
1. **Hexadecimal floating-point constants**: Usually used to describe floating-point values in a form that makes it easy to specify values that have exact representations. The general form 0XhPe is interpreted as h . 2<sup>e</sup>. Here, h is specified as a hexadecimal fraction. The exponent e is still specified as a decimal number.
1. **Integer character constant**: These are characters put between ' apostrophes, such as `'a'` or `'?'`. These have values that are only implicitly fixed by the C standard. For example, `'a'` corresponds to the integer code for the character `a` of the Latin alphabet. Among character constants, the `\` character has a special meaning. E.g `'\n'` for the newline character.
1. **String literals**: They specify text, such as that needed for the `printf` and `puts` functions. Again, the `\` character is special, as with character constants.

All but the last are numerical constants: they specify numbers.

- Numerical constants in C are fixed values that do not change during the execution of a program.

- String literals are an exception and can be used to specify text that is known at compile time. 



#### Consecutive String Literals are Concatenated

In C, if we place two or more string literals next to each other, the compiler automatically concatenates them into a single string. This can be useful for breaking long strings across multiple lines for better readability.

##### Example:

```C
#include <stdio.h>

int main() {
    // Two string literals are concatenated into one
    char *str = "Hello, "
                "world!";
    printf("%s\n", str); // Output: Hello, world!
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/test$ ./final
Hello, world!
```



#### Decimal integer constants are signed

- In C, when we write a decimal integer constant (e.g, 123, 456), it is treated as a signed integer by default. 
- This means it can represent both positive and negative values.

#### A decimal integer constant has the first of the three signed types that fits it.

1. **Three Signed Types:**
   - The three signed types in C are:
     1. `int`
     2. `long int`
     3. `long long int`
2. **First Type that Fits:**
   - When we write a decimal integer constant, the compiler determines its type based on its value. It chooses the first type from the list above that can represent the value without overflow.
   - For example:
     - If we write `123`, it fits within the range of `int`, so it is treated as an `int`.
     - If we write a larger number like `2147483648` (which exceeds the range of `int`but fits within `long int`), it is treated as a `long int`.
     - If we write an even larger number that exceeds the range of both `int` and `long int` but fits within `long long int`, it is treated as a `long long int`.

#### The same value can have different types.

- In C, the same value can indeed be interpreted as different types depending on the context. For example, the value `65` can be interpreted as an integer, or as the character `'A'` in ASCII.



#### Different Literals Can Have the Same Value

- For example, the integer literals `10`. `0xA`, and `012` all represent the same value, which is `10` in decimal.

```C
int main()
{
        int decimal = 10;
        int hexadecimal = 0xA;
        int octal = 012;

        printf("Decimal: %d\n", decimal);
        printf("Hexadecimal: %d\n", hexadecimal);
        printf("Octal: %d\n", octal);
        return 0;
}
```



```sh
chan@CMA:~/C_Programming/test$ ./final
Decimal: 10
Hexadecimal: 10
Octal: 10
```



#### The Effective Value of a Decimal Floating-Point Constant May Be Different from its Literal Value

- Floating-point constants in C may not be represented exactly due to the limitations of binary floating-point representation.
- For example, the literal `0.1` cannot be represented exactly in binary floating-point, so the effective value stored in the variable may be slightly different from `0.1`.



### Complex Constants

- Complex types are not necessarily supported by all C platforms. (Can be checked by `__STDC_NO_COMPLEX__`).
- To have full support of complex types, the header `complex.h` should be included.
- C provides no literals to specify constants of a complex type.
- It only has several macros that may ease the manipulation of these types.
- The first possibility to specify complex values is the macro `CMPLX`, which comprises two floating-point values, the real and imaginary parts, in one complex value.
- For example, `CMPLX(0.5, 0.5)` is a `double complex` value with the real and imaginary part of one-half.
- Analogously, there are `CMPLXF` for `float complex` and `CMPLXL` for `long double complex`.
- Another, more convenient, possibility is provided by the macro`I` represents a constant value of type `float complex` such that `I * I` has the value -1.
- `I` is reserved for the imaginary unit.



### Implicit conversions

Implicit conversion, also known as type coercion, is a feature in C where the compiler automatically converts one data type to another without explicit instruction from the programmer. This usually happens when different data types are used in expressions or function calls, and the compiler needs to ensure that the operations are performed correctly.

1. **Arithmetic Operations**:

- When performing arithmetic operations between different data types, the compiler converts the operands to a common type.
- For example, if you add an `int` and a `float`, the `int` is implicitly converted to a `float`.

```C
int a = 5;
float b = 3.2;
float result = a + b; // 'a' is implicitly converted to float
```

2. **Assignment**:

- When assigning a value of one type to a variable of another type, the value is implicitly converted to the type of the variable.

```C
double d = 3; // '3' is implicitly converted to double
```

3. **Function Calls**:

- When passing arguments to a function, if the argument types do not match the parameter types, implicit conversion may occur.

```C
void func(double x){
    // ...
}

int y = 10;
func(y); //'y' is implicitly converted to double
```

**Type Promotion**

Type promotion is a specific kind of implicit conversion where smaller integer types (like `char` and `short`) are promoted to a larger integer type (like `int` or `unsigned int`) when used in expressions.

#### Potential Issues with Implicit Conversion

1. **Loss of Precision**:

   - Converting from a larger type to a smaller type can result in loss of data.

   ```C
   double d = 3.14159;
   int i = d; 
   
   // 'd' is implicitly converted to int, resulting in loss of precision
   ```

2. **Unexpected Behavior:**

   - Implicit conversions can sometimes lead to unexpected results, especially when dealing with signed and unsigned types.

   ```C
   unsigned int u = 10;
   int i = -1;
   
   if(i < u){
       printf("i is less than u\n");
       // This may not behave as expected due to implicit conversion.
   }
   ```

   

#### Best Practices

- Be aware of the types you are working with and the potential for implicit conversions.
- Use explicit casting when necessary to make your intentions clear and avoid unexpected behavior.

```C
double d = 3.14159;
int i = (int)d; // Explicitly cast 'd' to int
```



#### Unary Operators in C

- In C, unary operators are operators that operate on a single operand.
- The unary `-` and `+` operators are used to indicate the sign of a number. 
- When applied to an operand, they result in the type of the promoted operand.

1. **Unary `-` (Negation)**:
   - This operator negates the value of its operand.
   - Example: `-x` where `x` is an operand.
2. **Unary `+` (Identity)**:
   - This operator returns the value of its operand without any change.
   - Example: `+x` where `x` is an operand.

#### Type Promotion

- When unary `-` or `+` is applied to an operand, the operand undergoes type promotion. 

- Type promotion is the process of converting a variable to a larger data type to ensure that operations are performed correctly.

#### Example

```C
#include <stdio.h>

int main() {
    char c = 'A'; // ASCII value 65
    int i = 10;
    float f = 3.14;

    // Unary - and + with type promotion
    int neg_c = -c; // char promoted to int, then negated
    int pos_c = +c; // char promoted to int, remains the same

    int neg_i = -i; // int remains int, then negated
    int pos_i = +i; // int remains int, remains the same

    float neg_f = -f; // float remains float, then negated
    float pos_f = +f; // float remains float, remains the same

    printf("neg_c: %d, pos_c: %d\n", neg_c, pos_c);
    printf("neg_i: %d, pos_i: %d\n", neg_i, pos_i);
    printf("neg_f: %.2f, pos_f: %.2f\n", neg_f, pos_f);

    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/test$ ./final
neg_c: -65, pos_c: 65
neg_i: -10, pos_i: 10
neg_f: -3.14, pos_f: 3.14
```

#### Explanation

1. **Unary `-` and `+` with `char`**:
   - `c` is a `char` with the value `'A'` (ASCII 65).
   - `-c` promotes `c` to `int`and then negates it, resulting in `-65`.
   - `+c` promotes `c` to `int` and keeps it the same, resulting in `65`.
2. **Unary `-` and `+` with `int`**:
   - `i` is an `int` with the value `10`.
   - `-i` negates `i`, resulting in `-10`.
   - `+i` keeps `i` the same, resulting in `10`.
3. **Unary `-` and `+` with `float`**:
   - `f` is a `float` with the value `3.14`.
   - `-f` negates `f`, resulting in `-3.14`.
   - `+f` keeps `f` the same, resulting in `3.14`.



This example demonstrates how unary `-` and `+` operators work and how type promotion affects their operands.



The type of an operand has an influence on the type of an operator expression such as `-1` or `-1U`: whereas the first is a **signed int**, the second is an **unsigned int**.

- The latter might be particularly surprising because an **unsigned int** has no negative values and so the value of `-1U` is a large positive integer.
- As we have discussed above, Unary `-` and `+` have the type of their promoted argument.
- So, these operators are examples where the type usually does not change.
- In case where they do change, we have to rely on C's strategy to do implicit conversions: that is, to move a value with a specific type to one that has another, desired type.



Consider the following examples, under the assumption that -2147483648 and 2147483648 are the minimal and maximal values of a **signed int**, respectively

|                                  |                                   |
| -------------------------------- | --------------------------------- |
| `double a = 1;`                  | Harmless; value fits type         |
| `signed short b = -1;`           | Harmless; value fits type         |
| `signed int c = 0x80000000;`     | Dangerous; value too big for type |
| `signed int d = -0x80000000;`    | Dangerous; value too big for type |
| `signed int e = -2147483648;`    | Harmless; value fits type         |
| `unsigned short g = 0x80000000;` | Loses information; has value 0    |

- The initializations of `a` and `b` are harmesss. The respective values are well in the range of the desired types, so the C compiler can convert them silently.

- The next two conversions for `c` and `d` are problematic.

- `0x80000000` is of type **unsigned int** and does not fit into a **signed int**.

- So, `c` receives a value that is implementation-defined, and we have to know what our platform has decided to do in such cases.

- It could just reuse the bit pattern of the value on the right or terminate the program.

- For the case of `d` , the situation is even more compilcated: `0x80000000` has the value 2147483648, and we might expect that `-0x80000000` is just -2147483648. But since effectively `-0x80000000` is again 2147483648, the same problem arises as for `c`.

- `e` is harmless, because we used a negated decimal literal -2147483648, which has type **signed long** and whose value effectively is -2147483648. 

- Since this value fits into a **signed int**, the conversion can be done with no problem.

- `g` is ambiguous in its consequences. A value that is too large for an unsigned type is converted according to the modulus.

- Here in particular, if we assume that the maximum value for **unsigned short** is 2<sup>16</sup> - 1, the resulting value is 0. 

- Whether or not such a "narrowing" conversion is the desired outcome is often difficult to tell.

  

#### Avoid narrowing conversions.

- A narrowing conversion occurs when a value of a larger data type is converted to a smaller data type, potentially leading to loss of data or precision. 
- For example, converting a `double` to an `int` can lose the fractional part, and converting an `int` to a `char` can lose significant bits if the `int` value is outside the range of the `char` type.

#### Don't use narrow types in arithmetic.

- Narrow types are data types that have a smaller range, such as `char` and `short`. 
- Using these types in arithmetic operations can lead to issues because they are often promoted to `int` or `unsigned int` during the operation, which can cause unexpected results.

#### Best Practices

1. **Use Wider Types**: Prefer using `int`or `double` for arithmetic operations to avoid type promotion and overflow issues.

2. **Explicit Casting**: When necessary, use explicit casting to handle conversions carefully.

   ```C
   double d = 3.14159;
   int i = (int)d; // Explicitly cast 'd' to 'int'
   ```




#### Avoid operations with operands of different signedness

- In C, integers can be either signed (e.g., `int`, `int8_t`) or unsigned (e.g., `unsigned int`, `uint8_t`).
- When we perform operations (like addition, subtraction, comparison) between signed and unsigned integers, the signed integer is implicitly converted to an unsigned integer.
- This can lead to unexpected results, especially if the signed integer is negative, because converting a negative signed integer to an unsigned integer results in a large positive value due to how integers are represented in memory.

```C
int a = -1;
unsigned int b = 1;

if (a < b) {
    // This condition is false because 'a' is converted to an unsigned integer,
    // resulting in a large positive value.
}
```



#### Use unsigned types whenever you can

- Unsigned types (e.g., `unsigned int`, `uint8_t`) can only represent non-negative values.
- Using unsigned types can help prevent bugs related to negative values and can make the intent of the code clearer (e.g., a variable that should never be negative).
- Unsigned types also have a larger positive range compared to their signed counterparts of the same size.

```C
uint8_t age = 25; 
// Age should never be negative, so using an unsigned type is appropriate.
```



#### Choose your arithmetic types such that implicit conversions are harmless

- Implicit conversions occur when we mix different types in an expression, and the compiler automatically converts one type to another.
- These conversions can sometimes lead to loss of precision or unexpected behavior.
- By carefully choosing types that naturally convert to each other without issues, we can avoid these problems.

```C
uint8_t smallNumber = 100;
uint16_t largerNumber = 1000;

// Adding these two variables is safe because both are unsigned and the result
// can be stored in a uint16_t without loss of precision.
uint16_t result = smallNumber + largerNumber;
```

#### Arrays with an incomplete type

- In C, an array with an incomplete type is an array whose size is not specified at the time of declaration. 
- This means that the compiler does not know how much memory to allocate for the array until the size is specified later.

##### Examples of Incomplete Type Arrays

1. **Declaration without Size:**

   - We can declare an array without specifying its size. This is often used in function parameters to indicate that the function will accept an array of any size.

     ```C
     void processArray(int arr[]);
     ```

2. **Extern Arrays:**

   - We can declare an array with the `extern` keyword without specifying its size. 
   - The size must be defined elsewhere in the program.

   ```C
   extern int numbers[];
   ```

3. **Flexible Array Members:**

   - In structures(`struct`), we can use flexible array members, which are arrays without a specified size. 
   - This is useful for creating structures that can hold a variable number of elements.

   ```C
   typedef struct {
       int count;
       int values[];
   } flexible_array_t;
   ```

##### Example of Incomplete Type Arrays

```C
#include <stdio.h>

// Function declaration with an incomplete type array
void printArray(int arr[], int size);

int main() {
    // Define the size of the array
    int size = 5;
    // Define and initialize the array
    int numbers[] = {1, 2, 3, 4, 5};

    // Call the function to print the array
    printArray(numbers, size);

    return 0;
}

// Function definition
void printArray(int arr[], int size) {
    for (int i = 0; i < size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
12345
```



#### Flexible Array Members Example

```C
#include <stdio.h>
#include <stdlib.h>

typedef struct {
    int count;
    int values[];
} flexible_array_t;

int main() {
    // Allocate memory for the structure and the flexible array
    int size = 5;
    flexible_array_t *fa = malloc(sizeof(flexible_array_t) + size * sizeof(int));
    fa->count = size;

    // Initialize the flexible array
    for (int i = 0; i < size; i++) {
        fa->values[i] = i + 1;
    }

    // Print the flexible array
    for (int i = 0; i < fa->count; i++) {
        printf("%d ", fa->values[i]);
    }
    printf("\n");

    // Free the allocated memory
    free(fa);

    return 0;
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
12345
```



#### All variables should be initialized

- Uninitialized variables can contain garbage values, leading to unpredictable behavior and bugs.

- Always initialize variables when you declare them to ensure they start with a known value.

- There are only a few exception to that rule: variable-length arrays (VLA) which don't allow for an initializer , and code that must be highly optimized.

- The latter mainly occurs in situations that use pointers.

- For most code that we are able to write so far, a modern compiler will be able to trace the origin of a value to its last assignment or its initialization. 

- Superfluous initializations or assignments will simply be optimized out.

- For scalar types such as integers and floating points, an initializer just contains an expression that can be converted to that type.

- Optionally, such an initializer expression may be surrounded with `{}`.

  ```C
  double a = 7.8;
  double b = 2 * a;
  double c = {7.8};
  double d = {0};
  ```

- Initializers for other types must have these `{}`.

- For example, array initializers container initializers for the different elements, each of which is followed by a comma.

```C
double A[] = {7.8, };
double B[3] = {2 * A[0], 7, 33};
double C[] = {[0] = 6, [3] = 1, };
```

|      | [0]            |
| ---- | -------------- |
| A    | **double** 7.8 |



|      | [0]             | [1]            | [2]             |
| ---- | --------------- | -------------- | --------------- |
| B    | **double** 15.6 | **double** 7.0 | **double** 33.0 |



|      | [0]            | [1]            | [2]            | [3]            |
| ---- | -------------- | -------------- | -------------- | -------------- |
| C    | **double** 6.0 | **double** 0.0 | **double** 0.0 | **double** 1.0 |



- Arrays that have an **incomplete type** because there is no length specification are completed by the initializer to fully specify the length.
- Here, `A` has only one element, where as `C` has four.
- For the first two initializers, the element to which the scalar initialization applies is deduced from the position of the scalar in the list: for example, `B[1]` is initialized to 7.
- Designated initializers as for `C` are by far preferable, since they make the code more robust against small changes in declarations.

#### Use designated initializers for all aggregate data types

- Aggregate data types include arrays, structs, and unions.
- Designated initializers allow you to initialize specific members of an aggregate data type, making the code more readable and less error-prone.

```C
typedef struct {
    float x;
    float y;
} coordinate_t;

coordinate_t point = { .x = 1.0F, .y = 2.0F };
```

