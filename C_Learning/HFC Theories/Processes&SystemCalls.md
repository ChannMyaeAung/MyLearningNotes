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
