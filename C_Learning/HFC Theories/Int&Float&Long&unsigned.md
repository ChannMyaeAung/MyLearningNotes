Different data types use different amounts of memory. So, you need to be careful that you don't try to store a value that's too large for the amount of space allocated to a variable. `short` variables take up less memory than `ints` and `ints` take up less memory than `longs`.

```C
short x = 15;
int y = x;
printf("The value of y = %i\n", y);
// The value of y = 15
```



The problems start to happen if you go the other way around.

```C
int x = 100000;
short y = x;
printf("The value of y = %hi\n", y);
// %hi is the proper code to format a short value.
// The value of y = -31072
```

```bash
chan@CMA:~/C_Programming/HFC$ make test
cc     test.c   -o test
chan@CMA:~/C_Programming/HFC$ ./test
The value of y = -31072

```



So why did putting a large number into `short` go negative? Numbers are stored in binary. This is what 100,000 looks like in binary.

```bash
x <- 0001 1000 0110 1010 0000
```

But when the computer tried to store that value into a `short`, it only allowed the value a couple of bytes of storage. The program stored just the righthand side of the number.

```bash
y <- 1000 0110 1010 0000
```



Signed values in binary beginning with a 1 in highest bit are treated as negative numbers. And this shortened value is equal to this in decimal:

```bash
-31072
```



#### Use casting to convert the numbers on the fly

```C
int x = 7;
int y = 2;
float z = x / y;
printf("z = %f\n", z);
// z = 3.0000
```

Why is that? Well x and y are both integers and if you divide integers you always get a rounded-off whole number-- in this case, 3.



To get a floating-point result,

```C
int x = 7;
int y = 2;
float z = (float) x / (float) y;
printf("z = %f\n", z);
// z = 3.5000
```

The `(float)` will cast an integer value into a float value. The calculation will then work just as if you were using floating-point values the entire time. In face, if the compiler sees you are adding, subtracting, multiplying or dividing a floating-point value with a whole number, it will automatically cast the numbers for you. That means you can cut down the number of explicit casts in your code:

```C
float z = (float) x / y;
// The compiler will automatically cast y to a float.
```



##### unsigned

- The number will always be positive because it doesn't need to worry about recording negative numbers, `unsigned` numbers can store larger numbers since there's now one more bit to work with.

- An `unsigned int` stores numbers from 0 to a maximum value that is about twice as large as the maximum number that can be stored inside an `int`.

- There's also a signed keyword, but we almost never see it, because all data types are signed by default.

  ```C
  unsigned char c;
  // This will probably store numbers from 0 to 255.
  ```



##### long

- We can prefix a data type with the word `long` and make it longer.

- A `long int` is a longer version of an `int` which means it can store a larger range of numbers.

- A `long long` is longer than a `long`.

- We can also use `long` with floating-point numbers.

  ```C
  long double d;
  // A really really precise number.
  ```

  



Q: Why are data types different on different OS? Wouldn't it be less confusing to make them all the same?

A: C uses different data types on different OS and processors because that allows it to make the most out of the hardware.



Q: In what way?

A: When C was first created, most machines were 8-bit. Now, most machines are 32- or 64- bit. Because C doesn't specify the exact size of its data types, it's been able to adapt over time. And as newer machines are created, C will be able to make the most of them as well.



Q: What do 8-bit and 64-bit actually mean?

A: Technically, the bit size of a computer can refer to several things, such as the size of its CPU instructions or the amount of data the CPU can read from memory. The bit size is really the favored size of numbers that the computer can deal with.



Q: So what does that have to do with the size of `ints` and `doubles`?

A: If a computer is optimized best to work with 32-bit number, it makes sense if the basic data type-- the `int` is set at 32-bits.



Q: What is the preprocessor?

A: Preprocessing is the first stage in converting the raw C source code into a working executable. Preprocessing creates a modified version of the source just before the proper compilation begins. In your code, the preprocessing step read the contents of the header file into the main file.



Q: Does the preprocessor create an actual file?

A: No, compilers normally just use pipes for sending the stuff through the phases of the compiler to make things more efficient.



Q: What directories will the compiler search when it is looking for header files?

A: The `gcc` compiler knows where the standard headers are stored. On a Unix-style OS, the header files are normally in places like `/usr/local/include`, `/usr/include`, and a few others.



Q: Why does the compiler do the preprocessing?

A: Strictly speaking, the compiler just does the compilation step; it converts the C source code into assembly code. But in a looser sense, all of the stages that convert the C source code into the final executable are normally called compilation, and the `gcc` tool allows you to control those stages. The `gcc` tool does preprocessing and compilation.



#### Bullet Points

- If a compiler finds a call to a function it hasn't heard of, it will assume the function returns an `int`.
- So if you try to call a function before you define it, there can be problems.
- Function declarations tell the compiler what your functions will look like before you define them.
- If function declarations appear at the top of your source code, the compiler won't get confused about return types.
- Function declarations are often put into header files.
- You can tell the compiler to read the contents of a header file using `#include`.
- The compiler will treat `include` code the same as code that's typed into the source file.