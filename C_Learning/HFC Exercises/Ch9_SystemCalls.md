### Chapter 9 - processes and system calls

#### Chapter 9 - Exercise 1

main.c

```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

char *now(){
    time_t t;
    time(&t); // Get the current calender time
    return asctime(localtime(&t));
    // Convert to local time and then to string
}

int main(){
    char comment[80];
    char cmd[120];
    fgets(comment, 80, stdin); // Read a comment from the user
    sprintf(cmd, "echo '%s %s' >> reports.log", comment, now()); // Format the command string
    
    system(cmd); // Execute the command to append to reports.log
    return 0;
}
```

Code Breakdown:

- `#include <time.h>`: Includes the time library, which provides functions for manipulating and formatting dates and times.
- `char *now()`: Declares a function named `now` that returns a pointer to a character (a string).
- `time_t t`: Declares a variable `t` of type `time_t`, which is typically used to store calendar time.
- `time(&t)`: Fills `t` with the current calendar time.
- **`localtime`**: Converts calendar time (`time_t`) to local time (`struct tm`).
- **`asctime`**: Converts local time (`struct tm`) to a human-readable string.
- `return asctime(localtime(&t))`: Converts the calendar time `t` to local time, formats it as a human-readable string and returns this string.
- `char comment[80]`: Declares an array `comment` to hold up to 79 characters of input plus a null terminator.

- `fgets(comment, 80, stdin)` - Using `fgets` for unstructured text. Reads up to 79 characters from the standard input (keyboard) into the `comment` array. It includes a newline character if there's space, and always null-terminates the string.
- `sprintf(cmd, "echo '%s %s' >> reports.log", comment, now())` - 
  - `sprintf` will print the characters to a string. it formats the string: `"echo 'comment current_time' >> reports.log"`, storing it in `cmd`.
  - `cmd` - The formatted string will be stored in the `cmd` array.
  - `"echo '%s %s' >> reports.log"` - This is the command template. The command will append the comment to a file.
  - `comment` - The comment will appear first.
  - `now()` - The timestamp appears second. `now()` is called which returns the current date and time as a string.
- `system(cmd)` - This runs the contents of the `cmd` string. Executes the command stored in `cmd` as if it were typed in the shell. This command appends the `comment` and current time to a file named `reports.log`.

Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_9/exercise_1
$ ./main
Checked in Crom - a compound interest program.

chan@CMA:~/C_Programming/HFC/chapter_9/exercise_1
$ ./main
Blue Leader reports breach in jet walls.

chan@CMA:~/C_Programming/HFC/chapter_9/exercise_1$ 
```

reports.log

```markdown
Checked in Crom - a compound interest program.
 Fri Jun 14 22:54:34 2024

Blue Leader reports breach in jet walls.
 Fri Jun 14 22:55:34 2024
```



Q: Does the `system()` function get compiled into my program?

A: No. The `system()` function -- like all system calls -- doesn't live in our program. It lives in the main OS.



Q: So, when I make a system call, I'm making a call to some external piece of code, like a library?

A: Kind of. But the details depend on the OS. On some OS, the code for a system call lives inside the kernel of the OS. On other OS, it might simply be stored in some dynamic library.