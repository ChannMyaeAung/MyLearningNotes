#### Automating the Dear John letters

Imagine you're writing a mail-merge program to send out different types of messages to different people. One way of creating the data for each response is with a `struct` like this:

practice.h

```C 
enum response_type {
    DUMP,
    SECOND_CHANCE,
    MARRIAGE
};

typedef struct{
    char *name;
    enum response_type type;
} response;

void dump(response r);

void second_chance(response r);

void marriage(response r);
```

The `enum` gives you the names for each of the three types of response you'll be sending out, and that response type can be recorded against each response. Then you'll be able to use your new `response` data type by calling one of these three functions for each type of response.

functions.c

```C
void dump(response r){
    printf("Dear %s,\n", r.name);
    puts("Unfortunately your last data contacted us to");
    puts("say that they will not be seeing you again");
}

void second_chance(response r){
    printf("Dear %s,\n", r.name);
    puts("Good news: your last data has asked us to");
    puts("arrange another meeting. Please call ASAP.");
}

void marriage(response r){
    printf("Dear %s,\n", r.name);
    puts("Congratulations! Your last data has contacted");
    puts("us with a proposal of marriage.");
}
```



practice.c (main file)

```C
int main(void){
    response r[] = {
        {"Mike", DUMP}, {"Luis", SECOND_CHANCE}, {"Matt", SECOND_CHANCE}, {"William", MARRIAGE};
    };
    int i;
    for(i = 0; i < 4; i++){
        switch(r[i].type){ // Testing the type field each time
            case DUMP:
                dump(r[i]);
                break;
            case SECOND_CHANCE:
                second_chance(r[i]);
                break;
            default:
                marriage(r[i]);
        }
    }
    return 0;
}
```



Code Execution

```bash
chan@CMA:~/C_Programming/practice
$ ./practice
Dear Mike,
Unfortunately your last date contacted us to
say that they will not be seeing you again
Dear Luis,
Good news: your last date has asked us to
arrange another meeting. Please call ASAP.
Dear Matt,
Good news: your last date has asked us to
arrange another meeting. Please call ASAP.
Dear William,
Congratulations! Your last date has contacted
us with a proposal of marriage.

chan@CMA:~/C_Programming/practice$ 

```

Well, it's good that it worked but there is quite a lot of code in there just to call a function for each piece of `response` data.

And what will happen if you add a fourth response type? You'll have to change every section of your program that looks like this. Soon, you will have a lot of code to maintain and it might go wrong.



#### Create an array of function pointers

The trick is to create an array of function pointers that match the different response types.

```C
void (*replies[])(response) = {dump, second_chance, marriage};

// void - each function in the array will be a void function
// (*replies[]) - The variable will be called "replies" and it's not just a function pointer; it's a whole array of them.
// (response) - Just one parameter with type "response".

```

| Return type | (*Pointer variable) | (Param types) |
| ----------- | ------------------- | ------------- |
| void        | (*replies[])        | (response)    |

#### But how does an array help?

It contains a set of function names that are in exactly the same order as the types in the `enum`:

```C
enum response_type{
    DUMP,
    SECOND_CHANCE,
    MARRIAGE
};
```

This is really important because when C creates an `enum`, it gives each of the symbols a number starting at 0.

- So `DUMP == 0`, `SECOND_CHANCE == 1`, and `MARRIAGE == 2`. And that's really neat because it means you can get a pointer to one of your sets of functions using a `response_type`.

```C
replies[SECOND_CHANCE] = second_chance;

// replies - this is your "replies" array of functions
// SECOND_CHANCE - has the  value 1.
// second_chance - it's equal to the name of the second_chance function.

// void (*replies[])(response) = {dump, second_chance, marriage};
// replies[0] will run the funciton "dump"
// replies[1] will run the function "second_chance"
// replies[2] will run the function "marriage"

// replies[0] == DUMP
// replies[1] == SECOND_CHANCE
// replies[2] == MARRIAGE .Since enum gives each of the symbols a number starting at 0 as mentioned above.
```



#### Updated function for Exercise 4

practice.c (main file)

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"

void (*replies[])(response) = {dump, second_chance, marriage};

int main(void){
    response r[] = {
        {"Mike", DUMP}, {"Luis", SECOND_CHANCE}, {"Matt", SECOND_CHANCE}, {"William", MARRIAGE}
    };
    int i;
    for(i = 0; i < 4; i++){
        (replies[r[i].type])(r[i]);
        // adding a * after the opening parenthesise will also work the same way.
        // (*replies[r[i].type])(r[i])
    }
}
```



Code Execution:

```bash
chan@CMA:~/C_Programming/practice
$ make all
Compiling practice file...
gcc -c practice.c 
Compiling the final exe file...
gcc practice.o functions.o -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
Dear Mike,
Unfortunately your last date contacted us to
say that they will not be seeing you again
Dear Luis,
Good news: your last date has asked us to
arrange another meeting. Please call ASAP.
Dear Matt,
Good news: your last date has asked us to
arrange another meeting. Please call ASAP.
Dear William,
Congratulations! Your last date has contacted
us with a proposal of marriage.

chan@CMA:~/C_Programming/practice$
```



Now instead of an entire `switch` statement, you just have this: 

```C
(replies[r[i].type])(r[i]);
```

If we have to call the response functions at several places in the program, we won't have to copy a lot of code. 

And if we decide to add a new type and a new function, we can just add it to the array:

```C
enum response_type {
    DUMP,
    SECOND_CHANCE,
    MARRIAGE,
    LAW_SUIT
};

void (*replies[])(response) = {dump, second_chance, marriage, law_suit};
```

Arrays of function pointers can make our code much easier to manage. They are designed to make our code scalable by making it shorter and easier to extend.

