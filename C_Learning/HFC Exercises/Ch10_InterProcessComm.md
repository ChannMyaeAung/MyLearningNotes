 ### Chapter 10 - Interprocess communication



#### Chapter 10 - Exercise 1

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <sys/type.h>
#include <sys/wait.h>

void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(1);
    // exit(1) will terminate our program with status 1 immediately.
}

int main(int argc, char *argv[]){
    // Check if the correct number of arguments is provided.
    if(argc < 2){
        error("Please provide a search phrase.");
    }
    
    // Get the search phrase from the command line arguments
    char *phrase = argv[1];
    
    // Set up an environment variable for the RSS feed URL
    char *vars[] = {"RSS_FEED=http://rss.cnn.com/rss/edition.rss", NULL};
    
    // Open a file called "stories.txt" for writing
    FILE *f = fopen("stories.txt", "w");
    
    // If opening the file fails, it calls the error function with a relavant message.
    if(!f){
        error("Can't open stories.txt");
    }
    
    // Create a new process by forking
    pid_t pid = fork();
    if(pid == -1)
        error("Can't fork process");
    
    // The child process code block
    if(!pid){
        
        // Print debug message indicating the child process has started
        printf("Child process starting.\n");
        
        // Redirect standard output to the file "stories.txt"
        if(dup2(fileno(f), 1) == -1){
            error("Can't redirect Standard Output");
        }
        
        // Print debug message with the search phrase
        printf("Executing script with phrase: %s\n", phrase);
        
        // Execute the Python script with the given phrase and environment variables
        if(execle("/usr/bin/python3", "/usr/bin/python3", "./dogriffiths-rssgossip-f0bb346/rssgossip.py", phrase, NULL, vars) == -1){
            error("Can't run script");
        }
    }
    
    // Wait for the child process to finish
    int pid_status;
    if(waitpid(pid, &pid_status, 0) == -1){
        error("Error waiting for child process");
    }
    
    // Print debug message indicating the child process has finished
    printf("Child process finished with status %d\n", pid_status);
    
    // Close the file.
    fclose(f);
    return 0;
    
}
```

- `if (!pid)` is equivalent to `if (pid == 0)`, meaning it executes only when `fork()` returns 0 (indicating the child process).



Code Execution:

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/RSS_Feeds
$ make newshound
cc     newshound.c   -o newshound

chan@CMA:~/C_Programming/HFC/chapter_9/RSS_Feeds
$ ./newshound
Please provide a search phrase.: Success

chan@CMA:~/C_Programming/HFC/chapter_9/RSS_Feeds
$ ./newshound "shares"
Child process starting.
Child process finished with status 0

chan@CMA:~/C_Programming/HFC/chapter_9/RSS_Feeds
$ cat stories.txt
Executing script with phrase: shares
'Scary, cold, hungry and lonely': Volunteer soldier shares experience on front line
Emma Heming Willis shares family photos as daughter Mabel turns 11

```

- When the program opened the `stories.txt` file with `fopen()`, the OS registered the file `f` in the descriptor table.
- `fileno(f)` was the descriptor number it used.
- The `dup2()` function set the Standard Output descriptor(1) to point to the same file.

The program will change the descriptor table in the child script to look like this:

| #    | Data stream      |
| ---- | ---------------- |
| 0    | The keyboard     |
| 1    | File stories.txt |
| 2    | The screen       |
| 3    | File stories.txt |