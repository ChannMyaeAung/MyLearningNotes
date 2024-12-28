## Chapter 6

#### Code Magnets - Exercise 1 Linked Lists

```C
#include <stdio.h>
#include <string.h>

typedef struct island{
    const char *name;
    const char *opens;
    const char *closes;
    struct island *next;
} island;

void display(island *start);

int main(void){
    island amity = {"Amity", "09:00", "17:00", NULL};
    island craggy = {"Craggy", "09:00", "17:00", NULL};
    island isla_nublar = {"Isla Nublar", "09:00", "17:00", NULL};
    island shutter = {"Shutter", "09:00", "17:00", NULL};
    
    amity.next = &craggy;
    craggy.next = &isla_nublar;
    isla_nublar.next = &shutter;
    
    island skull = {"Skull", "09:00", "17:00", NULL};
    isla_nublar.next = &skull;
    skull.next = &shutter;
    
    display(&amity);
    return 0;
}

void display(island *start){
    island *i = start;
    for(; i != NULL; i = i->next){
        printf("Name: %s open: %s-%s\n", i->name, i->opens, i->closes);
    }
}
```



```bash
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_1
$ make main
cc     main.c   -o main

chan@CMA:~/C_Programming/HFC/chapter_6/exercise_1
$ ./main
Name: Amity open: 09:00-17:00
Name: Craggy open: 09:00-17:00
Name: Isla Nublar open: 09:00-17:00
Name: Skull open: 09:00-17:00
Name: Shutter open: 09:00-17:00

```



Code Breakdown:

- The code starts with the definition of a `struct` called `island` with 4 fields: `name`, `opens`, `closes` and `next`. The `name`, `opens`, and `closes` are pointers to `char` which means they can hold the address of a string. The `next` field is a pointer to another `island` `struct`, which allows us to create a linked list of `island structs`.

- The display function takes a pointer to an `island` struct as an argument. 

  - The `island *i = start` is initializing a new pointer `i` to point to the same `island` struct that `start` is pointing to. The purpose of this line is to create a new pointer that can be used to traverse the linked list of islands without modifying the `start` pointer. This is important because the `start` pointer is needed to keep track of the beginning of the list. If you were to use `start` directly to traverse the list, you would lose the reference to the beginning of the list. 
  - In our code, the initialization part is empty, which is why we set a semicolon at the beginning of the for loop. This is because `i` is already initialized before the `for` loop `(island *i = start;)`.

  - It loops through the linked list of islands, starting from the given island and prints the `name`, `opens` and `closes` fields of each island. 
  - In the for loop that follows, `i` is updated to point to the next `island` in the list on each iteration `(i = i->next)`.
  - The loop continues until it reaches an island with `next` set to `NULL` which indicates the end of the list.

- In the main function, four `island` structs are created and initialized. The `next` field of each island is set to the address of the next island, creating a linked list.

- Then a new `island` struct `skull` is created and inserted into the list between `isla_nublar` and `shutter`.

- Finally the display function is called with `&amity` as the argument, which prints the details of all the islands in the linked list.



#### malloc() example

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

typedef struct island{
    const char *name;
    const char *opens;
    const char *closes;
    struct island *next;
} island;

void display(island *start);

island *create(char *name);

int main(){
    // predefined islands
    island amity = {"Amity", "09:00", "17:00", NULL};
    island craggy = {"Craggy", "09:00", "17:00", NULL};
    island isla_nublar = {"Isla Nublar", "09:00", "17:00", NULL};
    island shutter = {"Shutter", "09:00", "17:00", NULL};
    
    // Link islands
    amity.next = &craggy;
    craggy.next = &isla_nublar;
    isla_nublar.next = &shutter;
    
    // Adding a new island
    island skull = {"Skull", "09:00", "17:00", NULL};
    isla_nublar.next = &skull;
    skull.next = &shutter;
    
    // display the list of islands
    display(&amity);
    
    // Create new islands based on user input
    char name[80];
    // read name for the first island
    fgets(name, 80, stdin);
    // create the first island
    island *p_island0 = create(name);
    // read name for the second island
    fgets(name, 80, stdin);
    // create the second island
    island *p_island1 = create(name);
    // link the two newly created islands
    p_island0->next = p_island1;
    // display the newly created islands
    display(p_island0);
    
    return 0;
}

void display(island *start){
    island *i = start;
    
    for(;i != NULL; i = i->next){
        printf("Name: %s open: %s-%s\n", i->name, i->opens, i->closes);
    }
}

island *create(char *name){
    // allocate memory for the new island
    island *i = malloc(sizeof(island));
    // assign the name
    i->name = name;
    // default opening time
    i->opens = "09:00";
    // default closing time
    i->closes = "17:00";
    // initialize next to NULL
    i->next = NULL;
    
    // return the pointer to the new island
    return i;
}
```



We added a `create` function to create a new island `struct`, it uses dynamic memory allocation with malloc() to create the new island on the **heap** instead of the stack. 

- Memory on the heap persists until it's explicitly deallocated with free() so the island will continue to exist after create returns.
- `create` then returns a pointer to the new `island` on the heap.
- This is why the return type of `create` is `island*` (a pointer to an island) rather than island. The pointer allows us to access the new `island` on the heap after `create` returns.

But when we run the code, one issue remains:

Code Execution:

```bash
chan@CMA:~/C_Programming/practice
$ ./practice
Name: Amity open: 09:00-17:00
Name: Craggy open: 09:00-17:00
Name: Isla Nublar open: 09:00-17:00
Name: Skull open: 09:00-17:00
Name: Shutter open: 09:00-17:00
Atlantis
Titchmarsh Island
Name: Titchmarsh Island
 open: 09:00-17:00
Name: Titchmarsh Island
 open: 09:00-17:00

```

- The first island is now the same as the second one.
- What happened to the name of the first island?
- When the code records the name of the island, it doesn't take a copy of the whole name string; it just records the address where the name string lives in memory.
- The program asks the user for the name of each island, but both times it uses the name local `char` array to store the name.
- That means that the two islands share the same name string.
- As soon as the local variable gets updated with the name of the second island, the name of the first island changes as well.
- The `strdup()` function can reproduce a complete copy of the string somewhere on the heap:
- The `strdup()` function works out how long the string is, and then calls the malloc() function to allocate the correct number of characters on the heap.
- It then copies each of the characters to the new space on the heap.
- That means that `strdup()` always create space **on the heap**. It can't create space on the stack because that's for local variables, and local variables get cleared away too often.
- But because `strdup()` puts new strings on the heap, that means we must **always remember to release their storage with the free() function.**



#### Let's fix the code using the `strdup()` function

```C
island *create(char *name){
    island *i = malloc(sizeof(island));
    i->name = strdup(name);
    i->opens = "09:00";
    i->closes = "17:00";
    i->next = NULL;
    return i;
}
```

- We only need to put the `strdup()` function on the name field only because we are setting the opens and closes fields to string literals.
- String literals are stored in a **read-only** area of memory set aside for **constant values**.
- Because we always set the `opens` and `closes` fields to constant values, we don't need to take a defensive copy of them because they'll never change.
- But we had to take a defensive copy of the` name` array, because something might come and update it later.



```bash
chan@CMA:~/C_Programming/practice
$ make practice
cc     practice.c   -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
Name: Amity open: 09:00-17:00
Name: Craggy open: 09:00-17:00
Name: Isla Nublar open: 09:00-17:00
Name: Skull open: 09:00-17:00
Name: Shutter open: 09:00-17:00
Atlantis
Titchmarsh Island
Name: Atlantis
 open: 09:00-17:00
Name: Titchmarsh Island
 open: 09:00-17:00

```

Now that code works. Each time the user enters the name of an island, the create() function is storing it in a brand new string.



Q: If the island `struct` had a name array rather than a character pointer, would I need to use `strdup()` here?

A: No, each island `struct` would store its own copy, so you wouldn't need to make your own copy.



Q: So why would I want to use char pointers rather than char arrays in my data structures?

A: char pointers won't limit the amount of space you need to set aside for strings. If you use char arrays, you will need to decide in advance exactly how long your strings might need to be.



#### Pool Puzzle

```C
island *start = NULL;
island *i = NULL;
island *next = NULL;
char name[80];

// We'll keep looping until we don't get any more strings
for(; fgets(name, 80, stdin) != NULL; i = next){
    // This creates an island
    next = create(name);
    if(start == NULL){
        start = next;
    }
    if(i != NULL){
        i->next = next;
    } 
}
display(start);
```

Code Breakdown:

- `island *start = NULL`, This line initializes a pointer `start` to `NULL`. This pointer will eventually point to the first island in the list.
- `island *i = NULL`, This line initializes a pointer `i` to `NULL`. This pointer will be used to keep track of the current island in the list.
- `island *next = NULL`, This line initializes a pointer `next` to `NULL`. This pointer will be used to point to the next island to be added to the list.
- The for loop continues until we don't get any more strings from the Standard Input. At the end of each loop, set `i` to the next island we created which means that `i` always points to the last `island` that was added to the list.
- `next = create(name);` This line calls the `create` function with the name that was read from the standard input. The `create` function creates a new `island` with the given `name` and return a pointer to it. This pointer is then stored in `next`.
- `if(start == NULL) {start = next};` This `if` statement checks if `start` is `NULL`, which mean that the list is currently empty. If it is, `start` is set to `next`, which means that the first `island` in the list is the one that was just created. In other words, the first time through, `start` is set to `NULL`, so set it to the first island.
- `if (i != NULL) { i->next = next }` This if statement checks if `i` is not `NULL` which means that there is at least one island in the list. If there is, it sets the `next` field of the current `island` `i` to `next`, which adds the new `island` to the end of the list.



#### Free the memory when you're done

```C
void release(island *start){
    island *i = start;
    island *next = NULL;
    for(; i != NULL; i = next){
        next = i->next;
        free(i->name);
        free(i);
    }
}
```

Code Breakdown:

- `island *i = start;` - This line initializes a pointer `i` to `start`. This pointer will be used to traverse the list of islands.

- `island *next = NULL;` - This line initializes a pointer `next` to `NULL`. This pointer will be used to keep track of the next island in the list.

- `for (; i != NULL; i = next)` - This for loop continues as long as i is not NULL, which means that there are still islands left in the list. After each iteration, `i` is set to the current node and `next` will be set to the the next node before freeing the current node.

- `next = i->next;` - Inside the loop, this line sets `next` to `i->next`, which is the next island in the list. This is done before freeing `i` because freeing `i` will deallocate its memory, making `i->next` inaccessible.

  Before freeing the current node, save the pointer to the next node in `next`. This ensures that after freeing the current node, you still have a reference to the rest of the list.

- `free(i->name);` - This line frees the memory allocated for the name of the current island. The name was allocated with `strdup` in the create function, so it needs to be freed to avoid a memory leak.

- `free(i);` - This line frees the memory allocated for the current island. The island was allocated with `malloc` in the create function, so it needs to be freed to avoid a memory leak.

- After the for loop has finished, all memory allocated for the list of islands (both the islands themselves and their names) has been freed, and there are no memory leaks.



##### If we run the code

```bash
chan@CMA:~/C_Programming/practice
$ ./practice < trip1.txt
Name: Amity open: 09:00-17:00
Name: Craggy open: 09:00-17:00
Name: Isla Nublar open: 09:00-17:00
Name: Skull open: 09:00-17:00
Name: Shutter open: 09:00-17:00
Name: Delfino Isle
 open: 09:00-17:00
Name: Angel Island
 open: 09:00-17:00
Name: Wild Cat Island
 open: 09:00-17:00
Name: Neri's Island
 open: 09:00-17:00
Name: Great Todday
 open: 09:00-17:00
Name: Ramita de la Baya
 open: 09:00-17:00
Name: Island of the Blue Dolphins
 open: 09:00-17:00
Name: Fantasy Island
 open: 09:00-17:00
Name: Farne
 open: 09:00-17:00
Name: Isla de Muert
 open: 09:00-17:00
Name: Tabor Islandd
 open: 09:00-17:00
Name: Haunted Isle
 open: 09:00-17:00
Name: Sheena Island
 open: 09:00-17:00
chan@CMA:~/C_Programming/practice$ 
```

```bash
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_1$ 
make all
Compiling the main file...
gcc -c main.c 
Compiling the functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc main.o functions.o -o main 

chan@CMA:~/C_Programming/HFC/chapter_6/exercise_1$ 
./main < trip1.txt
Name: Amity open: 09:00-17:00
Name: Craggy open: 09:00-17:00
Name: Isla Nublar open: 09:00-17:00
Name: Skull open: 09:00-17:00
Name: Shutter open: 09:00-17:00
Name: Delfino Isle
 open: 09:00-17:00
Name: Angel Island
 open: 09:00-17:00
Name: Wild Cat Island
 open: 09:00-17:00
Name: Neri's Island
 open: 09:00-17:00
Name: Great Todday
 open: 09:00-17:00
Name: Ramita de la Baya
 open: 09:00-17:00
Name: Island of the Blue Dolphins
 open: 09:00-17:00
Name: Fantasy Island
 open: 09:00-17:00
Name: Farne
 open: 09:00-17:00
Name: Isla de Muert
 open: 09:00-17:00
Name: Tabor Islandd
 open: 09:00-17:00
Name: Haunted Isle
 open: 09:00-17:00
Name: Sheena Island
 open: 09:00-17:00
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_1$ 

```



It works. One thing worth noting in mind is that we had no way of knowing how long that file was going to be.

In this case, because we are just printing out the file, we could have simply printed it out without storing it all in memory. But because we do have it in memory, we're free to manipulate it. We could add in extra steps in the tour, or remove them. We could reorder to extend the tour.
