#### scanf()

- scanf() can cause buffer overflows if you forget to limit the length of the string that you read with scanf(), then any user can enter far more data than the program has space to store. 
- The extra data then gets written into memory that has not been properly allocated by the computer.
- `scanf()` always uses pointers.

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



#### Stack Smashing

- Stack smashing detected is a security alert that occurs when a program tries to write data beyond the boundary of a buffer allocated on the stack memory. 
- This can potentially lead to memory corruption and security vulnerabilities.
- The %s format specifier in scanf reads a string from the input until it encounters a whitespace character (space, newline, tab, etc.). 
- The problem is that scanf doesn't check the length of the input string and writes it directly into the food array, which has a fixed size of 5 characters.

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

- Clearly if you need to enter structured data with several fields, you'll want to use scanf(). `scanf()` means "scan formatted" because it's used to scan formatted input.
- If you are entering a single unstructured string, then fgets() is probably the way to go.

