### Chapter 10 - Opening a web page in a browser using system()

```C
void open_url(char *url){
    char launch[255];
    sprintf(launch, "x-www-browser '%s'", url);
    system(launch);
}

int main(){
    open_url("https://www.google.com");
    return 0;
}
```

The above code opens a web page in a browser on Linux.

For windows,

```C
sprintf(laung, "cmd /c start %s", url);
system(launch);
```

For Mac,

```C
sprintf(launch,"open '%s'", url);
system(launch);
```



Code Execution:

```sh
chan@CMA:~/C_Programming/test
$ ./final

[0702/213756.919081:ERROR:file_io_posix.cc(153)] open /home/chan/.config/google-chrome/Crash Reports/pending/20e95c88-a2fd-481d-9a23-075c5578e26a.lock: File exists (17)
Opening in existing browser session.

```

#### Chapter 10 - Exercise 2 (news_opener)



`hello.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/wait.h>
#include "hello.h"

void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerr(errno));
    exit(1);
}

void open_url(char *url){
    char launch[255];
    sprintf(launch, "x-www-browser '%s'", url);
    system(launch);
}
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
#include "hello.h"

int main(int argc, char *argv[]){
    if(argc < 2){
        error("Please provide a search phrase.")
    }
    
    char *phrase = argv[1];
    char *var[] = {"RSS_FEED=http://rss.cnn.com/rss/edition.rss", NULL};
    
    char *python_path = "/usr/bin/python3";
    char *script_path = "../HFC/chapter_9/RSS_Feeds/dogriffiths-rssgossip-f0bb346/rssgossip.py";
    
    int fd[2];
    
    // This will create the pipe and store its descriptors in fd[0] and fd[1]
    if(pipe(fd) == -1){
        error("Can't create the pipe.");
    }
    
    pid_t pid = fork();
    if(pid == -1){
        error("Can't fork process");
    }
    
    if(pid == 0){ // Child process
        close(fd[0]); // the child won't read from the pipe, so close the read end.
        
        if(dup2(fd[1], 1) == -1){ // This will set the Standard Output to the write end of the pipe
            error("Can't redirect Standard Output.");
        }
        
        if(execle(python_path, python_path, script_path, phrase, NULL, vars) == -1){
            error("Can't run script.");
        }
    }else{ // Parent process
        close(fd[1]); // close writing end in the parent because the parent won't write to it
        
        // Redirecting the Standard Inpuyt to the read end of the pipe
        if(dup2(fd[0], 0) == -1){
            error("Can't redirect Standard Input");
        }
        
        char line[255];
        
        // We will read from the stdin because that's connected to the pipe.
        // We could also put fd[0].
        while(fgets(line, 255, stdin)){
            printf("Received line: %s", line);
            if(line[0] == '\t'){
                open_url(line + 1); // open url if it starts with a tab
                // line + 1 is the string starting after the tab character.
            }else{
                printf("%s", line); // Print other lines to stdout
            }
        }
        
        close(fd[0]); // close reading end in the parent
        
        int pid_status;
        if(waitpid(pid, &pid_status, 0) == -1){
            error("Error waiting for child process");
            
            printf("Child process finished with status %d\n", pid_status);
        }
    }
    return 0;
}
```



Code Execution:

```sh
chan@CMA:~/C_Programming/test
$ make all

Compiling the main file
clang -Wall -Wextra -g -c main.c 
Linking and producing the final application
clang -Wall -Wextra -g main.o hello.o -o final

chan@CMA:~/C_Programming/test
$ ./final 'shares'

Received line: 'Scary, cold, hungry and lonely': Volunteer soldier shares experience on front line
'Scary, cold, hungry and lonely': Volunteer soldier shares experience on front line
Received line: 	https://www.cnn.com/videos/world/2023/04/03/soldiers-russia-ukraine-war-david-mckenzie-pkg-ovn-intl-ldn-vpx.cnn
[0702/223653.208950:ERROR:file_io_posix.cc(153)] open /home/chan/.config/google-chrome/Crash Reports/pending/20e95c88-a2fd-481d-9a23-075c5578e26a.lock: File exists (17)
Opening in existing browser session.
Received line: Emma Heming Willis shares family photos as daughter Mabel turns 11
Emma Heming Willis shares family photos as daughter Mabel turns 11
Received line: 	https://www.cnn.com/2023/04/03/entertainment/emma-willis-mabel-birthday-photos-intl-scli/index.html
[0702/223653.277338:ERROR:file_io_posix.cc(153)] open /home/chan/.config/google-chrome/Crash Reports/pending/20e95c88-a2fd-481d-9a23-075c5578e26a.lock: File exists (17)
Opening in existing browser session.
Child process finished with status 0
```

- The program ran the `rssgossip.py` in a separate process and told it to display URLs for each story it found.
- All of the output of the screen was redirected through a pipe that was connected to the parent process.
- Pipes are a great way of connecting processes together.
- Now, we have the ability to not only **run** processes and **control** their environments, but we also have a way of **capturing their output**.

