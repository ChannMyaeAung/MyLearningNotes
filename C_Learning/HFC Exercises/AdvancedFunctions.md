### Chapter 7 - Advanced Functions

### Exercises



#### Exercise - 1

Complete the find() function so it can track down all the sports fans in the list who don't also share a passion for Bieber.

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

nt NUM_ADS = 7;
char *ADS[] = {
    "William: SBM GSOH likes sports, TV, dining",
    "Matt: SWM NS likes art, movies, theater",
    "Luis: SLM ND likes books, theater, art",
    "Mike: DWM DS likes trucks, sports and bieber",
    "Peter: SAM likes chess, working out and art",
    "Josh: SJM likes sports, movies and theater",
    "Jed: DBM likes theater, books and dining"};

void find();

int main(){
    find();
    return 0;
}

void find(){
    int i;
    puts("Search results:");
    puts("--------------------");
    
    for(i = 0; i < NUM_ADS; i++){
        if(strstr(ADS[i], "sports") && !strstr(ADS[i], "bieber")){
            printf("%s\n", ADS[i]);
        }
    }
    puts("--------------------");
}
```

Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_7/exercise_1
$ make main
cc     main.c   -o main

chan@CMA:~/C_Programming/HFC/chapter_7/exercise_1
$ ./main
Search results: 
---------------------------
William: SBM GSOH likes sports, TV, dining
Josh: SJM likes sports, movies and theater
---------------------------

chan@CMA:~/C_Programming/HFC/chapter_7/exercise_1$ 

```



#### Exercise 2

practice.h

```C
extern int num_ADS;
extern char *ADS[];

int sports_no_bieber(char *s);

int sports_or_workout(char *s);

int ns_theater(char *s);

int arts_theater_or_dining(char *s);

void find(int (*match)(char*));

```

 `void find(int (*match)(char *))` -`find` is a function that takes one argument: a pointer to a function. This function, in turn, accepts a `char *` as its parameter and returns an `int`.

functions.c

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"

int NUM_ADS = 7;
char *ADS[] = {
    "William: SBM GSOH likes sports, TV, dining",
    "Matt: SWM NS likes art, movies, theater",
    "Luis: SLM ND likes books, theater, art",
    "Mike: DWM DS likes trucks, sports and bieber",
    "Peter: SAM likes chess, working out and art",
    "Josh: SJM likes sports, movies and theater",
    "Jed: DBM likes theater, books and dining"
};

int sports_no_bieber(char *s){
    return strstr(s, "sports") && !strstr(s, "bieber");
}

int sports_or_workout(char *s){
    return strstr(s, "sports") || strstr(s, "working out");
}

int ns_theater(char *s){
    return strstr(s, "NS") && strstr(s, "theater");
}

int arts_theater_or_dining(char *s){
    return strstr(s, "art") || strstr(s, "theater") || strstr(s, "dining");
}

void find(int (*match)(char *)){
    int i;
    puts("Search result: ");
    puts("--------------------");
    for(i = 0; i < NUM_ADS; i++){
        if(match(ADS[i])){
            printf("%s\n", ADS[i]);
        }
    }
    puts("--------------------");
    printf("\n");
}
```

practice.c (main file)

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"
int main(){
    printf("Sports but no Bieber:\n");
    find(sports_no_bieber);
    
    printf("Sports or workout:\n");
    find(sports_or_workout);
    
    printf("non-smoking and theater:\n");
    find(ns_theater);
    
    printf("Arts, or theater or dining:\n");
    find(arts_theater_or_dining);
    
    return 0;
}
```

Code Execution

```bash
chan@CMA:~/C_Programming/practice
$ make all
Compiling functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc practice.o functions.o -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
Sports but no Bieber: 
Search result: 
--------------------
William: SBM GSOH likes sports, TV, dining
Josh: SJM likes sports, movies and theater
--------------------

Sports or workout:
Search result: 
--------------------
William: SBM GSOH likes sports, TV, dining
Mike: DWM DS likes trucks, sports and bieber
Peter: SAM likes chess, working out and art
Josh: SJM likes sports, movies and theater
--------------------

non-smoking and theater:
Search result: 
--------------------
Matt: SWM NS likes art, movies, theater
--------------------

Arts, or theater or dining:
Search result: 
--------------------
William: SBM GSOH likes sports, TV, dining
Matt: SWM NS likes art, movies, theater
Luis: SLM ND likes books, theater, art
Peter: SAM likes chess, working out and art
Josh: SJM likes sports, movies and theater
Jed: DBM likes theater, books and dining
--------------------


```



### Exercise - 3

practice.h

```C
int compare_scores(const void *score_a, const void *score_b);

int compare_scores_desc(const void *score_a, const void *score_b);

int compare_areas(const void *a, const void *b);

int compare_names(const void *a, const void *b);

int compare_areas_desc(const void *a, const void *b);

int compare_names_desc(const void *a, const void *b);
```

functions.c

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"

// Sort integer scores with the smalles number first
int compare_scores(const void *score_a, const void *score_b){
    int a = *(int *)score_a;
    int b = *(int *)score_b;
    return a - b;
}

// Sort integer scores with the largest number first
int compare_scores_desc(const void *score_a, const void *score_b){
    int a = *(int *)score_a;
    int b = *(int *)score_b;
    
    // If we subtract the numbers the other way around, we'll reverse the order of the final sort
    return b - a;
}

typedef struct{
    int width;
    int height;
} rectangle;

// sort the rectangles in area order, smallest first
int compare_areas(const void *a, const void *b){
    // First convert the pointers to the correct type.
    rectangle *ra = (rectangle *)a;
    rectangle *rb = (rectangle *)b;
    
    // Then calculate the areas.
    int area_a = (ra->width * ra->height);
    int area_b = (rb->width * rb->height);
    
    // Then use the subtraction trick
    return area_a - area_b;
}

// Sort the list of names in alphabetical order. case-sensitive
int compare_names(const void *a, const void *b){
    // A string is a pointer to a char, so the pointers you're given are pointers to pointers.
    char **sa = (char **)a;
    char **sb = (char **)b;
    
    // We need to use the * operator to find the actual strings.
    return strcmp(*sa,*sb);
}

// Sort the rectangles in area order, largest first.
int compare_areas_desc(const void *a, const void *b){
    return compare_areas(b, a);
    
    // Or you could have used -compare_areas(a,b);
}


// Sort a list of names in reverse alphabetical order. case-sensitive
int compare_names_desc(const void *a, const void *b){
    return compare_names(b, a);
    
    // Or you could have used -compare_names(a,b);
}
```

practice.c

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"

int main(void){
    int scores[] = {543, 323, 32, 554, 11, 3, 112};
    int i;
    qsort(scores, 7, sizeof(int), compares_scores_desc);
    puts("These are the scores in order: ");
    for(i = 0; i < 7; i++){
        printf("Score = %i\n", scores[i]);
    }
    
    char *names[] = {"Karen", "Mark", "Brett", "Molly"};
    qsort(names, 4, sizeof(char *), compare_names);
    puts("These are the names in order: ");
    for(i = 0; i < 4; i++){
        printf("%s\n", names[i]);
    }
    return 0;
}
```

To get the number of elements in the array, instead of manually typing it, we can divide **the total size of the array** by **the size of one element**.

```C
int num_elements = sizeof(scores) / sizeof(scores[0]);
```



Code Execution:

```bash
chan@CMA:~/C_Programming/practice
$ make all
Compiling practice file...
gcc -c practice.c 
Compiling functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc practice.o functions.o -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
These are the scores in order: 
Score = 554
Score = 543
Score = 323
Score = 112
Score = 32
Score = 11
Score = 3
These are the names  in order: 
Brett
Karen
Mark
Molly

```

**Note: Sorting explanation in `HFCTheories.md`**

Q: I don't understand the comparator function for the array of strings. What does `char **` mean?

Q: Each item in a string array is a `char` pointer `(char *)`. When `qsort()` calls the comparator function, it sends pointers to two elements in the array. That means the comparator receives two pointers-to-pointers-to-char. In C notation, each value is a `char **`.



Q: OK, but when I call the `strcmp()` function, why does the code say `strcmp(*a, *b)` ? Why not `strcmp(a,b)`?

A: `a` and `b` are of type `char **`. The `strcmp()` function needs values of type `char*`.



Q: Does `qsort()` create  a sorted version of an array?

A: It doesn't make a copy, it actually modifies the original array.

