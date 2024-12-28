#### Signal Handler Exercise

`hello.c`

```C
#include <signal.h> // We need to include signal.h header.
// This is our new signal handler.
void diediedie(int sig){ // The OS passes signal to the handler.
	puts("Goodbye cruel world...\n");
    exit(1);
}

int catch_signal(int sig, void (*handler)(int)){
    
    struct sigaction action; // Declare a variable 'action' of type 'struct sigaction'
    
    action.sa_handler = handler; // Set the signal handler to the provided 'handler' function
    
    sigemptyset(&action.sa_mask); // Initialize the signal set to empty (no signals blocked during the handler execution)
    
    action.sa_flags = 0; // Set the flags to 0 (no special options)
    return sigaction (sig, &action, NULL);
    // Set the action for signal 'sig' using 'sigaction' system call
}
```

**`struct sigaction action;`**:

- This declares a variable `action` of type `struct sigaction`, which is used to specify how to handle a particular signal.

**`action.sa_handler = handler;`**:

- This sets the `sa_handler` member of the `struct sigaction` to the function pointer `handler`. This means that when the specified signal (`sig`) occurs, the `handler` function will be called.

**`sigemptyset(&action.sa_mask);`**:

- This initializes the signal set `sa_mask` to be empty. This means that no additional signals are blocked during the execution of the signal handler.

**`action.sa_flags = 0;`**:

- This sets the `sa_flags` member to 0, which means no special flags are set. It uses the default behavior for handling the signal.

**`return sigaction(sig, &action, NULL);`**:

- This calls the `sigaction` system call to change the action taken by the process on receipt of signal `sig`. It sets the action to the one specified by `action`. The third argument is `NULL`, which means we do not need to retrieve the old action.

`main.c`

```C
int main(){
    
    // SIGINT means we're capturing the interrupt signal
    if(catch_signal(SIGINT, diediedie) == -1){
        fprintf(stderr, "Can't map the handler");
        exit(2);
    }
    
    char name[30];
    printf("Enter your name: ");
    fgets(name, 30, stdin);
    printf("Hello %s\n", name);
    return 0;
}
```

- `SIGINT` - the interrupt signal typically triggered by pressing `Ctrl+C`. 
- When press `Ctrl+C`, the OS will automatically send the process an interrupt signal (`SIGINT`).
- The interrupt signal will be handled by the `sigaction` that was registered in the `catch_signal()` function. 
- The `sigaction` contains a pointer to the `diediedie()` function.
- This will then be called and the program will display a message and `exit()`.

Code Execution:

```sh
chan@CMA:~/C_Programming/test$ ./final
Enter your name: ^CGoodbye cruel world....

```

#### Chapter 10 - Long Exercise

#### `hello.c` 

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <signal.h>
#include "hello.h"

void error(char *msg)
{
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(1);
}

void diediedie(int sig)
{
    puts("Goodbye cruel world....\n");
    exit(1);
}

void end_game(int sig)
{
    printf("\nFinal score: %d\n", score);
    exit(0);
}

int catch_signal(int sig, void (*handler)(int))
{
    struct sigaction action;
    action.sa_handler = handler;
    sigemptyset(&action.sa_mask);
    action.sa_flags = 0;
    return sigaction(sig, &action, NULL);
}

void times_up(int sig)
{
    puts("\nTIME'S UP!");
    raise(SIGINT);
}

```

`hello.h`

```C
extern int score;

void error(char *msg);

void diediedie(int sig);

void end_game(int sig);
int catch_signal(int sig, void (*handler)(int));

void times_up(int sig);
```

`main.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <time.h>
#include <signal.h>
#include "hello.h"

int main()
{
    catch_signal(SIGALRM, times_up);
    catch_signal(SIGINT, end_game);
    
    // This makes sure we get different random numbers each time
    srandom(time(0));
    while (1)
    {
        int a = random() % 11;
        int b = random() % 11;
        char txt[4];
        
        // Set the alarm to fire in 5 seconds.
        // As long as we go through the loop in less than 5 seconds, the timer will be reset and it will never fire.
        alarm(5);
        printf("\nWhat is %d times %d? ", a, b);
        fgets(txt, 4, stdin);
        
        // convert a string received from the user through fgets into an integer to compare this input to the product of a * b to determine if the user's answer is correct.
        int answer = atoi(txt);
        if (answer == a * b)
            score++;
        else
            printf("\nWrong! Score: %d\n", score);
    }
    return 0;
}

```

The `srandom(time(0))`function call in C is used to seed the random number generator with a starting point. Here's a breakdown:

- `srandom(unsigned int seed)`is a function that initializes the random number generator used by the `random()` function. The `seed` parameter is used to start the random number sequence. Different seeds produce different sequences of numbers generated by `random()`.
- `time(0)` returns the current time as the number of seconds elapsed since the Unix epoch (00:00:00 UTC on 1 January 1970). Passing `0` as an argument to `time()`means you're asking for the current time. This value changes every second, providing a simple way to get a "random" seed.

So, `srandom(time(0))` seeds the random number generator with the current time, ensuring that you get a different sequence of numbers from `random()` each time your program runs. This is useful for simulations, games, or any application requiring random number generation where you want to avoid the same sequence of numbers each time.

Code Execution:

```sh
chan@CMA:~/C_Programming/test
$ ./final

What is 6 times 0? 
TIME'S UP!

Final score: 0

chan@CMA:~/C_Programming/test$ ./final

What is 6 times 4? 24

What is 5 times 4? 20

What is 10 times 2? 20

What is 1 times 2? 2

What is 10 times 7? 70

What is 2 times 1? 3

Wrong! Score: 5

What is 5 times 0? ^C
Final score: 5
chan@CMA:~/C_Programming/test$ 
```

- The first testing, instead of hitting `CTRL-C`, we waited for at least five seconds on one of the answers and we can see that the alarm signal (`SIGALRM`) fires. The program was waiting for the user to enter an answer but because he took so long, the timer signal was sent; the process immediately displays "TIME'S UP!" message and then escalates the signal to a `SIGINT` that causes the program to display the final score.
- The second time, we hit `CTRL-C`, which sends the process an interrupt signal (`SIGINT`) that makes the program display the final score and then `exit()`.
- Signals are a little complex, but incredibly useful.
- They allow our programs to end gracefully, and the interval timer can help us deal with tasks that are taking too long.