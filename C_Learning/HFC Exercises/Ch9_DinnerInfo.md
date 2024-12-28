#### Exercise 3 - Chapter 9 (diner info)

`diner_info.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){
    printf("We are inside diner_info.c.\n");
    printf("PID of diner_info.c: %d\n", getpid());
    
    char *my_env[] = {"JUICE=peach and apple", NULL};
    
    char *args[] = {"./main", "Fears", "quiet", "magic", "4", NULL};
    
    if(execve("./main", args, my_env)){
        perror("execve failed!");
    }
    
    printf("Back in diner_info.c.\n");
    
    return 0;
}
```



`main.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[]){
    printf("We are now inside main.c.\n");
    printf("PID of main.c: %d\n", getpid());
    
    int i;
    for(i = 0; i < argc; i++){
        printf("argv[%d] = %s\n", i, argv[i]);
    }
    
    printf("Diners: %s\n", argv[4]);
    printf("Juice: %s\n", getenv("JUICE"));
    
    return 0;
}
```

`Makefile`

```makefile
CC = clang
CFLAGS = -Wall -Wextra -g

main: main.c 
	$(CC) $(CFLAGS) main.c -o main

diner_info: diner_info.c
	$(CC) $(CFLAGS) diner_info.c -o diner_info
	
clean:
	rm -f main diner_info
```



Code Execution:

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/diner_info
$ make main
clang -Wall -Wextra -g main.c -o main

chan@CMA:~/C_Programming/HFC/chapter_9/diner_info
$ make diner_info
make: 'diner_info' is up to date.

chan@CMA:~/C_Programming/HFC/chapter_9/diner_info
$ ./diner_info

We are now inside diner_info.c
PID of diner_info.c: 6543
We are now inside main.c.
PID of main.c: 6543
argv[0]: ./main
argv[1]: Fears
argv[2]: quiet
argv[3]: magic
argv[4]: 4
Diners: 4
Juice: peach and apple

chan@CMA:~/C_Programming/HFC/chapter_9/diner_info$ 


```

