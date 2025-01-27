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
waitpid(pid, &pid_status, options)
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

#### Syntax

```C
long int strtol(const char *str, char **endptr, int base);
```

#### Parameters

1. **`str`**:
   - A pointer to the null-terminated string to be converted.
2. **`endptr`**:
   - A pointer to a pointer to a character. This parameter is used to store the address of the first invalid character after the number. If you do not need this information, you can pass `NULL`.
3. **`base`**:
   - An integer representing the base of the number in the string. It can be any value from 2 to 36, or 0. If `base` is 0, the base is determined by the format of the string (e.g., "0x" for hexadecimal, "0" for octal, and decimal otherwise).

#### Return Value

- The function returns the converted long integer value.
- If no valid conversion could be performed, it returns `0`.
- If the value is out of the range of representable values for a long integer, it returns `LONG_MAX` or `LONG_MIN` and sets `errno` to `ERANGE`.

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
        printf("Converted number: %ld\n", val);
    } else {
        printf("Converted number: %ld\n", val);  // Output: Converted number: 1234
    }

    return 0;
}

```

In this example:

- `strtol()` converts the string to a `long` and provides detailed error checking.
- `endptr` is used to determine where the conversion stopped, which can help identify invalid characters in the string.
- The `strtol` function stops converting when it encounters a character that is not a valid digit for the specified base (in this case, base 10). 
- The `endptr` pointer is set to point to the first character that was not converted.
- In this code, the string `"1234abc"` is partially converted to the integer `1234`, and `endptr` points to the `"abc"` part of the string. 
- The condition `*endptr != '\0'` is true because there are additional characters after the number, so the code prints `"Further characters after number: abc"`.
- `strtol` converts the initial part of the string (`"1234"`) to a long integer (`1234`).
- `endptr` is set to point to the first character that was not converted (`'a'` in `"abc"`).
- Since `*endptr` is not `'\0'` (it points to `'a'`), the code prints the remaining part of the string (`"abc"`).

`Output`

```sh
chan@CMA:~/C_Programming/test$ ./final
Further characters after number: abc
Converted number: 1234
```





Q: Are signals always received in the same order they are sent?

A: Not if they are sent very close together. The OS might choose to reorder the signals if it thinks one is more important than the others.

Q: Is that always true?

A: It depends on the platform. On most versions of Cygwin, for example, the signals will always be sent and received in the same order. But in general, we shouldn't rely on it.

Q: If I send the same signal twice, will it be received twice by the process?

A: Again, it depends. On Linux and the Mac, if the same signal is repeated very quickly, the kernel might choose to only send the signal once to the process. On Cygwin, it will always send both signals. But again, we shouldn't assume that just because we sent the same signal twice, it will be received twice.

#### Chapter 10 - Bullet Points

- The OS talks to processes using signals.
- Programs are normally stopped using signals.
- When a process receives a signal, it runs a handler.
- For most error signals, the default handler stops the program.
- Handlers can be replaced with `signal()` function.
- We can send signals to ourselves with `raise()`.
- The interval timer sends `SIGALRM` signals.
- The `alarm()` function sets the interval timer.
- There is one timer per process.
- Don't use `sleep()` and `alarm()` at the same time.
- `kill` sends signals to a process.
- `kill -KILL` will always kill a process.
- `exit()` stops the program immediately.
- `waitpid()` waits for a process to finish.
- `fileno()` finds the descriptor.
- `dup2()` duplicates a data stream.
- `pipe()` creates a communication pipe.
- Processes can communicate using pipes.
- Signals are messages from the OS.
- A program can send signals to itself with `raise()`.
- `sigaction()` lets us handle signals.
- `alarm()` sends a `SIGALRM` after a few seconds.