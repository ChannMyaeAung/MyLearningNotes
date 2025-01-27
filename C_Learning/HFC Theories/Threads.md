## Chapter 12 - Threads

- Programs often need to do several things at the same time.
- `POSIX` threads can make our code more responsive by **spinning off several pieces of code to run in parallel.**
- Threads are powerful tools, but we don't want them crashing into each other.

### In this chapter, we'll learn

- How to put up **traffic signs** and **lane markers** that will **prevent a code pileup.**
- By the end, we'll know how to **create POSIX threads** and how to use **synchronization mechanisms** to**protect the integrity of sensitive data.**



### Tasks are sequential.. or not...

Imagine we are writing something complex like a game in C. The code will need to perform several different tasks:

- It will need to update the graphics on the screen.
- It will need to read control information from the games controller or keyboard.
- It will need to calculate the latest locations of the objects that are moving in the game.
- It might need to communicate with the disk and the network.
- Not only will our code need to do all of these things, it will need to do them **all at the same time.**
- That's going to be true for many different programs.
- Chat programs will need to read text from the network and send data to the network at the same time.
- Media players will need to stream video to the display as well as watch for input from the user controls.



### How can our code perform several different tasks at once?

#### ... and processes are not always the answer

- We have already learned how to make the computer do several things at once: with **processes**.
- In the last chapter, we built a network server that could deal with several clients at once.
- Each time a new user connected, the server created a new process to handle a new session.
- That doesn't mean that whenever we want to do several things at once, we should just create a separate process.

Here is why:

1. Processes take time to create.
   - Some machines take a little while to create new processes.
   - Not much time but some.
   - If the extra task we want to perform takes just a few hundredths of a second, creating a process each time won't be very efficient.
2. Processes can't share data easily
   - When we create a child process, it automatically has a complete copy of all the data from the parent process.
   - But it's a copy of the data.
   - If the child needs to send data back to the parent, then we need something like a pipe to do that for us.
3. Processes are just plain difficult
   - We need to create a chunk of code to generate processes and that can make our programs long and messy.



We need something that starts a separate task quickly, can share all of our current data and won't need a huge amount of code to build.

**We need threads.**

### Simple processes do one thing at a time

Let's say we have a task list with a set of things that we need to do:

- We can't do all of the tasks at the same time, not by ourselves.
- If we work in a shop alone, we're like a simple process: we do one thing after another, but always one thing at a time.
- Surely, we can switch between tasks to keep everything going but what if there's a **blocking operation**?
- What if we're serving someone at the checkout and the phone rings?
- All of the programs we've written so far had **a single thread of execution**.
- It's like there's only been one person working inside the program's process.



### Employ extra staff: use threads

- A **multi-threaded** program is like a shop with several people working in it.
- If one person is running the checkout, another is filling the shelves, then everybody can work without interruptions.
- If one person answers the phone, it won't stop the other people in the shop.
- If we can employ more people, more than one thing can be done at once.
- In the same way that several people can work in the same shop, we can have several threads living inside the same process.
- All of the threads will have access to the same piece of heap memory.
- They will all be able to read and write to the same files and talk on the same network sockets.
- If one thread changes a global variable, all of the other threads will see the change immediately.
- That means we can give each thread a separate task and they'll all be performed at the same time.
- We can run each task inside a separate thread.
- If one thread has to wait for something, the other  threads can keep running.
- All of the threads can run inside a single process.



### How do we create threads?

- There are a few thread libraries, and we are going to use one of the most popular: the **POSIX thread library** or `pthread`.

```C
void* does_not(void *a){
    int i = 0;
    for(i = 0; i < 5; i++){
        sleep(1);
        puts("Does not!");
    }
    return NULL;
}
```

```C
void* does_too(void *a){
    int i = 0;
    for(i = 0; i < 5; i++){
        sleep(1);
        puts("Does too!");
    }
    return NULL;
}
```

- Thread functions need to have a `void*` return type.
- Nothing useful to return so just use NULL.
- A void pointer can be used to point to any piece of data in memory and we'll need to make sure that our thread functions have a `void*` return type.
- We're going to run each of these functions inside its own thread.
- We'll need to run both of these functions in parallel in separate threads.
- The `void *a` parameter is a way to pass an argument of any type to the function.
- In C, `void *` is a pointer to a memory location that has not been given a specific type.
- This means it can point to a variable of any data type, from a simple integer to a complex structure.
- This feature is particularly useful in threaded programming with the `pthreads` library, as it allows us to pass any type of data to the thread function.
- The `void *` argument could be anything such as configuration options, state information, or other data structures.
- Since the function signature must match what the `pthreads` library expects for thread routines, this parameter is necessary even if it is not used. 



### Create threads with `pthread_create`

```C
#include <pthread.h>

// This is the header for the pthread library.
```

- We are going to create two threads and each one needs to have its info stored in a `pthread_t` data structure.
- Then we can create and run a thread with `pthread_create()`.

```C
// These records all the information about the thread
pthread_t t0;
pthread_t t1;

// This creates the thread
// does_not and does_too are the names of the function the thread will run.
// &t0 and &t1 are the addresses of the data structure that will store the thread info.
if(pthread_create(&t0, NULL, does_not, NULL) == -1){
    error("Can't create thread t0");
}

if(pthread_create(&t1, NULL, does_too, NULL) == -1){
    error("Can't create thread t1");
}
```

- That code will run our two functions in separate threads.
- But we have not quite finished yet. If our program just ran this and then finished, the threads would be killed when the program ended.
- So we need to wait for our threads to finish.

```C
// The void pointer returned from each function will be stored here.
*void result;

// The pthread_join() function waits for a thread to finish
if(pthread_join(t0, &result) == -1){
    error("Can't join thread t0");
}
if(pthread_join(t1, &result) == -1){
    error("Can't join thread t1");
}
```

- The `pthread_join()` also receives the return value of our thread function and stores it in a void pointer variable.
- Once both threads have finished, our program can exit smoothly.



### `pthread_create()` and `pthread_join()`

The [`pthread_create`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) and `pthread_join` functions are used in POSIX threads programming (pthreads) for creating and joining threads, respectively. Here are their parameters:

#### `pthread_create`

- **thread**: A pointer to `pthread_t` that is an opaque, unique identifier for the newly created thread.
- **attr**: A pointer to `pthread_attr_t` that specifies the attributes for the thread. If this parameter is `NULL`, the thread is created with default attributes.
- **start_routine**: The function to be executed by the thread. This pointer points to a function that takes a single `void *` argument and returns a `void *`.
- **arg**: A pointer to the argument that will be passed to the `start_routine`. This can be `NULL` if no argument is needed.

#### `pthread_join`

- **thread**: The `pthread_t` identifier of the thread to join. This is the same identifier that was obtained when the thread was created with `pthread_create`.
- **retval**: A pointer to a location where the exit status of the joined thread will be stored. This can be a pointer to a `void *` variable. If it is `NULL`, the exit status is not stored.



### Chapter 12 - Q & A

Q: Does my machine have to have multiple processors to support threads?

A: No. Most machines have processors with multiple cores, which means that their CPUs contain mini-processors that can do several things at once. But even if our code is running on a single core/single processor, we will still be able to run threads.



Q: How?

A: The OS will switch rapidly between the threads and make it appear that it is running several things at once.



Q: Will threads make my programs faster?

A: Not necessarily. While threads can help us use more of the processors and cores on our machines, we need to be careful about the amount of locking our code needs to do. If our threads are locked too often, our code may run as slowly as single-threaded code.



Q: How can I design my thread code to be fast?

A: Try to reduce the amount of data that threads need to access. If threads don't access a lot of shared data, they won't need to lock each other out so often and will be much more efficient.



Q: Are threads faster than separate processes?

A: They usually are, simply because it takes a little more time to create processes than it does to create extra threads.



Q: I've heard that mutexes can lead to "deadlocks". What are they?

A: Say we have two threads, and they both want to get mutexes A and B. If the first thread already has A, and the second thread already has B, then the threads will be deadlocked. This is because the first thread can't get mutex B and the second thread can't get mutex A. They both come to a standstill.



### Chapter 12 - Bullet Points

- Simple processes do one thing at a time.
- Threads allow a process to do more than one thing at the same time.
- `pthread_create()` creates a thread to run a function.
- `pthread_join()` will wait for a thread to finish.
- `Mutexes` are locks that protect shared data.
- `POSIX threads (pthread)` is a threading library.
- Threads share the same global variables.
- `pthread_mutex_lock()` creates a mutex on code.
- `pthread_mutex_unlock()` releases the `mutex`. 
- Threads are "lightweight processes".
- If two threads read and update the same variable, our code will be unpredictable.



## Extra Materials

### Increments and decrements

```C
// ++i = Increase i by 1, then return the new value.

// i++ = Increase i by 1, then return the old value.

// --i = Decrease i by 1, then return the new value.

// i-- = Decrease i by 1, then return the old value.


```

- Each of these expression will change the value of `i`.
- The position of the `++` and `--` say whether or not to return the original of `i` or its new value. 

```C
int i = 3;
int j = i++; // After this line, j == 3 and i == 4;

```

- But instead of `i++` if we use `++i`, then j == 4 and i == 4;

```C
int main()
{
    // Example of i++
    int i = 10;
    printf("Original value of i: %d\n", i);
    printf("Using i++: %d\n", i++);
    printf("Value of i after i++: %d\n\n", i);

    // Resetting i to 10
    i = 10;

    // Example of i--
    printf("Original value of i: %d\n", i);
    printf("Using i--: %d\n", i--);
    printf("Value of i after i--: %d\n\n", i);

    // Resetting i to 10
    i = 10;

    // Example of ++i
    printf("Original value of i: %d\n", i);
    printf("Using ++i: %d\n", ++i);
    printf("Value of i after ++i: %d\n\n", i);

    // Resetting i to 10
    i = 10;

    // Example of --i
    printf("Original value of i: %d\n", i);
    printf("Using --i: %d\n", --i);
    printf("Value of i after --i: %d\n\n", i);

    return 0;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Original value of i: 10
Using i++: 10
Value of i after i++: 11

Original value of i: 10
Using i--: 10
Value of i after i--: 9

Original value of i: 10
Using ++i: 11
Value of i after ++i: 11

Original value of i: 10
Using --i: 9
Value of i after --i: 9

```

- As we can see when we print the values of  `i++` and `i--` , we print the original value.
- On the other hand, when we print the value of `++i` and `--i`, we can see the it returns the new value.



### Preprocessor directives

- We use a preprocessor directive every time we compile a program that includes a header file:

```C
#include <stdio.h>
```

- The preprocessor scans through our C source file and generates a modified version that will be compiled.
- In the case of the `#include` directive, the preprocessing inserts the contents of the `stdio.h` file. 
- Directives always appear at the start of a line, and they always begin with the hash (#) character.
- The next common directive after `#include` is `#define`.

```C
#define DAYS_OF_THE_WEEK 7
...
printf("There are %d days of the week\n, DAYS_OF_THE_WEEK");
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
There are 7 days of the week

```

- The `#define` directive creates a **macro**.
- The preprocessor will scan through the C source and replace the macro name with the macro's value.
- Macros aren't variables because they can never change at runtime.
- Macros are placed before the program even compiles.
- We can even create macros that work a little like functions.

```C
#define ADD_ONE(x) ((x) + 1)
...
printf("The answer is %d\n", ADD_ONE(3));
```

- `x` is a parameter to the macro
- Be careful to use parentheses with macros.
- The preprocessor will replace `ADD_ONE(3)` with ((3) + 1) before the program is compiled.

```sh
chan@CMA:~/C_Programming/practice$ ./practice
The answer is 4

```



### Conditions

- We can also use the preprocessor for **conditional compilation**.
- We can make it switch parts of the source code on or off.

```C
#ifdef SPANISH
char *greeting = "Hola";
#else
char *greeting = "Hello";
#endif
```

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Hello

```

- This code will be compiled differently if there is (or isn't) a macro called `SPANISH` defined.

```C
#define SPANISH
#ifdef SPANISH
char *greeting = "Hola";
#else
char *greeting = "Hello";
#endif
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

int main(){
    printf("%s\n", greeting);
    return 0;
}
```

- But if we have defined SPANISH, then the result will be:

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Hola!
```

### The static keyword

Imagine we want to create a function that works like a counter.

```C
int count = 0;
int counter(){
    return ++count;
}
```

- The problem with this code is that it uses a global variable called `count`.
- Any other function can change the value of `count` because it's in the global scope.
- If we start to write large programs, we need to be careful that we don't have too many global variables because they can lead to buggy code.
- Fortunately, C lets us create a global variable that is available only inside the local scope of a function:

```C
int counter(){
    static int count = 0;
    return ++count;
}
```

```C
int main()
{
    printf("%d\n", counter());
    printf("%d\n", counter());
    return 0;
}
```



- `count` is still a global variable, but it can only be accessed inside this function.
- The `static` keyword means this variable will keep its value between calls to `counter()`.
- The `static` keyword will store the variable inside the global area of memory, but the compiler will throw an error if some other function tries to access the `count` variable.

```sh
chan@CMA:~/C_Programming/practice$ ./practice
1
2

```

### static can also make things private

- We can use the `static` keyword outside of functions.
- `static` in this case means "only code in this `.c` file can use this".

```C
// We can only use this variable only inside the current source file.
static int days = 365;

// We can tell this function only from inside this source file
static void update_account(int x){
    ...
}
```

- The `static` keyword **controls the scope** of something. 
- It will prevent our data and functions from being accessed in ways that they weren't designed to be.