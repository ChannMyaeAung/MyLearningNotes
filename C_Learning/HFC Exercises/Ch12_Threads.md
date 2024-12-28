## Chapter 12 - threads

### Exercise 1 - Chapter 12



`functions.c`

```C
#define _XOPEN_SOURCE 700
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <strings.h>
#include <unistd.h>
#include <errno.h>
#include <pthread.h> // Include the pthread library for threading support.
#include "headers.h"

void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(1);
}

// Define a function that will be executed by a thread
// This function prints "Does not!" five times, with a 1-second pause between each print
void *does_not(void *a){
    // Initialize a loop counter
    int i = 0;
    // Loop 5 times
    for(i = 0; i < 5; i++){
        // Pause executaion for 1 second
        sleep(1);
        // Print "Does not!" to the standard output.
        puts("Does not!");
    }
    // Return NULL since this function does not need to return any value
    return NULL;
}

// Define another function that will be executed by a different thread
// This function prints "Does too!" five times, with a 1-second pause between each print
void *does_too(*void a){
    // Initialize a loop counter
    int i = 0;
    // Loop 5 times
    for(i = 0; i < 5; i++){
        sleep(1);
        puts("Does too");
    }
    return NULL;
}
```



`main.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h> // Include the pthread library for threading support.

int main(){
    // Declare thread identifiers.
    pthread_t t0;
    pthread_t t1;
    
    // Create the first thread, running the does_not function.
    if(pthread_create(&t0, NULL, does_not, NULL) == -1){
        // If thread creation fails, print an error message and exit.
        error("Can't create thread t0.");
    }
    
    // Create the second thread, running the does_too function.
    if(pthread_create(&t1, NULL, does_too, NULL) == -1){
        error("Can't create thread t1.")
    }
    
    
    // Declare a pointer to store the return value of the threads 
    void *result;
    
    // Wait for the first thread to finish and check for errors
    // If thread join fails, print error message and exit
    if(pthread_join(t0, &result) == -1){
        error("Can't join thread t0");
    }
    
    // Wait for the second thread to finish and check for errors
    if(pthread_join(t1, &result) == -1){
        error("Can't join thread t1.");
        
    }
    return 0;
}
```

`Makefile`

```makefile
CC = clang
CFLAGS = -Wall -Wextra -g
LIBS = -lpthread
OBJDIR = ./obj
LIBDIR = ./libs

all: main

main: $(OBJDIR)/main.o $(LIBDIR)/libfunctions.a
	$(CC) $(OBJDIR)/main.o -L$(LIBDIR) -lfunctions -o main $(LIBS)

$(OBJDIR)/main.o: main.c headers.h
	$(CC) $(CFLAGS) -c main.c -o $(OBJDIR)/main.o

$(OBJDIR)/functions.o: functions.c headers.h 
	$(CC) $(CFLAGS) -c functions.c -o $(OBJDIR)/functions.o

$(LIBDIR)/libfunctions.a: $(OBJDIR)/functions.o 
	ar -rcs $(LIBDIR)/libfunctions.a $(OBJDIR)/functions.o

clean: 
	@echo "Removing everything except the source files..."
	rm -f *.o $(OBJDIR)/*.o $(LIBDIR)/libfunctions.a main
```

- In C, the `sleep` function suspends the execution of the current thread for a given number of seconds. 
- Specifically, `sleep(1)` means the current thread is paused for 1 second. This function is useful for introducing delays or reducing CPU usage in programs where timing is not critical.
- Running code on two threads means that the program can execute two sets of instructions concurrently, utilizing the CPU's capability to run multiple threads in parallel. 
- This doesn't necessarily mean running two separate programs, but rather running different parts of the same program simultaneously. 
- Modern CPUs have multiple cores, each capable of running one or more threads, allowing for true parallel execution. 
- In the context of our main.c code, it means the `does_not` and `does_too` functions can run at the same time, each on its own thread, potentially on separate CPU cores, improving the program's overall execution efficiency and responsiveness.
- When we look carefully at our `Makefile`, we need to include `-lpthread` which will link the `pthread` library since we are using the `pthread` library.

`Code Execution`

```sh
chan@CMA:~/C_Programming/HFC/chapter_12/test$ ./main
Does not!
Does too!
Does not!
Does too!
Does too!
Does not!
Does too!
Does not!
Does too!
Does not!
chan@CMA:~/C_Programming/HFC/chapter_12/test$ 
```

- When we run the code, we can see both functions running at the same time.



Q: If both functions are running at the same time, why don't the letters in the messages get mixed up? Each message is on its own line.

A: That's because of the way the Standard Output works. The text from `puts()` will all get output at once.



Q: I removed the `sleep()` function, and the output showed all the output from one function and then all the output from the other function. Why is that?

A: Most machines will run the code so quickly that without the `sleep()` call, the first function will finish before the second thread starts running.



### Beers Magnet - Exercise 2 Chapter 12

`functions.c`

```C
void error(char *msg) {
    fprintf(stderr, "%s: %s\n", msg, strerror(errno)); // Print error message and description
    exit(1); // Exit the program with a status of 1 indicating an error
}

// Global variable representing the number of beers
int beers = 2000000;

// Function to be run by each thread
void *drink_lots(void *a){
    (void)a; // Cast argument to avoid unused parameter warning
    int i;
    // Each thread will decrement the beer count 100,000 times
    for(i = 0; i < 100000; i++){
        beers = beers - 1;
    }
    // Return NULl as the thread function has to return a void pointer
    return NULL;
}
```



`headers.h`

```C
void error(char *msg);

extern int beers;

void *drink_lots(void *a);
```



`main.c`

```C
int main(){
    // Array to hold thread identifiers for 20 threads
    pthread_t threads[20];
    int t;
    
    // Print initial number of beers
    printf("%i bottles of beer on the wall\n%i bottles of beer\n", beers, beers);
    
    // Create 20 threads
    for(t = 0; t < 20; t++){
        // Create a thread that runs the drink_lots function
        if(pthread_create(&threads[t], NULL, drink_lots, NULL) == -1){
            error("Can't create thread");
        }
    }
    
    // Pointer to store the return value of the threads
    void *result;
    //Wait for all 20 threads to finish
    for(t = 0; t < 20; t++){
        // Join each thread, ensuring it has completed before moving on
        if(pthread_join(threads[t], &result) == -1){
            error("Can't join threads");
        }
    }
    
    // Print the remaining number of beers
    printf("There are now %i bottles of beer on the wall\n", beers);
    return 0;
}
```

1. **Global Variable:**

   ```C
   int beers = 2000000;
   ```

   - `beers` is a global variable representing the total number of beers.

2. **Thread Function:**

   ```C
   void *drink_lots(void *a) {
       (void)a;
       int i;
       for (i = 0; i < 100000; i++) {
           beers = beers - 1;
       }
       return NULL;
   }
   ```

   - `drink_lots` is the function executed by each thread.
   - It takes a `void*` argument (not used in this case).
   - Each thread decrements the `beers` variable 100,000 times.
   - Returns `NULL` as the thread function must return a `void*`.

3. **Main Function:**

   ```C
   int main() {
       pthread_t threads[20];
       int t;
       printf("%i bottles of beer on the wall\n%i bottles of beer\n", beers, beers);
       for (t = 0; t < 20; t++) {
           if (pthread_create(&threads[t], NULL, drink_lots, NULL) == -1) {
               error("Can't create thread");
           }
       }
   
       void *result;
       for (t = 0; t < 20; t++) {
           if (pthread_join(threads[t], &result) == -1) {
               error("Can't join threads");
           }
       }
       printf("There are now %i bottles of beer on the wall\n", beers);
       return 0;
   }
   ```

   - **Thread Array:**

     ```C
     pthread_t threads[20];
     ```

     - Declare an array to hold the identifiers for 20 threads.

   - **Print Initial Beers:**

     ```C
     printf("%i bottles of beer on the wall\n%i bottles of beer\n", beers, beers);
     ```

     - Print the initial number of beers.

   - **Create Threads:**

     ```C
     for (t = 0; t < 20; t++) {
         if (pthread_create(&threads[t], NULL, drink_lots, NULL) == -1) {
             error("Can't create thread");
         }
     }
     ```

     - Create 20 threads using a loop.
     - Each thread runs the `drink_lots` function.
     - Handle errors if thread creation fails.

   - **Join Threads:**

     ```C
     void *result;
     for (t = 0; t < 20; t++) {
         if (pthread_join(threads[t], &result) == -1) {
             error("Can't join threads");
         }
     }
     ```

     - Use `pthread_join` to wait for each thread to finish.
     - Handle errors if joining a thread fails.

### Logic

- The main logic is to create 20 threads that each decrement a shared global variable (`beers`) 100,000 times.
- By using threads, the program simulates concurrent operations on the shared resource.
- After all threads complete, the program prints the final value of `beers`.



`Code Execution`

```sh
chan@CMA:~/C_Programming/HFC/chapter_12/test$ ./main
2000000 bottles of beer on the wall
2000000 bottles of beer
There are now 1758496 bottles of beer on the wall
chan@CMA:~/C_Programming/HFC/chapter_12/test$ ./main
2000000 bottles of beer on the wall
2000000 bottles of beer
There are now 1710145 bottles of beer on the wall
chan@CMA:~/C_Programming/HFC/chapter_12/test$ ./main
2000000 bottles of beer on the wall
2000000 bottles of beer
There are now 1736959 bottles of beer on the wall
```

- We can see the the result is quite unpredictable.
- Why doesn't the `beers` variable get set to zero when  all the threads have run?



### The code is not thread-safe

- The great thing about threads is that lots of different tasks can run at the same time and have access to the same data.
- The downside is that all those different threads have access to the same data.
- The threads in the second program are all reading and changing a shared piece of memory: the `beers` variable.
- To understand what's going on, let's see what happens if two threads try to reduce the value of `beers` using this line of code:

```C
beers = beers - 1;
```

![Screenshot from 2024-07-22 22-14-54](/home/chan/Pictures/Screenshots/Screenshot from 2024-07-22 22-14-54.png)

- Even though both of the threads were trying to reduce the value of `beers` by 1, they didn't succeed.
- Instead of reducing the value by 2, they only decreased it by 1.
- That's why the `beers` variable didn't get reduced to zero
- The threads kept getting in the way of each other.
- The reason why the result was so unpredictable is because the threads didn't always run the line of code at exactly the same time.
- Sometimes the threads didn't crash into each other, and sometimes they did.
- Be careful to look out for code that isn't thread-safe. How will we know? 
- If two threads read and write to the same variable, it's not.



### We need to add traffic signals

- Multithreaded programs can be powerful, but they can also behave in unpredictable ways, unless we put some controls in place.
- Imagine two cars want to pass down the same narrow stretch of road. 
- To prevent an accident, we can add traffic signals.
- Those traffic signals prevent the cars from getting access to a shared resource(the road) at the same time.
- It's the same thing when we want two or more threads to access a shared data resource.
- We need to add traffic signals so that no two threads can read the data and write it back at the same time.

![Screenshot from 2024-07-22 22-23-00](/home/chan/Pictures/Screenshots/Screenshot from 2024-07-22 22-23-00.png)

- The traffic signals that prevent threads from crashing into each other are called `mutexes`, and they are one of the simplest ways of making our code thread-safe.
- `Mutexes` are sometimes just called locks.
- MUT-EX = Mutually Exclusive.



### Use a `mutex` as a traffic signal

- To protect a section of code, we will need to create a mutex lock like this:

```C
pthread_mutex_t a_lock = PTHREAD_MUTEX_INITIALIZER;
```

- The `mutex` needs to be visible to all of the threads that might crash into each other, so that means we'll probably want to create it as a global variable.
- `PTHREAD_MUTEX_INITIALIZER` is actually a macro.
- When the compiler sees that, it will insert all of the code our program needs to create the `mutex` lock properly.



1. `Red means stop`.

   - At the beginning of our sensitive code section, we need to place our first traffic signal.
   - The `pthread_mutex_lock()` will let only **one thread** get past.
   - All the other threads will have to wait when they get to it.

   ```C
   // Only one thread at a time will get past this.
   pthread_mutex_lock(&a_lock);
   /* Sensitive code starts here...*/
   ```

2. `Green means go.`

   - When the thread gets to the end of the sensitive code, it makes a call to `pthread_mutex_unlock()`.
   - That sets the traffic signal back to green and another thread is allowed onto the sensitive code.

   ```C
   /* End of sensitive Code */
   pthread_mutex_unlock(&a_lock);
   ```

   - Now that we know how to create locks in our code, we have a lot of control over exactly how our threads will work.



### Passing Long Values to Thread Functions 

- Thread functions can accept a single void pointer parameter and return a single void pointer value.
- Quite often, we'll want to pass and return integer values to a thread.
- One trick is to use `long` values.
- `long`s can be stored in void pointers because they are the same size.

`functions.c`

```C
// A thread function can accept a single void pointer parameter
void *do_stuff(void *param){
    // Convert parameter to long to get thread number
    long thread_no = (long)param;
    // Print the thread number
    printf("Thread number %ld\n", thread_no);
    // Cast it back to a void pointer when it is returned.
    // Return thread number + 1 as the thread's return value
    return (void*)(thread_no + 1);
}
```

`main.c`

```C
int main(){
    // array to hold thread identifiers
    pthread_t threads[20];
    // Loop variable and thread argument
    long t;
    
    // Create 3 threads
    for(t = 0; t < 3; t++){
        // Create a thread and check for errors
        // (void*)t - Convert the long t value to a void pointer
        if(pthread_create(&threads[t], NULL, do_stuff, (void*)t) == -1){
            error("Cannot create threads");
        }
    }
    
    // Pointer to hold the return value of the threads
    void *result;
    // Join 3 threads
    
    for(t = 0; t < 3; t++){
        if(pthreads_join(threads[t], &result) == -1){
            error("Cannot join threads");
        }
        // Print the return value of the thread
        // Convert the return value to a long before using it.
        printf("Thread %ld returned %ld\n", t, (long)result);
    }
    return 0;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/test$ ./final
Thread number 0
Thread number 1
Thread number 2
Thread 0 returned 1
Thread 1 returned 2
Thread 2 returned 3

```

- Each thread receives its thread number.
- Each thread returns its thread number + 1.



### Long Exercise - Chapter 12

- There's no simple way to decide where to put the locks in our code.
- Where we put them will change the way the code performs.
- Here are two versions of `drink_lots()` function that lock the code in different ways.

`Version A`

```C
pthread_mutex_t beers_lock = PTHREAD_MUTEX_INITIALIZER;

void *drink_lots(void *a){
    int i;
    pthread_mutex_lock(&beers_lock);
    for(i = 0; i < 100000; i++){
        beers = beers - 1;
    }
    pthread_mutex_unlock(&beers_lock);
    printf("beers = %i\n", beers);
    return NULL;
}
```

- **Granularity**: Fine-grained locking.
- **Concurrency**: Higher concurrency, as the lock is held for a very short duration—just enough to decrement the `beers` variable. This allows other threads to acquire the lock between iterations of the loop.
- **Performance**: Potentially lower performance due to the overhead of acquiring and releasing the lock 100,000 times per thread. This can lead to significant contention and overhead if many threads are trying to lock and unlock in rapid succession.
- **Safety**: Safer in terms of data consistency. Each decrement operation is protected, ensuring that no two threads can modify `beers` simultaneously, which prevents race conditions.

`Version B`

```C
pthread_mutex_t beers_lock = PTHREAD_MUTEX_INITIALIZER;

void *drink_lots(void *a){
    int i;
    for(i = 0; i < 100000; i++){
        pthread_mutex_lock(&beers_lock);
        beers = beers - 1;
        pthread_mutex_unlock(&beers_lock);
    }
    printf("beers = %i\n", beers);
    return NULL;
}
```

- **Granularity**: Coarse-grained locking.
- **Concurrency**: Lower concurrency, as the lock is held for the duration of the entire loop, preventing other threads from accessing `beers` until the loop completes.
- **Performance**: Potentially higher performance due to reduced lock contention. The lock is acquired and released only once per thread, reducing the overhead associated with locking mechanisms.
- **Safety**: Still safe in terms of data consistency for the entire operation of decrementing `beers` 100,000 times, but it could lead to longer wait times for other threads, as they must wait for the current thread to finish the entire loop before getting a chance to execute.

Granularity, in the context of locking mechanisms like `mutexes` in multithreaded programming, refers to the scope and duration a lock covers during its acquisition. It's a measure of the size of the locked resource or the amount of code protected by the lock. Granularity can be broadly categorized into two types:

1. **Fine-Grained Locking**:
   - Locks are used to protect a small amount of data or a very specific part of the code.
   - This approach allows for higher concurrency, as it minimizes the time a lock is held and reduces the chances of contention among threads.
   - However, it can lead to higher overhead due to the frequent locking and unlocking operations.
2. **Coarse-Grained Locking**:
   - Locks are used to protect a large set of data or larger sections of code.
   - This approach simplifies the design and reduces the overhead of acquiring and releasing locks.
   - However, it can significantly reduce concurrency, as threads may be blocked for longer periods while waiting for access to the locked resource.

Choosing the right level of granularity is crucial for balancing performance and concurrency in multithreaded applications. Fine-grained locking is preferred for high-concurrency applications where performance is critical, while coarse-grained locking might be suitable for applications where simplicity and lower overhead are more important than maximizing concurrency.

`main.c`

```C
int main(){
    pthread_t threads[20];
    int t;
    printf("%d bottles of beers on the wall\n%d bottles of beers\n", beers, beers);
    for(t = 0; t < 20; t++){
        if(pthread_create(&threads[t], NULL, drink_lots, NULL) == -1){
            error("Can't create threads");
        }
    }
    
    void *result;
    for(t = 0; t < 20; t++){
        if(pthread_join(threads[t], &result) == -1){
            error("Can't join threads");
        }
    }
    
    printf("%d bottles of beers on the wall\n%d bottles of beers\n", beers, beers);
    return 0;
}
```



`Code Execution (Version A)`

```sh
chan@CMA:~/C_Programming/test$ ./final
2000000 bottles of beer on the wall
2000000 bottles of beer
beers: 1900000
beers: 1800000
beers: 1700000
beers: 1600000
beers: 1500000
beers: 1400000
beers: 1300000
beers: 1200000
beers: 1100000
beers: 1000000
beers: 900000
beers: 800000
beers: 700000
beers: 600000
beers: 500000
beers: 400000
beers: 300000
beers: 200000
beers: 100000
beers: 0
0 bottles of beer on the wall
0 bottles of beer

```

`Code Execution (Version B)`

```sh
chan@CMA:~/C_Programming/test$ ./final
2000000 bottles of beer on the wall
2000000 bottles of beer
beers: 93835
beers: 93728
beers: 88356
beers: 83394
beers: 75964
beers: 67523
beers: 55832
beers: 39614
beers: 27938
beers: 27074
beers: 9064
beers: 8584
beers: 8140
beers: 7523
beers: 6289
beers: 3813
beers: 1965
beers: 1301
beers: 357
beers: 0
0 bottles of beer on the wall
0 bottles of beer

```

- As we can see both pieces of code use a `mutex` to protect the `beers` variable.
- Each displays the value of `beers` before they exit.
- Because they are locking the code in different places, they generate different output on the screen.
- In summary, locking inside the loop offers better safety for each individual operation at the cost of performance due to frequent locking and unlocking. 
- Locking outside the loop improves performance by reducing lock contention but decreases concurrency, as threads have to wait longer for their turn to execute the loop.



### Locking Outside the Loop vs Inside The loop

The discrepancy in the starting value of `beers` when locking inside the loop, specifically starting from `93835` instead of decrementing in a predictable manner from `2000000`, can be attributed to the behavior of concurrent threads and the overhead of locking and unlocking in each iteration. Here's a breakdown to explain this phenomenon:

### Locking Outside the Loop

When the lock is placed outside the loop, the entire decrement operation (from `2000000` to `0`) is protected by a single lock for each thread. This means that once a thread acquires the lock, it will decrement `beers` by `100000` (assuming each thread is supposed to decrement `beers` by `1` a total of `100000` times) without any interruption from other threads. This results in a predictable, linear decrement of `beers` because threads execute their loops in complete isolation from each other, leading to the expected output where `beers` is decremented in large, consistent chunks.

### Locking Inside the Loop

When the lock is placed inside the loop, each decrement operation is individually protected. This means that after a thread decrements `beers` by `1`, it releases the lock, allowing another thread to acquire the lock and perform its decrement operation. This leads to a few key consequences:

1. **Concurrency Overhead**: The frequent locking and unlocking in each iteration introduce significant overhead, slowing down the decrement operations.
2. **Thread Scheduling and Preemption**: The operating system's scheduler determines which thread runs at any given time. Threads can be preempted (temporarily paused) to allow other threads to run. This scheduling is not deterministic from the perspective of your application, leading to non-linear decrements in `beers`.
3. **Contention**: With many threads contending for the same lock, some threads may acquire the lock more frequently than others, especially if they are preempted less often or scheduled more favorably. This can lead to an uneven rate of decrement across threads.

The starting value of `93835` suggests that by the time the program printed the first value of `beers`, multiple threads had already decremented it from `2000000` in a highly non-linear and unpredictable manner due to the reasons mentioned above. The specific starting value you see is a result of the complex interplay between thread scheduling, the overhead of locking and unlocking, and the specific behavior of your system's scheduler and CPU.

This unpredictability and the overhead associated with fine-grained locking inside the loop highlight the trade-offs between concurrency, performance, and the granularity of locking in multithreaded applications.