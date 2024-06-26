## C Programming Essentials

When we run `make` or when we run `clang`, four different things are happening under the hood.

- preprocessing
- compiling
- assembling
- linking

In C, the conventions for return values are as follows:

- `return 0;` typically means success or no error. This is the standard way to indicate that a program or function executed successfully.
- `return 1;` is often used to indicate a general error condition. It's a common convention to return 1 when an error occurs, but the specifics of the error are not differentiated.
- `return 2;`, `return 3;`, and so on, are sometimes used to represent different types of errors or failure conditions. However, there is no universal standard for what each non-zero return value means - it depends on the specific program or library.

Many programs and libraries define their own error codes, with different non-zero values representing different types of errors. For example, a library might return 2 for a file not found error, 3 for an invalid argument error, and so on.

It's important to note that these conventions are not enforced by the C language itself. They are simply common practices that programmers follow to indicate success or failure in their programs. The specific meaning of non-zero return values is up to the programmer or the library/API being used.

In general, if you see a non-zero return value from a C program or function, you should consult the documentation or source code to determine what that specific value means in that context.

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
    // Your code here
    return 0;  // Indicates success
}
```

- Returning `0` from the `main` function indicates that the program executed successfully.
- Returning a non-zero value indicates an error or abnormal termination. For example:

```C
int main()
{
    // Your code here
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
