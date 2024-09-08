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

#### Designated Initializers

- Designated initializers in C allow us to initialize specific elements of an array or members of a structure by specifying their indices or names. 
- This feature was introduced in the C99 standard. 
- Designated initializers improve code readability and maintainability by making it clear which elements or members are being initialized.

##### Example with Arrays

```C
int arr[5] = { [0] = 1, [2] = 3, [4] = 5 };
// This initializes arr[0] to 1, arr[2] to 3, and arr[4] to 5. The other elements are initialized to 0.
```

##### Example with Structures

```C
struct Point {
    int x;
    int y;
};

int main()
{
    // This initializes p.x to 10 and p.y to 20.
    struct Point p = {.x = 10, .y = 20};

    printf("x: %d\n", p.x);
    printf("y: %d\n", p.y);
    return 0;
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
x: 10
y: 20
```

##### Example with Nested Structures

```C
struct Inner {
    int a;
    int b;
};

struct Outer {
    struct Inner inner;
    int c;
};

struct Outer o = { .inner = { .a = 1, .b = 2 }, .c = 3 };
// This initializes o.inner.a to 1, o.inner.b to 2, and o.c to 3.
```

- Designated initializers make it easier to understand and maintain the code, especially when dealing with large structures or arrays.

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

#### {0} is a valid initializer for all object types that are not VLA

- In C, the initializer `{0}` can be used to initialize any object type that is not a Variable Length Array (VLA). 
- This is because `{0}` is a shorthand for zero-initialization, which sets all elements of an array or all members of a structure to zero (or the equivalent zero value for the type).
- The compiler might warns us about this, some compiler implementers don't know about this special rule.
- It is explicitly designed as a catch-all initializer in the C Standard.

##### Examples:

**For Arrays**:

```C
int arr[5] = {0}; // All elements of arr are initialized to 0.
```

**For Structures**:

```C
struct Point {
    int x;
    int y;
};

struct Point p = {0}; // Both p.x and p.y are initialized to 0.
```

**For Scalars:**

```C
int num = {0}; // num is initialized to 0.
```

---

## Named Constants

- Named constants in C are typically defined using the `#define` preprocessor directive or the `const` keyword. 
- They provide a way to give meaningful names to constant values, improving code readability and maintainability.

#### Using `#define`:

```C
#define PI 3.14159
#define MAX_SIZE 100

int main() {
    double radius = 5.0;
    double area = PI * radius * radius;
    int arr[MAX_SIZE];
    return 0;
}
```

#### Using `const`:

```C
const double PI = 3.14159;
const int MAX_SIZE = 100;

int main() {
    double radius = 5.0;
    double area = PI * radius * radius;
    int arr[MAX_SIZE];
    return 0;
}
```



#### All constants with a particular meaning must be named

- Any constant value used in our code should be given a meaningful name. 
- This practice improves code readability and makes it easier to understand the purpose of the constant. 
- It also makes the code easier to maintain, as changes to the constant value need to be made in only one place.

##### Example

Instead of using a magic number:

```C
double area = 3.14159 * radius * radius;
```

Use a named constant:

```C
#define PI 3.14159
double area = PI * radius * radius;
```



#### All constants with different meanings must be distinguished"

- Constants with different purposes should have distinct names, even if they have the same value. 
- This helps avoid confusion and makes the code more understandable by clearly indicating the role of each constant.

##### Example:

Even if two constants have the same value, they should be named according to their specific meanings:

```C
#define MAX_BUFFER_SIZE 1024
#define DEFAULT_TIMEOUT 1024

char buffer[MAX_BUFFER_SIZE];
int timeout = DEFAULT_TIMEOUT;
```

- In this example, `MAX_BUFFER_SIZE` and `DEFAULT_TIMEOUT` both have the value `1024`, but they serve different purposes. Naming them differently clarifies their roles in the code.

```C
char const *const bird[3] = {
    "raven",
    "magpie",
    "jay"};

char const *const pronoun[3] = {
    "we",
    "you",
    "they"};

char const *const ordinal[3] = {
    "first",
    "second",
    "third"};

int main()
{
    for (unsigned i = 0; i < 3; ++i)
    {
        printf("Corvid %u is the %s\n", i, bird[i]);
    }

    for (unsigned i = 0; i < 3; ++i)
    {
        printf("%s plural pronoun is %s\n", ordinal[i], pronoun[i]);
    }
    return 0;
}

```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Corvid 0 is the raven
Corvid 1 is the magpie
Corvid 2 is the jay
first plural pronoun is we
second plural pronoun is you
third plural pronoun is they
```

- Defining `char const *const` (or equivalently `const char *const`) for arrays of strings ensures both the pointers and the strings they point to are immutable. 

  Let's break down the syntax:

  ###### `char const *const bird[3]`

  - `char const *`: This means that the characters in the string are constant. We cannot modify the characters in the string through this pointer.
  - `const char *`: This is another way to write the same thing as `char const *`. It means the same thing: the characters are constant.
  - `*const`: This means that the pointer itself is constant. We cannot change the pointer to point to a different string.

##### Why Use `char const *const`?

1. **Immutable Strings**: By using `char const *`, we ensure that the strings themselves cannot be modified. This is useful for string literals, which should not be altered.
2. **Immutable Pointers**: By using `*const`, we ensure that the pointers themselves cannot be changed to point to different strings. This adds an extra layer of safety, ensuring that the array structure remains consistent throughout the program.

According to the above code, we have arrays of strings defined as `char const *const`:

```C
char const *const bird[3] = {
    "raven",
    "magpie",
    "jay"};

char const *const pronoun[3] = {
    "we",
    "you",
    "they"};

char const *const ordinal[3] = {
    "first",
    "second",
    "third"};
```

- **`bird[3]`**: An array of 3 constant pointers to constant characters. Neither the pointers nor the strings they point to can be modified.
- **`pronoun[3]`**: Similarly, an array of 3 constant pointers to constant characters.
- **`ordinal[3]`**: Again, an array of 3 constant pointers to constant characters.

##### Benefits

- **Safety**: Prevents accidental modification of string literals and pointers.
- **Clarity**: Makes it clear to anyone reading the code that these strings and pointers are not meant to be changed.

### Macros

- Macros in C are a feature of the C preprocessor that allows us to define constants, functions, or code snippets that can be reused throughout our code. 
- They are defined using the `#define` directive and are replaced by their corresponding values or code during the preprocessing phase, before the actual compilation of the code begins.

#### Type of Macros

1. **Object-like Macros**: These are simple constants.

   ```C
   #define PI 3.14159
   ```

2. **Function-like Macros**: These take arguments and can be used like functions.

   ```C
   #define SQUARE(x) ((x) * (x))
   ```



#### Example Usage

```C
#include <stdio.h>

#define PI 3.14159
#define SQUARE(x) ((x) * (x))

int main() {
    double radius = 5.0;
    double area = PI * SQUARE(radius);

    printf("Area of the circle: %f\n", area);
    return 0;
}
```

#### Key Points

- **Preprocessing**: Macros are expanded during the preprocessing phase, before the actual compilation.
- **No Type Checking**: Unlike functions, macros do not perform type checking, which can lead to unexpected behavior if not used carefully.
- **Parentheses**: It's important to use parentheses in macros to ensure correct order of operations.

#### Advantages

- **Code Reusability**: Macros can make code more reusable and easier to maintain.
- **Performance**: Since macros are expanded inline, they can sometimes be faster than function calls.

#### Disadvantages

- **Debugging**: Macros can make debugging more difficult because they are expanded before compilation.
- **No Type Safety**: Macros do not provide type safety, which can lead to errors.

#### Summary

- Macros in C are a powerful tool for defining constants and reusable code snippets. 
- They are expanded during the preprocessing phase and can improve code reusability and performance.
- But they should be used with caution due to potential issues with debugging and type safety.

---



## Read-only objects

- Don't confuse the term `constant`, which has a very specific meaning in C, with objects that can't be modified.

```C
char const *const bird[3] = {
    "raven",
    "magpie",
    "jay"};

char const *const pronoun[3] = {
    "we",
    "you",
    "they"};

char const *const ordinal[3] = {
    "first",
    "second",
    "third"};
```

- `bird`, `pronoun` and `ordinal` are not constants according to our terminology; they are **const-qualitifed** objects.
- This qualifier specifies that we don't have the right to change this object.
- For `bird`, neither the array entries nor the actual strings can be modified, and the compiler will give us a diagnostic if we try to do so.
- The above arrays use a pointer type, `char const*const`, to refer to a string literal.
- A visualization of such an array looks like this, `birds` for example

| bird | [0]                | [1]                | [2]                |
| ---- | ------------------ | ------------------ | ------------------ |
|      | `char const*const` | `char const*const` | `char const*const` |
|      | "raven"            | "magpie"           | "jay"              |

- That is, the string literals themselves are not stored inside the array `bird` but in some other place.
- `bird` only refers to those places.

### An object of **const-qualified** type is read-only

- In C, when we declare a variable with the `const` qualifier, it means that the value of that variable cannot be modified after it is initialized. 
- This makes the variable read-only. 

- That doesn't mean the compiler or run-time system may not perhaps change the value of such an object: other parts of the program may see that object without the qualification and change it.
- That fact that we cannot write the summary of our bank account directly (but only read it) doesn't mean it will remain constant over time.

```C
const int x = 10;
x = 20; // Error: assignment of read-only variable 'x'
```

### String literals are read-only

- String literals in C are arrays of `const` characters. 
- When we write a string literal, such as `"Hello, World!"`, the compiler stores this string in a read-only section of memory. 
- This means we cannot modify the contents of a string literal.

```C
char *ptr = "Hello, World!";
ptr[0] = 'h'; // Undefined behavior: attempting to modify a read-only string literal
```

---



## Enumerations

- Enumerations (enums) in C are a user-defined data type that consists of a set of named integer constants. 
- They are used to assign names to integral constants to make a program more readable and maintainable.

### Defining an Enumeration

- An enumeration is defined using the `enum` keyword followed by a name and a list of named integer constants enclosed in curly braces. 

```C
enum Color{
    RED,
    GREEN, 
    BLUE
};
```

- In this example, `Color` is an enumeration with three named constants: `RED`, `GREEN`, and `BLUE`. 
- By default, the values of these constants start from 0 and increment by 1. So, `RED` is 0, `GREEN` is 1, and `BLUE` is 2.

### Custom Values

- We can assign custom values to the enumeration constants. 
- If we do not specify a value for a constant, it will take the value of the previous constant plus one.

```C
enum Color {
    RED = 1,
    GREEN = 3,
    BLUE // BLUE will be 4
};
```

### Example

```C
#include <stdio.h>

enum Day {
    SUNDAY,
    MONDAY,
    TUESDAY,
    WEDNESDAY,
    THURSDAY,
    FRIDAY,
    SATURDAY
};

int main() {
    enum Day today = WEDNESDAY;

    if (today == WEDNESDAY) {
        printf("Today is Wednesday.\n");
    }

    return 0;
}
```

```sh
chan@CMA:~/C_Programming/test$ ./final
Today is Wednesday.
```

### Explanation

1. **Definition**: The `enum Day` defines an enumeration with the days of the week.
2. **Variable Declaration**: The variable `today` is declared of type `enum Day` and initialized to `WEDNESDAY`.
3. **Comparison**: The value of `today` is compared to `WEDNESDAY`, and if they are equal, a message is printed.

### Example 2

```C
enum corvid
{
    magpie,
    raven,
    jay,
    corvid_num
};

char const *const bird[corvid_num] = {
    [raven] = "raven",
    [magpie] = "magpie",
    [jay] = "jay",
};

int main()
{
    for (unsigned i = 0; i < corvid_num; i++)
    {
        printf("Corvid %u is the %s\n", i, bird[i]);
    }
    return 0;
}
```

```sh
chan@CMA:~/C_Programming/test$ ./final
Corvid 0 is the magpie
Corvid 1 is the raven
Corvid 2 is the jay
```



- Defines an enumeration `corvid` with four constants:
  - `magpie` (value 0)
  - `raven` (value 1)
  - `jay` (value 2)
  - `corvid_num` (value 3, used to represent the number of elements in the enumeration)
- Defines an array of constant pointers to constant characters (`char const *const`). The array `bird`has a size of `corvid_num`(3). The elements are initialized using designated initializers:
  - `bird[raven]` is set to `"raven"`
  - `bird[magpie]` is set to `"magpie"`
  - `bird[jay]` is set to `"jay"`
- **Enumerations**: Used to define a set of named integer constants.
- **Array of Strings**: Used to map enumeration values to their corresponding string representations.
- **Main Function**: Iterates over the enumeration values and prints their corresponding string representations.
- **Enumeration constants have either an explicit or a positional value.**
- Positional values start from 0 onward, so in our example, we have `raven` with value 0, `magpie` with 1, `jay` with 2 and `corvid_num`with 3. The last 3 is obviously the 3 we are interested in.
- This uses a different order for the array entities than before, and this is one of the advantages of the approach with enumerations.
- We do not have to manually track the order we used in the array. 
- The ording that is fixed in the enumeration type does that automatically.

Now if we want to add another corvid, we just put it in the list, anywhere before `corvid_num`.

```C
enum corvid
{
    magpie,
    raven,
    jay,
    chough,
    corvid_num
};

char const *const bird[corvid_num] = {
    [chough] = "chough",
    [raven] = "raven",
    [magpie] = "magpie",
    [jay] = "jay",
};

int main()
{
    for (unsigned i = 0; i < corvid_num; i++)
    {
        printf("Corvid %u is the %s\n", i, bird[i]);
    }
    return 0;
}
```

```sh
chan@CMA:~/C_Programming/test$ ./final
Corvid 0 is the magpie
Corvid 1 is the raven
Corvid 2 is the jay
Corvid 3 is the chough
```

#### Enumeration constants are of type `**signed int**`.

- In C, enumeration constants are defined using the `enum`keyword. 
- By default, the constants in an enumeration are of type `int`, which is a signed integer type. 
- This means they can represent both positive and negative values within the range of a signed integer.
- In the above example, `chough`, `raven` ,`magpie`, and `jay`are used as indices, and they are of type `signed int`. The compiler knows their values at compile time and uses them directly without needing any additional storage.

#### An integer constant expression doesn't require any object

- This means that an integer constant expression is a compile-time constant value that does not need to be stored in a variable (object) in memory. 

- Instead, it is directly embedded in the code by the compiler. 
- For example:

```C
#define SIZE 10
int array[SIZE];
```

- Here, `SIZE` is an integer constant expression. 
- The value `10` is used directly by the compiler to allocate memory for the array `array`. 
- There is no need to store `10` in a variable; it is a constant value known at compile time.



### Additional Notes about Enumerations

In C, enumerations (`enum`) are a way to define a set of named integer constants. However, there are a few nuances to understand about how they work:

1. **Underlying Type**: The constants defined in an `enum`are essentially integers. The compiler assigns integer values to these constants starting from 0 by default, unless explicitly specified otherwise. For example, in our `enum corvid`, `magpie` is 0, `raven` is 1, `jay` is 2, and so on.

2. **Type Conversion**: When we use an enumeration constant in expressions, it is treated as an integer. This means that even though we define an `enum`, the constants are not of a distinct enumeration type but are instead treated as integers. This is why we can use them in arithmetic operations and array indexing without any special handling.

3. **Variable Declaration**: Declaring variables of an enumeration type is not very common because the primary use of enums is to create named constants. When we do declare a variable of an enum type, it is still treated as an integer in terms of storage and operations. For example:

   ```C
   enum corvid bird_type = magpie;
   ```

   - Here, `bird_type` is essentially an integer variable with a value of 0 (the value of magpie`]).

4. **Enumeration Constants**: The constants themselves are not of the enumeration type but are simply integer constants. This is why we can use them interchangeably with other integers in our code.

In summary, 

- while enums provide a convenient way to define named constants, they are fundamentally treated as integers in C. 
- This allows for flexibility in using these constants in various contexts, such as array indexing and arithmetic operations, without needing special handling for a distinct enumeration type.

### Benefits of Enumerations

- **Readability**: Enums make the code more readable by replacing magic numbers with meaningful names.
- **Maintainability**: Enums make it easier to manage and update sets of related constants.
- **Type Safety**: Enums provide a level of type safety by restricting variables to a defined set of values.

### Summary

- Enumerations in C are a powerful feature for defining sets of named integer constants, improving code readability, maintainability, and type safety. 
- They are defined using the `enum` keyword and can be used to create variables that take on one of the predefined values.

---

## Compound Literals

- Compound literals in C are a feature introduced in the C99 standard that allows us to create unnamed objects with a specific type and value. 
- They are particularly useful for initializing arrays, structures, and other complex data types on the fly without having to declare a separate variable.

### Syntax

```C
(type){initializer_list}
```

- Where `type` is the type of the compound literal, and `initializer_list` is a list of values used to initialize the object.

### Examples

1. **Array Initialization:**

   ```C
   int *arr = (int[]) {1, 2, 3, 4, 5};
   ```

   - This creates an unnamed array of integers and assigns its address to `arr`.

2. Structure Initialization:

   ```C
   struct Point {
       int x, y;
   };
   
   struct Point p = (struct Point) { .x = 10, .y = 20 };
   ```

   - This creates an unnamed `struct Point` and initializes its members `x` and `y`.

3. **Function Arguments:** Compound literals can be used directly  as function arguments.

   ```C
   void print_point(struct Point p) {
       printf("Point(%d, %d)\n", p.x, p.y);
   }
   
   print_point((struct Point) { .x = 5, .y = 15 });
   ```

   - This passes an unnamed `struct Point` to the `print_point` function.

```C
#include <stdio.h>

struct Bird {
    const char *name;
    int wingspan;
};

int main() {
    struct Bird b = (struct Bird) { .name = "raven", .wingspan = 120 };
    printf("Bird: %s, Wingspan: %d cm\n", b.name, b.wingspan);
    return 0;
}
```

- This creates an unnamed `struct Bird` with the name "raven" and a wingspan of 120cm, and assigns it to the variable `b`.

### Benefits

- **Convenience:** Allows for quick initialization without declaring separate variables.
- **Readability:** Makes the code more readable by keeping initialization close to usage.
- **Flexibility:** Useful in function calls and complex initializations.

Compound literals are a powerful feature that can make our C code more concise and expressive.

---

## Boolean Values

- The Boolean data type in C is also considered an unsigned type.
- It has only values 0 and 1, so there are no negative values.
- The name `bool` as well as the constants `false` and `true` only come through the inclusion of `stdbool.h`.

---

## Binary Representations

### Unsigned integers

#### "The maximum value of any integer type is of the form 2<sup>p</sup>-1"

This statement refers to the maximum value that can be represented by an integer type in binary form. Here, `p` represents the number of bits used to store the integer.

- For an `n`-bit integer, the maximum value is given by the formula (2<sup>p</sup> - 1).
- This is because each bit can be either 0 or 1, and the maximum value is achieved when all bits are set to 1.

For example:

- An 8-bit unsigned integer (`uint8_t`) has 8 bits. The maximum value is (2<sup>8</sup> - 1 = 255).
- A 16-bit unsigned integer (`uint16_t`) has 16 bits. The maximum value is (2<sup>16</sup> - 1 = 65535).

#### "Arithmetic on an unsigned integer type is determined by its precision"

- This statement means that the arithmetic operations on unsigned integers are constrained by the number of bits (precision) used to represent them. 
- When performing arithmetic operations, the results are wrapped around if they exceed the maximum value that can be represented by the given number of bits.

##### Example

- If we have an 8-bit unsigned integer (`uint8_t`) and we add 1 to its maximum value (255), the result will wrap around to 0.

  ```C
  uint8_t a = 255;
  a = a + 1; // a becomes 0 due to wrap-around
  ```

- Similarly, if we subtract 1 from 0, it will wrap around to the maximum value (255).

  ```C
  uint8_t b = 0;
  b = b - 1; // b becomes 255 due to wrap-around
  ```

### Summary

- The maximum value of an integer type is determined by the number of bits used to represent it, following the formula (2<sup>p</sup> - 1).
- Arithmetic on unsigned integers wraps around when the result exceeds the maximum value that can be represented by the given number of bits.



#### Bit sets and bitwise operators

" A = 240 , representing {4, 5, 6, 7} and B = 287, the bit set {0, 1, 2, 3, 4, 8}"

**Binary Representation**

1. **240 in Binary**:
   - Decimal 240 can be represented in binary as `11110000`.
   - This means that the bits at positions 4, 5, 6, and 7 are set (counting from 0).
2. **287 in Binary**:
   - Decimal 287 can be represented in binary as `100011111`.
   - This means that the bits at positions 0, 1, 2, 3, 4, and 8 are set.

##### Explanation

- **A = 240**:
  - Binary: `11110000`
  - Bits set: {4, 5, 6, 7}
- **B = 287**:
  - Binary: `100011111`
  - Bits set: {0, 1, 2, 3, 4, 8}

##### Visual Representation

- 240 (A):

  ```
  Bit positions:  7 6 5 4 3 2 1 0
  Binary:         1 1 1 1 0 0 0 0
  ```

- 287 (B):

  ```
  Bit positions:  8 7 6 5 4 3 2 1 0
  Binary:         1 0 0 0 1 1 1 1 1
  ```

#### The Complement Operator

- Also an operator that operates on the bits of the value.

**"The complement ~A would have value 65295 and would correspond to the set {0,1,2,3,8,9,10,11,12,13,14,15}"**

##### Step-by-Step Explanation

1. **Original Value of A**:
   - `A = 240`
   - Binary representation of `A`: `0000000011110000` (assuming a 16-bit representation for clarity)
2. **Complement of A (`~A`)**:
   - The complement operator `~` inverts all the bits.
   - Binary representation of `~A`: `1111111100001111`
3. **Decimal Value of `~A`**:
   - Binary `1111111100001111` converts to decimal `65295`.
4. **Set of Bits in `~A`**:
   - The bits that are set (i.e., bits that are 1) in `~A` are at positions: {0, 1, 2, 3, 8, 9, 10, 11, 12, 13, 14, 15}.

##### Visual Representation

- **Original A (240):**

  ```
  Bit positions:  15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0
  Binary:         0  0  0  0  0  0  0 0 1 1 1 1 0 0 0 0
  ```

- **Complement ~A (65295)**:

  ```
  Bit positions:  15 14 13 12 11 10 9 8 7 6 5 4 3 2 1 0
  Binary:         1  1  1  1  1  1  1 1 0 0 0 0 1 1 1 1
  ```

##### Summary

- **Complement ~A**:
  - Decimal value: `65295`
  - Binary representation: `1111111100001111`
  - Set of bits: {0, 1, 2, 3, 8, 9, 10, 11, 12, 13, 14, 15}



### Signed Integers

- Signed types are a bit more complicated than unsigned types.
- A C implementation has to decide about two points:
  - What happens on arithmetic overflow?
  - How is the sign of a signed type represented?
- Signed and unsigned types come in pairs according to their integer rank, with the notable two exceptions, `char` and `bool`.

#### "Positive values are represented independently from signedness"



##### Explanation

1. **Binary Representation**:
   - Positive integers have the same binary representation in both signed and unsigned types.
   - For example, the binary representation of the decimal number `5` is `00000101` in an 8-bit system. This representation is the same whether `5` is stored in a signed or unsigned variable.
2. **Signed vs. Unsigned Types**:
   - **Signed Types**: Can represent both positive and negative values. For example, `int` can represent values from `-2,147,483,648` to `2,147,483,647` in a 32-bit system.
   - **Unsigned Types**: Can only represent non-negative values. For example, `unsigned int` can represent values from `0` to `4,294,967,295` in a 32-bit system.
3. **Positive Values**:
   - When dealing with positive values, the binary representation does not change based on whether the type is signed or unsigned.
   - For instance, the number `10` is represented as `00001010` in binary. This representation is the same for both `int` and `unsigned int`.

##### Example

```C
#include <stdio.h>

int main() {
    int signed_val = 10;
    unsigned int unsigned_val = 10;

    printf("Signed value: %d\n", signed_val);
    printf("Unsigned value: %u\n", unsigned_val);

    return 0;
}
```

- Output:

```sh
Signed value: 10
Unsigned value: 10
```

In this example, both `signed_val` and `unsigned_val` have the same binary representation (`00001010`), and thus they both represent the value `10` correctly.

##### Summary

- Positive values have the same binary representation regardless of whether they are stored in signed or unsigned types.
- This means that the representation of positive integers is independent of the signedness of the type.

#### Sign Representation

- The next thing the standard prescribes is that signed types have one additional bit, the **sign bit**.
- If it is 0, we have a positive value, if it is 1, the value is negative.
- Unfortunately, there are different concepts of how such a sign bit can be used to obtain a negative number.
- C allows three different **sign representations**:
  - **Sign and magnitude**
  - **Ones' complement**
  - **Two's complement**
- The first two nowadays probably only have historical or exotic relevance: for sign and magnitude, the magnitude is taken as positive values, and the sign bit simply specifies that there is a minus sign.
- Ones' complement takes the corresponding positive value and complements all bits.
- Both representations have the disadvantage that two values evaluate to 0: there is a positive and a negative 0.

#### Two's Complement Representation

**Commonly used on modern platforms** is the **two's complement representation**. 

- It performs exactly the same arithmetic for unsigned types, but the upper half of unsigned values (those with a high-order bit of 1) is interpreted as being negative.
- In C, the two's complement representation is used to represent signed integers. 
- This representation allows for straightforward binary arithmetic and simplifies the design of arithmetic circuits. 
- Here's a brief explanation of how two's complement works:

##### Two's Complement Representation

1. **Positive Numbers:**
   - Positive numbers are represented as usual in binary.
   - For example, `5` in an 8-bit system is `00000101`.
2. **Negative Numbers:**
   - To represent a negative number, you first write its absolute value in binary.
   - Then, invert all the bits (change `0`to `1` and `1` to `0`).
   - Finally, add `1` to the least significant bit (LSB).

##### Example: Representing `-5` in an 8-bit System

1. **Absolute Value:**
   - `5` in binary is `00000101`.
2. **Invert the Bits:**
   - Inverting `00000101` gives `11111010`.
3. **Add `1`:**
   - Adding `1` to `11111010` gives `11111011`.

So, `-5` in an 8-bit two's complement representation is `11111011`.

```C
#include <stdio.h>

int main() {
    signed char positive = 5;  // 00000101 in binary
    signed char negative = -5; // 11111011 in binary (two's complement)

    printf("Positive: %d, Binary: ", positive);
    for (int i = 7; i >= 0; i--) {
        printf("%d", (positive >> i) & 1);
    }
    printf("\n");

    printf("Negative: %d, Binary: ", negative);
    for (int i = 7; i >= 0; i--) {
        printf("%d", (negative >> i) & 1);
    }
    printf("\n");

    return 0;
}
```

###### Explanation

- The program prints the binary representation of `5` and `-5` using two's complement.
- The `>>` operator is used to shift bits to the right, and `& 1` is used to extract the least significant bit.

###### Iteration Details for Positive Number

- **i = 7:**
  - `positive >> 7` shifts the bits 7 positions to the right: `00000000`
  - `(00000000) & 1` results in `0`
  - Output: `0`
- **i = 6:**
  - `positive >> 6` shifts the bits 6 positions to the right: `00000000`
  - `(00000000) & 1` results in `0`
  - Output: `0`
- **i = 5:**
  - `positive >> 5` shifts the bits 5 positions to the right: `00000000`
  - `(00000000) & 1` results in `0`
  - Output: `0`
- **i = 4:**
  - `positive >> 4` shifts the bits 4 positions to the right: `00000000`
  - `(00000000) & 1` results in `0`
  - Output: `0`
- **i = 3:**
  - `positive >> 3` shifts the bits 3 positions to the right: `00000000`
  - `(00000000) & 1` results in `0`
  - Output: `0`
- **i = 2:**
  - `positive >> 2` shifts the bits 2 positions to the right: `00000001`
  - `(00000001) & 1` results in `1`
  - Output: `1`
- **i = 1:**
  - `positive >> 1` shifts the bits 1 position to the right: `00000010`
  - `(00000010) & 1` results in `0`
  - Output: `0`
- **i = 0:**
  - `positive >> 0` shifts the bits 0 positions to the right: `00000101`
  - `(00000101) & 1` results in `1`
  - Output: `1`

###### Final Output for Positive Number

- Binary representation: `00000101`

###### Negative Number: -5 (Binary: 11111011)

1. **Initial Value:**
   - `negative = -5` (binary `11111011` in two's complement)
2. **Loop Iteration:**
   - The loop iterates from `i = 7` to `i = 0`.
3. **Right Shift and Bitwise AND:**
   - For each iteration, the expression `(negative >> i) & 1` is evaluated.

###### Iteration Details for Negative Number

- **i = 7:**
  - `negative >> 7` shifts the bits 7 positions to the right: `11111111` (sign extension)
  - `(11111111) & 1` results in `1`
  - Output: `1`
- **i = 6:**
  - `negative >> 6` shifts the bits 6 positions to the right: `11111111` (sign extension)
  - `(11111111) & 1` results in `1`
  - Output: `1`
- **i = 5:**
  - `negative >> 5` shifts the bits 5 positions to the right: `11111111` (sign extension)
  - `(11111111) & 1` results in `1`
  - Output: `1`
- **i = 4:**
  - `negative >> 4` shifts the bits 4 positions to the right: `11111111` (sign extension)
  - `(11111111) & 1` results in `1`
  - Output: `1`
- **i = 3:**
  - `negative >> 3` shifts the bits 3 positions to the right: `11111111` (sign extension)
  - `(11111111) & 1` results in `1`
  - Output: `1`
- **i = 2:**
  - `negative >> 2` shifts the bits 2 positions to the right: `11111110`
  - `(11111110) & 1` results in `0`
  - Output: `0`
- **i = 1:**
  - `negative >> 1` shifts the bits 1 position to the right: `11111101`
  - `(11111101) & 1` results in `1`
  - Output: `1`
- **i = 0:**
  - `negative >> 0` shifts the bits 0 positions to the right: `11111011`
  - `(11111011) & 1` results in `1`
  - Output: `1`

###### Final Output for Negative Number

- Binary representation: `11111011`



### Additional Theories of signed integers

#### "Signed arithmetic may trap badly"

**Explanation**:

- **Signed arithmetic** refers to arithmetic operations involving signed integers (integers that can be both positive and negative).
- **Trapping** means causing an exception or error during execution.
- **May trap badly** implies that certain operations on signed integers can lead to undefined behavior or runtime errors.

**Example**:

- Consider an overflow scenario: if you add two large positive signed integers and the result exceeds the maximum value that can be stored in the integer type, it can cause an overflow, leading to undefined behavior.

#### "In two's complement representation, INT_MIN < -INT_MAX."

**Explanation**:

- **Two's complement** is a method for representing signed integers in binary.
- **INT_MIN** is the smallest value that can be represented by a signed integer type.
- **INT_MAX** is the largest value that can be represented by a signed integer type.
- In two's complement, the range of representable values is asymmetric. For example, in a 32-bit signed integer:
  - INT_MIN = -2,147,483,648
  - INT_MAX = 2,147,483,647
- Therefore, INT_MIN is less than -INT_MAX because:
  - INT_MIN = -2,147,483,648
  - -INT_MAX = -2,147,483,647

#### "Negation may overflow for signed arithmetic"

**Explanation**:

- **Negation** refers to changing the sign of a number (e.g., from positive to negative or vice versa).
- **Overflow** occurs when the result of an arithmetic operation exceeds the range that can be represented by the data type.
- For signed integers, negating the smallest possible value (INT_MIN) can cause overflow because there is no positive counterpart for INT_MIN in two's complement representation.
- For signed types, bit operations work with the binary representation.
- So the value of a bit operation depends in particular on the sign representation.
- In face, bit operations even allow us to detect the sign representation.
- The shift operations then become really messy. The semantics of what such an operation is for a negative value is not clear.

**Example**:

- In a 32-bit signed integer:
  - INT_MIN = -2,147,483,648
  - Negating INT_MIN would theoretically result in 2,147,483,648, which cannot be represented in a 32-bit signed integer (the maximum positive value is 2,147,483,647).

```C
char const *sign_rep[4] = {
    [1] = "sign and magnitude",
    [2] = "one's complement",
    [3] = "two's complement",
    [0] = "weird",
};

enum
{
    sign_magic = -1 & 3
};

int main()
{
    printf("Sign representation: %s\n", sign_rep[sign_magic]);
    return 0;
}
```

##### Explanation

- Declares an array of string literals (`sign_rep`) with 4 elements.
  - The array `sign_rep` is initialized using designated initializers, which allow us to specify the index for each element explicitly.
  - This feature makes the code more readable and maintainable, as we can see exactly which string is assigned to which index.
- The array is initialized using designated initializers:
  - `sign_rep[1]` is set to `"sign and magnitude"`.
  - `sign_rep[2]` is set to `"one's complement"`.
  - `sign_rep[3]` is set to `"two's complement"`.
  - `sign_rep[0]` is set to `"weird"`.
- Declares an anonymous enumeration with a single enumerator `sign_magic`.
- The value of`sign_magic` is calculated using the bitwise AND operation:
  - `-1` in binary (two's complement) is represented with all bits set to `1`.
  - `3` in binary is `00000011`.
  - The bitwise AND operation `-1 & 3` results in `3` because the last two bits of `-1` are `1`.

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Sign representation: two's complement
```



#### "Use unsigned types for bit operations."

The statement "Use unsigned types for bit operations" is a guideline suggesting that when performing bitwise operations, we should use unsigned integer types. Here's why:

##### Explanation

1. **Predictable Behavior**:
   - Unsigned integers have a well-defined behavior for bitwise operations. They do not have a sign bit, so all bits are used to represent the value, making the operations more predictable.
2. **Avoiding Undefined Behavior**:
   - Signed integers can lead to undefined behavior when performing bitwise operations, especially with shifts. For example, shifting a negative signed integer can produce unexpected results.
3. **Logical Representation**:
   - Bitwise operations are often used for manipulating individual bits, flags, or masks. Unsigned integers are more suitable for these purposes because they represent non-negative values, aligning better with the concept of bit manipulation.

##### Example

Let's consider the following example where bitwise operations are performed on both signed and unsigned integers:

```C
#include <stdio.h>

int main()
{
    int signedInt = -1;          // Signed integer
    unsigned int unsignedInt = -1; // Unsigned integer (wraps around to maximum value)

    printf("Signed int: %d\n", signedInt);
    printf("Unsigned int: %u\n", unsignedInt);

    // Bitwise shift operations
    printf("Signed int shifted right: %d\n", signedInt >> 1);
    printf("Unsigned int shifted right: %u\n", unsignedInt >> 1);

    return 0;
}
```

**Output**

```sh
Signed int: -1
Unsigned int: 4294967295
Signed int shifted right: -1
Unsigned int shifted right: 2147483647
```

##### Explanation of Output

- **Signed Integer**:
  - `signedInt` is initialized to `-1`, which in binary (two's complement) is represented with all bits set to `1`.
  - Shifting `signedInt` right by 1 bit (`signedInt >> 1`) results in `-1` due to sign extension (the sign bit is propagated).
- **Unsigned Integer**:
  - `unsignedInt` is initialized to `-1`, which wraps around to the maximum value for an unsigned integer (`4294967295` for a 32-bit unsigned int).
  - Shifting `unsignedInt` right by 1 bit (`unsignedInt >> 1`) results in `2147483647`, which is the expected logical right shift.

#### Summary

- **Signed arithmetic may trap badly**: Operations on signed integers can lead to undefined behavior or runtime errors, especially in cases of overflow.
- **In two's complement representation, INT_MIN < -INT_MAX**: The smallest representable value (INT_MIN) is less than the negation of the largest representable value (INT_MAX) due to the asymmetric range of two's complement representation.
- **Negation may overflow for signed arithmetic**: Negating the smallest possible signed integer (INT_MIN) can cause overflow because the result exceeds the representable range.
- Using unsigned types for bit operations ensures that the operations behave as expected without the complications introduced by sign bits and sign extension. This makes the code more robust and easier to understand.

These concepts are crucial for understanding the limitations and potential pitfalls of arithmetic operations on signed integers in programming.

---

## Fixed-width integer types

- Fixed-width integers in C are types defined in the `<stdint.h>` header that provide integer types with specified widths. 
- These types are particularly useful for ensuring consistent behavior across different platforms, as they guarantee the size of the integers.

- The precision for the integer types that we have seen so far can be inspected indirectly by using macros from `limits.h` such as `UINT_MAX` and `LONG_MIN`.
- The C Standard only gives us a minimal precision for them.
- For the unsigned types, these are

| Type               | Minimal precision |
| ------------------ | ----------------- |
| bool               | 1                 |
| unsigned char      | 8                 |
| unsigned short     | 16                |
| unsigned           | 16                |
| unsigned long      | 32                |
| unsigned long long | 64                |

- Under usual circumstances, these guarantees should give us enough information.
- But under some technical constraints, such guarantees might not be sufficient, or we might want to emphasize a particular precision.
- This may be the case if we want to use an unsigned quantity to represent a bit set of a known maximal set.
- If we know that 32-bit will suffice for our set, depending on the platform, we might want to choose `unsigned` or `unsigned long` to represent it.
- The C Standard provides names for **exact-width integer types** in `stdint.h`.
- As the name indicates, they are of an exact prescribed "width", which for provided unsigned types is guaranteed to be the same as their precision.

### Common Fixed-Width Integer Types

1. **Exact-width integer types**:
   - These types have a precise number of bits.
   - Examples:
     - `int8_t`: 8-bit signed integer
     - `uint8_t`: 8-bit unsigned integer
     - `int16_t`: 16-bit signed integer
     - `uint16_t`: 16-bit unsigned integer
     - `int32_t`: 32-bit signed integer
     - `uint32_t`: 32-bit unsigned integer
     - `int64_t`: 64-bit signed integer
     - `uint64_t`: 64-bit unsigned integer
2. **Minimum-width integer types**:
   - These types have at least the specified number of bits.
   - Examples:
     - `int_least8_t`: At least 8-bit signed integer
     - `uint_least8_t`: At least 8-bit unsigned integer
     - `int_least16_t`: At least 16-bit signed integer
     - `uint_least16_t`: At least 16-bit unsigned integer
     - `int_least32_t`: At least 32-bit signed integer
     - `uint_least32_t`: At least 32-bit unsigned integer
     - `int_least64_t`: At least 64-bit signed integer
     - `uint_least64_t`: At least 64-bit unsigned integer
3. **Fastest minimum-width integer types**:
   - These types are the fastest integer types with at least the specified number of bits.
   - Examples:
     - `int_fast8_t`: Fastest signed integer with at least 8 bits
     - `uint_fast8_t`: Fastest unsigned integer with at least 8 bits
     - `int_fast16_t`: Fastest signed integer with at least 16 bits
     - `uint_fast16_t`: Fastest unsigned integer with at least 16 bits
     - `int_fast32_t`: Fastest signed integer with at least 32 bits
     - `uint_fast32_t`: Fastest unsigned integer with at least 32 bits
     - `int_fast64_t`: Fastest signed integer with at least 64 bits
     - `uint_fast64_t`: Fastest unsigned integer with at least 64 bits
4. **Integer types capable of holding object pointers**:
   - These types can hold any pointer to an object.
   - Examples:
     - `intptr_t`: Signed integer type capable of holding a pointer
     - `uintptr_t`: Unsigned integer type capable of holding a pointer
5. **Greatest-width integer types**:
   - These types have the greatest width supported by the implementation.
   - Examples:
     - `intmax_t`: Greatest-width signed integer type
     - `uintmax_t`: Greatest-width unsigned integer type

- The presence and bounds can be tested with the macros `UINT8_MAX,..., UINT64_MAX` for unsigned types and `INT8_MIN, INT8_MAX,..., INT64_MIN, INT64_MAX,` respectively.
- To encode literals of the requested type, there are macros `UINT8_C,..., UINT64_C`, and `INT8_C,..., INT64_C` respectively.
- For example, on platforms where `uint64_t` is `unsigned long`, `INT64_C`(1) expands to `1UL`.

### Example Usage

Here's an example of how you might use fixed-width integers in a program:

```C
#include <stdio.h>

#include <stdint.h>

int main() {

  int32_t a = 100; // 32-bit signed integer

  uint64_t b = 10000000000ULL; // 64-bit unsigned integer

  printf("a: %d\n", a);

  printf("b: %llu\n", b);

  return 0;

}
```

### Benefits of Using Fixed-Width Integers

1. **Portability**: Ensures that the program behaves consistently across different platforms.
2. **Clarity**: Makes the code more readable and understandable by explicitly specifying the size of the integers.
3. **Precision**: Avoids issues related to integer overflow and underflow by using appropriately sized types.

By using fixed-width integers, we can write more robust and portable code, especially in systems programming, embedded systems, and applications where precise control over data size is crucial.

---

## Floating-point data

- Whereas integers come near the mathematical concepts of `N(unsigned)` or `Z (signed)`, floating-point types are close to `R(non-complex)` or `C (complex)`.
- The way they differ from these mathematical concepts is twofold.
- First there is a size restriction on what is representable. 
- The include file `float.h` for example, has constants, `DBL_MIN` and `DBL_MAX` that provide us with the minimal and maximal values for `double`.
- But be aware that, `DBL_MIN` is the smallest number that is strictly greater than `0.0`; the smallest negative double value is `-DBL_MAX`.
- But real numbers (R) have another difficulty when we want to represent them on a physical system: they can have an unlimited expansion such as the value <sup>1</sup>/<sub>3</sub>, which has an endless repetition of the digit 3 in decimal representation, or the value of `π`, which is "transcendent" and so has an endless expansion in any representation and doesn't repeat in any way.
- C and other programming languages deal with these difficulties by cutting off the expansion.
- The position where the expansion is cut is "floating" (thus the name) and depends on the magnitude of the number in question.

### Definition

- Floating-point data in C is used to represent real numbers that have fractional parts. 
- C provides several types for floating-point numbers, each with different precision and range. 
- These types are defined in the C standard library and are typically implemented using IEEE 754 standard.
  - The IEEE 754 standard is a technical standard for floating-point arithmetic established by the Institute of Electrical and Electronics Engineers (IEEE). 
  - It defines the representation and behavior of floating-point numbers in computers, ensuring consistency and portability across different platforms and programming languages.

### **Key Concepts of IEEE 754 in C:**

1. **Floating-Point Number Representation:**
   - Single Precision (32-bit):
     - 1 bit for the sign (0 for positive, 1 for negative)
     - 8 bits for the exponent (with a bias of 127)
     - 23 bits for the fraction (or mantissa/significand)
   - Double Precision (64-bit):
     - 1 bit for the sign
     - 11 bits for the exponent (with a bias of 1023)
     - 52 bits for the fraction
2. **Special Values:**
   - **Zero:** Represented with all bits of the exponent and fraction set to 0. There are two representations: +0 and -0.
   - **Infinity:** Occurs when all exponent bits are 1, and the fraction bits are all 0. There is +∞ and -∞.
   - **NaN (Not a Number):** Occurs when all exponent bits are 1, and the fraction is non-zero. It represents undefined or unrepresentable values like the result of `0/0`.
3. **Rounding Modes:**
   - IEEE 754 defines several rounding modes, with the most common being **round to nearest, ties to even** (default), where a value is rounded to the nearest representable number, and if it's exactly halfway, it rounds to the nearest even number.
4. **Precision and Accuracy:**
   - The standard ensures that arithmetic operations like addition, subtraction, multiplication, and division produce the most accurate result possible within the constraints of the precision.
   -  However, due to the way floating-point numbers are represented in memory, these operations can introduce rounding errors.
5. **Floating-Point Exceptions:**
   - IEEE 754 specifies exceptions like overflow, underflow, division by zero, invalid operation, and inexact result. These exceptions can trigger flags that can be handled programmatically.

### Floating-Point Types

1. **`float`**:
   - Single-precision floating-point.
   - Typically 32 bits.
   - Precision: about 7 decimal digits.
   - Example: `float a = 3.14f;`
2. **`double`**:
   - Double-precision floating-point.
   - Typically 64 bits.
   - Precision: about 15 decimal digits.
   - Example: `double b = 3.14;`
3. **`long double`**:
   - Extended-precision floating-point.
   - Typically more than 64 bits (implementation-dependent).
   - Precision: more than `double`.
   - Example: `long double c = 3.14L;`

### **Usage in C:**

- **Floating-Point Types:**

  - C provides `float` (single precision) and `double` (double precision) types that conform to the IEEE 754 standard.

  - Example:

    ```C
    float a = 1.0f / 3.0f;  // Single precision
    double b = 1.0 / 3.0;   // Double precision
    ```

- **Special Constants:**

  - C provides constants like `INFINITY` and `NAN` in the `math.h` header to represent infinity and NaN values.

  - Example:

    ```C
    #include <math.h>
    
    float inf = INFINITY;
    double not_a_number = NAN;
    ```

- **Rounding and Precision Control:**

  - The `fenv.h` header provides functions to control the rounding mode and handle floating-point exceptions.



#### "Floating-point operations are neither associative, commutative, nor distributive."

##### Associativity

- Associativity refers to the property that the grouping of operations does not affect the result. For example, in integer arithmetic, addition is associative: `[ (a + b) + c = a + (b + c) ]`

- However, floating-point addition is not associative due to rounding errors.

```C
#include <stdio.h>

int main() {
    float a = 1.0e10f;
    float b = -1.0e10f;
    float c = 1.0f;

    float result1 = (a + b) + c;
    float result2 = a + (b + c);

    printf("(a + b) + c = %.1f\n", result1);
    printf("a + (b + c) = %.1f\n", result2);

    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
(a + b) + c = 1.0
a + (b + c) = 0.0
```

- The results are different because of the way floating-point arithmetic handles precision and rounding.



##### Commutativity

- Commutativity refers to the property that the order of operations does not affect the result. For example, in integer arithmetic, addition is commutative: 

  [ a + b = b + a ]

- However, floating-point addition is not always commutative due to rounding errors.

```C
#include <stdio.h>

int main() {
    float a = 1.0e10f;
    float b = 1.0f;

    float result1 = a + b;
    float result2 = b + a;

    printf("a + b = %f\n", result1);
    printf("b + a = %f\n", result2);

    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
a + b = 10000000000.000000
b + a = 10000000000.000000
```

- In this case, the results are the same, but in more complex scenarios involving multiple operations, the results can differ due to rounding errors.

##### Distributivity

- Distributivity refers to the property that multiplication distributes over addition. For example, in integer arithmetic: [ a * (b + c) = (a * b) + (a * c) ]
- However, floating-point multiplication is not always distributive due to rounding errors.

```C
#include <stdio.h>

int main() {
    float a = 1.0e10f;
    float b = 1.0f;
    float c = 1.0e-10f;

    float result1 = a * (b + c);
    float result2 = (a * b) + (a * c);

    printf("a * (b + c) = %f\n", result1);
    printf("(a * b) + (a * c) = %f\n", result2);

    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
a * (b + c) = 10000000000.000000
(a * b) + (a * c) = 10000000000.000000
```

- In this case, the results are the same, but in more complex scenarios, the results can differ due to rounding errors.



#### "In this case, the results are the same, but in more complex scenarios, the results can differ due to rounding errors."

- Due to the way floating-point numbers are represented in memory, they can suffer from precision issues. 
- This means that two floating-point numbers that are mathematically equal might not be exactly equal when represented in binary form.

```C
int main()
{
    double a = 0.1;
    double b = 0.2;
    double c = a + b;

    if (c == 0.3)
    {
        printf("c is exactly 0.3\n");
    }
    else
    {
        printf("c is not exactly 0.3, it is %.17f\n", c);
    }
    return 0;
}

```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
c is not exactly 0.3, it is 0.30000000000000004
```

- In this example, `c` is not exactly `0.3` due to the precision issues inherent in floating-point arithmetic.
- Using float instead of double might yield "c is exactly 0.3" but it is platform-dependent.
- The exact behavior of floating-point arithmetic can vary slightly depending on the compiler and the platform.

##### Conclusion

- **Floating-point operations are neither associative, commutative, nor distributive**: This is due to rounding errors and precision issues inherent in floating-point arithmetic.
- **Never compare floating-point values for equality**: Instead, use a small tolerance to check if the values are close enough, accounting for precision issues.

## Bullet Points up until this point

- C programs run in an abstract state machine that is mostly independent of the specific computer where it is launched.
- All basic C types are kinds of numbers, but not all of them can be used directly for arithmetic.
- Values have a type and a binary representation.
- When necessary, types of values are implicitly converted to fit the needs of particular places where they are used.
- Variables must be explicitly initialized before their first use.
- Integer computations give exact values as long as there is no overflow.
- Floating-point computations give only approximated results that are cut off after a certain number of binary digits.

---

# Derived data types

- There are **four strategies** for deriving data types.
- Two of them are called **aggregate data types**, because they combine multiple instances of one or several other data types:
  - **Arrays**: These combine items that all have the same base type.
  - **Structures**: These combine items that may have different base types.
- The two other strategies to derive data types are more involved:
  - **Pointers**: Entities that refer to an object in memory.
  - **Unions**: These overlay items of different base types in the same memory location. Unions require a deeper understanding of C's memory model and are not of much use in a programmer's everyday life.
- There is a fifth strategy that introduces new names for types: **typedef**
  - Unlike the previous four, this does not create a new type in C's type system, but only creates a new name for an existing type.
  - In that way, it is similar to the definition of macros with `#define`: thus the choice for the keyword for this feature.



## Arrays

- Arrays allow us to group objects of the same type into an encapsulating object.
- Arrays and pointers are closely related in C.
- Arrays look like pointers in many contexts, and pointers refer to array objects.

### Array declaration

```C
double a[4];
signed b[N];
```

- Here, `a` comprises 4 subobjects of type `double` and `b` comprises `N` of type `signed`.

|      | [0]         | [1]                       | [2]         | [3]         |
| ---- | ----------- | ------------------------- | ----------- | ----------- |
| a    | `double` ?? | `double` ??               | `double` ?? | `double` ?? |
|      | [0]         |                           | [`N` - 1]   |             |
| b    | `signed` ?? | ...                  .... | `signed` ?? |             |

- The dots ... here indicate that there may be an unknown number of similar items between two boxes.

The type that composes an array may itself again be an array, forming a **multidimensional array**.

```C
double C[M][N];
double (D[M])[N];
```

- Both `C` and `D` are `M` objects of array type `double`.
- This means we have to read a nested array declaration from inside out to describe its structure:

|      |             | [0]  |             |             | [M-1] |              |
| ---- | ----------- | ---- | ----------- | ----------- | ----- | ------------ |
|      | `[0][0]`    |      | `[0][N-1]`  | `[M-1][0]`  |       | `[M-1][N-1]` |
| C    | `double` ?? | ...  | `double` ?? | `double` ?? | ...   | `double` ??  |

- During development, designated initializers help to make our code robust against small changes in array sizes or positions.

### Array operations

#### "An array in a condition evaluates to `true`."

- In C, when an array is used in a condition, it decays to a pointer to its first element. 
- This pointer is not `NULL`, so it evaluates to `true`.
- An array in a condition evaluates to `true` because it decays to a pointer to its first element, which is non-zero.

```C
int arr[5] = {1, 2, 3, 4 ,5};
if(arr){
    // This block will always execute because arr decays to a pointer to its first element.
    printf("Array is non_null\n");
}
```



#### "There are array objects but no array values."

- In C, arrays are treated as objects, but they do not have a single value that can be assigned or compared directly. Instead, they are collections of elements.
  - **Explanation**: Arrays are objects that hold multiple values (elements), but the array itself does not have a single value. You can access and manipulate individual elements, but you cannot treat the array as a single value.

```C
int arr[5] = {1, 2, 3, 4, 5};
// arr is an object containing 5 integers, but arr iteself does not have a single value.
```



#### "Arrays can't be compared"

- In C, we cannot directly compare arrays using comparison operators like `==` or `!=`.
  - **Explanation**: Arrays cannot be compared directly because the comparison operators do not work on array objects. Instead, we need to compare each element individually or use functions like `memcmp` from the standard library.

```C
int arr1[5] = {1, 2, 3, 4, 5};
int arr2[5] = {1, 2, 3, 4, 5};

// if (arr1 == arr2){
	// This is invalid in C
	// printf("Arrays are equal\n");
//}

// Correct way to compare arrays
int are_equal = 1;
for(int i = 0; i < 5; i++){
    if(arr1[i] != arr2[i]){
        are_equal = 0;
        break;
    }
}

if(are_equal){
    printf("Arrays are equal\n");
}
```



#### "Arrays can't be assigned to"

- In C, we cannot assign one array to another using the assignment operator `=`.
  - **Explanation**: Arrays cannot be assigned to each other directly because the assignment operator does not work on array objects. Instead, you need to copy each element individually or use functions like `memcpy` from the standard library.

```C
int arr1[5] = {1, 2, 3, 4, 5};
int arr2[5];

// arr2 = arr1; // This is invalid in C

// Correct way to copy arrays
for(int i = 0; i < 5; i++){
    arr2[i] = arr1[i];
}
```



### Array length

- There are two categories of arrays: **fixed-length arrays (FLA)** and **variable-length arrays (VLA)**.
- The first are a concept that has been present in C since the beginning; this feature is shared with many other programming languages.
- The second was introduced in C99 and is relatively unique to C, and it has some restrictions on its usage.

#### 1. "VLAs can't have initializers."

- Variable Length Arrays (VLAs) in C cannot be initialized at the time of declaration.
  - **Explanation**: Unlike fixed-length arrays, VLAs cannot have initial values assigned to them when they are declared. We must assign values to the elements of a VLA after its declaration.

```C
void example(int n){
    int arr[n]; //VLA
    // int arr[n] = {1, 2, 3}; // This is invalid
    for(int i = 0; i < n; i++){
        arr[i] = i + 1; // Assign values after declaration
    }
}
```



#### 2. "VLAs can't be declared outside functions"

- VLAs can only be declared within the scope of a function (i.e., they cannot be global or static).

- **Explanation**: VLAs are designed to have their size determined at runtime, which means they must be declared within a function where the size can be dynamically determined.

```C
int main(){
    int n = 5;
    int arr[n]; // VLA declared inside a function
    return 0;
}

// int arr[n]; // This is invalid, VLAs cannot be declared outside functions.
```



#### 3. "The length of an FLA is determined by an integer constant expression (ICE) or by an initializer"

- The length of a Fixed Length Array (FLA) must be specified by an integer constant expression (ICE) or deduced from an initializer.
  - **Explanation**: FLAs have a size that is known at compile time, either specified directly by an ICE or deduced from the number of elements in an initializer list.

```C
#define SIZE 10
int arr1[SIZE]; //FLA with size determined by an ICE

int arr2[] = {1, 2, 3, 4, 5}; // FLA with size determined by initializer (size is 5)
```



#### 4. "An Array-length specification must be strictly positive"

- The length of an array must be a positive integer.

- **Explanation**: Arrays in C cannot have a length of zero or a negative value. The length must be a positive integer.

```C
int arr[5]; // Valid
// int arr[0]; // Invalid, array length must be positive

// int arr[-1]; // Invalid, array length must be positive
```

- If the `[]` are left empty, the length of the array is determined from its initializer, if any:

```C
double E[] = {[3] = 42.0, [2] = 37.0};
double F[] = {22.0, 17.0, 1, 0.5};
```

- Here, `E` and `F` both are of type `double[4]`.
- Since such an initializer's structures can always be determined at compile time without necessarily knowing the values of the items, the array is still an **FLA**.

```
E[0] = double 0.0, E[1] = double 0.0,
E[2] = double 37.0, E[3] = double 42.0

F[0] = double 22.0, F[1] = double 17.0, F[2] = double 1.0, F[3] = double 0.5
```



#### 5. "An array with a length that is not an integer constant expression is a VLA"

- If the length of an array is not an integer constant expression (ICE), it is considered a Variable Length Array (VLA).
  - **Explanation**: VLAs have their size determined at runtime, which means their length is not a compile-time constant.

```C
void example(int n){
    int arr[n]; // VLA, length is not an ICE
}

int main(){
    int n = 5;
    int arr[n]; //VLA, length is not an ICE
    return 0;
}
```



#### 6. "The length of an array `A` is `(sizeof A) / (sizeof A[0])`"

- That is, it is the total size of the array object, divided by the size of any of the array elements.

```C
int main()
{
    int arr[] = {1, 2, 3, 4, 5, 6};

    int size = sizeof(arr) / sizeof(arr[0]);

    printf("Size: %d\n", size);
    return 0;
}
```



```sh
chan@CMA:~/C_Programming/practice$ ./practice
Size: 6
```



#### Summary

- **VLAs can't have initializers**: VLAs cannot be initialized at the time of declaration.
- **VLAs can't be declared outside functions**: VLAs must be declared within the scope of a function.
- **The length of an FLA is determined by an integer constant expression (ICE) or by an initializer**: FLAs have a size known at compile time, specified by an ICE or deduced from an initializer.
- **An array-length specification must be strictly positive**: Array lengths must be positive integers.
- **An array with a length that is not an integer constant expression is a VLA**: VLAs have their size determined at runtime, not at compile time.

### Arrays as parameters

1. **Decay to Pointers**:
   - When we pass an array to a function, it decays to a pointer to its first element. This means the function receives a pointer, not the entire array.
2. **Size Information**:
   - The size of the array is not passed to the function. If we need the size, we must pass it explicitly as an additional parameter.
3. **Modification**:
   - Since the function receives a pointer to the array, any modifications to the array elements within the function will affect the original array.
4. **Syntax**:
   - You can declare the function parameter as either `int arr[]` or `int *arr`. Both are equivalent in the context of function parameters.

#### "1.  The inner most dimension of an array parameter to a function is lost"

- When we pass an array to a function in C, what is actually passed is a pointer to the first element of the array. 
- This means that the size information of the innermost dimension is not available within the function. 
- For example, if we pass a 2D array, the function will receive a pointer to the first element of the array, and it won't know the size of the innermost dimension.

#### **"2. Don't use the sizeof operator on array parameters to functions"**:

- Since array parameters decay to pointers when passed to functions, using the `sizeof` operator on these parameters will give you **the size of the pointer**, **not the size of the array**. 
- This can lead to incorrect calculations and bugs. 
- For example, if you pass an array `int arr[10]` to a function, `sizeof(arr)` inside the function will give you the size of the pointer, not the size of the array (which would be `10 * sizeof(int)`).

#### **"3. Array parameters behave as if the array is passed by reference"**:

- When we pass an array to a function, what is actually passed is a pointer to the first element of the array. 
- This means that any changes made to the array elements within the function will affect the original array. 
- This behavior is similar to passing by reference, where the function can modify the original data.

#### Example

```C
#include <stdio.h>

void modifyArray(int arr[], int size) {
    // The size of arr is not known here, sizeof(arr) would give the size of the pointer
    for (int i = 0; i < size; i++) {
        arr[i] = i * 2; // Modifying the original array
    }
}

int main() {
    int myArray[5] = {1, 2, 3, 4, 5};
    modifyArray(myArray, 5);

    // Printing the modified array
    for (int i = 0; i < 5; i++) {
        printf("%d\n", myArray[i]);
    }
    return 0;
}
```

In this example:

- The `modifyArray` function receives a pointer to the first element of `myArray`.
- The `sizeof(arr)` inside `modifyArray` would give the size of the pointer, not the array.
- Changes made to `arr` inside `modifyArray` affect `myArray` in `main`, demonstrating pass-by-reference-like behavior.

```sh
chan@CMA:~/C_Programming/practice$ ./practice
0
2
4
6
8
```

#### Example 2

```C
void swap_double(double a[static 2])
{
    // Store the first element in a tmp variable;
    double tmp = a[0];
    // Assign the second element to the first element
    a[0] = a[1];
    // Assign the tmp variable (original first element) to the second element
    a[1] = tmp;
}

int main()
{
    // Initialize an array of two doubles
    double A[2] = {1.0, 2.0};

    // Call the function to swap the elements of the array
    swap_double(A);

    // Print the swapped elements
    printf("A[0] = %g, A[1] = %g\n", A[0], A[1]);
    return 0;
}
```

In this example:

- The function `swap_double` takes an array of two doubles as a parameter and swaps their values.
- The `static 2` in the parameter declaration indicates that the function expects an array of at least two elements. 
- This is a hint to the compiler for optimization and does not affect the runtime behavior.
- Here, `swap_double(A)` will act directly on array `A` and not on a copy. Therefore, the program will swap the values of the two elements of `A`.

---



## Strings are special

- There is a special kind of array that we have encountered several times and that, in contrast to other arrays, even has literals: **strings**.
- In C, the term "string" specifically refers to a sequence of characters terminated by a null character (`'\0'`). 
- This null character is what distinguishes a string from a mere array of characters. 

#### "A string is a 0-terminated array of `char`".

- A string like "hello" always has one more element than is visible, which contains the value 0, so here the array has length 6.

- Like all arrays, strings can't be assigned to, but they can be initialized from string literals:

  ```C
  char jay0[] = "jay";
  char jay1[] = {"jay"};
  char jay2[] = {'j', 'a', 'y', 0};
  char jay3[] = {'j', 'a', 'y'};
  ```

- These are all equivalent declarations. Be aware that not all arrays of `char` are strings, such as

```C
char jay4[3] = {'j', 'a', 'y'};
char jay5[3] = "jay";
```

- These both cut off after the 'y' character and so are not 0-terminated.

|      | [0]      | [1]      | [2]      | [3]       |
| ---- | -------- | -------- | -------- | --------- |
| jay0 | char 'j' | char 'a' | char 'y' | char '\0' |
| jay1 | char 'j' | char 'a' | char 'y' | char '\0' |
| jay2 | char 'j' | char 'a' | char 'y' | char '\0' |
| jay3 | char 'j' | char 'a' | char 'y' | char '\0' |
| jay4 | char 'j' | char 'a' | char 'y' |           |
| jay5 | char 'j' | char 'a' | char 'y' |           |



**Null-Terminated Strings**:

- A string in C is an array of characters that ends with a null character (`'\0'`). 
- This null character indicates the end of the string.
- Example:

```C
char str[] = "Hello"; // This is a string because it is null-terminated.
```

**Character Arrays**:

- An array of characters is simply a collection of characters stored in contiguous memory locations. It does not necessarily have to be null-terminated.
- Example:

```C
char arr[] = {'H', 'e', 'l', 'l', 'o'}; // This is not a string because it is not null-terminated.
```

**Implications**:

- Functions that operate on strings (like `strlen`, `strcpy`, `printf` with `%s`, etc.) expect the input to be null-terminated. 
- If we pass a character array that is not null-terminated to these functions, it can lead to undefined behavior because the functions will continue reading memory until they encounter a null character.
- Example:

```C
char arr[] = {'H', 'e', 'l', 'l', 'o'};
printf("%s\n", arr); // Undefined behavior because arr is not null-terminated.
```



#### Example

```C
#include <stdio.h>
#include <string.h>

int main() {
    // This is a string (null-terminated)
    char str[] = "Hello";
    printf("String: %s\n", str); // Prints "Hello"
    printf("Length of string: %zu\n", strlen(str)); // Prints 5

    // This is a character array, not a string
    char arr[] = {'H', 'e', 'l', 'l', 'o'};
    printf("Character array: %s\n", arr); // Undefined behavior
    printf("Length of character array: %zu\n", strlen(arr)); // Undefined behavior

    return 0;
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
String: Hello
Length of string: 5
Character array: HelloHello
Length of character array: 10
```



In this example:

- `str` is a string because it is null-terminated.
- `arr` is a character array but not a string because it lacks the null terminator.
- Using `strlen` or `printf`with `%s` on `arr` leads to undefined behavior because these functions expect a null-terminated string.
- `char arr[] = {'H', 'e', 'l', 'l', 'o'};` is not null-terminated.
- `printf("Character array: %s\n", arr);` prints "HelloHello". prints `"Hello"` followed by whatever additional characters happen to be in memory after `'o'`. This is why we see `"HelloHello"`; the `printf` function continues reading past the end of `arr` until it encounters a null terminator.
- `printf("Length of character array: %zu\n", strlen(arr));` prints 10 which is incorrect and may vary depending on what is in memory. This is because `strlen` also continues reading memory until it finds a null character, counting all characters it encounters.

**Key Points**

- **Undefined Behavior**: Since `arr` is not null-terminated, using `strlen` or `printf` with `%s` on it results in undefined behavior. The code is relying on the presence of a null terminator somewhere in memory, which can lead to unpredictable results.

- **Correct Approach**: To avoid undefined behavior and ensure proper string handling, always include a null terminator in character arrays that are used as strings. For example:

  ```C
  char arr[] = {'H', 'e', 'l', 'l', 'o', '\0'};
  ```

- **Memory Handling**: The extra characters being printed after `"Hello"` are just whatever happens to be in memory at that location, which is why we're seeing unexpected results.

- **Undefined Behavior (General)**: The term "undefined behavior" means that the C standard does not define what should happen in this situation. The program may crash, produce incorrect results, or appear to work correctly. The behavior can vary between different compilers, compiler versions, and even different runs of the same program.

##### Correct Approach

```C
int main()
{
    char str[] = "Hello";
    printf("String: %s\n", str);
    printf("Length of string: %zu\n", strlen(str));

    // Corrected character array to be null-terminated
    char arr[] = {'H', 'e', 'l', 'l', 'o', '\0'};
    printf("Character array: %s\n", arr);
    printf("Length of character array: %zu\n", strlen(arr));
    return 0;
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
String: Hello
Length of string: 5
Character array: Hello
Length of character array: 5
```

#### `mem` & `str` 

- In C, `mem` and `str` are common prefixes used in the names of standard library functions that operate on memory and strings, respectively. 
- Here’s a brief overview of some of these functions:

##### Memory Functions (`mem`)

These functions operate on raw memory blocks, treating them as arrays of bytes:

1. **`memcpy`**:
   - Copies `n` bytes from memory area `src` to memory area `dest`.
   - Prototype: `void *memcpy(void *dest, const void *src, size_t n);`
2. **`memmove`**:
   - Similar to `memcpy`, but handles overlapping memory areas correctly.
   - Prototype: `void *memmove(void *dest, const void *src, size_t n);`
3. **`memset`**:
   - Fills the first `n` bytes of the memory area pointed to by `s` with the constant byte `c`.
   - Prototype: `void *memset(void *s, int c, size_t n);`
4. **`memcmp`**:
   - Compares the first `n` bytes of the memory areas `s1` and `s2`.
   - Prototype: `int memcmp(const void *s1, const void *s2, size_t n);`

##### String Functions (`str`)

These functions operate on null-terminated strings:

1. **`strcpy`**:
   - Copies the string pointed to by `src` to `dest`.
   - Prototype: `char *strcpy(char *dest, const char *src);`
2. **`strncpy`**:
   - Copies up to `n` characters from the string pointed to by `src` to `dest`.
   - Prototype: `char *strncpy(char *dest, const char *src, size_t n);`
3. **`strcat`**:
   - Appends the string pointed to by `src` to the end of the string pointed to by `dest`.
   - Prototype: `char *strcat(char *dest, const char *src);`
4. **`strncat`**:
   - Appends up to `n` characters from the string pointed to by `src` to the end of the string pointed to by `dest`.
   - Prototype: `char *strncat(char *dest, const char *src, size_t n);`
5. **`strlen`**:
   - Computes the length of the string `s`.
   - Prototype: `size_t strlen(const char *s);`
6. **`strcmp`**:
   - Compares the string `s1` to the string `s2`.
   - Prototype: `int strcmp(const char *s1, const char *s2);`
7. **`strncmp`**:
   - Compares up to `n` characters of the string `s1` to the string `s2`.
   - Prototype: `int strncmp(const char *s1, const char *s2, size_t n);`
8. **`strchr`**:
   - Locates the first occurrence of `c` in the string `s`.
   - Prototype: `char *strchr(const char *s, int c);`
9. **`strrchr`**:
   - Locates the last occurrence of `c` in the string `s`.
   - Prototype: `char *strrchr(const char *s, int c);`
10. **`strstr`**:
    - Finds the first occurrence of the substring `needle` in the string `haystack`.
    - Prototype: `char *strstr(const char *haystack, const char *needle);`

#### Example Usage

```C
#include <stdio.h>
#include <string.h>

int main() {
    char src[] = "Hello, World!";
    char dest[50];

    // Using strcpy to copy a string
    strcpy(dest, src);
    printf("Copied string: %s\n", dest);

    // Using strcat to concatenate strings
    strcat(dest, " How are you?");
    printf("Concatenated string: %s\n", dest);

    // Using memset to set memory
    memset(dest, '*', 5);
    dest[5] = '\0'; // Null-terminate the string
    printf("Memory set string: %s\n", dest);
	printf("dest: %s\n", dest);
    return 0;
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Copied string: Hello, World!
Concatenated string: Hello, World! How are you?
Memory set string: *****
dest: *****
```

###### Notes

- **`memset`**: Fills a block of memory with a specified value.

- `memset(dest, '*', 5);` sets the first 5 bytes of `dest` to the character `'*'`.
- The string is then null-terminated at the 6th position to ensure it is a valid C string.



#### Example Usage 2

`practice.h`

```C
enum{
    ERROR_CODE = -1
};
```

`practice.c`

```C
int main(int argc, char *argv[argc + 1]{
    // Check if there is at least one command-line argument provided
    if(argc < 2){
        printf("Please provide a program name as an argument\n");
        return ERROR_CODE;
    }
    
    // Calculate the length of the first command-line argument (argv[1])
    size_t const len = strlen(argv[1]);
    
    // Create a character array 'name' with enough space to hold the string and the null terminator
    char name[len + 1];
    
    // Copy the string from argv[1] to the 'name' array
    memcpy(name, argv[1], len);
    
     // Null-terminate the copied string
    name[len] = '\0';
    
    // Compare the copied string with the original string
    if(!strcmp(name, argv[1])){
        printf("program name \"%s\ successfully copied\n" , name);
    }else{
        printf("copying %s leads to different string %s\n", argv[1], name);
    }
})
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Please provide a program name as an argument
chan@CMA:~/C_Programming/practice$ ./practice Chan
program name "Chan" successfully copied
chan@CMA:~/C_Programming/practice$ ./practice Chan Myae Aung
program name "Chan" successfully copied
chan@CMA:~/C_Programming/practice$ ./practice ChanMyae
program name "ChanMyae" successfully copied

```

In this example:

- `int argc`: The number of command-line arguments.
- `char *argv[argc + 1]`: An array of strings representing the command-line arguments. The `+ 1` accounts for the null terminator of the array.
- `strlen(argv[1])` calculates the length of the first command-line argument (excluding the null terminator).
- `memcpy(name, argv[1], len)` copies `len` bytes from `argv[1]` to `name`.
- Adds a null terminator to the end of the copied string `name` to make it a valid C string.
- `strcmp(name, argv[1])` compares the copied string with the original string.
  - If they are identical, it prints a success message.
  - If they are different, it prints an error message.

##### Key notes

- `memcpy(target, source, len)` can be used to copy one array to another. These have to be known to be distinct arrays. The number of `char` s to be copied must be given as a third argument `len`.
- `memcmp(s0, s1, len)` compares two arrays in lexicographic order. That is, it first scans the initial segments of the two arrays that happen to be equal and then returns the difference between the two characters that are distinct. If no differing elements are found up to `len`, 0 is returned.
- `memchr(s, c, len)` searches array `s` for the first appearance of character `c`.
- Difference Between `memcmp` and `strcmp`:
- **`memcmp`**:
  - Compares the first `n` bytes of two memory areas.
  - Returns an integer less than, equal to, or greater than zero if the first `n` bytes of `s1` are found to be less than, equal to, or greater than the first `n` bytes of `s2`, respectively.
  - Suitable for comparing binary data or non-null-terminated strings.
- **`strcmp`**:
  - Compares two null-terminated strings.
  - Returns an integer less than, equal to, or greater than zero if `s1` is found to be less than, equal to, or greater than `s2`, respectively.
  - Stops comparison at the first null terminator.

#### Example Usage of `memcmp`

```C
int main()
{
    char buffer1[] = "Hello, World!";
    char buffer2[] = "Hello, World!";
    char buffer3[] = "Hello, C Programming!";

    // Compare buffer1 and buffer2
    if (memcmp(buffer1, buffer2, strlen(buffer1)) == 0)
    {
        printf("buffer1 and buffer2 are identical.\n");
    }
    else
    {
        printf("buffer1 and buffer2 are different.\n");
    }

    // Compare buffer1 and buffer3
    if (memcmp(buffer1, buffer3, strlen(buffer1)) == 0)
    {
        printf("buffer1 and buffer3 are identical.\n");
    }
    else
    {
        printf("buffer1 and buffer3 are different.\n");
    }

    return 0;
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
buffer1 and buffer2 are identical.
buffer1 and buffer3 are different.
```

In this example,

- `buffer1` and `buffer2` are identical because they contain the same string `"Hello, World!"`.

- **Comparison of `buffer1` and `buffer2`**:

  - Both `buffer1` and `buffer2` contain the string `"Hello, World!"`.
  - `memcmp(buffer1, buffer2, strlen(buffer1))` compares the first `strlen(buffer1)` bytes of `buffer1` and `buffer2`.
  - Since both buffers contain the same string, the comparison returns `0` , indicating they are identical.

- **Comparison of `buffer1`and `buffer3`**:

  - `buffer3` contains the string `"Hello, C Programming!"`.

  - `memcmp(buffer1, buffer3, strlen(buffer1))` compares the first `strlen(buffer1)`bytes of `buffer1` and `buffer3`.

  - Since `buffer3` has a different string, the comparison returns a non-zero value, indicating they are different.

    

    

##### Summary

- `memcmp` is used for comparing a specified number of bytes in memory ,not the memory addresses themselves, suitable for binary data. It compares the actual content of the memory areas byte by byte.
- `strcmp` is used for comparing null-terminated strings.



#### Example Usage of `memchr`

```C
void *memchr(const void *s, int c, size_t n);
```

- **Parameters**:
  - `s`: Pointer to the memory area to be scanned.
  - `c`: Character to be located. It is passed as an `int`, but it is internally converted to `unsigned char`.
  - `n`: Number of bytes to be analyzed.
- The `memchr` function scans the initial `n` bytes of the memory area pointed to by `s` for the first instance of `c`. Both `c` and the bytes of the memory area pointed to by `s` are interpreted as `unsigned char`.
- **Return Value**:
  - Returns a pointer to the matching byte, or `NULL` if the character does not occur in the given memory area.

```C
int main(){
    char buffer[] = "Hello, World!";
    char ch = 'W';
    char *ptr = NULL;
    
    // Find the first occurrence of 'W' in buffer
    ptr = memchr(buffer, ch, strlen(buffer));
    
    if(ptr != NULL){
        printf("Character '%c' found at position %ld\n", ch, ptr - buffer);
    }else{
        printf("Character '%c' not found.\n", ch);
    }
    
    return 0;
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Character 'W' found at position 7
```

In this example:

- Finding Character `W` in `buffer`:
  - `memchr(buffer, ch, strlen(buffer))` searches for the first occurrence of `'W'` in the buffer.
  - If found, it returns a pointer to the character within the buffer.
  - If not found, it returns `NULL`.
- Why Subtract `ptr`from `buffer`:
  - **`ptr`**: This is a pointer to the first occurrence of the character `'W'` in the `buffer`.
  - **`buffer`**: This is a pointer to the start of the `buffer`.
  - When we subtract `ptr` from `buffer`, we are calculating the offset (or index) of the character `'W'` within the `buffer`. This is done by pointer arithmetic, which gives the position of the character relative to the start of the `buffer`.

##### Summary

- `memchr` is used to find the first occurrence of a character in a memory area.
- Subtracting `ptr` from `buffer` gives the position of the found character within the `buffer`.
- This technique is useful for determining the index of a character in a string or memory block.



#### Bullet Points between memory functions and string functions

- `strlen(s)` returns the length of the string `s`. 
  - This is simply the position of the first 0 character and not the length of the array. 
  - It is our duty to ensure that `s` is indeed a string: that is 0-terminated.
- `strcpy(target, source)` works similarly to `memcpy`. 
  - It only copies up to the string length of the `source` and therefore it doesn't need a `len` parameter.
  - Again, `source` must be 0-terminated.
  - Also, `target` must be big enough to hold the copy.
- `strcmp(s0, s1)` compares two arrays in lexicographic order, similar to `memcmp` but may not take some language specialities into account.
  - The comparison stops at the first 0 character that is encountered in either `s0` or `s1` . 
  - Again, both parameters have to be 0-terminated.
- `strcoll(s0, s1)` compares two arrays in lexicographic order, respecting language-specific environment settings.
- `strchr(s, c)` is similar to `memch` , only the string `s` must be 0-terminated.
- `strspn(s0, s1)` returns the length of the initial segment in `s0` that consists of characters that also appear in `s1`.
- `strcspn(s0, s1)` returns the length of the initial segment in `s0` that consists of characters that do not appear in `s1`.



### Explanation of `strspn` and `strcspn`

```C
size_t strspn(const char *s, const char *accept);
size_t strcspn(const char *s, const char *reject);
```

- **`strspn`**: Calculates the length of the initial segment of `s` which consists entirely of characters in `accept`.
- **`strcspn`**: Calculates the length of the initial segment of `s` which consists entirely of characters not in `reject`.

```C
#include <stdio.h>
#include <string.h>

int main() {
    const char *str1 = "abcde312$#@";
    const char *accept = "abcde";
    const char *reject = "1234567890";

    // strspn example
    size_t span = strspn(str1, accept);
    printf("The initial segment of '%s' containing only characters from '%s' is %zu characters long.\n", str1, accept, span);

    // strcspn example
    size_t cspan = strcspn(str1, reject);
    printf("The initial segment of '%s' containing no characters from '%s' is %zu characters long.\n", str1, reject, cspan);

    return 0;
}
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
The initial segment of 'abcde312$#@' containing only characters from 'abcde' is 5 characters long.
The initial segment of 'abcde312$#@' containing no characters from '1234567890' is 5 characters long.

```

##### Differences

- **`strspn`**:
  - Calculates the length of the initial segment of `s` which consists entirely of characters in `accept`.
  - Stops at the first character not in `accept`.
- **`strcspn`**:
  - Calculates the length of the initial segment of `s` which consists entirely of characters not in `reject`.
  - Stops at the first character in `reject`.

##### Detailed Steps

1. **Initialization:**

   ```C
   const char *str1 = "abcde312$#@";
   const char *accept = "abcde";
   const char *reject = "1234567890";
   ```

   - `str1` is the string to be analyzed.
   - `accept` contains the characters to be accepted by `strspn`.
   - `reject` contains the characters to be rejected by `strcspn`.

2. **Using `strspn`**:

   ```C
   size_t span = strspn(str1, accept);
   printf("The initial segment of '%s' containing only characters from '%s' is %zu characters long.\n", str1, accept, span);
   ```

   - `strspn(str1, accept)` calculates the length of the initial segment of `str1` which consists entirely of characters in `accept`.
   - The result is `5` because the first 5 characters of `str1` (`"abcde"`) are all in `accept`.

3. **Using `strcspn`**:

   ```C
   size_t cspan = strcspn(str1, reject);
   printf("The initial segment of '%s' containing no characters from '%s' is %zu characters long.\n", str1, reject, cspan);
   ```

   - `strcspn(str1, reject)` calculates the length of the initial segment of `str1` which consists entirely of characters not in `reject`.
   - The result is `5` because the first 5 characters of `str1` (`"abcde"`) do not contain any characters from `reject`.

##### Summary

- `strspn` and `strcspn` are used to calculate the length of the initial segment of a string based on character inclusion or exclusion.
- `strspn` stops at the first character not in the `accept` set.
- `strcspn` stops at the first character in the `reject` set.



#### "Using a string function with a non-string has undefined behavior."

- In C, string functions such as `strspn`, `strcspn`, `strlen`, `strcpy`, etc., are designed to operate on null-terminated strings. 
- A null-terminated string is an array of characters that ends with a null character (`'\0'`). 
- This null character signals the end of the string.
- **Undefined behavior** occurs when the code executes in a way that is unpredictable and not specified by the C standard. This can lead to various issues, including crashes, incorrect results, or security vulnerabilities.

##### Why Non-Strings Cause Undefined Behavior

1. **Lack of Null Terminator**:
   - If a character array is not null-terminated, string functions will continue reading memory beyond the intended array boundary until they encounter a null character. This can lead to accessing invalid memory locations, causing crashes or other unpredictable behavior.
2. **Memory Access Violations**:
   - Accessing memory outside the bounds of an array can lead to segmentation faults or other memory access violations.
3. **Incorrect Results**:
   - Without a null terminator, string functions may produce incorrect results because they rely on the null character to determine the end of the string.

**Incorrect Usage**

```C
#include <stdio.h>
#include <string.h>

int main() {
    char non_string[] = {'a', 'b', 'c', 'd', 'e'}; // No null terminator
    const char *accept = "abcde";

    // Using strspn with a non-string
    size_t span = strspn(non_string, accept);
    printf("The initial segment length is %zu\n", span);

    return 0;
}
```

**Potential Issues**

- Undefined Behavior:
  - The `strspn` function expects a null-terminated string. 
  - Since `non_string` is not null-terminated, `strspn` will continue reading memory beyond the array until it encounters a null character, leading to undefined behavior.

**Correct Usage**

- To avoid undefined behavior, always ensure that character arrays used with string functions are null-terminated:

```C
#include <stdio.h>
#include <string.h>

int main() {
    char valid_string[] = "abcde"; // Null terminator is present
    const char *accept = "abcde";

    // Using strspn with a valid string
    size_t span = strspn(valid_string, accept);
    printf("The initial segment length is %zu\n", span);

    return 0;
}
```



##### Summary

- **String Functions**: Designed to operate on null-terminated strings.
- **Undefined Behavior**: Occurs when using string functions with non-strings (arrays without null terminators).
- **Avoiding Issues**: Ensure character arrays are null-terminated when using string functions to prevent undefined behavior.
- In real life, common symptoms for such misuse may be:
  - Long times for `strlen` or similar scanning functions because they don't encounter a 0-character.
  - Segmentation violations because such functions try to access elements after the boundary of the array object.
  - Seemingly random corruption of data because the functions write data in places where they are not supposed to.
- In other words, we should be careful and make sure all our strings are really strings.
- If we know the length of the character array, but we do not know if it is 0-terminated, `memchr` and pointer arithmetic can be used as a safe replacement for `strlen`.
- If a character array is not known to be a string, it is better to copy it by using `memcpy`.



### Pointers as opaque types

- The main property of pointers is that they do not directly contain the information that we are interested in: rather, they  refer, or point, to the data.

```C
char const* const p2string = "some text";
```

It can be visualized like this:

| p2string | char const* const |
| -------- | ----------------- |
|          | &#8595;           |
|          | "some text"       |



#### "1. Pointers are opaque objects"

- **Meaning**: Pointers are abstract references to memory locations. The actual memory address they hold is not directly manipulated or interpreted by the programmer.

- **Example**:

  ```C
  int *ptr; // ptr is a pointer to an integer, but its actual address is opaque to the programmer.
  ```



#### "2. Pointers are valid, null, or indeterminate."

- **Valid Pointer**: Points to a valid memory location.
- **Null Pointer**: Points to no memory location (typically represented by `NULL`).
- **Indeterminate Pointer**: Uninitialized or invalid pointer, leading to undefined behavior if dereferenced.
- So, the really "bad" state of a pointer is indeterminate, since this state is not observable.

```C
int *valid_ptr = malloc(sizeof(int)); // Valid pointer
int *null_ptr = NULL; // Null pointer
int *indeterminate_ptr; // Indeterminate pointer
```



#### "3. Initialization or assignment with 0 makes a pointer null"

- **Meaning**: Assigning `0` or `NULL` to a pointer sets it to a null pointer, which means it points to no valid memory location.

- **Example**:

  ```C
  int *ptr = 0; // ptr is a null pointer
  int *ptr2 = NULL; // ptr2 is also a null pointer
  ```



#### "4. In logical expressions, pointers evaluate to false if they are null"

- **Meaning**: In conditional statements, null pointers are treated as `false`, while non-null pointers are treated as `true`.

- **Example**:

  ```C
  int *ptr = NULL;
  if (!ptr) {
      printf("Pointer is null.\n"); // This will be printed
  }
  ```



#### "5. Indeterminate pointers lead to undefined behavior"

- **Meaning**: Using pointers that have not been initialized or have been invalidated (e.g., after freeing memory) can cause unpredictable behavior.

- **Example**:

  ```C
  int *ptr; // Indeterminate pointer
  *ptr = 10; // Undefined behavior
  ```

  

#### "6. Always initialize pointers"

- **Meaning**: To avoid undefined behavior, always initialize pointers to a valid memory location or `NULL`.

- **Example**:

  ```C
  int *ptr = NULL; // Safe initialization
  int *valid_ptr = malloc(sizeof(int)); // Valid initialization
  ```

---



## Structures

- Arrays combine several objects of the same base type into a larger object.
- If we have to combine objects of different type, the, structures introduced by the keyword `struct` come into play.
- C structures allow for a more systematic approach by giving names to so-called **members (or field)** in an aggregate:

```C
struct birdStruct{
    char const* jay;
    char const* magpie;
    char const* raven;
    char const* chough;
};

struct birdStruct const aName = {
    .chough = "Henry",
    .raven = "Lissy",
    .magpie = "Frau",
    .jay = "Joe",
};
```

- So, instead of declaring four elements that are bound together in an array, here we name the different members and declare types for them.
- Such declaration of a structure type only explains the type: it is not yet the declaration of an object of that type and even less a definition for such an object.
- Then we declare and define a variable called `aName` of the new type.
- In the initializer and in later usage, the individual members are designated using a notation with a dot (.).
- Instead of `bird[raven]`, for the array, we use `aName.raven` for the structure.
- Note that the individual members again only refer to the strings.
- For example, the member `aName.magpie` refers to an entity "Frau" that is located outside the box and is not considered part of the`struct` itself.



**Example Scenario 2**

- Calendar time is a complicated way of counting, in years, month, days, minutes and seconds.
- The different time periods such as months or years can have different lengths, and so on. 

The use of this array would be ambiguous: would we store the year in element[0] or [5]?

- To avoid ambiguities, we could again use our trick with an `enum`.
- But the C standard has chosen a different way.
- In `time.h`, it uses a `struct` that looks similar to the following:

```C
struct tm{
  int tm_sec; // Seconds after the minute [0, 60]
  int tm_min; // Minutes after the hour [0, 59]
  int tm_hour; // Hours since midnight [0, 23]
  int tm_mday; // Day of the month [1, 31]
  int tm_mon; // Months since January [0, 11]
  int tm_year; // Years since 1900 
  int tm_wday; // Days since Sunday [0, 6]
  int tm_yday; // Days since January [0. 365]
  int tm_isdst; // Daylight Saving Time flag
};
```

- This `struct` has **named members**, such as `tm_sec` for the seconds and `tm_year` for the year.
- Encoding a date such as the date of this writing

```sh
chan@CMA:~/C_Programming/practice$ LC_TIME=C date -u
Sat Sep  7 15:11:23 UTC 2024
```

- **`LC_TIME=C`**: Sets the locale for date and time formatting to the default "C" locale, which is a standard POSIX locale. This ensures that the date and time are displayed in a consistent, language-independent format.
- **`date -u`**: Displays the current date and time in Coordinated Universal Time (UTC).

is relatively simple:

```C
struct tm today = {
    .tm_year = 2024 - 1900,
    .tm_mon = 9 - 1,
    .tm_mday = 7,
    .tm_hour = 15,
    .tm_min = 11,
    .tm_sec = 47,
}
```

- This creates a variable of type `struct` `tm` and initializes members with the appropriate values.
- The order or position of the members in the structure usually is not important:  using the name of the member preceded with a dot `.` suffices to specify where the corresponding data should go.
- A proper `struct` type creates an additional level of abstraction. 
- This `struct` `tm` is a proper type in C's type system.
- Accessing the members of the structure is simple:

```C
printf("this year is %d, next year will be %d\n", today.tm_year+1900, today.tm_year+1900+1);
```

```sh
chan@CMA:~/C_Programming/test$ ./final
this year is 2024, next year will be 2025
```

- There are three other members in `struct tm` that we didn't even mention in our initializer list: `tm_wday`, `tm_yday`, and `tm_isdst`.
- Since we didn't mention them, they are automatically set to 0.



#### "A `struct` initializer must initialize at least one member"

- When we use a designated initializer for a `struct`, we must initialize at least one member of the `struct`. 
- We cannot have an empty initializer list.

```C
struct example {
    int a;
    float b;
    char c;
};

// Valid initializer
struct example ex1 = { .a = 5 };

// Invalid initializer (will cause a compilation error)
struct example ex2 = { };
```

- In the invalid example, `ex2` does not initialize any member, which is not allowed.



#### "`Struct` parameters are passed by value"

- When we pass a `struct` to a function in C, the entire struct is copied, and the function receives its own copy of the `struct`. 
- This means that changes made to the `struct` inside the function do not affect the original `struct` outside the function.

```C
#include <stdio.h>

struct example {
    int a;
    float b;
    char c;
};

void modifyStruct(struct example ex) {
    ex.a = 10; // This change will not affect the original struct
}

int main() {
    struct example ex = { .a = 5, .b = 3.14, .c = 'x' };
    modifyStruct(ex);
    printf("ex.a = %d\n", ex.a); // Output will be 5, not 10
    return 0;
}
```

- In this example, `modifyStruct` receives a copy of `ex`. 
- Modifying `ex.a` inside `modifyStruct` does not change the original `ex` in `main`. 
- It only modifies the member of the parameter of the function `ex`.

```sh
chan@CMA:~/C_Programming/test$ ./final
ex.a = 5
```



#### "Structures can be assigned with = but not compared with == or !=."

- In C, we can assign one struct to another using the `=` operator. 
- This copies all the members from one struct to the corresponding members of another struct.

```C
struct example {
    int a;
    float b;
    char c;
};

int main() {
    struct example ex1 = { .a = 5, .b = 3.14, .c = 'x' };
    struct example ex2;

    ex2 = ex1; // All members of ex1 are copied to ex2

    printf("ex2.a = %d, ex2.b = %.2f, ex2.c = %c\n", ex2.a, ex2.b, ex2.c);
    return 0;
}
```

- In this example, `ex2` will have the same values as `ex1` after the assignment.

```sh
chan@CMA:~/C_Programming/test$ ./final
ex2.a = 5, ex2.b = 3.14, ex2.c = x

```



##### Comparison with `==` or `!=`

- However, we cannot directly compare two structs using the `==` or `!=` operators. 
- This is because these operators are not defined for struct types in C. 
- To compare structs, we need to compare each member individually.

```C
#include <stdio.h>
#include <stdbool.h>

struct example {
    int a;
    float b;
    char c;
};

bool areStructsEqual(struct example ex1, struct example ex2) {
    return (ex1.a == ex2.a) && (ex1.b == ex2.b) && (ex1.c == ex2.c);
}

int main() {
    struct example ex1 = { .a = 5, .b = 3.14, .c = 'x' };
    struct example ex2 = { .a = 5, .b = 3.14, .c = 'x' };

    if (areStructsEqual(ex1, ex2)) {
        printf("Structs are equal\n");
    } else {
        printf("Structs are not equal\n");
    }

    return 0;
}
```

- In this example, the `areStructsEqual` function compares each member of the structs to determine if they are equal.

```sh
chan@CMA:~/C_Programming/test$ ./final
Structs are equal
```



#### "A structure layout is an important design decision."

- The layout of a struct, i.e., the order and types of its members, can significantly impact the performance and memory usage of our program. 
- Here are some considerations:

##### Memory Alignment and Padding

- **Alignment**: Different data types have different alignment requirements. For example, an `int` might need to be aligned on a 4-byte boundary.
- **Padding**: The compiler may insert padding bytes between members to satisfy alignment requirements, which can lead to wasted memory.

```C
struct example1 {
    char c;
    int a;
    float b;
};

struct example2 {
    int a;
    float b;
    char c;
};
```

In `example1`, there might be padding bytes between `c` and `a` to align `a` on a 4-byte boundary. In `example2`, the members are already aligned, potentially reducing the size of the struct.

##### Performance Considerations

- **Cache Efficiency**: Structs that are frequently accessed should be designed to fit within cache lines to improve performance.

- **Access Patterns**: Grouping related members together can improve data locality and access speed.

- **Example**:

  ```C
  struct example {
      int id;
      char name[50];
      float salary;
  };
  ```

- In this example, if `id` and `salary` are frequently accessed together, placing them close to each other can improve cache efficiency.

##### Summary

- **Assignment**: Structs can be assigned using `=`, which copies all members.
- **Comparison**: Structs cannot be compared using `==` or `!=`; we must compare each member individually.
- **Layout Design**: The layout of a struct affects memory alignment, padding, and performance. Proper design can optimize memory usage and access speed.
