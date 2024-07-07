## Theories from HFC Book 

**Compiler Flags**: `CFLAGS = -Wall -Wextra -g` 

- `-Wall`: Enables all the commonly used warning messages.
- `-Wextra`: Enables additional warning messages that are not included by `-Wall`.
- `-g`: Generates debug information to use with GDB (or any debugger).

#### Chapter 2 - Memory and pointers



**Pointer** - A pointer is just the address of a piece of data in memory.

- Pointers help you do both these things: avoid copies and share data.



Every time you declare a variable, the computer creates space for it somewhere in memory. If you declare a variable inside a function like main(), the computer will store it in a section of memory called the **stack**. If a variable is declared outside any function, it will be stored in the **globals** section of memory.



- **&** to find out the memory address of the variable.

  ```C
  printf("x is stored at %p\n", &x);
  
  // x is stored at 0x7ffdd281e4b4
  
  ```

  

- The address of the variable tells you where to find the variable in memory. That's why an address is also called a **pointer** because it **points** to the variable in memory.



Q: Why are local variables stored in the stack and globals stored somewhere else?

A: Local and global variables are used differently. You will only ever get one copy of a global variable, but if you write a function that calls itself, you might get very many instances of the same local variable.



- Pointers make it easier to share memory, to let functions share memory. The data created by one function can be modified by another function, so long as it knows where to find it in memory.



Three things you need to know in order to use pointers to read and write data.

1. Get the address of a variable.

   - A pointer variable is just a variable that stores a memory address. When declaring a pointer variable, you need to say what kind of data is stored at the address it will point to:

     ```C
     int x = 4;
     int *address_of_x = &x;
     ```

     

2. Read the contents of an address.

   - When you have  a memory address, you will want to read the data that's stored there with * operator:

     ```C
     int value_stored = *address_of_x;
     // This will read the contents at the memory address given by address_of_x. This will be set to 4: the value originally stored in the x variable.
     ```

   - Notes: **The & operator takes a piece of data and tells you where it's stored while the * operator takes an address and tells you what's stored there.**

   - Because pointers are sometimes called references, the * operator is said to dereference a pointer. (dereferencing in details below)

3. Change the contents of an address.

   - If you have a pointer variable and want to change the data at the address where the variable's pointing, you can just use * operator again but on the **left side** of an assignment:

     ```C
     *address_of_x = 99;
     ```

     

```C
int x = 10; //Declare an integer x
int *p; //Declare a pointer p;
p = &x; //Store the address of x in p
printf("%d", *p); // Print the value at address stored in p (which is x)

//This will output 10 because *p gives the value at the address stored in p which is the value of x.
```





Summary Bullet Points

- Variables are allocated storage in memory.
- Local variables live in the stack.
- Global variables live in the globals section.
- Pointers are just variables that store memory addresses.
- The & operator finds the address of a variable.
- The * operator can read the contents of a memory address.
- The * operator can also set the contents of a memory address.
- An array variable can be used as a pointer. 
- The array variable points to the first element in the array.
- If you declare an array argument to a function, it will be treated as a pointer.
- The **sizeof** operator returns the space taken by a piece of data. It returns the size of the pointer, which is typically 4 or 8 bytes depending on the system's architecture, not the size of the original array.
- You can also call **sizeof** for a data type such as **sizeof(int)**.
- **sizeof(a pointer)** returns 4 on 32-bit OS and 8 on 64-bit. On 32-bit OS, a memory address is stored as a 32-bit number. That's why it's called a 32-bit system. 32 bits == 4 bytes. That's why a 64-bit system uses 8 bytes to store an address.
- **sizeof** is not a function but it's an operator.
- An operator is compiled to a sequence of instructions by the compiler. But if the code calls a function, it has to jump to a separate piece of code.
- If I create a pointer variable, does the pointer variable live in memory? Yes. A pointer variable is just a variable storing a number. You can find the address of a pointer variable using the & operator.



#### Dereferencing (In Details)

In C programming, dereferencing is the process of accessing the value that a pointer is pointing to. A pointer is a variable that stores the memory address of another variable, and dereferencing is the way to retrieve or modify the value stored at that address.

The dereference operator in C is the asterisk (`*`). When you have a pointer variable, you can use the dereference operator to access the value it points to.

Here's an example to illustrate dereferencing:

```
cCopy code#include <stdio.h>

int main() {
    int x = 10;  // Declare and initialize an integer variable
    int *ptr;    // Declare a pointer variable

    ptr = &x;    // Store the address of x in the pointer variable ptr

    printf("Value of x: %d\n", x);            // Output: Value of x: 10
    printf("Value stored at address %p: %d\n", ptr, *ptr);  // Output: Value stored at address 0x7ffeebfc4b9c: 10

    *ptr = 20;   // Dereference ptr to modify the value it points to

    printf("Value of x: %d\n", x);            // Output: Value of x: 20

    return 0;
}
```

In this example:

1. We declare an integer variable `x` and initialize it with the value `10`.
2. We declare a pointer variable `ptr` of type `int*`, which means it will store the address of an integer variable.
3. We store the address of `x` in the pointer variable `ptr` using the address-of operator `&`.
4. We print the value of `x` and the value stored at the address contained in `ptr` using the dereference operator `*ptr`.
5. We use the dereference operator `*ptr` on the left-hand side of an assignment to modify the value that `ptr` is pointing to, changing the value of `x` to `20`.
6. We print the new value of `x` to confirm that it has been modified.

Dereferencing is a fundamental concept in C programming, as it allows you to manipulate the values stored in memory locations using pointers. It's essential to understand dereferencing to work with dynamic memory allocation, data structures like linked lists, and other advanced programming concepts in C.

However, it's important to be careful when dereferencing pointers, as dereferencing an invalid or uninitialized pointer can lead to undefined behavior and potential program crashes.

---



#### Arrays

An array variable can be used as a pointer to the first element in an array. That means you can read the first element of the array either by using the brackets notation or using the * operator like this

The index is just the number that's added to the pointer to find the location of the element.



```C
int drinks[] = {4,2,3};
printf("1st order: %i drinks\n", drinks[0]);
printf("1st order: %i drinks\n", *drinks);
// The above two lines of code are equivalent.
// drinks[0] == *drinks

printf("3rd order: %i drinks\n", drinks[2]);
printf("3rd order: %i drinks\n", *(drinks + 2));
```



#### Why pointers have types

Pointer types exist so that the compiler knows how much to adjust the pointer arithmetic.

For instance, if you add 1 to a char pointer, the pointer will point to the very next memory address because a char occupies 1 byte of memory.



**int** usually takes 4 bytes of space, so if you add 1 to an int pointer, the complied code will actually add 4 to the memory address.

```C
int nums[] = {1,2,3};
printf("nums is at %p\n", nums);
printf("nums + 1 is at %p\n", nums + 1);

// nums is at 0x7ffdca81cabc
// nums + 1 is at 0x7ffdca81cac0

// nums + 1 is 4 bytes away from nums.
// these addresses are printed in hex format.

int arr[5] = {1,2,3,4,5};
printf("result: %p %p %p\n", arr, arr + 1, arr + 2);

// result: 0x7fff82772940 0x7fff82772944 0x7fff82772948
// 4 bytes away from each other.
```



##### Bullet Points

- Array variables can be used as pointers
- but array variables are not quite the same
- Array variables are not pointers themselves. They are just a special type of variable that can be used as pointers in certain contexts thanks to the implicit conversion provided by the C language.
- Array variables have a fixed size and cannot be reassigned to point to a different memory location unlike pointers.
- *sizeof* is different for array and pointer variables.
- Array variables can't point to anything else.
- Passing an array variable to a pointer decays it.
- You can pass array variables as arguments to functions that expect pointers. The array variable decays into a pointer to its first element when passed to the function and the function has no way to determine the size of the original array.
- Arrays start at zero because of pointer arithmetic.
- You can perform pointer arithmetic on array variables just like you would with regular pointers. For example, `arr+1` points to the second element of the array `arr`.
- You can dereference an array variable using * operator, which accesses the value at the memory address it points to. For example, `*arr` gives you the value of the first element of the array.
- Pointer variables have types so they can adjust pointer arithmetic.



#### scanf()

scanf() can cause buffer overflows if you forget to limit the length of the string that you read with scanf(), then any user can enter far more data than the program has space to store. The extra data then gets written into memory that has not been properly allocated by the computer.



```C
char first_name[20];
    char last_name[20];

    printf("Enter your first and last name: \n");
    scanf("%19s %19s", first_name, last_name);

    printf("First: %s Last: %s\n", first_name, last_name);

// scanf() uses the same kind of format strings as printf() but when we print a string with printf(), we just use %s. Well, if you just use %s in scanf(), there can be a problem if someone gets a little type-happy:

char food[5];
printf("Enter favorite food: ");
scanf("%s", food);
printf("Favorite food: %s\n", food);

// Enter favorite food: liver-tangeraine-raccoon-toffee
// Favorite food: liver-tangeraine-raccoon-toffee
// *** stack smashing detected ***: terminated
// Aborted (core dumped)

```



Stack smashing detected is a security alert that occurs when a program tries to write data beyond the boundary of a buffer allocated on the stack memory. This can potentially lead to memory corruption and security vulnerabilities.

In the code you provided, the issue occurs due to the scanf statement:

The %s format specifier in scanf reads a string from the input until it encounters a whitespace character (space, newline, tab, etc.). The problem is that scanf doesn't check the length of the input string and writes it directly into the food array, which has a fixed size of 5 characters.

When you enter the string "liver-tangeraine-raccoon-toffee", which is longer than 5 characters, scanf writes the entire string into the food array, overwriting memory beyond the boundaries of the food array. This memory area could be used for other variables, function call information, or other important data stored on the stack.

Overwriting this memory is what causes the "stack smashing" error. The stack is a critical region of memory used for storing function call information, local variables, and other data. When data is written beyond the boundaries of the stack, it can corrupt this information, leading to undefined behavior, crashes, or even potential security vulnerabilities if an attacker can control the input data.

To fix this issue, you need to use a safer string input function that limits the number of characters read or use a dynamically allocated buffer with a sufficient size. For example, you can use the fgets function to read a line of input with a maximum length:

```C
#include <string.h>

int main(void){
    char food[100];
    printf("Enter favorite food: ");
    if(fgets(food, sizeof(food), stdin) != NULL){
        food[strcspn(food, "\n")] = "\0";
        printf("Favorite food: %s\n", food);
    }
    return 0;
}
```



In this example, fgets reads a line of input up to a maximum of sizeof(food) - 1 characters (accounting for the null terminator). The strcspn function is used to remove the newline character from the input string.

Stack smashing is a serious issue that can lead to security vulnerabilities, crashes, and data corruption. It's important to always validate input data and use safe functions or techniques to prevent buffer overflows and stack smashing errors.



#### fgets() is an alternative to scanf()

Unlike the scanf() function, the `fgets()` function must be given a maximum length:

```C
char food[5];
printf("Enter favorite food: ");
fgets(food, sizeof(food), stdin);

// First it takes a pointer to a buffer
// Next it takes a maximum size of the string ("\0 included")
// stdin just means the data will be coming from the keyboard.
```

- fgets() buffer size includes the final `\0` character. So we don't need to subtract 1 from the length as you do with scanf().

- `sizeof` returns the amount of space occupied by a variable.

- If `food` was a simple pointer, you'd give an explicit length, rather than using `sizeof`.

  ```C
  printf("Enter favorite food: ");
  fgets(food, 5, stdin);
  ```

  



|                                                              | scanf()                                                      | fgets()                                                      |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Do you limit the number of characters that a user can enter? | scanf() can limit the data entered, so long as you remember to add the size to the format string. | fgets() has a mandatory limit. Nothing gets past him.        |
| Can you be used to enter more than one field?                | Yes! scanf() will not only allow you to enter more than one field, but it also allows you to enter structured data including the ability to specify what characters appear between fields. | fgets() allows you to enter just one string into a buffer. No other data types. Just strings. Just one buffer. |
| If someone enters a string, can it contain spaces?           | When scanf() reads a string with the %s, it stops as soon as it hits a space. So if you want to enter more than one word, you either have to call it more than once, or use some fancy regular expression trick. | No problem with spaces at all. fgets() can read the whole string every time. |

- Clearly if you need to enter structured data with several fields, you'll want to use scanf(). If you are entering a single unstructured string, then fgets() is probably the way to go.



#### Three-card monte

```C
#include <stdio.h>

int main(){
    char *cards = "JQK";
    char a_card = cards[2];
    cards[2] = cards[1];
    cards[1] = cards[0];
    cards[0] = cards[2];
    cards[2] = cards[1];
    cards[1] = a_card;
    puts(cards);
    return 0;
}
```



The above code will result in an error because the string literals can never be updated.

A variable that points to a string literal can't be used to change the contents of the string:

```C
char *cards = "JQK"; // This variable can't modify this string
```

But if you create an array from a string literal, then you can modify it:

```C
char cards[] = "JQK";
```



**Note to myself:** Refer to page 113 in Head First C book for visualization.

1. The computer loads the string literal.
   - when the computer loads the program into memory, it puts all of the constant values into the constant memory block. This section of memory is read-only.
2. The program creates the cards variable on the stack.
3. The cards variable is set to the address of "JQK".
   - The cards variable will contain the address of the string literal "JQK".
4. The computer  tries to change the string.
   - It cannot change because it's read-only.



#### If you're going to change a string, make a copy

If you create a copy of the string in an area of memory that's not read-only, there won't be a problem if you try to change the letters it contains.

We can make a copy just by creating the string as a new array.

```C
char cards[] = "JQK";
// cards is not just a pointer. cards is now an array.
```

In the old code, cards was just a pointer. In the new code, it's an array. By declaring an array called cards and then set it to a string literal, the cards array will be a completely new copy. The variable isn't just pointing at the string literal. It's a brand-new array that contains a fresh copy of the string literal.



```C
int my_function{
    char cards[] = "JQK";
    // cards is an array
    // There is no array size given, so you have to set it to something immediately.
}

// The following two functions are equivalent.
void stack_deck(char cards[]){
    ...
}

void stack_deck(char *cards){
    ...
}

```



With our new code `char cards[] = "JQK"`,

1. The computer loads the string literal.
2. The program creates a new array on the stack.
3. The program initializes the array.
   - As well as allocating the space, the program will also copy the contents of the string literal "JQK" into the stack memory.



**Difference:** the original code used a pointer to point to a read-only string literal. But if you initialize an array with a string literal, you then have a copy of the letters and you can change them as much as you like.



```C
#include <stdio.h>

int main(void){
    char cards[] = "JQK";
    char a_card = cards[2];
    cards[2] = cards[1];
    cards[1] = cards[0];
    cards[0] = cards[2];
    cards[2] = cards[1];
    cards[1] = a_card;
    puts(cards);

    return 0;
}

// Output: QKJ
```



The `cards` variable now points to a string in an unprotected section of memory, so we are free to modify its contents.



**Tips: **

One way to avoid this problem in the future is to never write code that sets a simple char pointer to a string literal value like:

```C
char *s = "Some string";
```



There's nothing wrong with setting a pointer to a string literal - the problems only happen when you try to modify a string literal. Instead, if you want to set a pointer to a literal, always make sure you use the `const` keyword.

```C
const char *s = "some string";
```



That way if the compiler sees some code that tries to modify the string, it will give you a compile error:

```C
s[0] = 'S'; 
```



**Note to myself:** Refer to page 115 on HFC book to see the five-minute mystery case emphasizing memory and pointers.



##### Bullet points

- if you see * in a variable declaration, it means the variable will be a pointer.
- String literals are stored in read-only memory.
- Why are string literals stored in read-only memory? Because they are designed to be constant. If you write a function to print "Hello World", you don't want some other part of the program modifying the "Hello World" string literal.
- If you want to modify a string, you need to make a copy in a new array.
- You can declare a `char` pointer as `const char *` to prevent the code from using it to modify a string. The `const` modifier means that the compiler will complain if you try to modify an array with that particular variable.
- When the program is complied, all the references to array variables are replaced with the addresses of the array. So the truth is that the array variable won't exist in the final executable. That's OK because the array variable will never be needed to point anywhere else.
- `scanf()` means "scan formatted" because it's used to scan formatted input.



#### Memory memorizer

**Stack:** A section of memory used for local variable storage. Every time you call a function, all of the function's local variables get created on the stack. It's called the stack because it's like a stack of plates: variables get added to the stack when you enter a function, and get taken off the stack when you leave. The stack actually works upside down, it starts at the top of memory and grows downward.



**Heap:** The heap is for dynamic memory: pieces of data that get created when the program is running and then hang around a long time.



**Globals:** A global variable is a variable that lives outside all of the functions and is visible to all of them. Globals get created when the program first runs and you can update them freely. But that's unlike...



**Constants:** Created when the program first runs and they are stored in read-only memory. E.g things like string literals that you'll need when the program is running but you'll never want them to change.



**Code:** Finally the code segment. A lot of OS place the code right down in the lowest memory addresses. The code segment is also read-only. The is the part of the memory where the actual assembled code gets loaded.



**Refer to page 121 for visualization**.



**Summary of Chapter 2 Theories:**

- `scanf("%i", &x)` will allow a user  to enter a number x directly.
- ints are different sizes on different machines.
- A char pointer variable x is declared as `char *x`.
- String literals are stored in read-only memory.
- Initialize a new array with a string and it will copy it.
- Read the contents of an address a with `*a`.
- `fgets(buf, size, stdin)` is a simpler way to enter text.
- Local variables are stored on the stack.
- `&x` returns the address of x.
- `&x` is called a pointer to x.

---



### Chapter 2.5

Q: Why do I have to put the find_track () function before main()?

A: C needs to know what parameters a function takes and what its return type is before it can be called.



- You can create an array of arrays with `char strings[...][...]`.
- The first set of brackets is used to access the outer arrray.
- The second set of brackets is used to access the details of each of the inner arrays.
- The `string.h` header file gives you access to a set of string manipulation functions in the C Standard Library.
- You can create several functions in C program but the computer will always run `main()` first.



#### Array of arrays vs array of pointers

An array of arrays is used to stored a sequence of strings.

An array of pointers is actually a list of memory addresses stored in an array. It's very useful if you want to quickly create a list of string literals.

```C
char *names_for_dog[] = {"Browser", "Bonza", "Snodgrass"};
// This is an array that stores pointers.
// There will be one pointer pointing at each string literal.
```



You can access the array of pointers just like you accessed the array of arrays.

---



### Chapter 3

##### Small tools can solve big problems

A small tool does one task and does it well. OS like Linux are mostly made up of hundreds and hundreds of small tools.

If one small part of your program needs to convert data from one format to another, that's the perfect kind of task for a small tool.

- `scanf()` always uses pointers.

- The OS controls how data gets into and out of the Standard Input and Output.
- If you run a program from the command prompt or terminal, the operating system will send all of the keystrokes from the keyboard into the Standard Input.
- If the OS reads any data from the Standard Output, by default it will send that data to the display.
- The `scanf()` and `printf()` functions just read and write Standard Input and the Standard Output.
- You can redirect the Standard Input and Standard Output with `<` and `>`so that they read and write data somewhere else such as to and from files.

- By default, the Standard Error is sent to the display.



**`fprintf()` prints to a data stream.**

`printf()` is just a version of a more general function called `fprintf()`. When you call `printf()`, it actually calls `fprintf()`.

```C
printf("I like Turtles!");

fprintf(stdout, "I like Turtles!"); // This will send data to the data stream

// These two calls are equivalent.
```



The `fprintf()` function allows you to choose where you want to send text to. You can tell `fprintf()` to send text to `stdout`(the Standard Output) or `stderr`(the Standard Error).

- `fscanf()` can be used to read the Standard input which is just like `scanf()` but you can specify the data steam.



`>` redirects the Standard Output. But `2>` redirects the Standard Error.

```bash
$ ./hello 2> errors.txt
```



**Bullet Points**

- The `printf()` function sends data to the Standard Output.
- The Standard Output goes to the display by default.
- You can redirect the Standard Output to a file using `>` on the command line.
- You can redirect the Standard Input to read a file by using `<` on the command line.
- You can redirect the Standard Error using `2>`.
- The Standard Error is reserved for outputting error messages.
- `scanf()` reads data from the Standard Input.
- The Standard Input reads data from the keyboard by default.

In C and many other programming languages, including C++, Java, and Python, the concept of "truthiness" differs from JavaScript.

In JavaScript, certain values are considered "falsy" and others are considered "truthy" when evaluated in a conditional context. For example, the number `0`, empty strings `""` or `''`, `null`, `undefined`, and `NaN` are considered falsy, while all other values are considered truthy.

However, in C and other languages mentioned above, the condition inside a conditional statement is typically evaluated based on whether the expression evaluates to zero (false) or non-zero (true). So, in C, the value `0` is considered false in a conditional context, while any non-zero value is considered true.

For example, in C:

```c
int num = 0;
if (num) {
    printf("This won't be printed because num is 0\n");
} else {
    printf("This will be printed because num is 0\n");
}
```

In the above code, the `if` condition evaluates `num`, and since `num` is `0`, which is considered false in C, the code inside the `else` block will be executed, printing "This will be printed because num is 0".



#### Tips for Designing Small Tools

Small tools should follow these design principles:

- They can read data from the Standard Input.
- They can display data on the Standard Output.
- They deal with text data rather than obscure binary formats.
- They each perform one simple task.





Q: Why is it important that small tools use the Standard Input and Standard Output?

A: Because it makes it easier to connect them with pipes.



Q: Why does that matter?

A: Small tools usually don't solve an entire problem on their own, just a small technical problem like converting from one format to another. But if you can combine them together, then you can solve large problems.



Q: What is a pipe, actually?

A: The exact details depend on the OS. Pipes might be made from sections of memory or temporary files. The important thing is that they accept data in one end and send the data out of the other in sequence.



Q: So if two programs are piped together, does the first program have to finish running before the second program can start?

A: No. Both of the programs will run at the same time; as output is produced by the first program, it can be consumed by the second program.



Q: Why do small tools use text?

A: It's the most open format. If a small tool uses text, it means that any other programmer can easily read and understand the output just by using a text editor. Binary formats are normally obscure and hard to understand.



Q: Can I connect several programs together with pipes?

A: Yes, just add more `|` between each program name. A series of connected processes is called a pipeline.



Q: If several processes are connected together with pipes and then I use `>` and `<` to redirect the Standard Input and Output, which processes will have their input and output redirected?

A: The `<` will send a file's content to the first process in the pipeline. The `>` will capture the Standard Output from the last process in the pipeline.



##### Bullet Points

- If you want to perform a different task, consider writing a separate small tool.
- Design tools to work with Standard Input and Standard Output.
- Small tools normally read and write text data.



`fscanf()` and `scanf()` returns the number of items successfully read.



#### `fprintf()` and `fscanf()`

- `fprintf()`: This function is used to write formatted output to a file. It takes at least two arguments:

  1. A pointer to a `FILE` object that identifies the stream where the content is to be written. This can be `stdout` (standard output), `stderr` (standard error), or a file opened using `fopen()`.

  2. A format string that specifies how subsequent arguments are converted for output. It can contain ordinary characters (which are copied directly to the output) and conversion specifiers (beginning with %) which are replaced by the values of the subsequent arguments.

  3. Zero or more additional arguments supplying values to be printed. Their type should match the corresponding format specifier in the format string.

     ```C
     fprintf(FILE *stream, "%s\n", line);
     ```

- `fscanf()`: This function is used to read formatted input from a file. It also takes at least two arguments:

  1. A pointer to a `FILE` object that identifies the stream from which characters are read. This can be `stdin` (standard input) or a file opened using `fopen()`.

  2. A format string that specifies how to read the input.

  3. A variable number of arguments corresponding to the conversion specifiers in the format string. These arguments are pointers to memory locations where the values read from the input stream will be stored, according to the format string.

     ```C
     fscanf(FILE *stream, "%s\n", line)
     ```

  In both functions, the return value indicates the number of items successfully formatted or read and stored. If an error occurs or the end-of-file (EOF) is reached before any data is read, `EOF` is returned.

#### Roll your own data streams

When a program runs, the OS gives it three file data streams: the Standard Input, the Standard Output and the Standard Error. But sometimes you need to create other data streams on the fly.

The good news is that the operating system doesn't limit you to the ones you are dealt when the program starts. You can roll your own as the program runs.



**Each data stream is represented by a pointer to a file** and you can create  a new data stream using the `fopen()` function:

```C
FILE *in_file = fopen("input.txt", "r");
// This will create a data stream to read from a file.
// fopen(filename, mode);
FILE *out_file = fopen("output.txt", "w");
```

The mode is:

"w" = write, "r" = read and "a" = append.

Once you have created a data stream, you can print to it using `fprintf()`. `fscanf()` to read from a file.

```C
fprintf(out_file, "Don't wear %s with %s", "red", "green");

fscanf(in_file, "%79[^\n]\n", sentence);
```



Finally when you're finished with a data stream, you need to close it. The truth is that all data streams are automatically closed when the program ends but it's still a good idea to always close the data stream yourself:

```C
fclose(in_file);
fclose(out_file);
```



Q: How many data streams can I have?

A: It depends on the OS, but usually a process can have up to 256. The key thing is there's a limited number of them, so make sure you close them when you're done using them.



Q: Why is FILE in uppercase?

A: It's historic. FILE used to be defined using a macro. Macros are usually given uppercase names.



Any program we write is going to need options. If we create a chat program, it's going to need preferences. If we write a game, a user will want to change the shape of the blood spots. Similarly, if we are writing a command-line tool, we are probably going to need to add command-line options.

Command-line options are the little switches we often see with command-line tools:

```bash
ps -ae 
// Display all the processes, including thier environments.

tail -f logfile.out
// Display the end of the file, but wait for new data to be added to the end of the file.
```



#### Let the library do the work for you

Many programs use command-line options. So, there's a special library function you can use to make dealing with them a little easier and it's called `getopt()`. and each time you call it, it returns the next option it finds on the command line.

```bash
rocket_to -e 4 -a Brasilia Tokyo London

-e = engines
4 = use four engines
-a = awesomeness
```



This program needs one option that will take a vluae (-e = engines) and another that is simply on or off (-a). You can handle these options by calling `getopt()` in a loop like this:

The `unistd.h` header is not actually part of the standard C library. Instead it gives your programs access to some of the POSIX libraries. POSIX was an attempt to create a common set of functions for use across all popular OS.

```C
// We will need to include this header.
#include <unistd.h>

// "ae:" means "The a option is valid; so is the e option"
// ":" means that the e option needs an argument
while((ch = getopt(argc, argv, "ae:")) != EOF){
    // The code to handle each option goes here.
    switch(ch){
        case 'e':
            engine_count = optarg;
    };
}
	// These final two lines make sure we skip past the options we read.
	argc -= optind;
	argv += optind;
	// optind stores the number of strings read from the command line to get past the options.

	
```



The string `ae:` tells the `getopt()` function that a and e are valid options. The e is followed by a colon to tell `getopt()` that the `-e` needs to be followed by an extra argument. `getopt()` will point to that argument with the `optarg` variable.

When the loop finishes, you tweak `argv` and `argc` variables to skip past all of the options and get to the main command-line arguments. That will make your `argv` array look like this:

Barasilia - This is `argv[0]`.

Tokyo - This is `argv[1]`.

London - This is `argv[2]`.

After processing the arguments, the 0th argument will no longer be the program name. `argv[0]` will instead point to the first command-line argument that follows the options.



##### optarg

`optarg` is a global variable that is part of the `getopt` function in the `unistd.h` library in C.

When `getopt` is called to parse command-line options and arguments, if it encounters an option that takes an argument, it sets `optarg` to point to the argument value.

For example, if you run your program with `-d delivery`, `optarg` would point to the string "delivery".

`optarg` is only valid after `getopt` has been called and returned a character that requires an argument. It's undefined at other times.

```bash
./order_pizza -d now
// now is optarg
```



#### Should we always declare string variables as pointers?

While pointers are commonly used to work with strings in C, especially when dealing with dynamic memory allocation or modifying string contents, it's not always required to declare string variables as pointers.

1. **Static Strings:** If you have a string with a fixed size known at compile time, you can declare it as a static array of characters(`char myString[size]`) without the need for pointers.

   ```C
   char staticString[20] = "Hello, World";
   ```

2. **Dynamic Strings:** If the size of the string is not known at compile time or if you need to dynamically allocate memory for the string, using pointers is more appropriate. This allows you to allocate memory as needed using functions like `malloc` or `calloc` and to resize or modify the string contents as requried.

3. **Function Arguments:** When passing strings as function arguments, you can use either pointers or arrays. Strings are passed by reference in C, so modifying the string inside the function affects the original string. However, passing a pointer to a string allows more flexibility and efficiency especially with large strings.

4. **Null-Terminated Strings:** Whether you use pointers or arrays, remember that C strings are null-terminated, meaning they end with a null character(`0\`). Ensure that your string variables are properly null-terminated to avoid undefined behavior.

---



### Chapter 4

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



#### Compilation Behind the scenes

1. **Preprocessing: fix the source**: The first thing the compiler needs to do is fix the source code. It needs to add in any extra header files it's been told about using the `#include` directive. It might also need to expand or skip over some sections of the program. 

2. **Compilation: translate into assembly:** The C programming language isn't low level enough for the computer to understand. The computer only really understands very low-level machine code instructions and the first step to generate machine code is convert the C source code into **assembly language symbols**.

   - Assembly language describes the individual instructions the central processor will have to follow when running the program.
   - The C compiler has a whole set of recipes for each of the different parts of the C language.
   - These recipes will tell the compiler to convert an `if` statement or a function call into a sequence of assembly language instructions. 
   - But even assembly isn't low level enough for the computer. That's why it needs..

3. **Assembly: generate the object code:** The compiler will need to assemble the symbol codes into machine or object code. This is the actual binary code that will be executed by the circuits inside the CPU.

   ```code
   10010101 00100101 11010101 01011100
   ```

   After all, we've taken the original C source code and converted it into the 1s and 0s that the computer's circuits need.

   - If we give the computer several files to compile for a program, the compiler will generate a piece of object code for each source file.
   - But in order for these separate object files to form a single executable program, one more thing has to occur.

   

4. **Linking: put it all together:** Once we have all of the separate pieces of object code, we need to fit them together like jigsaw pieces to form executable program. The compiler will connect the code in one piece of object code that calls a function in another piece of object code.
   - Linking will also make sure that the program is able to call library code properly.
   - The program will be written out into the executable program file using a format that is supported by the OS.
   - The file format is important because it will allow the OS to load the program into memory and make it run.

**So how do we tell `gcc` that we want to make one executable program from several separate source files?**

```bash
gcc message_hider.c encrypt.c -o message_hider
```



Exercise in `HFCExercises.md`



#### Bullet Points

- You can share code by putting it into a separate C file.
- You need to put the function declarations in a separate .h header file.
- Include the header file in every C file that needs to use the shared code.
- List all of the C files needed in the compiler command.
- The `fgets()` function returns a pointer to the string read on success and `NULL` on failure. If it reaches the end of file or encounters an error, it returns `NULL`.
- On the other hand, `scanf` returns the number of successful assignments which can be less than the number of requested assignments. It will return 1 if it successfully reads a string, and 0 if it encounters end of file or an error before any assignments are made.



**Page 231 for visual representations in HFC.**

#### How do we tell `gcc` to save the object code in a file? And how do we then get the compiler to link the object files together?

---

##### First, compile the source into object files

You want object code for each of the source files, and you can do that by typing this command:

```bash
gcc -c *.c
//This will create object code for every file.
// The OS will replace *.c with all the C filenames.
```



The `*.c` will match every C file in the current directory, and the `-c` will tell the compiler that you want to create an object file for each source file, but you don't want to link them together into a full executable program.



##### Then, link them together

Now that you have a set of object files, you can link them together with a simple compile command. But instead of giving the compiler the names of the C source files, you tell the names of the object files:

```bash
gcc *.o -o launch

```

The compiler is smart enough to recognize the files as object files rather than source files, so it will skip most of the compilation steps and just link them together into an executable program called `launch`.

So, if you change just one of the files, you'll only need to recompile that single file and then relink the program.

```bash
// This will recreate the thruster.o file.
// This is the only file that's changed.
gcc -c thruster.c

gcc *.o -o launch
//This will link everything together
```

Even though you have to type two commands, you are saving a lot of time.



#### Automate your builds with the make tool

You can compile your applications really quickly in `gcc` as long as you keep track of which files have changed. That's a tricky thing to do, but it's also pretty straightforward to automate.

```bash
thruster.c ---> thruster.o
// If the thruster.c file is newer, you need to recompile.
// If the thruster.o file is newer, you don't need to recompile.
```

`make` is a tool that can run the compile command for you. The `make` tool will check the timestamps of the source files and the generated files, and then it will only recompile the files if things have gotten out of date.

But before you can do all these things, you need to tell `make` about your source code. It needs to know the details of which files depend on which files. And it also needs to be told exactly how you want to build the code.



#### What does `make` need to know?

Every file that `make` compiles is called a target. A target is any file that is generated from some other files.

For every target, `make` needs to be told two things:

1. **The dependencies:** Which files the target is going to be generated from.
2. **The recipe:** The set of instructions it needs to run to generate the file.

Together, the dependencies and the recipe form a rule. A rule tells `make` all it needs to know to create the target file.



#### How make works

Let's say you want to compile `thruster.c` into some object code in `thruster.o`. What are the dependencies and what's the recipe?

```code
thruster.c --> thruster.o
```

- The `thruster.o` file is called the target, because it's the file you want to generate.

- `thruster.c` is a dependency because it's a file the compiler will need in order to create `thruster.o`. And what will the recipe be? That's the compile command to convert `thruster.c` into `thruster.o`.

  ```code
  gcc -c thruster.c
  ```

- If you tell the `make` tool about the dependencies and the recipe, you can leave it to `make` to decide when it needs to recompile `thruster.o`.

- Once you build the `thruster.o` file, you're going to use it to create the `launch` program. That means the `launch` file can also be set up as a target, because it's a file you want to generate. The dependency files for `launch` are all of the `.o` object files. The recipe is this command:

  ```bash
  gcc *.o -o launch
  ```

- Once `make` has been given the details of all of the dependencies and rules, all you have to do is tell it to create the `launch` file. `make` will work out the details.

**But how do you tell `make` about the dependencies and recipes?**

All of the details about the targets, dependencies and recipes need to be stored in a file called either `makefile` or `Makefile`.

**Page 240 in HFC for visualization.**

1. The `launch` program is made from the `launch.o` and `thruster.o` files.
2. The `launch.o` is compiled from `launch.c` and `launch.h` and also from `thruster.h`.
3. `thruster.o` is compiled from `thruster.h` and `thruster.c.`



The `launch` program is made by linking the `launch.o` and `thruster.o` files. Those files are compiled from their matching C and header files, but the `launch.o` file also depends on the `thruster.h` file because it contains code that will need to call a function in the `thruster` code.

```bash
launch.o: launch.c launch.h thruster.h
	gcc -c launch.c // this is the recipe.
// The recipes must begin with a tab character.

thruster.o: thruster.h thruster.c
	gcc -c thruster.c

launch: launch.o thruster.o
	gcc launch.o thruster.o -o launch
```



#### Bullet Points

- It can take a long time to compile a large number of files.

- You can speed up compilation time by storing object code in *.o files. 

  ```bash
  gcc -c my_program.c
  //This command compiles my_program.c into my_program.o without linking it to other object files.
  
  ```

- You can then link the objects files together to create the final executable.

- The `gcc` can compile programs from object files as well as source files.

- The `make` tool can be used to automate your builds.

- `make` knows about the dependencies between files, so it can compile just the files that change.

- `make` needs to be told about your build with a makefile.

- Be careful formatting your makefile: don't forget to indent lines with tabs instead of spaces.

- `chars` are  numbers.

- Use `shorts` for small whole numbers. Use `ints` for most whole numbers. Use `longs` for really big whole numbers. Use `float` for most floating points. Use `double` for really precise floating points.

- Split function declaration from definitions. Put declarations in a header file.

- `#include <>` for library headers. `#include ""` for local headers.

- Save object code into files to speed up your builds.

- Use `make` to manage your builds.

---



### Chapter 6 - structs, unions, and bitfields



#### Struct

Create your own structured data types with a `struct`.

- If you have a set of data that you need to bundle together into a single thing, then you can use a `struct`.

- The word `struct` is short for `structured data type`.

- A `struct` will let you take all of those different pieces of data into the code and wrap them up into one large new data type.

  - ```C
    struct fish{
        const char *name;
        const char *species;
        int teeth;
        int age;
    }
    ```

- It's a little bit like an array except:

  - It's fixed length.
  - The pieces of data inside the `struct` are given names.

- We can create pieces of data that use it:

  - ```C
    struct fish snappy = {"Snappy", "Piranha", 69, 4}
    ```

  - `struct fish` is the data type.

  - `snappy` is the variable name.

  - `"Snappy"` is the name.

  - `"Piranha"` is the species.

  - `69` is the number of teeth.

  - `4`is Snappy's age.



Q: What's that `const char` thing again?

A: `const char *` is used for strings that you don't want to change. That means it's often used to record string literals.



Q: Does this `struct` store the string?

A: In this case, no. The `struct` here just stores a pointer to a string. That means it's just recording an address, and thee string lives somewhere else in memory.



Q: But you can store the whole string in there if you want?

A: Yes, if you define a `char` array in the string, like `char name[20]`;



**View the exercise in `HFCExercises.md`**.



#### Read a struct's fields with the "." operator

Even though a struct stores fields like an array, the only way to access them is by name.

```C
struct fish{
    const char *name;
    const char *species;
    int teeth;
    int age;
}

struct fish snappy = {"Snappy", "piranha", 69, 4}

printf("Name = %s\n", snappy.name);
//This will return the string "Snappy".
// Name = Snappy
```



- One of the great things about data passing around inside `struct`s is that you can change contents of your `struct` without having to change the functions that use it.

- For example, let's say you want to add an extra field to `fish`:

  ```C
  struct fish{
      const char *name;
      const char *species;
      int teeth;
      int age;
      int favorite_music;
  }
  ```

  - All the `catalog()` and `label()` which are the functions that uses the `struct fish` which can be found in `HFCExercises.md` have been told is they're going to be handed a `fish`. They don't know (and don't care) that the `fish` now contains more data, so long as it has all the fields they need.
  - That means that `struct`s don't just make your code easier to read, they also make it better able to cope with change.

Q: is a `struct` just an array?

A: No but like an array, it groups a number of pieces of data together.



Q: An array variable is just a pointer to the array. Is a `struct` variable a pointer to a `struct`?

A: No, a `struct` variable is a name for the `struct` itself.



Q: Are `structs` like classes in other languages?

A: They're similar but it's not so easy to add methods to `struct`s.



#### Structs In Memory Up Close

- The assignment copies the pointers to strings, not the strings themselves.

- When you assign one `struct` to another, the contents of the `struct` will be copied.

- But if that includes pointers, the assignment will just copy the pointer values.

  ```C
  struct fish{
      const char *name;
      const char *species;
      int teeth;
      int age;
  };
  
  struct fish snappy = {"Snappy", "Piranha", 69, 4};
  struct fish gnasher = snappy;
  
  // gnasher and snappy both point to the same strings.
  ```

  - That means the `name` and `species` fields of `gnasher` and `snappy` both point to the same strings.

- When we define a new variable, the computer will need to create some space in memory for an instance of the `struct`.

- That space in memory will need to be big enough to contain all of the fields within the `struct`.

- The computer will create a brand-new copy of the `struct` when we assign a `struct` to another variable.

- That means it will need to allocate another piece of memory of the same size, then copy over each of the fields.

- When we are assigning `struct` variables, we are telling the computer to copy data.



#### Can yhou put one struct inside another?

When we define a `struct`, we are actually creating a new data type.

If a `struct` creates a data type from existing data types, that means we can also **create `structs` from other `structs`.**

```C
struct preferences{
    const char *food;
    float exercise_hours;
};

struct fish{
    const char *name;
    const char *species;
    int teeth;
    int age;
    struct preferences care;
}
```

- This is a `struct` inside a `struct`. This is called nesting.
- Our new field is called "care", but it will contain fields defined by the "preferences" `struct`.
- The above code tells the computer one `struct` will contain another `struct`.

```C
struct fish snappy = {"Snappy", "Piranha", 69, 4, {"Meat", 7.5}};
// "Meat" - The value for care.food
// 7.5 - The value for care.exercise_hours
// {"Meat", 7.5} - struct data for the care field.
```



- Once we've combined `struct` together, we can access the fields using a chain of "." operators:

  ```C
  printf("Snappy likes to eat %s", snappy.care.food);
  printf("Snappy likes to exercise for %f hours", snappy.care.exercise_hours);
  ```

  

#### Typedef

C allows you to create an **alias** for any `struct` that you create. If you add the word **typedef** before the `struct` keyword and a **type name** after the closing brace, you can call the new type whatever you like:

Before:

```C
struct cell_phone{
    int cell_no;
    const char *wallpaper;
    float minutes_of_charge;
};

struct cell_phone p = {5557879, "sinatra.png", 1.35};
```



After:

```C
typedef struct cell_phone{
    int cell_no;
    const char *wallpaper;
    float minutes_of_charge;
} phone;

phone p = {5557879, "sinatra.png", 1.35};

```



- `typedef` means you are going to give the `struct` type a new name.

- `phone` will become an alias for `struct cell_phone`.

- Now, when the compiler sees "phone", it will treat it like `struct cell_phone`.

- `typedef` can shorten your code and make it easier to read.

- If you use `typedef` to create an alias for a `struct`, you will need to decide what your alias will be.

- The alias is just the name of your type that means there are two names to think about: the name of the `struct(struct cell_phone)` and the name of the `type(phone)`.

- Why have two names? You usually don't need both. The compiler is quite happy for you to skip the `struct` name like this:

  ```C
  typedef struct{
      int cell_no;
      const char *wallpaper;
      float minutes_of_charge;
  } phone;
  phone p = {5557879, "sinatra.png", 1.35};
  ```

  



#### Bullet Points

- A `struct` is a data type made from a sequence of other data types.
- `struct`s are fixed length.
- `struct` fields are accessed by name, using `<struct>.<field name>` syntax(aka dot notation).
- `struct` fields are stored in memory in the same order they appear in the code.
- You can nest `struct`s.
- `typedef` creates an alias for a data type.
- If you use `typedef` with a `struct`, then you can skip giving the `struct` a name.



Q: Do `struct`  fields get placed next to each other in memory?

A: Sometimes there are small gaps between the fields.



Q: Why is that?

A: The computer likes data to fit inside the word boundaries. SO, if a computer uses 32-bit words, it won't want a `short` , say, to be split over a 32-bit boundary.



Q: So it would leave a gap and start the `short` in the next 32-bit word?

A: Yes.



Q: Does that mean each field takes up a whole word?

A: No, the computer leaves gaps only to prevent fields from splitting across word boundaries. If it can fit several fields into a single word, it will.



Q: Why does the computer care so much about word boundaries?

A: it will read complete words from the memory. If a field was split across more than one word, the CPU would have to read several locations and somehow stitch the value together. That'd be slow.



Q: In languages like Java, if I assign an object to a variable, it doesn't copy the object, it just copies a reference. Why is it different in C?

A: In C, all assignments copy data. If you want to copy a reference to a piece of data, you should assign a pointer.



Q: I'm really confused about `struct` names. What's the `struct` name and what's the alias?

A: The `struct` name is the word that follows the `struct` keyword. If you write `struct peter_parker{...}`, then the name is `peter_parker` and when you create variables, you would say `struct peter_parker x`. Sometimes you don't want to keep using the `struct` keyword when you declare variables, so `typedef` allows you to create a single word alias. In `typedef struct peter_parker{...} spider_man;`, `spider_man` is the alias.



Q: What's an anonymous `struct`?

A: One without a name, so `typedef struct {...} spider_man;` has an alias of `spider_man` but no name. Most of the time, if you create an alias, you don't need a name.



#### how do you update a struct?

A `struct` is really jsut a bundle of variables, grouped together and treated like a single piece of data.

We can change the fileds of a `struct` just like any other variable.

```C
fish snappy = {"Snappy", "piranha", 69, 4};
printf("Hello %s\n", snappy.name);
snappy.teeth = 68;
```



If we take a look at this code:

```C
#include <stdio.h>

typedef struct{
    const char *name;
    const char *species;
    int age;
} turtle;

void happy_birthday(turtle t){
    t.age = t.age + 1;
    printf("Happy Birthday %s! You are now %i years old!\n");
}

int main(){
    turtle myrtle = {"Myrtle", "Leatherback sea turtle", 99};
    happy_birthday(myrtle);
    printf("%s's age is now %i\n", myrtle.name, myrtle.age);
    return 0;
}
```



When we run:

```bash
chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ make all
Compiling the main file...
gcc -c main.c 
Compiling the final turtle exe file...
gcc main.o functions.o -o turtle

chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ ./turtle
Happy Birthday Myrtle! You are now 100 years old!
Myrtle's age is now 99

```



- Even though the `age` was updated by the function, when the code returned to the main function. the `age` seemed to reset itself.

- The code is cloning the turtle. When we assign a `struct`, its values get copied to the new `struct`.

- ```C
  happy_birthday(myrtle);
  ```

  - myrtle - This is the turtle that we are passing to the function. The myrtle `struct` will be copied to this parameter.

- In C, parameters are passed to functions by **value**. That means that when we call a function, the values we pass into it are assigned to the parameters.

- So in this code, it's almost as if we had written something like this:

  ```C
  turtle t = myrtle;
  ```

- When we assign `structs` in C, the values are copied.

- When we call the function, the parameter `t` will contain a copy of the `myrtle struct`.

- It's as if the function has a clone of the original turtle.

- So, the code inside the function does update the age of the turtle, but it's a different turtle.

- What happens when the function returns?

- The `t` parameter disappears, and the rest of the code in `main()` uses `myrtle struct`. But the value of `myrtle` was never changed by the code. It was always a completely separate piece of data.



#### So what do we do if we want to pass a `struct` to a function that needs to update it?

##### We need a pointer to the `struct`

- When we pass a variable to the `scanf()` function, we couldn't pass the variable itself to `scanf()`; we have to pass a pointer:

  ```C
  scanf("%f", &length_of_run);
  ```

  

- If we tell the `scanf()` function where the variable lives in memory, then the function will be able to update the data stored at the place in memory, which means it can update the variable.

- We can just do the same with `struct`.

- ```C
  void happy_birthday(turtle *t){
     (*t).age = (*t).age + 1;
      printf("Happy Birthday %s! You are now %i years old!\n", (*t).name, (*t).age);
  }
  happy_birthday(&myrtle);
  ```

- The parentheses are really important because `(*t).age` and `*t.age` are very different.

**Example exercises can be found in `HFCExercises.md`.**



- `(*t).age != *t.age`. 

- `(*t).age`. If t is a pointer to a turtle `struct`, then this is the age of the turtle. (I am the age of the turtle pointed to by t.)

- `*t.age` . If t is a pointer to a turtle `struct`, then this expression is wrong. (I am the contents of the memory location given by `t.age`).

- So the expression `*t.age` is really the same as `*(t.age)`. It means "the contents of the memory location given by `t.age`". But `t.age` isn't a memory location.

- The reason `(*t).age` and `*t.age` are not the smae is due to the precedence of the operators in C.

  - `(*t).age` first dereferences the pointer `t` to access the `turtle struct` and then accesses the `age` field of that `struct`.
  - `*t.age` tried to access the `age` field of the pointer `t` (which doesn't exist because `t` is a pointer, not a `struct`), and then tries to dereference the result, which is not valid and will likely cause a compile error.

- In C, the `.` operator has higher precedence than the `*` operator, so `*t.age` is interpreted as `*(t.age)`, not `(*t).age`.

- To avoid confusion, C provides the `->` operator for accessing the members of a struct through a pointer, so instead of writing `(*t).age` , you can write `t->age`, which is clearer and easier to read.

- `t->age` means `(*t).age`. (The `age` field in the `struct` that `t` points to).

  ```C
  void happy_birthday(turtle *a){
      a->age = a->age + 1;
      printf("Happy Birthday %s! You are now %i years old!\n", a->name, a->age);
  }
  ```

  

- By passing a pointer to the `struct`, we allowed the function to update the original data rather than taking a local copy.

Q: Why are values copied to parameter variables?

A: The computer will pass values to a function by assigning values to the function's parameters. And all assignments copy values.



#### Bullet Points

- When you call a function, the values are copied to the parameter variables.
- You can create pointers to `struct`s, just like any other type.
- The `->` notation cuts down on parentheses and makes the code more readable.



#### Union

- In C, a union is a user-defined data type that allows you to store different types of data in the same memory location.

- It is similar to a `struct` but with a key difference.

- In a union, all members share the same memory location. You can define a union with many members, but only one member can contain a value at any given time.

- In a `struct`, each member has its own memory space.

- A union lets you reuse memory space. The size of a union is always equal to the size of its largest data member.

  ```C
  quantity(might be a float or a short);
  
  // If a float takes 4 bytes, and a short takes 2, then this space will be 4 bytes long.
  ```

  

- Every time we create an instance of `struct`, the computer will lay out the fields in memory, one after the other.

- A union is different. A union will use the space for just one of the fields in its definition.

  - For instance, if we have a union called `quantity`, with fields called `count`, `weight`, and `volume`, and the computer will give the union enough space for its largest field, and then leave it up to you which value you will store in there. Whether you set the `count`, `weight` or `volume` field, the data will go into the same space in memory.

    ```C
    typedef union{
        short count;
        float weight;
        float volume;
    } quantity;
    
    // Each of these fields will be stored in the same space.
    ```

    

  - When we assign a value to a member of the union, it stores the value in its memory location. If you then assign  a value to a different member, it overwrites the previous value because both members share the same memory location.

- Unions are commonly used in situations where you need to conserve memory or when you want to represent a single value in different ways.




```C
union Data{
    int i;
    float f;
    char str[20];
}

int main(){
    union Data data;
    
    data.i = 10;
    printf("data.i : %d\n", data.i);
    
    data.f = 220.5;
    printf("data.f :%.2f\n", data.f);
    
    strcpy(data.str, "C Programming");
    printf("data.str :%s\n", data.str);
    
    return 0;
}
```



In this example, a union `Data` is defined with three members: i, f and `str`. 

- The size of the union will be the size of `str` because `str` is the largest member.
- When we assign a value to `data.i`, it is stored in the memory location of `data`.
- Then when we assign a value to `data.f`, it overwrites the value of `data.i`, because i and f share the same memory location. The same happens when we assign a value to `data.str`.
- So, when we print the values, only `data.str` will output the expected value ("C Programming") because it was the last one to be assigned. 
- The values of `data.i` and `data.f` will be lost.



#### How do we use a union?

1. C89 style for the first field.

   - To give the union a value for its first field, just wrap the value in braces:

     ```C
     quantity q = {4};
     
     // This means the quantity is a count of 4.
     ```

2.  Designated initializers set other values.

   - A designated initializer sets a union field value by name, like this:

     ```C
     quantity q = {.weight=1.5};
     
     // This will set the union for a floating point weight value.
     ```

3. Set the value with dot notation

   - The third way of setting a union value is by creating the variable on one line and setting a field value on another line:

   - ```C
     quantity q;
     q.volume = 3.7;
     ```



**Remember: whichever way we set the union's value, there will only ever be one piece of data stored. The union just gives you a way of creating a variable that supports several different data types.**



Q: Why is a union always set to the size of the largest field?

A: The computer needs to make sure that a union is always the same size. The only way it can do that is by making sure it is large enough to contain any of the fields.



Q: Why does the C89 notation only set the first field? Why not set it to the first float if I pass it a float value?

A: To avoid ambiguity, if you had, let's say, a float and a double field, should the computer store `{2.1}` as a float or a double? By always storing the value in the first field, you know exactly how the data will be initialized.



- Designated initializers allow you to set `struct` and `union` fields by name and are part of the C99 C standard. 

- Objective C supports designated initializers but C++ does not.

- Designated initializers can be used to set the initial values of fields in `structs` as well.

- They can be very useful if you have a `struct` that contains a large number of fields and you initially just want to set a few of them.

  ```C
  typedef struct{
      const char *color;
      int gears;
      int height;
  } bike;
  bike b = {.height=17, .gears=21};
  ```

  

#### unions are often used with `struct`s

```C
typedef union{
    short count;
    float weight;
    float volume;
} quantity;

typedef struct{
    const char *name;
    const char *country;
    quantity amount;
} fruit_order;
```



We can access the values in the `struct/union` combination using the dot or `->` notation:

```C
fruit_order apples = {"apples", "England", .amount.weight=4.2};
// It's .amount because that's the name of the struct quantity variable.
// Here we are using a double designated identifier .amount for the struct and .weight for the .amount.

printf("This order contains %2.2f lbs of %s\n", apples.amount.weight, apples.name);

//This order contains 4.20 lbs of apples.
```



```C
margarita m = {2.0, 1.0, {0.5}};
// This one compiles perfectly. 

margarita m;
m = {2.0, 1.0, {0.5}};
// This one doesn't compile becase the compiler will only know that {2.0, 1.0, {0.5}} represents a struct if it's used on the same line that a struct is declared. When it's on a separate line, the compiler thinks it's an array.
```



- We can store lots of possible values in a union, but we have no way of knowing what type it was once it's stored.

- The compiler won't be able to keep track of the fields that are set and read in a union, so there's nothing to stop us setting one field and reading another. 

- Sometimes it can be a BIG PROBLEM.

  ```C
  #include <stdio.h>
  typedef union{
      float weight;
      int count;
  } cupcake;
  
  int main(){
      cupcake order = {2};
      printf("Cupcakes quantity: %i\n", order.count);
      return 0;
  }
  ```

  - In this code, let's say the code has set the weight not the count in the line `cupcake order ={2}` by mistake.

  - But on the next line, he's reading the count.

  - Let's see what happens when he runs the code:

  - ```C
    Cupcakes quantity: 1073731824
    ```

  - That's a lot of cupcakes.

  - We need some way of keeping track of the values we've stored in a union.

  - One trick is to create an `enum`.



#### An enum variable stores a symbol

Sometimes you don't want to store a number or a piece of text. Instead, you want to store something from a list of symbols.

`enum` short for enumeration in C is a user-defined data type that consists of integral constants. To define an `enum`, 

- We use `enum` keyword followed by `enum_name` (optional), and then in curly braces`{}`, we define a list of identifiers known as enumerators as values. 
- By default, the first enumerator has the value 0, and the value of each successive enumerator is increased by one.

enums let us create a list of symbols like this:

```C
enum colors{RED, GREEN, PUCE};

//Values are separated by commas.
```

- Any variable that is defined with a type of `enum colors` can then only be set to one of the keywords in the list. 

- So we might define an `enum colors` variables like this:

  ```C
  enum colors favorite = PUCE;
  ```

- The computer will just assign numbers to each of the symbols in our list, and the `enum` will just store a number. We don't need to worry about what the numbers are: our C code can just refer to the symbols. That'll make our code easier to read and it will prevent storing values like `RED` or `PUSE`:

  - ```C
    enum colors favorite = PUSE;
    
    // The computer will spot that this is not a legal value, so it won't compile.
    ```

  - Let's say we have days of the week.

    ```C
    enum Day {Sunday, Monday, Tuesday, Wednesday, Friday, Saturday};
    
    ```

  - In this `enum`, Sunday is 0, Monday is 1, Tuesday is 2 and so on.

- We can also assign specific values to the enumerators:

  - ```C
    enum Day {Sunday = 1, Monday, Tuesday, Wednesday, Thursday, Friday, Saturday};
    ```

  - in this `enum`, Sunday is 1, Monday is 2, Tuesday is 3 and so on.



#### Bitfields

- Sometimes you want control at the bit level.

- Let's say we have a `struct` that contain a lot of yes/no values,

  ```C
  typedef struct{
      short low_level_vcf;
      short filter_coupler;
      short reverb;
      short sequential;
      ... // Each of these fileds will contain 1 for true or 0 for false.
  } syth;
  ```

  - Each field will use many bits.
  - A `short` field will take up a lot more space than the single bit that you need for `true/false` values. 
  - It's wasteful. It would be much better if we could create a `struct` that could hold a sequence of single bits for the values which is why `bitfields` are created.

- When we're dealing with binary value, it would be great if we had some way of specifying the 1s and 0s in a literal like:

  ```C
  int x = 01010100;
  ```

- Unfortunately, C doesn't support `binary literals`, but it does support `hexadecimal literals`.

- Everytime C sees a number beginning with 0x, it treats the number as base `6:

  ```C
  int x = 0x54;
  ```

- We can convert hex to binary one digit at a time.

  ```C
  0x54
  5 - 0101
  4 - 0100
  ```

- Each hexadecimal digit matches a binary digit of length 4.



#### Bitfields store a custom number of bits

A bitfield lets you specify how many bits an individual field will store.

```C
typedef struct{
    unsigned int low_pass_vcf:1;
    unsigned int filter_coupler:1;
    unsigned int reverb:1;
    unsigned int sequential:1;
    ...
        // Each field will only use 1 bit of storage.
}synth;
```

- By using bitfields, we can make sure each field takes up only one bit. 
- If we have  a sequence of bitfields, the computer can squash them together to save space.
- If we have eight single-bit bitfields, the computer can store them in a single byte.
- Bitfields can save space if they are collected together in a `struct`.
- If the compiler finds a single bitfield on its own, it might still have to pad it out to the size of a word. That's why bitfields are usually grouped together.



```C
typedef struct{
    unsigned int first_visit:1;
    unsigned int come_again:1;
    unsigned int fingers_lost:4;
    unsigned int shark_attack:1;
    unsigned int days_a_week:3;
} survey;
```



- 1 bit can store 2 values: true/false.
- 4 bits are needed to store up to 10.
- 3 bits can store numbers up to 7.

With 4 bits, you can represent 2*4 = 16 different values. Since we are starting counting from 0, the range of numbers you can represent is from 0 to 2^4 - 1 = 15. So, with 4 bits, you can represent values from 0 to 15.



Similarly with 3 bits, you can represent 2^3 = 8 different values.  Again, starting counting from 0, the range of numbers you can represent is from 0 to 2^3 - 1 = 7. So, with 3 bits, you can represent values from 0 to 7.



The formula 2^n represents the number of unique combinations you can have with n bits.



Q: Why doesn't C support binary literals?

A: Because they take up a lot of space, and it's usually more  efficient to write hex values.



Q: Why do I need 4 bits to store a value up to 10?

A: Four bits can store values from 0 to binary 1111 which is 15. But 3 bits can only store values up to binary 111, which is 7.



Q: So what if I try to put the value 9 into a 3-bit field?

A: The computer will store a value of 1 in it, because 9 is 1001 in binary so the computer transfer 001.



Q: Are bitfields really just used to save space?

A: No, they are important if you need to read low-level binary information such as if you're reading or writing some sort of custom binary file.



#### Bullet Points - Chapter 5

- A union allows you to store different data types in the same memory location.

- A designated initializer sets a field value by name.

- Designated initializers are part of the C99 Standard. They are not supported in C++.

- If you declare a union with a value in {braces}, it will be stored with the type of the first field.

  ```C
  #include <stdio.h>
  union SampleUnion{
      int integer;
      float floating_point;
      char character;
  };
  
  int main(){
      // Initializing with an integer value.
   	union SampleUnion myUnion = {10};
      
      //Accessing fields
      printf("Integer: %d\n", myUnion.integer);
      printf("Floating Point: %f\n", myUnion.floating_point);
      printf("Character: %c\n", myUnion.character);
      return 0;
      
  }
  ```

  - When we initialize `myUnion` with a value in braces `{10}`, it is stored with the type of the first field which is `integer`, and the other fields(`floating_point` and `character`) are left uninitialized.

    Code Execution:

    ```bash
    chan@CMA:~/C_Programming/practice
    $ make practice
    cc     practice.c   -o practice
    
    chan@CMA:~/C_Programming/practice
    $ ./practice
    Integer: 10
    Floating Point: 0.000000
    Character: 
    
    ```

- The compiler will let you store one field in a union and read a completely different field. But be careful. This can cause bugs.

- `enum`s store symbols.

- Bitfields allow you to store a field with a custom number of bits.

- Bitfields should be declared as `unsigned int` due to the following reasons:

  1. **Logical Operations**: Bit manipulation operations like shifting, masking, and logical operations (AND, OR, XOR) work more predictably with unsigned integers. Using signed integers might lead to unexpected results due to sign extension during operations.
  2. **Defined Behavior**: The C standard specifies the behavior of unsigned integer operations more precisely than signed integer operations. This makes the behavior of bit manipulation operations more predictable when using `unsigned int` for bitfields.
  3. **Avoiding Sign Extension**: When using signed integers for bitfields, there's a risk of sign extension if the most significant bit (MSB) is set. This can lead to unintended behavior when manipulating the bitfield. Using unsigned integers avoids this issue entirely.
  4. **Compiler Optimization**: Compilers often generate more efficient code when dealing with unsigned integers, especially in the context of bit manipulation operations.
  5. **Consistency**: Using `unsigned int` for bitfields ensures consistency in the codebase. Developers expect bitfields to behave like unsigned integers, so using signed integers might introduce confusion and make the code harder to understand.

- A `struct` combines data types together. You can initialize `struct`s with {array, like, notation}.

- You can read `struct` fields with dot notation. `->` notation lets you easily update fields using a `struct` pointer.

- Designated initializers let you set `struct` and union fields by name.

  ```C
  typedef struct{
      int x;
      int y;
      int z;
  } Point;
  
  int main(){
      struct Point p = {.y = 10, .z=20};
      // p = {0, 10, 20}
      
      return 0;
  }
  ```

  - We can also use designated initializers with arrays:

    ```C
    int arr[5] = {[20] = 10, [4] = 20};
    ```

- `typedef` lets you create an alias for a data type.

- `union` can hold different data types in one location.

- `enums` let you create a set of symbols.

- Bitfields give you control over the exact bits stored in a `struct`.

---



### Chapter 6 - Data Structures and Dynamic memory

- To store a flexible amount of data, you need something more extensible than an array, you need a linked list.

- Linked lists are like chains of data.

- A linked list is an example of an abstract data structure. 

- It's called an abstract data structure because a linked list is general: it can be used to store a lot of different kinds of data.

- A linked list stores a piece of data and a link to another piece of data.

- In a linked list, as long as you know where the list starts, you can travel along the list of links, from one of piece of data to the next, until you reach the end of the list.

- Page 309 in HFC book for visual representation.

- Linked lists allow inserts: inserting data is very quick which is another advantage linked lists have over arrays.

- If you wanted to insert a value into the middle of an array, you would have to shuffle all the pieces of data that follow it along by one:

  | Amity | Craggy | Isla Nublar | Shutter |
  | ----- | ------ | ----------- | ------- |

  - This is an array.
  - Let's say we have an array with 3 items, which are Amity, Craggy, and Shutter.
  - If we wanted to insert an extra value after Craggy Island, we'd have to move the other values along one space.
  - And because an array is fixed length, we'd lose Shutter Island.

- So linked lists allow us to store a variable amount of data and they make it simple to add more data.



#### How do we create a linked list in C?

Create a recursive structre.

Each one of the `structs` in the list will need to connect to the one next to it.

A `struct` that contains a link to another `struct` of the same type is called a recursive structure.

- Recursive structures contain pointers to other structures of the same type.

- So if you have a flight schedule for the list of islands that you're going to visit, you can use a recursive structure for each island.

  ```C
  typedef struct island{
      char *name;
      char *opens;
      char *closes;
      struct island *next;
  } island;
  ```

  - You'll record these details for each island.
  - For each island, you'll also record the next island.
  - You must give the `struct` a name.
  - You'll use strings for the name and opening times.
  - You store a **pointer** to the next island in the `struct`.
  - If you use the `typedef` command, you can normally skip giving the `struct` a proper name. 
  - But in recursive structure, you need to include a pointer to the same type.
  - C syntax won't let you use the `typedef` alias, so you need to give the `struct` a proper name.
  - We store a link from one `struct` to the next with a **pointer**.
  - That way the island data will contain the address of the next island that we're going to visit.
  - So, whenever our code is at one island, it'll always be able to hop over to the next island.



#### Create islands in C...

Once we have defined an island data type, we can create the first set of islands like this:

```C
island amity = {"Amity", "09:00", "17:00", NULL};
island craggy = {"Craggy", "09:00", "17:00", NULL};
island isla_nublar = {"Isla Nublar", "09:00", "17:00", NULL};
island shutter = {"Shutter", "09:00", "17:00", NULL};
```



- In C, NULL actually has the value 0 but it's set aside specially to set pointers to 0.

- Once we've created each island, we can then connect them together:

  ```C
  amity.next = &craggy;
  craggy.next = &isla_nublar;
  isla_nublar.next = &shutter;
  ```

- We have to be careful to set the next field in each island to the address of the next island.

- We'll use `struct` variables for each of the islands.

- But what if we want to insert an excursion to Skull Island between Isla Nublar and Shutter Island?



#### Inserting values into the list

We can insert islands by changing the values of the pointers between islands.

```C
island skull = {"Skull", "09:00", "17:00", NULL};

isla_nublar.next = &skull;
skull.next = &shutter;
```



**Exercises are in `HFCExercises.md`.**



Q: Other languages, like Java, have linked lists built in. Does C have any data structures?

A: C doesn't really come with any data structures built in. You have to create them yourself.



Q: What if I want to use the 700th item in a really long list? Do I have to start at the first item and then read all the way through?

A: Yes, you do.



Q: That's not very good. I thought a linked list was better than an array.

A: You shouldn't think of data structures as being better or worse. They are either appropriate or inappropriate for what you want to use them for.



Q: So if I want a data structure that lets me insert things quickly, I need a linked list but if I want a direct access I might use an array?

A: Exactly.



Q: You've shown a `struct` that contains a pointer to another `struct`. Can a `struct` contain a whole recursive `struct` inside itself?

A: No.



Q: Why not?

A: C needs to know the exact amount of space a `struct` will occupy in memory. If it allowed full recursive copies of the same `struct`, then one piece of data would be a different size than another.





Each island `struct` needed its own variable. 

If we wanted the code to store more than four islands, we would need another local variable. That's fine if we know how much data we need to store at compile time,But quite often programs don't know how much storage they need until runtime. 

- If we are writing a web browser, for instance, we won;t know how much data we'll need to store a web page until we read the web page.
- So C programs need some way to tell the OS that they need a little extra storage at the moment they need it.



#### Program need dynamic storage

##### Use the heap for dynamic storage

- Most of the memory we've been using so far has been in the **stack**.
- The stack is the area of memory that's used for local variables.
- Each piece of data is stored in a variable and each variable disappears as soon as you leave its function.
- It's harder to get more storage on the stack at runtime and that's where the **heap** comes in.
- The **heap** is the place where a program stores data that will need to be available longer term.
- It won't automatically get cleared away, so that means it's the perfect place to store data structures like our linked list.
- We can think of heap storage as being a bit like reserving a locker in a locker room.



##### First, get your memory with malloc()

- We tell the malloc() function exactly how much memory we need and it asks the OS to set that much memory aside in the heap.

- The malloc() function then returns a **pointer** to the new heap space, a bit like getting a key to the locker.

- It allows you access to the memory, and it can also be used to keep track of the storage locker that's been allocated.

  | Stack     |
  | --------- |
  | Heap      |
  | Globals   |
  | Constants |
  | Code      |

- The malloc() function will give you a pointer to the space in the heap.



#### Give the memory back when you're done

When we use the stack, we don't need to worry about returning memory; it all happened automatically.

- Every time we leave a function, the local storage is freed from the stack.

The heap is different. When we've asked for space on the heap, it will never be available for anything else until we tell the C Standard Library that we are finished with it.

- If we keep asking for more and more heap space, our program will quickly start to develop memory leaks.
- A memory leak happens when a program asks for more and more memory without releasing the memory it no longer needs.
- Memory leaks are among the most common bugs in C programs and they can be really hard to track down.
- The heap has only a fixed amount of storage available, so be sure you use it wisely.

#### Free memory by calling the free() function

-  The malloc() function allocates space and gives us a pointer to it.
- We'll need to use this pointer to access the data and then when we're finished with the storage, we need to release the memory using the free() function.
- It's a bit like handing your locker key back to the attendant so that the locker can be reused.
- Every time some part of our code requests heap storage with the malloc() function, there should be some part of our code that hands the storage back with the free() function.
- When our program stops running, all of its heap storage will be released automatically, but it's always good practice to explicitly call free() on every piece of dynamic memory we've created.



#### Ask for memory with malloc()

The function that asks for memory is called malloc() for memory allocation.

- malloc() taks a single parameter: the number of bytes you need.

- Most of the time, you probably don't know exactly how much memory you need in bytes, so malloc() is almost always used with an operator called `sizeof`, like this:

  ```C
  #include <stdlib.h>
  // We need to include the stdlib.h header file to pick up the malloc() and free() functions.
  
  ...
  malloc(sizeof(island));
  // This means, "Give me enough space to store an island struct"
  ```

- `sizeof` tells us how many bytes a particular data type occupies on our system.

- It might be a `struct`, or it could be some base data type, like `int` or `double`.

- The malloc() function sets aside a chunk of memory for us, then returns a pointer containing the start address.

- malloc() actually returns a general-purpose pointer, with type `void*`.

  ```C
  island *p = malloc(sizeof(island));
  
  // This means "Create enough space for an island, and store the address in variable p."
  ```



#### ...and free up the memory with free()

- Once we have created the memory on the heap, we can use it for as long as we like.

- But once we've finished, we need to release the memory using the free() function.

- free() needs to be given the address of the memory that malloc() created.

  ```C
  free(p);
  // This means "Release the memory you allocated from heap address p."
  ```



**Key takeaway: if you allocated memory with malloc() in one part of your program, you should always release it later with the free() function.**



**Exercises can be found in `HFCExercises.md`**



#### `strdup()` function

- The `strdup()` function can reproduce a complete copy of the string somewhere on the heap:
- The `strdup()` function works out how long the string is, and then calls the malloc() function to allocate the correct number of characters on the heap.
- It then copies each of the characters to the new space on the heap.
- That means that `strdup()` always create space **on the heap**. It can't create space on the stack because that's for local variables, and local variables get cleared away too often.
- But because `strdup()` puts new strings on the heap, that means we must **always remember to release their storage with the free() function.**



**Dynamic memory allocation lets us create the memory we need at RUNTIME. And they way we access dynamic heap memory is with malloc() and free().**



Q: Why is the heap called the heap?

A: Because the computer doesn't automatically organize it. It's just a big heap of data.



Q: What's a garbage collection?

A: Some languages track when you allocate data on a heap and then when you're no longer using the data, they free the data from the heap.



Q: Why doesn't C contain garbage collection?

A: C is quite an old language; when it was invented, most languages didn't do automatic garbage collection.



Q: I understand why I needed to copy the name of the island in the example in `HFCExercises.md`. Why didn't I need to copy the `opens` and `closes` values?

A: The `opens` and `closes` values are set to string literals. String literals can't be updated, so it doesn't matter if several data items refer to the same string.



Q: Does `strdup()` actually call the `malloc()` function?

A: It will depend on how the C standard library is implemented, but most of the time, yes.



Q: Do I need to free all my data before the program ends?A: You don't have to; the OS will clear away all of the memory when the program exits. But it's good practice to always explicitly free anything you've created.



#### Bullet Points

- Dynamic data structures allow you to store a variable number of data items.
- A linked list is a data structure that allows you to easily insert items.
- Dynamic data structures are normally defined in C with recursive `struct`s.
- A recursive `struct` contains one or more pointers to a  similar `struct`.
- The stack is used for local variables and is managed by the computer.
- The heap is used for long-term storage. You allocate space with `malloc()`.
- The `sizeof` operator will tell you how much space a `struct` needs.
- Data will stay on the heap until you release it with `free()`.
- `valgrind` checks for memory leaks.
- `valgrind` works by intercepting the calls to `malloc()` and `free()`.
- When a program stops running, `valgrind` prints details of what's left on the heap.
- If you compile your code with debug information, `valgrind` can give you more information.
- If you run your program several times, you can narrow the search for the leak.
- `valgrind` can tell you which lines of code in your source put the data on the heap.
- `valgrind` can be used to check that you've fixed a leak.
- A linked list is more extensible than an array.
- Data can be inserted easily into a linked list.
- A link list is a dynamic data structure.
- Dynamic data structures use recursive structs.
- Recursive structs contain one or more links to similar data.
- `malloc()` allocates memory on the heap.
- `free()` releases memory on the heap.
- Unlike the stack, heap memory is not automatically released. 
- The stack is used for local variables.
- `strdup()` will create a copy of a string on the heap.
- A memory leak is allocated memory you can no longer access.
- `valgrind` can help you track down memory leaks.

---



### Chapter 7 - Advanced Functions

The extern keyword in C is used to declare a variable or function and specify that its actual definition is located elsewhere, not within the same block where it is declared. This can be in a different file or above/below in the same file.

When you declare a variable with extern, you're telling the compiler that this variable will be defined somewhere else in the program. This allows you to use that variable in different source files.

For example, if you have a variable int count that you want to use in multiple .c files, you can declare it in one file like this:

```C
 int count = 0;
```


And then in any other file where you want to use count, you would declare it with extern:

```C
extern int count;
```


This tells the compiler that count is an integer variable that's defined in another file. The linker then takes care of making sure all the extern declarations point to the correct location when the program is linked.



####  Every function name is a pointer to the function...

The name of a function really is a way of referring to the piece of code. And that's just what a pointer is: a way of referring to something in memory.

- That's why, in C, function names are also pointer variables.

- When you create a function called `go_to_warp_speed(int speed)`, you are also creating a pointer variable called `go_to_warp_speed` that contains the address of the function.

- ```C
  int go_to_warp_speed(int speed){
      dilithium_crystals(ENGAGE);
      warp = speed;
      reactor_core(c, 125000 * speed, PI);
      clutch(ENGAGE);
      brake(ENGAGE);
      return 0;
  }
  ```

- Whenever you create a function, you also create a function pointer with the same name.

- | STACK                        |
  | ---------------------------- |
  | HEAP                         |
  | GLOBALS                      |
  | CONSTANTS "go_to_warp_speed" |
  | CODE                         |

- ```C
  go_to_warp_speed(4);
  ```

- When you call the function, you are using the function pointer.



#### but there's no function data type

Usually, it's pretty easy to declare pointers in C. If you have a data type like int, you just need to add an asterisk to the end of the data type name, and you declare a pointer with `int *`. Unfortunately, C doesn't have a function data type, so you can't declare a function pointer with anything like `function *`.

```C
int *a;
```

This declares an int pointer.

```C
function *f;
```

but this won't declare a function pointer.



#### Why doesn't C have a function data type?

C doesn't have a function data type because there's not just one type of function.

- When you create a function, you can vary a lot of things, such as the return type or the list of parameters it takes.
- That combination of things is what defines the type of the function.

```C
int go_to_warp_speed(int speed){
    ...
}
```

```C
char **album_names(char *artist, int year){
    ...
}
```

There are many different types of functions. These two functions above are different types because they have different return types and parameters.



#### How to create function pointers

Say you want to create a pointer variable that can store the address of each of the functions.

```C
int (*warp_fn) (int);

warp_fn = go_to_warp_speed; <- // This will create a variable called warp_fn that can store the address of the go_to_warp_speed() function.
    
warp_fn(4); <- // This is just like calling go_to_warp_speed(4)
    
    
char ** (*names_fn) (char*, int);
names_fn = album_names; <- // This will create a variable called names_fn that can store the address of the album_names() function.
    
char **results = names_fn("Sacha Distel", 1972);
```



We need to tell C the return type and the parameter types the function will take. But once we have declared a function pointer variable, you can use it like any other variable. You can assign values to it, you can add it to arrays and you can also pass it to functions.



Q: What does `char **` mean? Is it a typing error?

A: `char **` is a pointer normally used to point to an array of strings.



**Exercises are in `HFCExercises.md`. **

| Return type | (*Pointer variable) | (Param types) |
| ----------- | ------------------- | ------------- |
| char**      | (*names_fn)         | (char*, int)  |



`(*names_fn)` - This is the name of the variable we are declaring.



Q: If function pointers are just pointers, why don't you need to prefix them with a * when you call the function?

A: You can. In the program in `HFCExercises.md`, instead of writing `match(ADS[i])`, you could have written `(*match)(ADS[i])`.



Q: And could I have used & to get the address of a method?

A: Yes. Instead of `find(sports_or_workout)`, you could have written `find(&sports_or_workout)` in the exercise `HFCExercises.md`.



Q: Then why didn't I?

A: Because it makes the code easier to read. If you skip the * and &, C will still understand what you're saying.



#### Get it sorted with the C Standard Library

Lots of programs need to sort data. If the data is something simple like a set of numbers, then sorting is pretty easy. Numbers have their own natural order. But it's not so easy with other types of data.



**How could a sort function sort any type of data at all?**



#### Use function pointers to set the order

The C Standard Library has a sort function that accepts a pointer to a comparator function which will be used to decide if one piece of data is **the same as**, **less than** or **greater than** another piece of data.



This is what the `qsort()` function looks like:

```C
qsort(void *array, <- // This is a pointer to 										an array.
     size_t length, <- // This is the length of 									the array.
     size_t item_size, <- // This is the size 					of each element in the array.
     int(*compar)(const void *, const void *));

// (*compar) - This is a pointer to a function that compares two items in the array.
// const void * - Remember, a void* pointer can point to anything.
```



- The `qsort()` function compares pairs of values over and over again, and if they are in the wrong order, the computer will switch them.
- And that's what the comparator function is for. It will tell `qsort()` which order a pair of elements should be in. 
- It does this by returning three different values.
- If the first value is greater than the second value, it should return a positive number.
- If the first value is less than the second value, it should return a negative number.
- If the two values are equal, it should return zero.



Let's say we have an array of integers and we want to sort them in increasing order.

```C
int scores[] = {543, 323, 32, 554, 11, 3, 112};
```



If you look at the **signature** of the comparator function that `qsort()` needs, it takes two **void pointers** given by `void *`.

A void pointer can store the address of **any kind of data**, but you always need to cast it to something more specific before you can use it.



**A void pointer `void *` can store a pointer to anything.**



The `qsort()` function works by comparing pairs of elements in the array and then placing them in the correct order. It compares the values by calling the comparator function that you give it.

```C
int compare_scores(const void* score_a, const void* score_b){
    ...
}
```



- Values are always passed to the function as pointers, so the first thing we need to do is get the integer values from the pointers.

- ```C
  int a = *(int*)score_a;
  int b = *(int*)score_b;
  ```

- We need to cast the void pointer to an integer pointer.

- The first * then gets the int stored at address score_b.

- Then we need to return a positive, negative or zero value depending on whether `a` is greater than, less than, or equal to `b`.

- For integers, that's pretty easy to do - we just subtract one number from the other.

- ```C
  return a - b;
  
  // If a > b, this is positive;
  // If a < b, this is negative;
  // If a and b are equal, this is zero
  ```

- And this is how we ask `qsort()` to sort the array

- ```C
  qsort(scores, 7, sizeof(int), compare_scores);
  ```

  

#### Why use `*(int*)score_a`?

The function `compare_scores` needs to compare two `int` values.

1. **Casting the Void Pointer:** `score_a` is of type `const void *`, which is a generic pointer type in C. Since `void *` does not point to any specific type, it must be cast to the appropriate type before dereferencing. In this case, we cast it to `int *`.

   ```C
   (int *)score_a
   ```

   This tells the compiler that `score_a` is actually pointing to an `int`.

2. **Dereferencing the Pointer:** After casting `score_a` to `int *`, we then dereference it to get the `int` value it points to:

   ```C
   *(int *)score_a
   ```

   This `*` operator is used to access the value at the address stored in the pointer. This effectively converts `score_a` from a pointer to a void type into an `int` value.



```C
int compare_scores(const void *score_a, const void *score_b){
    int a = *(int*)score_a;
    int b = *(int*)score_b;
    return a - b;
}

qsort(scores, 7, sizeof(int), compare_scores);
```



#### How it works

- `score_a` and `score_b`: These are pointers to `int` values but they are passed as `const void *` for generality as required by `qsort`.
- `Casting and Dereferencing`: By casing `score_a` and `score_b` to `int *` and then dereferencing them, you obtain the actual `int` values that need to be compared.
- `Returning the Result`: The function returns the difference between `a` and `b`. This is a common way to write a comparison function in C.

**Refer to the example exercise in `HFCExercises.md`.**



#### `strcmp`

The `strcmp` function in C starts comparing the first characters of both strings. If they are equal, it moves on to the next characters, and so on. This process continues until it finds a pair of characters that differ or until it reaches the end of strings.

- If the two strings are equal, `strcmp` returns 0.
- If the first string is lexicographically greater than the second, `strcmp` returns a positive integer.
- If the first string is lexicographically less than the second, `strcmp` returns a negative integer.

```C
#include <stdio.h>
#include <string.h>

int main() {
    char *str1 = "Hello";
    char *str2 = "World";
    char *str3 = "Hello";

    printf("strcmp(str1, str2): %d\n", strcmp(str1, str2));
    printf("strcmp(str1, str3): %d\n", strcmp(str1, str3));

    return 0;
}
```

Code Execution

```bash
chan@CMA:~/C_Programming/test$ ./final
strcmp(str1, str2): -15
strcmp(str1,str3): 0

```

In this example, "Hello" and "World" both have 5 characters, but "Hello" is considered lexicographically less than "World". This is because the ASCII value of 'H'(72) is less than the ASCII value of 'W'(87).



#### `qsort()`

The `qsort` function in C is used to sort items in an array. It takes 4 parameters.

1. `base`: A pointer to the first element of the array to be sorted.
2. `nitems`: The number of elements in the array.
3. `size`: The size in bytes of each element in the array. This is typically calculated as `sizeof(type)`, where `type` is the data type of the elements.
4. `compar`: A function pointer to a comparison function. This function takes two pointers (pointing to two elements to be compared) and returns an integer less than, equal to, or greater than zero if the first argument is considered to be respectively less than, equal to, or greater than the second.

```C
int scores[] = {543, 323, 32, 554, 11, 3, 112};

qsort(scores, 7, sizeof(int), compare_scores_desc);

char *names[] = {"Karen", "Mark", "Brett", "Molly"};

qsort(names, 4, sizeof(char *), compare_names);
```



#### Sorting Explanation

- < 0 if x should go before y.
- 0 if x is equivalent to y.
- `> 0` if x should go after y. 

```C
int a[] = {8, 7, 2, 4, 6, 3, 5, 1, 9, 0};
```



Considering the above array:

```C
return x - y;
```

Let's say `x` = 2 and `y` = 7;

x - y = 2 - 7 = - 5 which is less than zero so x should go before y which means in ascending order. (2...7)



```C
return y - x;
```

Now same as above `x` = 2 and `y` = 7;

y - x = 7 - 2 = 5 which is greater than zero so x should go after y which means in descending order. (7....2)



#### pointer-to-pointer

In C, a pointer is a variable that holds the memory address of another variable. A pointer-to-pointer, denoted by two asterisks(**), is a form of multiple indirection, or a chain of pointers. Essentially, a pointer-to-pointer is a pointer that points to another pointer.

```C
int x = 5; // An integer
int *p = &x; // A pointer that holds the address of x
int **q = &p; // A pointer-to-pointer that holds the address of p
```

In this example, `p` points to `x`, and `q` points to `p`. So, `q` is a pointer-to-pointer.

**Now why do we use a double asterisk or a pointer-to-pointer?**

One common use of pointer-to-pointer is in functions that need to modify the content of a pointer passed as arguments.

- In C, everything is passed by value, including pointers. This means that if you pass a pointer to a function, the function works with a copy of the pointer, not the original pointer itself.
- If you want to change the original pointer, you need to pass a pointer to the pointer, i.e. , a pointer-to-pointer.

```C
void change_pointer(int **p){
    int y = 10;
    *p = &y;
}

int main(){
    int x = 5,
    int *p = &x;
    printf("Before calling change_pointer: %d\n", *p); // Before modification, this will print 5.
    change_pointer(&p);
    printf("After calling change_pointer: %d\n", *p); // After modification, this will print 10
    return 0;
}
```

In this example:

1. In the `change_pointer` function:
   - It takes a pointer to a pointer to an integer (`int **p`) as its parameter.
   - Inside the function, a local integer variable `y` is declared and assigned the value 10.
   - Then, the pointer `p` is updated to point to the address of `y` using the address-of operator (`&`). This means that `p` now holds the address of the local variable `y`.
2. In the `main` function:
   - An integer variable `x` is declared and assigned the value 5.
   - A pointer `p` is declared and initialized to point to the address of `x`.
   - The value of `p` is printed before and after the function call to `change_pointer`.
   - After the function call, the value of `p` has been changed to point to the address of the local variable `y` within the `change_pointer` function.

So, `p` is a pointer to an integer (`int *`), and `&p` is a pointer to a pointer to an integer (`int **`). By passing `&p` to the `change_pointer` function, we allow the function to modify the value of `p`, effectively changing what it points to.



#### Create an array of function pointers

The trick is to create an array of function pointers that match the different response types.

```C
void (*replies[])(response) = {dump, second_chance, marriage};

// void - each function in the array will be a void function
// (*replies[]) - The variable will be called "replies" and it's not just a function pointer; it's a whole array of them.
// (response) - Just one parameter with type "response".

```

| Return type | (*Pointer variable) | (Param types) |
| ----------- | ------------------- | ------------- |
| void        | (*replies[])        | (response)    |

#### But how does an array help?

It contains a set of function names that are in exactly the same order as the types in the `enum`:

```C
enum response_type{
    DUMP,
    SECOND_CHANCE,
    MARRIAGE
};
```

This is really important because when C creates an `enum`, it gives each of the symbols a number starting at 0.

- So `DUMP == 0`, `SECOND_CHANCE == 1`, and `MARRIAGE == 2`. And that's really neat because it means you can get a pointer to one of your sets of functions using a `response_type`.

```C
replies[SECOND_CHANCE] = second_chance;

// replies - this is your "replies" array of functions
// SECOND_CHANCE - has the  value 1.
// second_chance - it's equal to the name of the second_chance function.

// void (*replies[])(response) = {dump, second_chance, marriage};
// replies[0] will run the funciton "dump"
// replies[1] will run the function "second_chance"
// replies[2] will run the function "marriage"

// replies[0] == DUMP
// replies[1] == SECOND_CHANCE
// replies[2] == MARRIAGE .Since enum gives each of the symbols a number starting at 0 as mentioned above.
```



#### Chapter 7 - Bullet Points

- Function pointers store the addresses of functions.
- The name of each function is actually a function pointer.
- If you have a function `shoot()`, then `shoot` and `&shoot` are both pointers to that function.
- You declare a new function pointer with `return-type (*var-name) (param-types)`.
- If `fp` is a function pointer, you can call it with `fp(params, ...)`.
- Or yo can use `(*fp)(params, ...)`. C will work the same way.
- The C Standard Library has a sorting function called `qsort()`.
- `qsort()` accepts a pointer to a comparator function that can test for (in)equality.
- The comparator function will be passed pointers to two items in the array being sorted.
- If you have an array of data, you can associate functions with each data item using function pointer arrays.



Q: Why is the function pointer array syntax so complex?

A: Because when you declare a function pointer, you need to say what the return and parameter types are. That's why there are so many parentheses.



Q: This looks a little like the sort of object-oriented code in other languages. Is it?

A: It's similar. Object-oriented languages associate a set of functions (called methods) with pieces of data. In the same way, you can use function pointers to associate functions with pieces of data.



Q: Hey, so does that mean that C is object-oriented?

A: No, C is not object-oriented, but other languages that are built on C, like Objective-C and C++, create a lot of their object-oriented features by using function pointers under the covers.



#### Make your functions stretchy

The `printf()` function has one really cool feature that we have used: it can take a **variable number of arguments**.

```C
printf("%i bottles of beer on the wall, %i bottles of beer\n", 99, 99);

// You can pass the printf() as many arguments as you need to print.
```



#### Variadic Functions

A function that takes a variable number of parameters is called a **variadic function**. 

- The C Standard Library contains a set of **macros** that can help you create your own variadic functions.

- To see how they work, we'll create a function that can print out series of `ints`:

  ```C
  print_ints(3, 79, 101, 32);
  
  // 3 - Number of ints to print
  // 79, 101, 32 - The ints that need to be printed
  ```

```C
#include <stdarg.h>

void print_ints(int args, ...){
    va_list ap;
    va_start(ap, args);
    // va_start says where the variable arguments start. The variable arguments will start after the args parameter.
    int i;
    
    // This will loop through all of the other arguments.
    for(i = 0; i < args; i++){ // args contains a count of how many variables there are.
        printf("argument: %i\n", va_arg(ap, int));
    }
    va_end(ap);
}
```

Let's break it down and take a look at it, step by step

1. **Include the `stdarg.h` header**. 

   All the code to handle variadic functions is in `stdard.h`, so we need to make sure we include it.

2. **Tell our function there's more to come**.

   An ellipsis "...", in C, after the argument of a function means there are more arguments to come.

3. **Create a va_list**.

   A `va_list` will be used to store the extra arguments that are passed to our function.

4. **Say where the variable arguments start**.

   C needs to be told the name of the **last fixed argument**. In the case of our function, that'll be the `args` parameter.

5. **Then read off the variable arguments, one at a time**.

   Now our arguments are all stored in the `va_list`, we can read them with `va_arg`. `va_arg`takes two values: the `va_list` and the **type** of the next argument. In our case, all of the arguments are `ints`.

6. **Finally...end the list**.

   After we've finished reading all of the arguments, we need to tell C that we're finished. We do that with the `va_end` macro.

7. **Now we can call our function**.

   Once the function is complete, we can call it:

   ```C
   print_ints(3, 79, 101, 32);
   
   // This will print out 79, 101, and 32 values.
   ```



#### Functions vs. macros

A macro is used to rewrite your code before it's compiled. The macros we are using here(`va_start`, `va_arg`, and `va_end`) might look like functions, but they actually hide secret instructions that tell the preprocessor how to generate lots of extra smart code inside our program, just before compiling it.



Q: Why are `va_end` and `va_start` called macros? Aren't they just normal functions?

A: No, they are designed to look like ordinary functions, but they actually are replaced by the preprocessor with other code.



Q: And the preprocessor is?

A: The preprocessor runs just before the compilation step. Among other things, the preprocessor includes the headers into the code.



Q: Can I have a function with just variable arguments, and no fixed arguments at all?

A: No, you need to have at least one fixed arguments in order to pass its name to `va_start`.



Q: What happens if I try to read more arguments from `va_arg` than have been passed in?

A: Random errors will occur.



Q: What if I try to read an `int` argument as a double, or something?

A: Random errors will occur.



#### Bullet Points - Chapter 7

- Functions that accept a variable number of arguments are called variadic functions.
- To create variadic functions, you need to include `stdarg.h` header file.
- The variable argument will be stored in a `va_list`.
- You can control the `va_list` using `va_start()`, `va_arg()` and `va_end()`.
- You will need at least one fixed parameter.
- Be careful that you don't try to read more parameters than you've been given.
- You will always need to know the data type of every parameter you read.
- Function pointers let you pass functions around as if they were data.
- The name of every function is **a pointer** to the function.
- Arrays of function pointers can help run different functions for different types of data.
- Each sort function needs a pointer to a comparator function.
- `qsort()` will sort an array.
- Comparator functions decide how to order two pieces of data.
- Function pointers are the only pointers that don't need * and & operators but you can still use them if you want to.

---



### Chapter 8 - Static and dynamic libraries

#### Hot-swappable code



#### Explanation of Encrypt function and checksum function

encrypt.c

```C
#include "encrypt.h"

void encrypt(*message){
    while(*message){
        *message = *message ^ 31;
        message++;
    }
}
```

#### What it does:

- The `encrypt` function performs a simple bitwise XOR encryption on a string.
- It iterates through each character in the `message` string and applies the XOR operation with the value `31`.

#### How it works:

1. **Pointer to the message**: The function takes a pointer to a character array (`char *message`), which is the string to be encrypted.
2. **Loop through the string**: The `while (*message)` loop continues until it reaches the null terminator (`'\0'`) of the string.
3. **Bitwise XOR operation**: Inside the loop, the statement `*message = *message ^ 31;` takes the current character, performs a bitwise XOR with `31`, and stores the result back in the same character position.
4. **Move to the next character**: The `message++` statement moves the pointer to the next character in the string.

#### Example:

If the original message is "abc", the ASCII values are 97, 98, and 99. After the XOR operation with 31, the values become:

- 'a' (97) ^ 31 = 126 (ASCII for '~')
- 'b' (98) ^ 31 = 125 (ASCII for '}')
- 'c' (99) ^ 31 = 124 (ASCII for '|')

So, "abc" would be encrypted to "~}|".



checksum.c

```C
#include "checksum.h"

int checksum(char *message){
    int c = 0;
    while(*message){
        c += c ^ (int)(*message);
        message++;
    }
    return c;
}
```

#### What it does:

- The `checksum` function calculates a checksum value for the string using a combination of addition and bitwise XOR operations.

#### How it works:

1. **Pointer to the message**: The function takes a pointer to a character array (`char *message`), which is the string to calculate the checksum for.

2. **Initialize checksum**: An integer variable `c` is initialized to `0`.

3. **Loop through the string**: The `while (*message)` loop continues until it reaches the null terminator (`'\0'`) of the string.

4. Calculate checksum

   : Inside the loop, the statement 

   ```
   c += c ^ (int)(*message);
   ```

    does the following:

   - It converts the current character to its integer (ASCII) value.
   - It performs a bitwise XOR operation between the current checksum value `c` and the integer value of the character.
   - It adds the result of the XOR operation to `c`.

5. **Move to the next character**: The `message++` statement moves the pointer to the next character in the string.

6. **Return checksum**: The function returns the final checksum value `c`.

#### Example:

For the string "abc":

- 'a' (97): `c += c ^ 97` when `c` is 0 -> `c = 0 + (0 ^ 97) = 97`
- 'b' (98): `c += c ^ 98` when `c` is 97 -> `c = 97 + (97 ^ 98) = 97 + 3 = 100`
- 'c' (99): `c += c ^ 99` when `c` is 100 -> `c = 100 + (100 ^ 99) = 100 + 7 = 107`



```markdown
   01100001  (97)
^  01100010  (98)
-----------
   00000011  (3)

```



### Bit-by-Bit Explanation

1. The first bit (leftmost) of both numbers is `0`. XOR of `0` and `0` is `0`.
2. The second bit of both numbers is `1`. XOR of `1` and `1` is `0`.
3. The third bit of both numbers is `1`. XOR of `1` and `1` is `0`.
4. The fourth bit of both numbers is `0`. XOR of `0` and `0` is `0`.
5. The fifth bit of both numbers is `0`. XOR of `0` and `0` is `0`.
6. The sixth bit of the first number is `0` and the second number is `1`. XOR of `0` and `1` is `1`.
7. The seventh bit of the first number is `1` and the second number is `0`. XOR of `1` and `0` is `1`.
8. The eighth bit of both numbers is `1`. XOR of `1` and `1` is `0`.

Combining these results, we get `00000011` in binary, which is `3` in decimal.

So, the XOR operation `97 ^ 98` results in `3`.

So, the checksum for "abc" would be 107.



#### Angle brackets are for standard headers

##### Where are the standard header directories?

So, if you include headers using angle brackets, where does the compiler go searching for the header files? 

You'll need to check the documentation that came with your compiler, but typically on a Unix-style system like the Mac or a Linux machine, the compiler will look for the files under these directories:

`/usr/local/include`

`/usr/include`

It will check `/usr/local/include` first.  `/usr/local/include` is often used for header files for third-party libraries.

`/usr/include` is normally used for OS headerfiles.



#### Sharing .h header files

1. **Store them in a standard directory.**

   If you copy your header files into one of the standard directories like `/usr/local/include`, you can include them in your source code using angle brackets.

   ```C
   #include <encrypt.h>
   
   // You can use angle brackets if your header files are in a standard directory.
   ```

2. **Put the full pathname in your include statement.**

   If you want to store your header files somewhere else, such as  `/my_header_files`, you can add the directory name to your include statement.

   ```C
   #include "/my_header_files/encrypt.h"
   ```

3. **You can tell the compiler where to find them.**

   The final option is to tell the compiler where it can find your header files. You can do this with the `-I` option on `gcc`:

   ```bash
   gcc -I/my_header_files test_code.c ... -o test_code
   
   // This tells the compiler to look in /my_header_files as well as the standard directories.
   ```

   - The `-I` option tells the `gcc` compiler that there's another place where it can find header files. It will still search in all the standard places, but first it will check the directory names in the `-I` option.



#### Share .o object files by using the full pathname

We can always put our .o object files into some sort of shared directory. Once we have done that, we can then jut add the full path to the object files when we're compiling the program that uses them:

```makefile
gcc -I/my_header_files test_code.c /my_object_files/encrypt.o /my_object_files/checksum.o -o test_code
```



- Using the full pathname to the object files means we don't need a separate copy for each C project.
- /my_object_files is like a central store for our project files.



Makefile

```makefile
all: final

final: object_files/encrypt.o object_files/checksum.o object_files/main.o
	@echo "Compiling the final exe file..."
	gcc object_files/encrypt.o object_files/checksum.o object_files/main.o -o final

main.o: main.c
	@echo "Compiling the main file..."
	gcc -I/header_files -c main.c -o object_files/main.o

encrypt.o: encrypt.c 
	@echo "Compiling the encrypt file..."
	gcc -I/header_files -c encrypt.c -o object_files/encrypt.o 

checksum.o: checksum.c 
	@echo "Compiling the checksum file..."
	gcc -I/header_files -c checksum.c -o object_files/checksum.o

clean: 
	@echo "Removing everything except the source files..."
	@rm object_files/encrypt.o object_files/checksum.o object_files/main.o final

```

header_files

- `encrypt.h`
- `checksum.h`

object_files

- `main.o`
- `encrypt.o`
- `checksum.o`

encrypt.c

```C
#include "../header_files/encrypt.h"

void encrypt(char *message){
    while(*message){
        *message = *message ^ 31;
        message++;
    }
}
```

checksum.c

```C
#include "../header_files/checksum.h"

int checksum(char *message){
    int c = 0;
    while(*message){
        c += c ^ (int)(*message);
        message++;
    }
    return c;
}
```



If we compile our code with the full pathname to the object files we want to use, then all our C programs can share the same `encrypt.o` and `checksum.o` files.

```markdown
Hmm.. That's OK if I just have one or two object files to share, but what if I have alot of object files? I wonder if there's some way of telling the compiler about a bunch of them...
```



**Yes, if we create an archive of object files, we can tell the compiler about a whole set of object files all at once.**

An archive is just a bunch of object files wrapped up into a single file. By creating a single archive file of all of your security code, you can make it a lot easier to share the code between projects.



#### An archive contains .o files

Ever used a .zip or .tar file? Then you know how easy it is to create a file that contains other files. That's exactly what a .a archive file is: **a file containing other files.**



```bash
chan@CMA:~$ 
cd /usr/local/lib

chan@CMA:/usr/local/lib
$ ls

libcs50.a  libcs50.so  libcs50.so.11  libcs50.so.11.0.2  python3.10

chan@CMA:/usr/local/lib
$ nm libcs50.a

libcs50.o:
0000000000000000 b allocations
                 U atexit
                 U __ctype_b_loc
                 U __errno_location
                 U fgetc
                 U free
00000000000003ce T get_char
00000000000004ee T get_double
00000000000006fd T get_float
00000000000008fd T get_int
0000000000000aa3 T get_long
0000000000000c3d T get_long_long
0000000000000000 T get_string
                 U __isoc99_sscanf
                 U realloc
0000000000000e3a t setup
                 U setvbuf
                 U __stack_chk_fail
                 U stdin
                 U stdout
                 U strcspn
0000000000000008 b strings
                 U strlen
                 U strtod
                 U strtof
                 U strtol
                 U strtoll
0000000000000dd7 t teardown
                 U ungetc
                 U vprintf
chan@CMA:/usr/local/lib$ 

```



The `nm` command lists the **names** that are stored inside the archive.





#### Create an archive with the `ar` command

The `archive comand (ar)` will store a set of object files in an archive file:

```makefile
ar -rcs libhfsecurity.a encrypt.o checksum.o
```

- `-rcs` - The `r` means the .a file will be updated if it already exists. The `c` means that the archive will be created without any feedback. The `s` tells `ar` to create an index at the start of the .a file.
- `libhfsecurity.a` - This is the name of the .a file to create.
- The last two files are the files that will be stored in the archive.

Did you notice that all of the .a files have names like `lib<something>.a`? That's the standard way of naming archives. The names begin with `lib` because they are `static libraries`. 

```markdown
**Make sure you always name your archives lib<something>.a.
If you don't name them this way, your compiler will have problems tracking them down.
```

#### then store the .a in a library directory

Once you have an archive, you can store it in a library directory. Which library directory should you store it in? It's up to you, but you have a couple of choices:

1. **You can put your .a file in a standard directory like `/usr/local/lib`.**

   Some coders like to install archives into a standard directory once they are sure it's working. On Linux, on Mac and in Cygwin, the `/usr/local/lib` directory is a good choice because that's the directory set aside for your own local custom libraries.

2. **Put the .a file in some other directory.**

   If you are still developing your code, or if you don't feel comfortable installing your code in a system directory, you can always create your own library directory. For example, `/my_lib`.

   - On most machines, you need to be an administrator to put files in `/usr/local/lib`.

#### Finally, compile your other programs

The whole point of creating a library archive was so you could use it with other programs. If you've installed your archive in a standard directory, you can compile your code using the `-l` switch:

```bash
gcc test_code.c -lhfsecurity -o test_code
```

- `test_code.c` - Remember to list your source files before your `-l` libraries.
- `-lhfsecurity` - hfsecurity tells the compiler to look for an archive called `libhfsecurity.a`.
- If you're using several archives, you can set several `-l` options.
- Do you need an `-I` option? It depends on where you put your headers.

Can you see why it's so important to name your archive `lib<someting>.a`?

```markdown
The name that follows the -l option needs to match part of the archive name. So if your archive is called libawesome.a, you can compile your program with the -lawesome switch.
```

But what if you put your archive somewhere else, like `/my_lib`? In that case, you will need to use the `-L` option to say which directories to search:

```bash
gcc test_code.c -L/my_lib -lhfsecurity -o test_code
```



#### Chapter - 8 Bullet points

- Headers in angle brackets (< >) are read from the standard directories.
- Examples of standard header directories are `/usr/include` and `C:/MinGW/include`.
- A library archive contains several object files.
- You can create an archive with `ar -rcs libarchive.a file0.o file1.o...`.
- Library archive names should begin `lib` and end `.a`.
- If you need to link to an archive called `libfred.a`, use `-lfred`.
- The `-L` flag should appear after the source files in the `gcc` command.



Q: How do I know what the standard library directories are on my machine?

A: You need to check the documentation for your compiler. On most Unix-style machines, the library directories include `/usr/lib` and `/usr/local/lib`.



Q: When I try to put a library archive into my `/usr/lib` directory, it won't let me. Why is that?

A: Almost certainly security. Many OS will prevent you from writing files to the standard directories in case you accidentally break one of the existing libraries.



Q: is the `ar` format the same on all systems?

A: No. Different platforms can have slightly different archive formats. And the object code the archive contains will be completely different for different OS.



Q: If I've created a library archive, can I see what's inside it?

A: Yes. `ar -t <filename>` will list the contents of the archive.



Q: Are the object files in the archive linked together like an executable?

A: No. The object files are stored in the archive as distinct files.



Q: Can I put any kind of file in a library archive?

A: No. The `ar` command will check the file type before including it.



Q: Can I extract a single object file from an archivee?

A: Yes. To extract the `encrypt.o` file from `libhfsecurity.a` , use `ar -x libhfsecurity.a encrypt.o`.



Q: Why is it called "static" linking?

A: Because it can't change once it's been done. When two files are linked together statically, it's like mixing coffee with milk: you can't separate them afterwards.



An object file might need to call a function that's stored in some other file. The Linker links together the point in one file where the function call is made to the point in another file where the function lives.



```markd
We've already seen that we can build programs using different pieces of object code. We've created .o files and .a archives and we've linked them together.
but once they're linked, we can't change them as they are static.
Once we've created a single executable file from those separate pieces of object code, we really have no way of changing any of the ingredients without rebuilding the whole program.
```



#### Dynamic linking happens at runtime

The reason we can't change the different pieces of object code in an executable file is because they are all contained in a single file. They were statically linked together when the program was compiled.

But if our program wasn't just a single file - if our program was made up of lots of separate files that only joined together when the program was run- we would avoid the problem.



```markdo
The trick, then, is to find a way of storing pices of object code in separate files and then dynamically linking them together only when the program runs.
```



#### Dynamic libraries are object files on steroids

- Dynamic libraries are similar to those .o object files but they are not quite the same.
- Like an archive file, a dynamic library can be built from several .o object files, but unlike an archive, the object files are properly linked together in a dynamic library to form a single piece of object code.
- A dynamic library contains extra information that the OS will need to link the library to other things.
- At the heart of a dynamic library is a single piece of object code.
- The library is built from one or more .o files.



#### How do we create our own dynamic libraries

#### First, create an object file

If we're going to convert the `hfcal.c` code into a dynamic library, then we need to begin by compiling it into a .o object file, like this:

**The exercise is in `HFCExercises.md`.**

```makefille
gcc -I/header_files -fPIC -c hfcal.c -o hfcal.o
```

- `-I/header_files` - The `hfcal.h` header is in `./header_files`.
- `-c` - means "Don't link the code".
- `-fPIC`- This tells `gcc` that we want to create `position-independent code`.
- Position-independent code can be moved around in memory.
- Some OS and processors need to build libraries from position-independent code so that they can decide at runtime where they want to load it in memory.
- The truth is that on most systems we don't need to specify this option.

```markd
What is position-independent code?

Position-independent code is code that doesn't mind where the computer loads it into memory. Imagine you had a dynamic library that expected to find the value of some piece of global data 500 bytes away from where the library is loaded. Bad things would happen if the OS decided to load the library somewhere else in memory. If the compiler is told to create position-independent code, it will avoid problems like this.

Some OS, like Windows, use a technique called memory mapping when loading dynamic libraries, which means all code is effectively position-independent. If you compile your code on Windows, you might find that gcc will give you a warning that the -fPIC option is not needed.
```



#### What you call your dynamic library depends on your platform

- Dynamic libraries are available on most OS and they all work in pretty much the same way but what they're called can vary a lot.
- On Windows, dynamic libraries are usually called `dynamic link libraries` and they have the extension `.dll`.
- On Linux and Unix, they're `shared object files (.so)`.
- On Mac, they're just called `dynamic libraries (.dylib)`.

```makefile
gcc -shared hflocal.o -o /libs/libhfcal.so
```

- `C:\libs\hfcal.dll` - MinGW on Windows
- `/libs/libhfcal.dll.a` - Cygwin on Windows
- `/libs/libhfcal.dylib` - Mac



```markdo
The -shared option tells gcc that we want to convert a .o object file into a dynamic library.
When the compiler creates the dynamic library, it will store the name of the library inside the file.
So, if you create a library called libhfcal.so on a Linux machine, the libhfcal.so file will remember that its library name is hfcal.
Why is that important? It means that if we compile a library with one name, we can't just rename the file afterwards.
If we need to rename a library, recompile it with the new name.
```



#### Compiling the elliptical program from `HFCExercises.md`

- Once we've created the dynamic library, we can use it just like a static library. 

```makefile
gcc -I./header_files -c elliptical.c -o elliptical.o
gcc elliptical.o -L./libs -lhfcal -o elliptical
```

- Even though these are the same commands we would use if `hfcal` were a static archive, the compile will work differently.
- Because the library is dynamic, the compiler won't include the library code into the executable file. Instead, it will insert some placeholder code that will track down the library and link to it at runtime.



If we want to store it in standard libraries directory,

```makefile
gcc -shared hfcal.o -o /libs/libhfcal.so

gcc -I\include -c elliptical.c -o elliptical.o

gcc elliptical.o -L\libs -lhfcal -o elliptical
```



Makefile

```makefile
all: elliptical

elliptical: elliptical.o libhfcal.so  
	gcc elliptical.o -L./libs -lhfcal -Wl,-rpath,./libs -o elliptical 
	
elliptical.o: elliptical.c ./header_files/hfcal.h
	gcc -I./header_files -c elliptical.c -o elliptical.o

hfcal.o: hfcal.c ./header_files/hfcal.h
	gcc -I./header_files -c hfcal.c -o hfcal.o

libhfcal.so: hfcal.o 
	gcc -shared hfcal.o -o ./libs/libhfcal.so
```



**Important:**

- When we link against a shared library, the runtime linker needs to be able to find that library at runtime. 

- By default, it looks in several standard locations (like `/lib` and `/usr/lib`) but it won't look in your `./libs` directory.

- We can tell the runtime linker to look in `./libs` by setting the `LD_LIBRARY_PATH` environment variable to include `./libs`. 

  ```makeflie
  export LD_LIBRARY_PATH=./libs:$LD_LIBRARY_PATH
  ./elliptical
  
  ```

  ```bash
  chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
  $ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:./libs
  chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
  $ ./elliptical
  Weight: 115.20 lbs
  Distance: 11.30 miles
  Calories burned: 1028.39 cal
  ```

  

  This command will add `./libs` to the list of directories that the runtime linker searches. The `:$LD_LIBRARY_PATH` part ensures that the linker will still search all the standard locations as well.

- If we want to avoid having to set `LD_LIBRARY_PATH` every time, we can add `-Wl,-rpath,./libs` option when we link `elliptical` .

  ```bash
  chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
  $ make all
  gcc -shared hfcal.o -o ./libs/libhfcal.so
  gcc elliptical.o -L./libs -lhfcal -o elliptical
  
  chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
  $ ./elliptical
  ./elliptical: error while loading shared libraries: libhfcal.so: cannot open shared object file: No such file or directory
  
  chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
  $ make all
  gcc -shared hfcal.o -o ./libs/libhfcal.so
  gcc elliptical.o -L./libs -lhfcal -Wl,-rpath,./libs -o elliptical 
  
  chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
  $ ./elliptical
  Weight: 115.20 lbs
  Distance: 11.30 miles
  Calories burned: 1028.39 cal
  ```

  

- This will embed `./libs` as a runtime search path in the `elliptical` executable itself.



#### Bullet Points - Chapter 8

- Dynamic libraries are linked to programs at run time.
- Dynamic libraries are created from one or more object files.
- The `-shared` compiler option creates a dynamic library.
- Dynamic libraries have different names on different systems.
- The `ar` command creates a library archive of object files.
- `# include <>` looks in standard directories such as `/usr/include`.
- `gcc -shared` converts object files into dynamic libraries.
- Dynamic libraries have `.so`, `.dylib`, `.dll`, or `.dll.a` extensions.
- `-l<name>` links to a file in standard directories such as `/usr/lib`.
- `-I<name>` adds a directory to the list of standard include directories.
- `-L<name>` adds a directory to the list of standard library directories.
- Library archives have names like `libsomething.a`.
- Library archives are statically linked.



Q: Why are dynamic libraries so different on different OS?

A: OS like to optimize the way they load dynamic libraries, so they've each evolved different requirements for dynamic libraries.



Q: I tried to change the name of my library by renaming the file, but the compiler couldn't find it anymore. Why not?

A: When the compiler creates a dynamic library, it stores the name of the library inside the file. If you rename the file, it will then have the wrong name inside the file and will get confused. If you want to change its name, you should recompile the library.



Q: Why doesn't Linux just store library pathnames in executables? That way, you wouldn't need to set `LD_LIBRARY_PATH`?

A: It was a design choice. By not storing the pathname, it gives you a lot more control over which version of a library a program can use--- which is great when you're developing new libraries.



Q: Which is better? Static or dynamic linking?

A: It depends. Static linking means you get small, fast executable file that is easier to move from machine to machine. Dynamic linking means that you can configure the program at run time more.



Q: If different programs use the same dynamic library, does it get loaded more than once? Or is it shared in memory?

A: That depends on the OS. Some OS will load separate copies for each process. Others load shared copies to save memory.



Q: Are dynamic libraries the best way of configuring an application?

A: Usually, it's simpler to use configuration files. But if you're going to connect to some external device, you'd normally need separate dynamic libraries to act as drivers.

In C, both `strstr` and `strchr` are functions used to search within strings, but they have different purposes and behaviors:

### `strstr`

- **Prototype**: `char *strstr(const char *haystack, const char *needle);`
- **Purpose**: Finds the first occurrence of the substring `needle` in the string `haystack`.
- **Returns**: A pointer to the beginning of the found substring in `haystack`, or `NULL` if the substring is not found.

**Example**:

```C
#include <stdio.h>
#include <string.h>

int main() {
    const char *haystack = "hello world";
    const char *needle = "world";
    char *result = strstr(haystack, needle);

    if (result) {
        printf("Found: %s\n", result);  // Output: "Found: world"
    } else {
        printf("Not found.\n");
    }

    return 0;
}
```

### `strchr`

- **Prototype**: `char *strchr(const char *str, int c);`
- **Purpose**: Finds the first occurrence of the character `c` in the string `str`.
- **Returns**: A pointer to the first occurrence of the character in the string, or `NULL` if the character is not found.

**Example**:

```C
#include <stdio.h>
#include <string.h>

int main() {
    const char *str = "hello world";
    char ch = 'o';
    char *result = strchr(str, ch);

    if (result) {
        printf("Found: %s\n", result);  // Output: "Found: o world"
    } else {
        printf("Not found.\n");
    }

    return 0;
}
```

### Key Differences

1. **Search Type**:
   - `strstr`: Searches for a substring.
   - `strchr`: Searches for a single character.
2. **Return Value**:
   - `strstr`: Returns a pointer to the beginning of the found substring or `NULL` if the substring is not found.
   - `strchr`: Returns a pointer to the first occurrence of the character or `NULL` if the character is not found.
3. **Input Parameters**:
   - `strstr`: Takes two `const char *` parameters (strings).
   - `strchr`: Takes a `const char *` (string) and an `int` (character, which is internally converted to `char`).
4. **Common Use Cases**:
   - `strstr`: Used when you need to find a specific sequence of characters (substring) within another string.
   - `strchr`: Used when you need to find the position of a specific character within a string.

Both functions are part of the C standard library and can be found in the `<string.h>` header.



### Static vs. Dynamic Libraries in C

In C programming, libraries are used to share code across multiple programs. There are two primary types of libraries: static libraries and dynamic libraries. Here’s a detailed comparison of the two:

### Static Libraries

1. **Definition**:
   - A static library is a collection of object files that are linked into the program at compile-time. The code from the static library becomes part of the executable.
2. **File Extension**:
   - Typically has a `.a` extension on Unix/Linux (e.g., `libmylib.a`).
3. **Linking**:
   - Linked at compile-time. The linker copies the code from the static library into the executable.
4. **Advantages**:
   - **Portability**: The executable is self-contained and can run on any system without needing the library files.
   - **Performance**: Since all code is included in the executable, there’s no need to load external libraries at runtime.
5. **Disadvantages**:
   - **Size**: Executables can become very large because they include all the library code.
   - **Updates**: If a library is updated, each program that uses it must be recompiled.

### Dynamic Libraries

1. **Definition**:
   - A dynamic library (also known as a shared library) is a collection of object files that are linked into the program at runtime. The library code is not included in the executable but is loaded as needed.
2. **File Extension**:
   - Typically has a `.so` extension on Unix/Linux (e.g., `libmylib.so`).
3. **Linking**:
   - Linked at runtime. The operating system loads the dynamic library when the executable is run.
4. **Advantages**:
   - **Smaller Executables**: The executable doesn’t include the library code, so it’s smaller.
   - **Easy Updates**: Updating the library does not require recompiling the executables that use it.
5. **Disadvantages**:
   - **Dependency Management**: The correct version of the dynamic library must be present on the system.
   - **Performance**: Loading the library at runtime can introduce some overhead.

### Creating a Static Library in C

To create a static library, you need one or more object files. Here’s how you can create and use a static library:

1. **Compile Source Files to Object Files**:

   ```bash
   gcc -c file1.c
   gcc -c file2.c
   ```

2. **Create the Static Library**:

   ```bash
   ar rcs libmylib.a file1.o file2.o
   ```

3. **Link the Static Library to Your Program**:

   ```bash
   gcc -o myprogram main.c -L. -lmylib
   ```

In this example:

- `file1.c` and `file2.c` are compiled into object files `file1.o` and `file2.o`.
- These object files are archived into a static library `libmylib.a` using the `ar` command.
- The program `main.c` is compiled and linked with the static library `libmylib.a`.

### Summary

- **Static Libraries**: Linked at compile-time, larger executable size, easier to manage for distribution, but harder to update.
- **Dynamic Libraries**: Linked at runtime, smaller executable size, easier to update, but requires careful management of dependencies.





---



### Chapter 9 - processes and system calls

#### System calls are your hotline to the OS

- C programs rely on the OS for pretty much everything.

- They make **system calls** if they want to talk to the hardware. 

- System calls are just functions that live inside the OS's kernel.

- Most of the code in the C Standard Library depends on them. 

- Whenever you call `printf()` to display something on the command line, somewhere at the back of things, a system call is made to the OS to send the string of text to the screen.

- `system()` takes a single string parameter and executes it as if we had typed it on the command line:

  ```C
  system("dir D:");
  // This will print out the contents of the D: drive.
  
  system("gedit");
  // This will launch an editor on Linux
  
  system("say 'End of line'");
  // This will read to you on the Mac.
  
  ```

- The `system()` function is an easy way of running other programs from our code --- particularly if we are creating a quick prototype and we'd sooner call external programs rather than write lots and lots of C code.



#### `sprintf()`

In C, `sprintf` is a function used to format and store a series of characters and values into a string buffer. It is part of the standard input/output library and is declared in the `<stdio.h>` header file.

### Prototype

```C
int sprintf(char *str, const char *format, ...);
```

### Parameters

- **`char \*str`**: A pointer to the buffer where the resulting C-string is stored.
- **`const char \*format`**: A format string that specifies how to format the output. It can contain plain characters and format specifiers (which begin with `%`).
- **`...`**: An ellipsis (`...`) indicates a variable number of arguments that are formatted according to the format string.

### Return Value

- The function returns the number of characters written to the buffer (excluding the null-terminating character) on success.
- If an encoding error occurs, a negative value is returned.

### Usage Example

Here’s a simple example to illustrate how `sprintf` works:

```C
#include <stdio.h>

int main() {
    char buffer[100];
    int age = 25;
    float height = 175.5;
    const char *name = "John Doe";

    // Using sprintf to format and store the string in buffer
    sprintf(buffer, "Name: %s, Age: %d, Height: %.1f cm", name, age, height);

    // Print the formatted string stored in buffer
    printf("%s\n", buffer);

    return 0;
}
```

### Output

```bash
Name: John Doe, Age: 25, Height: 175.5 cm
```

### Format Specifiers

Here are some commonly used format specifiers:

- **`%d`** or **`%i`**: Signed decimal integer.
- **`%u`**: Unsigned decimal integer.
- **`%f`**: Decimal floating point.
- **`%s`**: String of characters.
- **`%c`**: Single character.
- **`%x`** or **`%X`**: Unsigned hexadecimal integer.
- **`%o`**: Unsigned octal integer.
- **`%p`**: Pointer address.
- **`%%`**: A literal percent sign.

### Considerations

1. **Buffer Size**:
   - Ensure that the buffer `str` is large enough to hold the resulting string, including the null-terminating character.
2. **Security**:
   - Be cautious with `sprintf` as it can lead to buffer overflows if the buffer is not large enough to accommodate the formatted string. Consider using `snprintf`, which allows you to specify the maximum number of characters to write to the buffer, providing additional safety against buffer overflows.

### Example with `snprintf`

Here’s how you might use `snprintf` to avoid buffer overflows:

```C
#include <stdio.h>

int main() {
    char buffer[100];
    int age = 25;
    float height = 175.5;
    const char *name = "John Doe";

    // Using snprintf to safely format and store the string in buffer
    snprintf(buffer, sizeof(buffer), "Name: %s, Age: %d, Height: %.1f cm", name, age, height);

    // Print the formatted string stored in buffer
    printf("%s\n", buffer);

    return 0;
}
```

This ensures that the formatted string does not exceed the size of the buffer, thus preventing buffer overflow.



### `localtime`

**Prototype**:

```C
struct tm *localtime(const time_t *timep);
```

**Function**:

- **Purpose**: Converts a `time_t` value (which represents calendar time) to a `struct tm` representing local time.
- **Parameters**: Takes a pointer to a `time_t` object.
- **Return Value**: Returns a pointer to a `struct tm` that represents the local time.

**Details**:

- The `time_t` type is typically used to store the number of seconds since the Epoch (00:00:00 UTC on 1 January 1970).
- The `struct tm` structure holds the time broken down into its components (e.g., year, month, day, hour, minute, second).
- `localtime` adjusts the time for the local time zone and Daylight Saving Time if applicable.

### Example Usage of `localtime`:

```C
#include <stdio.h>
#include <time.h>

int main() {
    time_t t;
    time(&t); // Get the current calendar time
    struct tm *local = localtime(&t); // Convert to local time

    printf("Local time: %s", asctime(local));
    return 0;
}
```

### `asctime`

**Prototype**:

```C
char *asctime(const struct tm *tm);
```

**Function**:

- **Purpose**: Converts a `struct tm` representing a broken-down time to a human-readable string.
- **Parameters**: Takes a pointer to a `struct tm` object.
- **Return Value**: Returns a pointer to a statically allocated string representing the time in the form: "Www Mmm dd hh:mm yyyy\n".

**Details**:

- The `asctime` function formats the `struct tm` into a fixed 26-character string.
- This string includes the day of the week, month, day of the month, hour, minute, second, and year.

### Example Usage of `asctime`:

```C
#include <stdio.h>
#include <time.h>

int main() {
    time_t t;
    time(&t); // Get the current calendar time
    struct tm *local = localtime(&t); // Convert to local time

    char *time_str = asctime(local); // Convert to string
    printf("Formatted local time: %s", time_str);
    return 0;
}
```

### Combined Example

Here is how both functions work together in our code:

```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

char *now() {
    time_t t;
    time(&t); // Get the current calendar time
    return asctime(localtime(&t)); // Convert to local time and then to string
}

int main() {
    char comment[80];
    char cmd[120];

    fgets(comment, 80, stdin); // Read a comment from the user
    sprintf(cmd, "echo '%s %s' >> reports.log", comment, now()); // Format the command string
    system(cmd); // Execute the command to append to reports.log

    return 0;
}
```

### Summary:

- **`localtime`**: Converts calendar time (`time_t`) to local time (`struct tm`).
- **`asctime`**: Converts local time (`struct tm`) to a human-readable string.



#### Downside to the `system()` function

It's quick and easy tot use but it's also kind of sloppy.

The code worked by stitching together a string containing a command like this:

```C
echo '<comment> <timestamp>' >> reports.log
```



But what if someone entered a comment like this?

```C
echo '&& ls / && echo <timestamp>' >> reports.log
```



By injecting some command-line code into the text, one can make the program run whatever code he likes:



```bash
chan@CMA:~/C_Programming/HFC/chapter_9/exercise_1
$ ./main
'&& ls / && echo' 

bin    dev   lib    libx32	mnt   root  snap      sys  var
boot   etc   lib32  lost+found	opt   run   srv       tmp
cdrom  home  lib64  media	proc  sbin  swapfile  usr
sh: 3: echo
 Sun Jun 16 21:49:14 2024
: not found

```



#### Security's not the only problem

This example injects a piece of code to list the contents of the root directory, but it could have deleted files or launched a virus. But we shouldn't just worry about security:

- What if the comments contain apostrophes? 

  This might break the quotes in the command.

- What if the PATH variable causes the `system()` function to call the wrong program?

- What if the program we're calling needs to have a specific set of environment variables set up first?



The `system()` function is easy to use, but most of the time, we're going to need something more structured --- some way of calling a specific program, with a set of command-line arguments and maybe even some environment variables.

**System calls are the functions that our program uses to talk to the kernel.**

**Note - detailed explanations of the kernel are in the `essentials.md` file inside the CS folder.**



#### The `exec()` functions give you more control

- When we call the `system()` function, the OS has to interpret the command string and decide which programs to run and how to run them.
- And that's where the problem is: the OS needs to interpret the string and we've already seen how easy it is to get that wrong.
- The solution is to remove the **ambiguity** and tell the OS precisely which program we want to run. That's what the the `exec()` functions are for.



#### `exec()` functions replace the current process

- A process is just a program running in memory.

- If we type **taskmgr** on Windows or `ps -ef` on most other machines, we'll see the processes running on our system.

- The OS tracks each process with a number called the **process identifier (PID).**

  ```bash
  chan@CMA:~$ ps -ef
  UID          PID    PPID  C STIME TTY          TIME CMD
  root           1       0  0 14:18 ?        00:00:01 /sbin/init splash
  root           2       0  0 14:18 ?        00:00:00 [kthreadd]
  ....
  ```

​	![](/home/chan/Pictures/Screenshots/Screenshot from 2024-06-17 17-47-29.png)

- The `exec()` functions **replace the current process** by running some other program.
- We can say which *command-line-arguments* or *environment variables* to use, and when the new program starts, it will have exactly the same *PID* as the old one.
- It's like a relay race, where your program hands over its process to the new program.
- By definition, the `exec` functions replace the current process image with a new process image. This means the process ID remains the same, but the code, data and stack are replaced by those of the new program.

#### `exec` family of functions

- `execl`
- `execle`
- `execlp`
- `execv`
- `execve`
- `execvp`

Two commonly used functions are `execv` and `execvp`.

#### Two groups of `exec()` functions:

- The list functions
- The array functions
- The `exec()` functions are in `unistd.h`.



#### The list functions: `execl()`, `execlp()`, `execle()`

- The list functions accept command-line arguments as a list of parameters like this:

- **The program**

  This might be the full pathname of the program -- `execl() / execle()` -- or just a command name to search for --- `execlp()` --- but the first parameter tells the `exec()` function what program it will run.

- **The command-line arguments**

  We need to list one by one the command-line arguments we want to use. The first command-line argument is always the name of the program. That means the first two parameters passed to a list version of `exec()` should always be the same string.

- **NULL**

  After the last command-line argument, we need a **NULL**. This tells the function that there are no more arguments.

- **Environment variables (maybe)**

  If we call an `exec()` function whose name ends with `...e()`, we can also pass an array of environment variables. This is just an array of strings like `"POWER=4"`, `"SPEED=17"`, `"PORT=OPEN"`, ...

```C
execl("/home/flynn/clu", "/home/flynn/clu", "paranoids", "contract", NULL);
// execl = a LIST of arguments
// all the parameters except the first one are the arguments.
// The second parameter should be the same as the first.
// We should end the list with NULL.


execlp("clu", "clu", "paranoids", "contract", NULL);
// execLP = a LIST of arguments + search on the PATH.


execle("/home/flynn/clu", "/home/flynn/clu", "paranoids", "contract", NULL, env_vars);

// execlE = a LIST of arguments + ENVIRONMENT variables.
// env_vars = an array of strings containing environment variables.
```

#### The array functions: `execv()`, `execvp()`, `execve()`

If we already have our command-line arguments stored in an array, we might find these two versions easier to use:

```C
execv("/home/flynn/clu", my_args);

// execV = an array or VECTOR of arguments.

execvp("clu", my_args);

// execVP = an array/VECTOR of arguments + search on the PATH.

// my_args = The arguments need to stored in the my_args string array.
```

The only difference between these two functions is that `execvp` will search for the program using the PATH variable.

```C
int execv(const char *path, char *const argv[]);

```

- `path`: The path to the executable file.
- `argv` : An array of argument strings passed to the new program. The last element of this array must be NULL.

`execvp` is similar to `execv` but it searches for the executable in the directories listed in the `PATH` environment variable.

#### How to remember the `exec()` functions

`execl` - the list functions

`execv`- the array functions

We can see `l` and `v` at the end of `exec` where `l` means lists and `v` means vector (array). 

- Each `exec()` function can be followed by one or two characters that must be `l`, `v`, `p` or `e`. The characters tell us which feature we want to use.

```markdown
execle = exec + l + e = LIST of arguments + an ENVIRONMENT
```

- The `l` and `v` characters always come before `p` and `e` , and the `p` and `e` characters are optional.



| Uses                 | Character |
| -------------------- | --------- |
| List of args         | `l`       |
| Array/vector of args | `v`       |
| Search the path      | `p`       |
| Environment vars     | `e`       |



exec --> l or v --> p or e

- All `exec()` functions begin with `exec`.
- `l` - take a list of arguments
- `v` - Take a vector/array of arguments
- `p` - Search for the program on the path.
- `e` - Use an array of environment strings.
- We don't have to include `p` or `e`.



#### Passing environment variables

- Every process has a set of environment variables. 
- These are the values we see when we type `set` or `env` on the command line and they usually tell the process useful information, such as where to find the commands.
- C programs can read environment variables with the `getenv()` system call.
- To use `exec()` functions, we need to include `unistd` first like `#include <unistd.h>`.

```C
char *my_env[] = {"JUICE=peach and apple", NULL};
```

- We can create a set of environment variables as an array of string pointers.
- Each variable in the environment is name=value.
- The last item in the array must be NULL.

```C
execle("diner_info", "diner_info", "4", NULL, my_env)
```



- The `execle()` function will set the command-line arguments and environment variables and then replace the current process with `diner_info`.

​	`diner_info.c`

```C
#include <unistd.h>
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char *argv[]){
    printf("Diners: %s\n", argv[1]);
    printf("Juice: %s\n", getenv("JUICE"));
    return 0;
}
```

- `getenv()` in `stdlib.h` lets us read environment variables.

#### But what if there's a problem?

- If there is a problem calling the program, the existing process will keep running.



#### Most system calls go wrong in the same way

- Because system calls depend on something outside our program, they might go wrong in some way that we can't control. To deal with this problem, most system calls go wrong in the same way.
- If an `exec()` call is successful, the current program stops running.
- So if the program runs anything after the call to `exec()`, there must have been a problem.

```C
execle("diner_info", "diner_info", "4", NULL, my_env);
puts("Dude - the diner_info code must be busted");
```

- If `execle()` worked, the `puts` line would never run.
- But just telling if a system call worked is not enough. We normally want to know why a system call failed.
- That's why most system calls follow the **golden rules of failure.**

#### The Golden Rules of Failure

- Tidy up as much as you can.
- Set the `errno` variable to an error value.
- Return -1.

The `errno` variable is a global variable that's defined in `errno.h` along with a whole bunch of standard error values, like:

```markd
EPERM= 1   Operation not permitted
ENOENT= 2  No such file or directory
ESRCH=3    No such process
EMULLET=81 Bad haircut // This value is not available on all systems.
```

Now we could check the value of `errno` against each of these values, or we could look up a standard piece of error text using a function in `string.h` called `strerror()`:

```C
puts(strerror(errno));

// strerror() converts an error number into a message.

```

So, if the system can't find the program we are running and it sets the `errno` variable to `ENOENT`, the above code will display this message:

```sh
No such file or directory
```



[`int argc`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) and `char *argv[]` are parameters to the [`main`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) function that represent the command-line arguments to the program.

- [`argc`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) (argument count) is an integer that specifies the number of arguments passed to the program from the command line, including the name of the program itself.
- [`argv`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) (argument vector) is an array of character pointers (strings). Each element in this array points to a string that represents each argument passed to the program. The first element, [`argv[0\]`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html), is the name of the program itself. The array is terminated by a `NULL` pointer.

We use a pointer to char (`char *`) for [`argv`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) because each command-line argument is a string, and in C, strings are represented as an array of characters. Since an array in C is essentially a constant pointer to its first element, we can represent an array of strings as an array of character pointers (`char *argv[]`).

Here's an example to illustrate this:

If you run your program like this:

```bash
./myprogram arg1 arg2 arg3
```

Then `argc` would be 4, and `argv` would be an array that looks like this:

```C
argv[0] = "./myprogram";

argv[1] = "arg1";

argv[2] = "arg2";

argv[3] = "arg3";

argv[4] = NULL;
```

So [`argv`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) is an array of pointers, where each pointer points to the first character of a string representing a command-line argument.

#### Summary

- The `exec` family of functions allows a process to replace its current image with a new one.
- The process ID remains unchanged, but the new program takes over, starting from its `main` function.
- If `exec` is successful, it does not return to the original program.
- If it fails, the original continues executing, allowing for error handling.



Q: Isn't `system()` just easier to use than `exec()`?

A: Yes. But because the OS needs to interpret the string we pass to `system()`, it can be a bit buggy. Particularly if we create the command string dynamically.



Q: Why are there so many `exec()` functions?

A: Over time, people wanted to create processes in different ways. The different versions of `exec()` were created for more flexibility.



Q: Do I always have to check the return value of a system call? Doesn't it make the program really long?

A: If we make system calls and don't check for errors, our code will be shorter. But it will probably also have more bugs. It is better to thank about errors when we first write code. It will make it much easier to catch bugs later on.



Q: If I call an `exec()` function, can I do anything afterward?

A: No. if the `exec()` function is successful,  it will change the process so that it runs the new program instead of our program. That means  the program containing the `exec()` call will stop as soon as it runs the `exec()` function.



#### Bullet Points - Chapter 9

- System calls are functions that live in the OS.
- When we make a system call, we are calling code outside our program.
- `system()` is a system call to run a command string.
- `system()` is easy to use but it can cause bugs.
- The `exec()` system calls let us run programs with more control.
- There are several versions of the `exec()` system call.
- System calls usually, but not always return -1 if there's a problem.
- They will also set the `errno` variable to an error number.



#### `exec()` is the end of the line for our program

- The `exec()` functions replace the current function by running a new program. 
- But what happens to the original program? It terminates immediately.
- But what if we want to start another process and keep our original process running?

#### `fork()` will clone our process

- `fork()` makes a complete copy of the current process.
- The brand-new copy will be running the same program, on the same line number.
- It will have exactly the same variables that contain exactly the same values.
- The only difference is that the copy process will have a different process identifier from the original.
- The original process is called the parent process and the newly created copy is called the child process.
- Return -1 for errors, 0 to the new process, and the process ID of the new process to the old process.

```C
#include <sys/types.h>
int main(int argc, char *argv[]){
    pid_t id = fork();
    printf("Hello world from id: %d\n", id);
}
```

​	Code Execution:

```sh
chan@CMA:~/C_Programming/test
$ ./final
Hello world from id: 12479 // parent
Hello world from id: 0 //Child 

```



#### Running a child process with `fork()` + `exec()`

The trick is to only call an `exec()~ function on a child process. That way, our original parent process will be able to continue running.

1. Make a copy
   - Begin by making a copy of our current process by calling the `fork()` system call.
   - The processes need some way of telling which of them is the parent process and which is the child.
   - So, the `fork()` function returns 0 to the child process and it will return **non-zero** value to the parent process.
2. If you're the child process, call `exec()`
   - At this point, we have two identical processes running, both of them using identical code, But the child process (the one that received a 0 from the `fork()` call) now needs to replace itself by calling `exec()`.



#### what the fork()?

```C
pid_t pid = fork();
```

- `fork()` will actually return an integer value that is 0 for the child process and positive for the parent process.
- The parent process will receive the process identifier of the child proces.
- But what is `pid_t`? 
- Different OS use different kinds of integers to store process IDs: some might use `short`s and some might use `int`s. So `pid_t` is always set to the type that the OS uses.
- To use `pid_t`, we have to include `#include <sys/types.h>` header file.
- On Ubuntu Linux, and generally in GNU systems, `__pid_t` is an internal type definition used in system headers. The double underscore (__) prefix indicates that it is intended for internal use by the implementation and is not meant to be used directly by application code. Instead, we should use the standard `pid_t` type.



```C
#include <sys/types.h>
int main(int argc, char *argv[]){
    fork();
    fork();
    printf("Hello World.\n");
    return 0;
}
```

- If we call `fork()` two times, the result will be outputted 4 times because both the parent and child will create another child processes.
- The number of fork() calls = 2**n (if we call 4 times, then we get 16.)

Code Execution

```sh
chan@CMA:~/C_Programming/test
$ ./final
Hello world.
Hello world.
Hello world.
Hello world.
```

But what if we only want to have 3 processes ? 

Well, we just need to use the id since fork() returns 0 to the new process (child), we can say we will only fork the new process from the main process.

```C
#include <sys/types.h>
int main(int argc, char *argv[]){
    pid_t id = fork();
    if(id != 0){
        fork();
    }
    print("Hello World.\n");
    return 0;
}
```

​	Code Execution:

```sh
chan@CMA:~/C_Programming/test$ ./final
Hello world.
Hello world.
Hello world.

```

Q: Does `system()` run programs in a separate process?

A: Yes. But `system()` gives us less control over exactly how the program runs.



Q: Isn't fork-ing processes really inefficient? I mean, it copies an entire process and a moment later we replace the child process by doing an `exec()`?

A: OS use lots of tricks to make `fork`-ing processes really quick. For example, the OS cheats and avoids making an actual copy of the parent process's data. Instead the child and parent processes share the same data.



Q: But what if one of the processes changes some data in memory? Won't that screw things up?

A: It would, but the OS will catch that a piece of memory is going to change and then it will make a separate copy of that piece of memory for the child process.



Q: Why doesn't Windows support the `fork()` system call?

A: Windows manages processes very differently from other OS, and the kinds of tricks `fork()` needs to do in order to work efficiently are very hard to do on Windows. This may be why there isn't a version of `fork()` built in. Cygwin lets `fork()` but it is slower.

Q: Won't the output of the various feeds get mixed up?

A: The OS will make sure that each string is printed completely.



Q: Is a `pid_t` just an int?

A: It depends on the platform. The only thing we know is that it will be some integer type.



Q: I stored the result of a `fork()` call in an `int` and it worked just fine.

A: It's best to always use `pid_t` to store process IDs. If we don't, we might cause problems with other system calls or if our code is compiled on another machine. While `pid_t` is often an alias for `int` on many systems, using `int` directly can reduce the portability and clarity of your code. Different systems might have different underlying representations for process IDs, and relying on `pid_t` ensures that our code adapts to these differences. For getting the return value of `fork()`, you should use `pid_t` rather than `int`. This ensures that our code is portable and correctly represents process IDs according to the POSIX standard.



#### Bullet Points - Chapter - 9

- `system()` will run a string like a console command.

- System calls are functions that live in the kernel.

- The `exec()` functions give us more control than `system()`.

- The `exec()` functions replace the current process.

- The `fork()` function duplicates the current process.

- System calls usually return -1 if they fail.

- Failed system calls set the `errno` variable to the error number.

- ```markdown
  execl() = list of args
  execle() = list of args + environment
  execlp() = list of args + search on path
  execv() = array of args
  execve() = array of args + environment
  execvp() = array of args + search on path
  ```

- `fork()` + `exec()` creates a child process.

### End of Chapter 9

---

### Chapter 10 - Interprocess communication

Interprocess communication lets processes work together to get the job done.

- Redirecting input and output, and making processes wait for each other, are all simple forms of **interprocess communication**.
- When processes are able to cooperate by sharing data or by waiting for each other, they become much more powerful.

- The Standard Output is one of the three default **data streams**.
- A data stream is exactly what it sounds like: a stream of data that goes into, or comes out of, a process.
- There are data streams for the Standard Input, Output, and Error, and there are also data streams for other things, like files or network connections.



#### A look inside a typical process

- Every process will contain the program it's running, as well as space for stack and heap data.
- But it will also need somewhere to record where data streams like the Standard Output are connected. 
- Each data stream is represented by a file descriptor, which, under the surface, is just a number.
- A file descriptor is a number that represents a data stream.
- The process keeps everything straight by storing the file descriptors and their data streams in a **descriptor table**.

![](/home/chan/Pictures/Screenshots/Screenshot from 2024-06-27 16-26-49.png)

- The descriptor table has one column for each of the file descriptor numbers.
- Even though these are called file descriptors, they might not be connected to an actual file on the hard disk.
- The first three slots in the table are always the same. Slot 0 is the Standard Input, slot 1 is the Standard Output and slot  2 is the Standard Error.
- The other slots in the table are either empty or connected to data streams that the process has opened.
- For example, every time our code opens a file for reading or writing, another slot is filled in the descriptor table.
- File descriptors don't necessarily refer to files.
- When the process is created, the Standard Input is connected to the keyboard, and the Standard Output and Error are connected to the screen. And they will stay connected that way until something redirects them somewhere else.



#### Redirection just replaces data streams

The Standard Input, Output and Error are always fixed in the same places in the descriptor table. But the data streams they point to can change.

That means, if we want to redirect the Standard Output, we just need to switch the data stream against descriptor 1 in the table.

| #    | Data Stream                     |
| ---- | ------------------------------- |
| 0    | The keyboard                    |
| 1    | ~~The screen~~ File stories.txt |
| 2    | The screen                      |
| 3    | Database connection             |

All of the functions, like `printf()`, that send data to the Standard Output will first look in the descriptor table to see where descriptor 1 is pointing. They will then write data out to the correct data stream.

#### So, that's why it's 2>

- We can redirect the Standard Output and Standard Error on the command line using the > and 2> operators.

  ```sh
  ./myprog > output.txt 2> errors.log
  ```

- The `2` refers to the number of the Standard Error in the descriptor table.

- On most OS, we can use `1>` as an alternative way of redirecting the Standard Output, and on Unix-based systems, we can even redirect the Standard Error to the same place as the Standard Output.

  ```sh
  ./myprog 2>&1
  ```

  2> means "redirect Standard Error". &1 means "to the Standard Input"

- So, `./myprog 2>&1` means `"run myprog and redirect its error output (stderr) to the same place as its standard output (stdout)"`. This is useful when you want to capture all output from a program (both normal and error output) in the same place, such as when piping the output to another command or saving it to a file.

#### Processes can redirect themselves

- Every time we've used redirection so far, it's been from the command line using the > and < operators. 
- But processes can their own redirection by **rewiring the descriptor table**.



#### `fileno()` tells you the descriptor

- Every time we open a file, the OS registers a new item in the descriptor table.

- Let's say we open a file with something like this:

  ```C
  FILE *my_file = fopen("guitar.mp3", "r");
  ```

- The OS will open the `guitar.mp3` file and return a pointer to it, but it will also skim through the descriptor table until it finds an empty slot and register the new file there.

- Once we've got a file pointer, we find it in the descriptor table by calling the `fileno()` function.

  | #    | Data stream         |
  | ---- | ------------------- |
  | 0    | The keyboard        |
  | 1    | The screen          |
  | 2    | The screen          |
  | 3    | Database connection |
  | 4    | File guitar.mp3     |

  

  ```C
  int descriptor = fileno(my_file);
  
  // This will return the value 4.
  ```

- `fileno()` is one of the few system functions that doesn't return `-1` it it fails.

- As long as we pass `fileno()` the pointer to an open file, it should always return **the descriptor number**.



#### `dup2()` duplicates data streams

Opening a file will fill a slot in the descriptor table, but what if we want to change the data stream already registered against a descriptor?

What if we want to change the file descriptor 3 to point to a different data stream?

- We can do it with the `dup2()` funciton.

- `dup2()` duplicates a data stream from one slot to another.

- So, if we have a file pointer to `guitar.mp3` plugged in to file descriptor 4, the following code will connect it to the file descriptor 3 as well.

  ```C
  dup2(4,3);
  ```

  | #    | data stream                             |
  | ---- | --------------------------------------- |
  | 0    | The keyboard                            |
  | 1    | The screen                              |
  | 2    | The screen                              |
  | 3    | ~~Database connection~~ File guitar.mp3 |
  | 4    | File guitar.mp3                         |

- There's still just one `guitar.mp3` file, and there's still just one data stream connected to it.

- But the data stream(the `FILE*`) is now registered with file descriptors 3 and 4.



#### Removing duplicated code block

```C
pid_t pid = fork();
if(pid == -1){
    fprintf(stderr, "Can't fork process: %s\n", strerror(errno));
    return 1;
}

if(execle(...) == -1){
    fprintf(stderr, "Can't run script: %s\n", strerror(errno));
    return 1;
}
```

- Duplicated code can be the cause of unwarranted coding stress.
- By creating a simple fire-and-forget error() function, we'll make our duplicated code a thing of the past.
- The `exit()` system call is the fastest way to stop our program in its tracks.
- No more worrying about returning to main(), just call exit() and our program's history!
- First, we remove all of our error code into **a separate function** called error() and replace that tricky return with a call to `exit()`.
- To ensure we have the exit system call available, we need to include `stdlib.h`.

```C
void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(1);
}
// exit(1) will terminate our program with status 1 immediately.
```

```C
pid_t pid = fork();
if(pid == -1){
    error("Can't fork process");
}

if(execle(...) == -1){
    error("Can't run script");
}
```

**Warning: offer limited to one `exit()` call per program execution. Do not operate `exit()` if you have a fear of sudden program termination.**



#### Sometimes we need to wait...

- The `newshound` program in `HFCExercises.md` fires off a separate process to run the `rssgossip.py` script.
- But once that child process gets created, it's **independent** of its parent.
- We could run the `newshound` program and still have an empty `stories.txt`., just because the `rssgossip.py` isn't finished yet.
- That means the OS has to give us some way of waiting for the child process to complete.

#### The `waitpid()` function

- The `waitpid()` function won't return until the child process dies.

- That means we can add a little code to our program so that it won't exit until the `rssgossip.py` script has stopped running.

  ```C
  #include <sys/wait.h>
  ```

  We need to include the `sys/wait.h` header.

```C
int pid_status;
if(waitpid(pid, &pid_status, 0) == -1){
    error("Error waiting for child process");
}
return 0;

// pid - The process ID
// &pid_status - (A pointer to an int) This variable is used to store information about the process.
// 0 - We can add options here.
```

`waitpid()` takes three parameters:

```C
waitpid(pid, pid_status, options)
```

- **pid** - This is the process ID that the parent process was given when it forked the child.
- **pid_status**- This will store exit information about the process, `waitpid()` will update it, so it needs  to be a pointer.
- **options** - There are several options we can pass to `waitpid()`, and typing `man waitpid` will give us more info. If we set the options to `0`, the function waits until the process finishes.

#### What's the status?

- When the `waitpid()` function has finished waiting, it stores a value in `pid_status` that tells us how the process did.

- To find the exit status of the child process, we'll have to pass the `pid_status` value through a macro called `WEXITSTATUS()`.

- ```C
  if(WEXITSTATUS(pid_status)){ // if the exit status is not zero
      puts("Error status non-zero");
  }
  ```

- The reason we need the macro is because the `pid_status` contains several pieces of information, and only the first 8 bits represent the exit status.

- The macro tells us the value of just those 8 bits.

- Adding a `waitpid()` to the program was easy to do and it made the program more reliable.

- Before, we couldn't be sure that the subprocess had finished writing, and that meant there was no way we could use the `newshound` program as a proper tool.

- We couldn't use it in scripts and we couldn't create a GUI frontend for it.

- Redirecting input and output, and making processes wait for each other, are all simple forms of **interprocess communication**.

- When processes are able to cooperate by sharing data or by waiting for each other, they become much more powerful.

- `exit()` is a quick way of ending a program.

- All open files are recorded in the descriptor table.

- We can redirect input and output by changing the descriptor table.

- `fileno()` will find a descriptor in the table.

- `dup2()` can be used to change the descriptor table.

- `waitpid()` will wait for processes to finish.

Q: Does `exit()` end the program faster than just returning from `main()`?

A: No, but if we call `exit()`, we don't need to structure our code to get back to the `main()` function. As soon as we call `exit()`, our program is dead.



Q: Should I check for -1 when I call `exit()`, in case it doesn't work?

A: No. `exit()` doesn't return a value, because `exit()` never fails. `exit()` is the only function that is guaranteed never to return a value and never to fail.



Q: Is the number I pass to `exit()` the exit status?

A: Yes.



Q: Are the Standard Input, Output, and Error always in slots 0, 1, and 2 of the descriptor table?

A: Yes, they are.



Q: So, if I open a new file, it is automatically added to the descriptor table?

A: Yes



Q: Is there a rule about which slot it gets?

A: New files are always added to the available slot with the lowest number. So, if slot number 4 is the first available one, that's the one our new file will use.



Q: How big is the descriptor table?

A: It has slots from 0 to 255.



Q: The descriptor table seems kinda complicated, Why is it there?

A: Because it allows us to rewire the way a program works. Without the descriptor table, redirection isn't possible.



Q: Is there a way of sending data to the screen without using the Standard Output?

A: On some systems. For example, on Unix-based machines, if we open `/dev/tty`, it will send data directly to the terminal.



Q: Can I use `waitpid()` to wait for any process? Or just the processes I started?

A: We can use `waitpid()` to wait for any proceess.



Q: Why isn't the `pid_status` in `waitpid(..., &pid_status, ...)` just an exit status?

A: Because the `pid_status` contains other information such as `WIFSIGNALED(pid_status)` will be false if a process ended naturally, or true if something killed it off.



Q: How can an integer variable like `pid_status` contain several pieces of information?

A: It stores different things in different bits. The first 8 bits store the exit status. The other information is stored in the other bits.



Q: So, if I can extract the first 8 bits of the `pid_status` value, I don't have to use `WEXITSTATUS()`?

A: It's always best to use `WEXITSTATUS()`. It's easier to read and it will work on whatever the native `int` size is on the platform.



Q: Why is `WEXITSTATUS()` in uppercase?

A: Because it's a macro rather than a function. The compiler replaces macro statements with small pieces of code at runtime.



#### Connect the processes with pipes

- Pipes are used on the command line to connect the **output** of one process with the **input** of another process.
- The `grep` filters the output of the script.



#### Piped commands are parents and children

- Whenever we pipe commands together on the command line, we are actually connecting them together as parent and child processes.

![](/home/chan/Pictures/Screenshots/Screenshot from 2024-07-02 17-13-08.png)

- Pipes are used a lot on the command line to allow users to connect processes together.

#### Pipe() opens two data streams

- Because the child is going to send data to the parent, we need a pipe that's connected to the Standard Output of the child and the Standard Input of the parent.
- We can create the pipe using the `pipe()` function.
- `pipe()` function creates two connected streams and adds them to the descriptor table.
- Whatever is written into one stream can be read  from the other.

| #    | DAta stream                    |
| ---- | ------------------------------ |
| 0    | Standard Input                 |
| 1    | Standard Output                |
| 2    | Standard Error                 |
| 3    | Read-end of the pipe // fd[0]  |
| 4    | Write-end of the pipe // fd[1] |

- Calling `pipe()` creates these two descriptors.

- When `pipe()` creates the two lines in the descriptor table, it will store their file descriptors in a two-element array:

  ```C
  int fd[2]; // The descriptors will be stored in this array
  
  // We pass the name of the array to the pipe() function
  if(pipe(fd) == -1){
      error("Can't create the pipe");
  }
  ```

- The `pipe()` command creates a pipe and tells us two descriptors: `fd[1]` is the descriptor that **writes** to the pipe and `fd[0]` is the descriptor that **reads** from the pipe.

- `fd[1]` writes to the pipe; `fd[0]` reads from it.

- Once we've got the descriptors, we'll need to use them in the parent and child processes.



#### In the child

- In the child process, we need to **close** the `fd[0]` end of the pipe and then change the child process's Standard Output to point to the same stream as descriptor `fd[1]`.

```C
// The child won't read from the pipe.
close(fd[0]); // This will close the read end of the pipe.

dup2(fd[1], 1); // The child then connects the write end to the Standard Output.

```

| #    | DATA STREAM                               |
| ---- | ----------------------------------------- |
| 0    | Standard Input                            |
| 1    | ~~Standard output~~ Write-end of the pipe |
| 2    | Standard Error                            |
| 3    | ~~Read-end of the pipe~~                  |
| 4    | Write-end of the pipe                     |

- Slot 3 is `fd[0]`, the read end of the pipe.
- Slot 4 is `fd[1]`, the write end of the pipe.
- The child won't read from the pipe but will write.



#### In the parent

- In the parent process, we need to close the `fd[1]` end of the pipe because we won't be writing to it.
- And then redirect the parent process's Standard Input to read its data from the same place as descriptor `fd[0]`.

```C
dup2(fd[0], 0); // fd[0] is the read end of the pipe

close(fd[1]); // This will close the write end of the pipe.
```

| #    | DATA STREAM                             |
| ---- | --------------------------------------- |
| 0    | ~~Standard Input~~ Read-end of the pipe |
| 1    | Standard Output                         |
| 2    | Standard Error                          |
| 3    | Read-end of the pipe                    |
| 4    | ~~Write-end of the pipe~~               |

- The parent will read from the pipe, but won't write.
- Everything that the child writes to the pipe will be read through the Standard Input of the parent process.



Q: Is a pipe a file?

A: It's up to the OS how it creates pipes, but pipes created with the `pipe()` function are not normally files.



Q: So pipes might be files?

A: It's possible to create pipes based on files, which are normally called named pipes or FIFO(first-in/first-out) files.



Q: Why would anyone want a pipe that uses a file?

A: Pipes based on files have names. That means they are useful if two processes need to talk to each other and they are not parent and child processes. As long as both processes know the name of the pipe, they can talk with it.



Q: Great! So how do I use named pipes?

A: Using the `mkfifo()` system call. 



Q: If most pipes are not files, what are they?

A: Usually, they are just pieces of memory. Data is written at one point and read at another.



Q: What happens if I try to read from a pipe and there's nothing in there?

A: Our program will wait until something is there.



Q: How does the parent know when the child is finished?

A: When the child process dies, the pipe is closed and the `fgets()` command receives an end-of-file, which means the `fgets()` function returns 0 and the loop ends.



Q: Can parents speak to children?

A: Absolutely. There's no reason why we can't connect our pipes the other way around, so that the parent sends data to the child process.



Q: Can you have a pipe that works in both directions at once? That way, my parent and child processes could have a two-way conversation.

A: No, we can't do that. Pipes always work in only one direction. But we can create two pipes: one from the parent to the child, and one from the child to the parent.



#### Bullet Points

- Parent and child processes can communicate using pipes.
- The `pipe()` function creates a pipe and two descriptors.
- The descriptors are for the read and write ends of the pipe.
- You can redirect Standard Input and Output to the pipe.
- The parent and child processes use different ends of the pipe.



#### The OS controls our program with signals

- When we call the `fgets()` function, the OS reads data from the keyboard, and when it sees the user hit **CTRL-C**, sends an interrupt signal to the program.
- The process runs its default interrupt handler and calls `exit()`.
- A signal is just a short message: a single integer value.
- When the signal arrives, the process has to stop whatever it's doing and go deal with the signal.
- The process looks at a table of signal mappings that link each signal with a function called the **signal handler**.
- The default signal handler for the interrupt signal just calls the `exit()` function.
- So, why doesn't the OS just kill the program? Because the signal table lets us run our **own code** when our process receives a signal.

| Signal | Handler     |
| ------ | ----------- |
| SIGURG | Do nothing  |
| SIGINT | Call exit() |

- `SIGINT` is the interrupt signal and has the value 2.



#### Catching signals and running our own code

Sometimes we'll want to run our own code if someone interrupts our program.

For example, if our process has files or network connections open, it might want to close things down and tidy up before exiting.

We can do it with `sigaction`s.

#### A `sigaction` is a function wrapper

- A `sigaction` is a `struct` that contains a pointer to a function.
- `sigaction`s are used to tell the OS which function it should call when a signal is sent to a process.
- So, if we have a function called `diediedie()` that we want the OS to call if someone sends an interrupt signal to our process, we'll need to wrap the `diediedie()` function up as a `sigaction`.

```C
// Create a new action.
struct sigaction action;

// diediedie is the name of the function we want the computer to call.
// The function that the sigaction wraps is called a handler.
action.sa_handler = diediedie;

// The mask is a way of filtering the signals that the sigaction will handle. We'll want to use an empty mask, like here.
sigemptyset(&action.sa_mask);

// some additional flags. We can just set them to zero.
action.sa_flags = 0;


```

- The function wrapped by a `sigaction` is called the **handler** because it will be used to deal with or handle a signal that's sent to it.
- If we want to create a handler , it will need to be written in a certain way.

#### All handlers take signal arguments

- Signals are just integer values, and if we create a custom handler function, it will need to accept an `int` argument like this:

```C
void diediedie(int sig){
    puts("Goodbye cruel world...\n");
    exit(1);
}

// int sig - the signal number the handler has caught.
```

- Because the handler is passed the number of the signal, we can reuse the same handler for several signals.
- Or , we can have a separate handler for each signal.
- Handlers are intended to be short, fast pieces of code.



#### `sigactions` are registered with `sigaction()`

- Once we have created a `sigaction`, we will need to tell the OS about it. We can do it with `sigaction()` function:

```C
sigaction(signal_no, &new_action, &old_action);
```

- `sigaction()` takes three parameters:
  - **The signal number**: The integer value of the signal we want to handle. Usually, we'll pass one of the standard signal symbols, like `SIGINT` or `SIGQUIT`.
  - **The new action**: This is the **address** of the new `sigaction` we want to register.
  - **The old action**: If we pass a pointer to another `sigaction`, it will be filled with details of the current handler that we're about to replace. If we don't care about the existing signal handler, we can set this to NULL.
- The `sigaction()` function will return -1 if it fails and will also set the `errno` function.

```C
// int sig - The signal number
// (*handler) - A pointer to the handler function

int catch_signal(int sig, void (*handler)(int)){
	// Create an action
    struct sigaction action;
    
    // set the action's handler to the handler function that was passsed in.
    action.sa_handler = handler;
    sigemptyset(&action.sa_mask);
    action.sa_flags = 0;
    
    
    // return the value of sigaction(), so we can check for errors.
    return sigaction (sig, &action, NULL);
}
```

- This function will allow us to set a signal handler by calling `catch_signal()` with a signal number and a function name:

```C
catch_signal(SIGINT, diediedie);
```

### Understanding SIGQUIT and Core Dump Files

**SIGQUIT** is a signal in Unix-like operating systems that is sent to a process to instruct it to quit and dump its core. It is typically initiated by the user by pressing `Ctrl+\`.

### What Happens When SIGQUIT is Sent?

1. **Process Interruption**:
   - When a process receives the `SIGQUIT` signal, it is expected to terminate. Unlike `SIGINT` (triggered by `Ctrl+C`), which politely asks a process to terminate, `SIGQUIT` also forces the process to produce a core dump before quitting.
2. **Core Dump File**:
   - A core dump (or core file) is a file that captures the memory of a running process at a specific point in time, usually when the process crashes. It contains:
     - The contents of the process's memory.
     - The state of the CPU registers.
     - The state of the process's stack and heap.
     - Information about the process's environment, open files, etc.
   - The core dump file is usually named `core` and is placed in the directory where the program was running, although this can vary depending on system configuration.

### Purpose of Core Dumps:

1. **Debugging**:
   - Core dumps are primarily used for debugging. They allow developers to analyze the state of a program at the moment it crashed, helping them to identify and fix bugs.
2. **Post-mortem Analysis**:
   - By loading a core dump into a debugger (like `gdb`), developers can inspect the state of the program, such as variable values, the call stack, and memory contents, to understand why the program terminated unexpectedly.

### Example Scenario:

Imagine you have a program that crashes due to a segmentation fault or some other unexpected error. If you run this program and it receives a `SIGQUIT`, the operating system will generate a core dump file before the program exits. You can then use tools like `gdb` to load this core dump and investigate the issue.

### Commands and Usage:

- **Generating a Core Dump**:

  - You can manually trigger a core dump by sending `SIGQUIT` to a process:

    ```sh
    kill -QUIT <pid>
    ```

- **Analyzing a Core Dump**:

  - Use a debugger like `gdb` to load and analyze the core dump:

    ```sh
    gdb <program> core
    ```

  - Once inside `gdb`, you can use various commands to inspect the state of the program:

    - `bt` (backtrace): to see the call stack.
    - `info registers`: to see the state of CPU registers.
    - `list`: to view the source code.
    - `print <variable>`: to inspect the value of a variable.

### Example Code Handling SIGQUIT:

Here’s a small example to illustrate how a program might handle `SIGQUIT`:

```C
#include <stdio.h>
#include <stdlib.h>
#include <signal.h>

// Signal handler function
void handle_sigquit(int sig) {
    printf("Received SIGQUIT, preparing to dump core...\n");
    abort(); // This will create a core dump
}

int main() {
    // Set up the SIGQUIT signal handler
    if (signal(SIGQUIT, handle_sigquit) == SIG_ERR) {
        perror("Unable to catch SIGQUIT");
        exit(EXIT_FAILURE);
    }

    printf("Running program. Send SIGQUIT (Ctrl+\\) to create a core dump.\n");

    // Infinite loop to keep the program running
    while (1) {
        pause(); // Wait for signals
    }

    return 0;
}
```

In this example:

- The `handle_sigquit` function is set up to handle `SIGQUIT`.
- When `SIGQUIT` is received, the handler prints a message and then calls `abort()`, which generates a core dump.
- The `main` function sets up the signal handler and then waits for signals indefinitely using `pause()`.

### Conclusion:

- **SIGQUIT** instructs a process to quit and produce a core dump.
- **Core Dumps** are valuable debugging tools that capture the state of a process at the time of its crash.
- Developers use core dumps to perform post-mortem debugging and analyze the causes of crashes.



| signals    | cause                                                        |
| ---------- | ------------------------------------------------------------ |
| `SIGINT`   | The process was interrupted.                                 |
| `SIGQUIT`  | Someone asked the process to stop and dump the memory in a core dump file. |
| `SIGFPE`   | Floating-point error.                                        |
| `SIGTRAP`  | The debugger asks where the process is.                      |
| `SIGSEGV`  | The process tried to access illegal memory.                  |
| `SIGWINCH` | The terminal window changed size.                            |
| `SIGTERM`  | Someone just asked the kernel to kill the process.           |
| `SIGPIPE`  | The process wrote to a pipe that nothing's reading.          |



Q: If the interrupt handler didn't call `exit()`, would the program still have ended?

A: No.



Q: So, I could write a program that completely ignores interrupts?

A: You could, but it is not a good idea. In general, if our program receives an error signal, it's best to exit with an error, even if we run some of our own code first.



#### Use `kill` to send signals

- `kill` sends a signal to a process. By default, the command sends a `SIGTERM` signal to the process but we can use it to send any signal we like.

```sh
> ps
77868 ttys003  0:00.02 bash
78222 ttys003  0:00.01 ./testprog

>kill 78222
>kill -INT 78222
>kill -SEGV 78222
>kill -KILL 78222
```

- `ps` displays our current processes.
- `kill 78222` - This sends `SIGTERM` to the program
- `kill -INT 78222` - This sends `SIGINT` to the program.
- `kill -SEGV 78222`- This sends `SIGSEGV` to the program
- `kill -KILL 78222` - This sends `SIGKILL` which can't be ignored.
- Each of these `kill` commands will send signals to the process and run whatever handler the process has configured.
- The exception is the `SIGKILL` signal.
- The `SIGKILL` signal can't be caught by code and it can't be ignored.
- That means if we have a bug in our code and it is ignoring every signal, we can **always** stop the process with `kill -KILL`.
- `kill -KILL <pid>` will always kill our program.



#### Send signals with `raise()`

- Sometimes we might want a process to send a signal to **itself**, which we can do with the `raise()` command.

```C
raise(SIGTERM);
```

- Normally, the `raise()` command is used inside our own custom signal handlers.
- It means our code can receive signal for something minor and then choose to raise a more serious signal. This is called **signal escalation**.



#### Sending our code a wake-up call

- The OS sends signals to a process when something has happened that the process needs to know about.
- It might be that the user has tried to interrupt the process, or someone has tried to kill it or even that the process has tried to do something it shouldn't have, like trying to access a restricted piece of memory.
- But signals are not just used when things go wrong.
- Sometimes, a process might actually want to generate its own signals.
- One example of that is the **alarm signal, `SIGALRM`**.
- The alarm signal is usually created by the process's **interval timer**.
- The interval timer is like an alarm clock: we set it for some time in the future and in the meantime our program can go and do something else:

```C
// This will make the timer fire in 120 seconds.
alarm(120);

// Meanwhile our code does something else.
do_important_busy_work();
do_more_busy_work();
```

- But even though our  program is busy doing other things, the timer is still **running in the background**.

So, when the 120 seconds are up, **the timer fires a `SIGALRM` signal**.

- When a process receives a signal, it **stops doing everything else** and handles the signal.
- However, with an alarm signal, it **stops the process**.

```C
catch_signal(SIGALRM, pour_coffee);
alarm(120);
```

- It is **not recommended** to use `alarm()` and `sleep()` at the same time.
- The `sleep()` function puts our program to sleep for a few seconds, but it works by using the same interval timer as the `alarm()` function.
- So, if we try to use the two functions at the same time, they will interfere with each other.
- Alarm signals let us **multitask**.
- If we need to run a particular job every few seconds, or if we want to limit the amount of time we spend doing a job, then alarm signals are a great way of getting a program to **interrupt itself**.

We've seen how to set custom signal handlers, but...

#### What if we want to restore the default signal handler?

- The `signal.h` header has a special symbol `SIG_DFL` which means **handle it the default way**.

```C
catch_signal(SIGTERM, SIG_DFL);
```

- `SIG_IGN` tells the process to completely **ignore** a signal.

```C
catch_signal(SIGINT, SIG_IGN);
```

- But, we should be very careful before we choose to ignore a signal.
- Signals are an important way of controlling, and stopping processes.

Q: Can I set an alarm for less than a second?

A: Yes, but it's a little more complicated. We need to use a different function called `setitimer()`. It lets us set the process's interval timer directly in either seconds or fractions of a second.

Q: Why is there only one timer for a process?

A: The timers have to be managed by the OS's kernel and if processes had lots of timers, the kernel would go slower and slower. To prevent this from happening, the OS limits each process to one timer.



Q: What happens if I set one timer and it had already been set?

A: Whenever we call the `alarm()` function, we reset the timer. That means if we set the alarm for 10 seconds, then a moment later we set it for 10 minutes, the alarm won't fire until 10 minutes are up. The original 10-second timer will be lost.



#### `atoi()`

- The `atoi()` function takes a C-style string (`const char *str`) and converts the initial portion of the string to an `int` value. If the string does not contain a valid integer, the result is undefined.
- It is part of the C standard library, defined in `<stdlib.h>`. 
- The function scans the input string for the first sequence of characters that can be interpreted as a representation of an integer, ignoring any initial whitespace characters. 
- It then converts this part of the string into an integer value. 
- If the first sequence of non-whitespace characters in the string is not a valid integer (or if there are no such sequences), the function returns zero.
- It converts the subsequent characters (digits) into an integer until it encounters a non-digit character or the end of the string.

##### Function Prototype

```C
int atoi(const char *str);
```



```C
#include <stdio.h>
#include <stdlib.h>

int main() {
    char str1[] = "1234";
    char str2[] = "   -567";
    char str3[] = "42abc";
    char str4[] = "abc42";

    int num1 = atoi(str1);
    int num2 = atoi(str2);
    int num3 = atoi(str3);
    int num4 = atoi(str4);

    printf("String: '%s' -> Integer: %d\n", str1, num1);  // Output: String: '1234' -> Integer: 1234
    printf("String: '%s' -> Integer: %d\n", str2, num2);  // Output: String: '   -567' -> Integer: -567
    printf("String: '%s' -> Integer: %d\n", str3, num3);  // Output: String: '42abc' -> Integer: 42
    printf("String: '%s' -> Integer: %d\n", str4, num4);  // Output: String: 'abc42' -> Integer: 0

    return 0;
}

```

### Important Considerations

1. **Error Handling**: `atoi()` does not provide a way to detect errors. If the string does not contain a valid integer, `atoi()` returns 0, which can be ambiguous as 0 might also be a valid result.
2. **Undefined Behavior**: If the value is out of the range of `int`, the behavior is undefined.
3. **Better Alternatives**: Due to its limitations, it's often better to use functions like `strtol()`, `strtoll()`, `strtoul()`, or `strtoull()`, which provide error checking and can handle larger integer types.

### Alternative with `strtol()`

Here's an example of how to use `strtol()` for better error handling:

```C
#include <stdio.h>
#include <stdlib.h>
#include <errno.h>
#include <limits.h>

int main() {
    char str[] = "1234abc";
    char *endptr;
    errno = 0; // To distinguish success/failure after call

    long val = strtol(str, &endptr, 10);

    // Check for various possible errors
    if (errno == ERANGE && (val == LONG_MAX || val == LONG_MIN)) {
        printf("Out of range error\n");
    } else if (errno != 0 && val == 0) {
        printf("Conversion error\n");
    } else if (endptr == str) {
        printf("No digits were found\n");
    } else if (*endptr != '\0') {
        printf("Further characters after number: %s\n", endptr);
    } else {
        printf("Converted number: %ld\n", val);  // Output: Converted number: 1234
    }

    return 0;
}

```

In this example:

- `strtol()` converts the string to a `long` and provides detailed error checking.
- `endptr` is used to determine where the conversion stopped, which can help identify invalid characters in the string.
