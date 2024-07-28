## C Programming Essentials

- C is a language behind the Internet and almost every operating system
- C is a language that's used to write almost all the other languages.
- C is a language that can write for almost every processor in existence, from watches and phones to places and satellites.

When we run `make` or when we run `clang`, four different things are happening under the hood.

- preprocessing
- compiling
- assembling
- linking

Speaking of compiling, some of the most widely used and modern C compilers include:

1. **GCC (GNU Compiler Collection)**:
   - Highly portable and widely used.
   - Supports various standards including C99, C11, and C18.
   - Available on most Unix-like systems, including Linux and macOS.
2. **Clang**:
   - Part of the LLVM project.
   - Known for its fast compilation times and excellent diagnostics.
   - Supports modern C standards and is highly portable.
   - Often used as the default compiler on macOS.
3. **MSVC (Microsoft Visual C++)**:
   - The primary compiler for Windows development.
   - Integrated with Visual Studio.
   - Supports modern C standards and provides excellent integration with Windows-specific features.

In C, the conventions for return values are as follows:

- `return 0;` typically means success or no error. This is the standard way to indicate that a program or function executed successfully.
- `return 1;` is often used to indicate a general error condition. It's a common convention to return 1 when an error occurs, but the specifics of the error are not differentiated.
- `return 2;`, `return 3;`, and so on, are sometimes used to represent different types of errors or failure conditions. However, there is no universal standard for what each non-zero return value means - it depends on the specific program or library.

Many programs and libraries define their own error codes, with different non-zero values representing different types of errors. For example, a library might return 2 for a file not found error, 3 for an invalid argument error, and so on.

It's important to note that these conventions are not enforced by the C language itself. They are simply common practices that programmers follow to indicate success or failure in their programs. The specific meaning of non-zero return values is up to the programmer or the library/API being used.

In general, if you see a non-zero return value from a C program or function, you should consult the documentation or source code to determine what that specific value means in that context.

### `POSIX`

In C programming, POSIX stands for "Portable Operating System Interface for Unix". It refers to a family of standards specified by the IEEE (Institute of Electrical and Electronics Engineers) that define the API (Application Programming Interface) for software compatible with Unix and Unix-like operating systems. These standards aim to ensure compatibility between different Unix systems, allowing software written for POSIX to be easily portable across various Unix platforms.

### `(*pointer)` vs `(pointer *)`

There is a significant difference between `(*pointer)` and `(pointer*)` in C. They are used in different contexts and serve different purposes.

### `(*pointer)`

`(*pointer)` is used to dereference a pointer. Dereferencing a pointer means accessing the value that the pointer is pointing to. Here, `pointer` is a variable that holds the address of another variable. The `*` operator, when placed before the `pointer`, accesses the value at the address stored in `pointer`.

Example:

```C
int value = 10;
int *pointer = &value; // pointer holds the address of value
printf("%d\n", *pointer); // prints 10, the value stored at the address
```

### `(pointer*)`

`(pointer*)` is a type cast operation in C. It is used to cast one type of pointer to another type. This is typically done to interpret the data in a different way.

Example:

```C
void *void_pointer;
int value = 10;
void_pointer = &value; // void_pointer now holds the address of value

int *int_pointer = (int*)void_pointer; // casting void_pointer to int*
printf("%d\n", *int_pointer); // prints 10, the value stored at the address
```

### Key Differences

1. **Dereferencing (`*pointer`)**:
   - Accesses the value stored at the address pointed to by `pointer`.
   - Used to get or set the value of the variable that `pointer` is pointing to.
2. **Type Casting (`(pointer*)`)**:
   - Changes the type of a pointer.
   - Used to interpret the pointed-to data in a different type.

### Examples to Illustrate the Difference

**Dereferencing Example**:

```C
int value = 42;
int *pointer = &value; // pointer holds the address of value
printf("Value: %d\n", *pointer); // Dereferencing pointer to print value: 42
```

**Type Casting Example**:

```C
void *void_pointer;
int value = 42;
void_pointer = &value; // void_pointer now holds the address of value

// Casting void_pointer to int* and then dereferencing to get the value
printf("Value: %d\n", *(int*)void_pointer); // Type casting and then dereferencing: 42
```

In summary, `(*pointer)` is for dereferencing to access the value stored at the pointer's address, whereas `(pointer*)` is for type casting a pointer to a different pointer type.

#### while(1)

The `while(1)` loop in C is an infinite loop. It continuously executes the block of code inside the loop without stopping, because the condition `1` (which represents a non-zero value, and thus `true` in C) never changes. To exit from such a loop, you typically need to use a break statement, return statement, or any other form of interrupt (like a signal handler in the case of a Unix-like system) that can alter the flow of control out of the loop.

```C
while(1){
    printf("This will print indefinitely.\n");
}
```

```C
int count = 0;
while(1){
    printf("Count: %d\n", count);
    count++;
    if(count == 10){
        break; //Exit the loop when the count reaches 10
    }
}
printf("Exited the loop.\n");
```



### Conditional Statements

- **In conditional statements**, `1` (or any non-zero value) typically means **true**, and `0` means **false**.
- This is why `if (!pid)` checks if `pid` is `0` (false), meaning the code inside this block runs in the child process, because `fork()` returns `0` in the child process.

### Return Values in `main` Function

- **In the `main` function**, returning `0` means **success**.
- Returning a **non-zero value** indicates an **error** or some other special condition.

### Example Breakdown

#### Conditional Statement:

```C
if (!pid)
{
    // This block runs in the child process
}
```

- Here, `!pid` evaluates to true if `pid` is `0` (which is the case in the child process after a successful `fork()`).

#### `main` Function Return:

```C
int main()
{
    // code
    return 0;  // Indicates success
}
```

- Returning `0` from the `main` function indicates that the program executed successfully.
- Returning a non-zero value indicates an error or abnormal termination. For example:

```C
int main()
{
    // code
    return 1;  // Indicates an error
}
```

### Summary

- **Conditional Statements**: `1` (or any non-zero value) means true; `0` means false.
- **`main` Function Return**: `0` means success; non-zero means error.

These conventions are widely used in C and many other programming languages.

#### C Concepts

- An array in C is essentially a constant pointer to its first element, we can represent an array of strings as an array of character pointers.

- Strings in C are compared using the `strcmp` function from the `<string.h>` library because strings are arrays of characters, and the `==` operator compares the addresses of the arrays rather than their contents.

- String comparison is based on the lexicographical order which is determined by the Unicode values (or ASCII values for simplicity) of the characters in the strings. 

- Lexicographical order compares strings character by character from left to right. If the characters are different, the comparison stops and the result is based on the difference between these characters. If they are the same, the comparison moves to the next character.

- Example: "apple" vs "banana". First characters: 'a' (Unicode: 97) and 'b' (Unicode: 98). Since 97 ('a') is less than 98 ('b'), "apple" is considered less than "banana". No further comparison is needed because the first character comparison already determined the result.

- ```C
  // Character Array
  char str1[] = "apple";
  
  // Pointer to a String Literal
  char *str2 = "apple";
  
  // Array of String Literals
  char *str3[] = {"apple", "banana", "cherry"};
  ```

- `char str1[] = "apple"` : We can modify `str1` (e.g., `str1[0] = 'A';`). Allocates memory on the stack and copies the string into it.

- `char *str1 = "apple"`: Modifying the string literal via `str1` (e.g., `str1[0] = 'A';`) leads to undefined behavior because string literals are stored in read-only memory. The pointer points to a fixed location in memory where the string literal is stored.

- What does `i->name` equivalent in pointer notation? `i->name` is equivalent to `(i*).name`. This notation dereferences the pointer `i` to access the `name` member of the structure(`struct`) it points to.

- In C, `void (*handler)(int)` is a declaration of a pointer to a function that takes an integer as its argument and returns nothing (`void`). This syntax is used for defining function pointers, which are variables that can store the address of a function. In this context, `handler` is a function pointer that is expected to point to a function designed to handle signals, with the signal number being passed as the integer argument.

  Here's a breakdown of the syntax:

  - `void` indicates the return type of the function that `handler`can point to, meaning the function does not return a value.
  - `(*handler)` is the function pointer itself. The parentheses around `*handler` are necessary to indicate that `handler` is a pointer to a function, not a function returning a pointer.
  - `(int)` specifies the type of argument that the function pointed to by `handler` takes, which is a single integer.

  Function pointers like `handler` are commonly used in C for callback functions, where you pass the address of a function into another function to be called later. 


#### Escaping Sequence

\n - newline

\t - tab

```c
#include <stdio.h>

int main(void){
    printf("hello\n");
    printf("1\t2\t3\t");
    printf("\"I like Pizza\" - Abraham Lincoln probably\n"); // "I like Pizza" - Abraham Lincoln probably
}
```

\r - carriage return character

In C, `\r` is an escape sequence that represents the carriage return character. When output to a terminal or console, the carriage return (`\r`) moves the cursor to the beginning of the current line without advancing to the next line. This can be used to overwrite the contents of the current line in the terminal.

For example, if you print `"Hello, World!\rGoodbye!"`, the output will end up being `"Goodbye! World!"` because the cursor returns to the beginning of the line after `"Hello, World!"` is printed, and `"Goodbye!"` starts overwriting `"Hello, World!"` from the beginning of the line.

---



#### Variable

Allocated space in memory to store a value. We need to declare what type of data we are storing.

%s = string, %d = decimal, %i = integer

```c
#include <stdio.h>

int main(void){
    int x;
    x = 123;
    int y = 321;
    int age = 21;
    float gpa = 2.05; // floating point number
    char grade = 'C'; // Single character
    char name[] = "Bro"; //array of characters
    
    printf("Hello %s\n", name); // Hello Bro
    
    printf("You are %d years old",age); // You are 21 years old
    
    printf("Your average grade is %c", grade);
    // Your average grade is C
    
    printf("Your gpa is %f\n", gpa);
    // %0.15f = 15 decimal point
    // Your gpa is 2.05000
    return 0;
}
```



---



#### Data Types in C

- **char a = 'C';**  (Single character %c)
- **char b[] = "Bro";**  (Array of characters %s)
- **char f = 100;** (1 byte (-128 to +127) %d or %c)
- **unsigned char g = 255;** (1 byte (0 to +255) %d or %c)
- **float c = 3.141592;**  (4 bytes (32 bits of precision) 6 - 7 digits %f)
- **double d = 3.141592653589793;** (8 bytes (64 bits of precision) 15- 16 digits %lf)
- **int j = 2121323;**  (4 bytes (-2147,483,648 to +2,147,483,647) %d)
- **short int h = 32767;** (2 bytes (-32,768 to +32767) %d)
- **unsigned short int i = 65535;** (2 bytes (0 to +65535) %d)
- **unsigned int k = 4294967295;** (4 bytes (0 to +4,294967295) %u)
- **bool e = true;** (1 byte (true of false) %d) #include <stdbool.h>

---



#### Format Specifier % 

Defines and formats a type of data to be displayed.

- %c - character
- %s - string (array of characters)
- %f - float
- %lf - double
- %d - integer
- %.1 - decimal precision
- %1 - minimum field width
- % - left align

---



#### Constants

fixed value that cannot be altered by the program during its execution.

It's a good practice to change the variable name to uppercase.



```c
#include <stdio.h>
int main(){
    const float PI = 3.14159;

	printf("%f\n", PI);
    return 0;
}

```



---



#### User Inputs

fgets and scanf are both used to read input from the user.

1. **fgets:** a function used to read a line of text from the standard input and store it as a string.

   Syntax: `fgets(buffer, size, stdin);`

   Parameters:

   - `buffer`: A pointer to a character array (string) where the input will be stored.
   - `size`: The maximum number of characters to read, including the null terminator.
   - `stdin`: The standard input stream, usually `stdin` (the keyboard).
   - fgets reads characters from the input stream until it encounters a newline character(`'\n'`) or reaches the specified size limit.

2. **scanf:** a function used to read formatted input from the standard input and assign values to variables.

   Syntax: `scanf(format, &variable);`

   Parameters:

   - `format`: A string specifying the format of the input to be read. It can include format specifiers like `%d` for integers, `%f` for floats, `%s` for strings, etc.
   - `&variable`: The address of the variable where the input value will be stored.
   - scanf skips leading whitespace characters (such as spaces, tabs and newline characters) and reads characters based on the specified format until it encounters whitespace or reaches the end of the input.

```c
int main(){
    char name[25]; // bytes
    int age;
    
    printf("What's your name?\n ");
    fgets(name, 25, stdin);
    
    printf("How old are you?\n");
    scanf("%d", &age);
    
    printf("Hello %s, how are you?", name);
    printf("You are %d years old", age);
 	return 0;
}
```



**Note:** You will notice that the output will be displayed on the next line. That's because when using the fgets function, it will include a newline character when you hit ENTER. 

To get rid of that newline whenever you hit ENTER, 

```c
#include <string.h>

int main(){
    char name[25];
    //Just below the fgets function, 
    name[strlen(name)-1] = '\0';
}
```





---



#### Math functions

```c
#include <stdio.h>
#include <math.h> // Must be included.

int main(){
    double A = sqrt(9); // 3.000000
    double B = pow(2,4); // 16.000000
    int C = round(3.14); // 3
    int D = ceil(3.14); // 4
    int E = floor(3.99); // 3
    double F = fas(-100); // 100 
    double G = log(3); // 1.098612
	double H = sin(45); // 0.850904
	double I = cos(45); // 0.525322
	double J = tan(45); //1.619775

    
    return 0;
}
```



---



#### Switch

A more efficient alternative to using many "else if" statements. It allows a value to be tested for equality against many cases.

```c
#include <stdio.h>
#include <ctype.h> // To be able to convert uppercase or lowercase the input.

int main(){
    char grade;
    
    printf("What is your grade? \n");
    scanf("%c", &grade);
    
    //Conver the input grade to uppercase
    grade = toupper(grade);
    
    switch(grade){
        case 'A':
            printf("Perfect!\n");
            break;
        case 'B':
            printf("You did good!\n");
            break;
        case 'C':
            printf("You did okay!\n");
            break;
        case 'D':
            printf("At least it's not an F!\n");
            break;
        case 'F':
            printf("YOU FAILED!\n");
            break;
        default:
            printf("Please enter only valid grades");
            
    }
    
    return 0;
}

```



---



#### Arguments & Parameters

Arguments - the actual data values that are passed into the function or method when it is called.

Parameters - defined by the function or method and act as placeholders for data. (values what a function expects when it is invoked.)



```c
#include <stdio.h>

void birthday(char name[], int age); // Parameters

int main(){
    char name[] = "Bro";
    int age = 21;
    
    birthday(name, age); // Arguments
    
    return 0;
}

void birthday(char name[], int age){
    printf("Happy birthday dear %s\n", name);
    printf("You are %d years old\n", age);
}
```



---



#### String Functions

```c
#include <stdio.h>
#include <string.h>

int main(void){
    char string1[] = "Sun";
    char string2[] = "Moon";
    
    // appends string2 to the end of string1
    strcat(string1, string2); // SunMoon
    
    // appends n characters from string2 to string1
    strncat(string1, string2, 1); // SunM
    
    //copy string2 to string1
    strcpy(string1, string2); // Moon
    
    //copy n characters of string2 to string1
    strncpy(string1, string2, 1); // Mun
    
}
```



---



#### Arrays

```c
#include <stdio.h>

float average(int length, int array[]);

int main(void){
    int scores[3]; //defining the length of the array which is 3.
    
    //Getting user inputs 
    for(int i = 0; i < 3; i++){
        printf("Enter your score %d: \n", i);
        scanf("%d", &scores[i]); // dynamically assigning the location of the array .
    }
    
    printf("Your average is: %2f\n", average(N, scores));
}

float average(int length, int array[]){
    //Calculate the average
    int sum = 0;
    for(int i = 0; i < length; i++){
        sum += array[i];
    }
    return sum / (float) length;
}
```

---

### Key Differences:

- **Memory Allocation:**
  - `char arr[]` allocates contiguous memory for the characters of the string and null terminator.
  - `char *arr[]` allocates memory for an array of pointers, where each pointer can point to a different string.
- **Modification:**
  - You can modify `char arr[]` directly because it's an array of characters.
  - You can modify `char *arr[]` to point to different strings, but modifying the string literals themselves (`"apples"`, etc.) directly is not allowed because they are typically stored in read-only memory.
- **Usage:**
  - `char arr[]` is used for a single mutable string.
  - `char *arr[]` is used for an array of mutable pointers to strings or string literals.

In summary, `char arr[] = {"apples"}` represents a single mutable string stored as an array of characters, while `char *arr[] = {"apples"}` represents an array of pointers where each pointer can point to a string or string literal. The choice between them depends on whether you need a single mutable string (`char arr[]`) or an array of pointers to strings (`char *arr[]`).

#### `strcasecmp()` and `strcmp()`

To use the `strcasecmp` function in C, we need to include the `strings.h` header file. 

- This function is used to compare two strings in a case-insensitive manner.

Here's a brief example of how to use `strcasecmp`:

```C
#include <stdio.h>
#include <strings.h>

int main() {
    const char *str1 = "Hello";
    const char *str2 = "hello";

    if (strcasecmp(str1, str2) == 0) {
        printf("The strings are equal (case-insensitive comparison).\n");
    } else {
        printf("The strings are not equal (case-insensitive comparison).\n");
    }

    return 0;
}
```

In this example:

- The `strcasecmp` function is used to compare `str1` and `str2` without considering the case of the characters.
- If `strcasecmp` returns 0, the strings are considered equal.
- Otherwise, the strings are considered not equal.



There are important differences between `strcmp` and `strcasecmp` in C:

1. **Case Sensitivity**:
   - `strcmp`: This function compares two strings in a case-sensitive manner. This means it distinguishes between uppercase and lowercase characters.
   - `strcasecmp`: This function compares two strings in a case-insensitive manner. It treats uppercase and lowercase characters as equivalent.
2. **Function Signatures**:
   - `strcmp`: The signature is `int strcmp(const char *s1, const char *s2);`
   - `strcasecmp`: The signature is `int strcasecmp(const char *s1, const char *s2);`
3. **Return Values**: Both functions return an integer value:
   - If the strings are equal, both functions return `0`.
   - If the first non-matching character in `s1` is less than the corresponding character in `s2`, both functions return a negative value.
   - If the first non-matching character in `s1` is greater than the corresponding character in `s2`, both functions return a positive value.

Here's an example to illustrate the difference:

```C
#include <stdio.h>
#include <string.h>
#include <strings.h>

int main() {
    const char *str1 = "Hello";
    const char *str2 = "hello";

    // Case-sensitive comparison
    if (strcmp(str1, str2) == 0) {
        printf("strcmp: The strings are equal.\n");
    } else {
        printf("strcmp: The strings are not equal.\n");
    }

    // Case-insensitive comparison
    if (strcasecmp(str1, str2) == 0) {
        printf("strcasecmp: The strings are equal.\n");
    } else {
        printf("strcasecmp: The strings are not equal.\n");
    }

    return 0;
}
```

Output:

```sh
strcmp: The strings are not equal.
strcasecmp: The strings are equal.
```

In this example, `strcmp` reports that the strings are not equal because it considers the case of the characters, while `strcasecmp` reports that the strings are equal because it ignores the case of the characters.



#### `strncasecmp()`

The `strncasecmp` function in C is similar to `strcasecmp` but with an additional parameter that limits the number of characters to compare. This function performs a case-insensitive comparison of the first `n` characters of two strings.

### Function Signature

```C
int strncasecmp(const char *s1, const char *s2, size_t n);
```

### Parameters

- `s1`: A pointer to the first null-terminated string.
- `s2`: A pointer to the second null-terminated string.
- `n`: The maximum number of characters to compare.

### Return Value

The return value is similar to `strcmp` and `strcasecmp`:

- If the first `n` characters of `s1` and `s2` are equal (ignoring case), the function returns `0`.
- If the first non-matching character in `s1` is less than the corresponding character in `s2`, the function returns a negative value.
- If the first non-matching character in `s1` is greater than the corresponding character in `s2`, the function returns a positive value.

### Example Usage

Here's an example to illustrate how `strncasecmp` works:

```C
#include <stdio.h>
#include <strings.h>

int main() {
    const char *str1 = "HelloWorld";
    const char *str2 = "helloEveryone";

    // Compare first 5 characters in a case-insensitive manner
    if (strncasecmp(str1, str2, 5) == 0) {
        printf("The first 5 characters of the strings are equal (case-insensitive comparison).\n");
    } else {
        printf("The first 5 characters of the strings are not equal (case-insensitive comparison).\n");
    }

    // Compare first 10 characters in a case-insensitive manner
    if (strncasecmp(str1, str2, 10) == 0) {
        printf("The first 10 characters of the strings are equal (case-insensitive comparison).\n");
    } else {
        printf("The first 10 characters of the strings are not equal (case-insensitive comparison).\n");
    }

    return 0;
}
```

Output:

```sh
The first 5 characters of the strings are equal (case-insensitive comparison).
The first 10 characters of the strings are not equal (case-insensitive comparison).
```

In this example:

- When comparing the first 5 characters, `strncasecmp` considers the strings equal because "Hello" and "hello" match in a case-insensitive manner.
- When comparing the first 10 characters, `strncasecmp` finds a difference beyond the 5th character ("W" vs "E"), so it determines the strings are not equal.
