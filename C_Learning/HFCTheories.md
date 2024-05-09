## Theories from HFC Book 



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

- In a union, all members share the same memory location.

- In a `struct`, each member has its own memory space.

- A union lets you reuse memory space.

- Everytime we create an instance of `struct`, the computer will lay out the fields in memory, one after the other.

- A union is different. A union will use the space for just one of the fields in its definition.

- Unions are commonly used in situations where you need to conserve memory or when you want to represent a single value in different ways.

  
