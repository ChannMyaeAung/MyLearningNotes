#### Chapter 6 Final Exercise - SPIES Program

header.h

```C
typedef struct node {
    char *question;
    struct node *no;
    struct node *yes;
} node;

int yes_no(char *question);
node *create(char *question);
void release(node *n);
```

main.c

```C
#include <stdio.h>
#include <string.h>
#include "header.h"

int main(void) {
    char question[80];
    char suspect[20];

    // Create the initial decision tree
    node *start_node = create("Does suspect have a mustache");
    start_node->no = create("Loretta Barnsworth");
    start_node->yes = create("Vinny the Spoon");

    // Pointer to traverse the tree
    node *current;

    // do-while loop that continues running based on the user's response to "Run again".
    do {
        // Initialize current to point to the root node at the start of each iteration.
        current = start_node;
        while (1) {
            // Ask the current question.
            if (yes_no(current->question)) {
                // If the answer is yes and the 'yes' child exists, move to the 'yes' child.
                if (current->yes) {
                    current = current->yes;
                } else {
                    // If a suspect is identified (current->yes or current->no is NULL), print "SUSPECT IDENTIFIED".
                    printf("SUSPECT IDENTIFIED\n");
                    break;
                }
            } else if (current->no) {
                // If the answer is no and the 'no' child exists, move to the 'no' child.
                current = current->no;
            } else {
                // If the suspect is not identified, prompt the user for the new suspect's name and a distinguishing question.
                printf("Who's the suspect? ");
                fgets(suspect, 20, stdin);
                suspect[strcspn(suspect, "\n")] = '\0';  // Remove newline character

                // Make the yes-node the new suspect name.
                //This process effectively adds a new branch to the decision tree for the new suspect. The new question becomes the question in the current node, the 'yes' child becomes the new suspect, and the 'no' child becomes the old suspect/question.
                node *yes_node = create(suspect);
                current->yes = yes_node;

                // Make the no-node a copy of this question.
                node *no_node = create(current->question);
                current->no = no_node;

                // Then replace this question with the new question.
                printf("Give me a question that is TRUE for %s but not for %s? ", suspect, current->question);
                fgets(question, 80, stdin);
                question[strcspn(question, "\n")] = '\0';  // Remove newline character
                free(current->question);
                current->question = strdup(question);
                break;
            }
        }
    } while (yes_no("Run again"));

    // Release memory for the entire tree.
    release(start_node);

    return 0;
}

```

Code Breakdown:

1. **Create Initial Tree:**

   - `node *start_node = create("Does suspect have a mustache");`: Creates the root node with the initial question.

   - `start_node->no = create("Loretta Barnsworth");`: Creates a child node for the "no" answer.

   - `start_node->yes = create("Vinny the Spoon");`: Creates a child node for the "yes" answer.

2. **Traversal and User Interaction:**

   - The do-while loop ensures the program runs repeatedly based on user input.
   - `current = start_node;`: Sets current to the root node at the start of each iteration.
   - Inside the `while(1)` loop, the program:
   - Asks the current question using `yes_no(current->question)`.
   - Moves to the `yes` or `no` child node based on the user's response.
   - If a leaf node is reached (no child nodes), it prompts for a new suspect and question.
   - Updates the tree with the new suspect and question.

3. **Prompt for New Suspect and Question:**
   - `fgets(suspect, 20, stdin);`: Reads the new suspect's name from input.
   - `suspect[strcspn(suspect, "\n")] = '\0';`: Removes the newline character.
   - Creates `yes_node` for the new suspect and updates `current->yes`.
   - Creates `no_node` as a copy of the current question and updates `current->no`.
   - Prompts for a new question distinguishing the new suspect and updates `current->question`.

4. **Loop Continuation:**
   - `while (yes_no("Run again"));`: Asks if the user wants to run the program again.

5. **Release Memory:**
   - `release(start_node);`: Frees all allocated memory for the tree nodes.

functions.c

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "header.h"

int yes_no(char *question) {
    char answer[3];
    printf("%s? (y/n): ", question);
    // if the user choose to terminate the program, exit.
    if (fgets(answer, 3, stdin) == NULL) {
        printf("\n");
        exit(0);
    }
    // returns 1 (true) if the first character of the user's answer is 'y', and 0 (false) otherwise. This is how the function determines whether the user answered 'yes' or 'no' to the question.
    return answer[0] == 'y' || answer[0] == 'Y';
}

node *create(char *question) {
    // Allocate memory for a new node.
    node *n = malloc(sizeof(node));
    // Copy the question string into the node.
    n->question = strdup(question);
    // Initializes yes and no pointers to NULL.
    n->no = NULL;
    n->yes = NULL;
    //Return the newly created node
    return n;
}

void release(node *n) {
    // Recursively free memory for all nodes in the tree.
    if (n) {
        if (n->no) {
            release(n->no);
        }
        if (n->yes) {
            release(n->yes);
        }
        
        // Free the question string and the node itself.
        if (n->question) {
            free(n->question);
        }
        free(n);
    }
}

```



Makefile

```makefile
all: spies

spies: main.o functions.o 
	@echo "Compiling the final exe file..."
	gcc -g main.o functions.o -o spies 

main.o: main.c 
	@echo "Compiling the main file..."
	gcc -c main.c 

functions.o: functions.c 
	@echo "Compiling the functions file..."
	gcc -c functions.c 

clean: 
	@echo "Removing everything except the source files..."
	@rm spies main.o functions.o
```



Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_2
$ make all
Compiling the functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc -g main.o functions.o -o spies 

chan@CMA:~/C_Programming/HFC/chapter_6/exercise_2
$ valgrind --leak-check=full ./spies
==23531== Memcheck, a memory error detector
==23531== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==23531== Using Valgrind-3.18.1 and LibVEX; rerun with -h for copyright info
==23531== Command: ./spies
==23531== 
Does suspect have a mustache? (y/n): n
Loretta Barnsworth? (y/n): n
Who's the suspect? Hayden Fantucci
Give me a question that is TRUE for Hayden Fantucci
 but not for Loretta Barnsworth? Has a facial scar
Run again? (y/n): y
Does suspect have a mustache? (y/n): n
Has a facial scar
? (y/n): Y
Hayden Fantucci
? (y/n): y
SUSPECT IDENTIFIED
Run again? (y/n): Y
Does suspect have a mustache? (y/n): y
Vinny the Spoon? (y/n): y
SUSPECT IDENTIFIED
Run again? (y/n): N
==23531== 
==23531== HEAP SUMMARY:
==23531==     in use at exit: 19 bytes in 1 blocks
==23531==   total heap usage: 13 allocs, 12 frees, 2,287 bytes allocated
==23531== 
==23531== 19 bytes in 1 blocks are definitely lost in loss record 1 of 1
==23531==    at 0x4848899: malloc (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
==23531==    by 0x491658E: strdup (strdup.c:42)
==23531==    by 0x10950A: create (in /home/chan/C_Programming/HFC/chapter_6/exercise_2/spies)
==23531==    by 0x10928B: main (in /home/chan/C_Programming/HFC/chapter_6/exercise_2/spies)
==23531== 
==23531== LEAK SUMMARY:
==23531==    definitely lost: 19 bytes in 1 blocks
==23531==    indirectly lost: 0 bytes in 0 blocks
==23531==      possibly lost: 0 bytes in 0 blocks
==23531==    still reachable: 0 bytes in 0 blocks
==23531==         suppressed: 0 bytes in 0 blocks
==23531== 
==23531== For lists of detected and suppressed errors, rerun with: -s
==23531== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_2$ 

```



Visualization

```plaintext
"Does suspect have a mustache?"
|
├── yes: "Vinny the Spoon"
|
└── no: "Loretta Bransworth"
```



```plaintext
Does suspect have a mustache?
├── Yes: Vinny the Spoon
└── No: Has a facial scar?
    ├── Yes: Hayden Fantucci
    └── No: Loretta Barnsworth

```



But there's one problem. It was using almost **twice the amount of memory** it needed.

We have managed to detect it using `valgrind` tool which is used on the **Linux** OS. It can monitor the pieces of data that are allocated space on the heap. 

- It works by creating its own **fake version of malloc()**.
- When our program wants to allocate some heap memory, `valgrind` will intercept our calls to `malloc()` and `free()` and run its own versions of those functions.
- The `valgrind` version of `malloc()` will take note of which piece of code is calling it and which piece of memory it allocated.
- When the program ends, `valgrind` will report back on any data that was left on the heap and tell us where in our code the data was created.
- As we can see in our **makefile**, we used `gcc -g main.o functions.o -o spies` to tells the compiler to record the line numbers against the code it compiles.



If we look closely,

```bash
==23531==    definitely lost: 19 bytes in 1 blocks

==23531==   total heap usage: 13 allocs, 12 frees, 2,287 bytes allocated

==23531==    at 0x4848899: malloc (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)

==23531==    by 0x491658E: strdup (strdup.c:42)

==23531==    by 0x10950A: create (in /home/chan/C_Programming/HFC/chapter_6/exercise_2/spies)

==23531==    by 0x10928B: main (in /home/chan/C_Programming/HFC/chapter_6/exercise_2/spies)
```

- 19 bytes of memory were allocated but not freed.

- Looks like we allocated new pieces of memory 13 times, but freed only 12 of them.


Now that we've collected quite a few pieces of evidence.

1. Location

   We ran the code two times. The first time, there was no problem. The memory leak only happened when we entered a new suspect name. That means the leak can't be in the code that ran the first time. The problem lies in this section of code.

   ```C
   else if (current->no)
               {
                   current = current->no;
               }
               // If the suspect is not identified, prompt the user for the new suspect's name and a distinguishing question.
               // Create new nodes for the suspect and update the current node's 'yes' and 'no' pointers accordingly.
               else
               {
                   // Make the yes-node the new suspect name
                   printf("Who's the suspect? ");
                   fgets(suspect, 20, stdin);
   
                   node *yes_node = create(suspect);
                   current->yes = yes_node;
   
                   // Make the no-node a copy of this question
                   node *no_node = create(current->question);
                   current->no = no_node;
   
                   // Then replace this question with the new question
                   printf("Give me a question that is TRUE for %s but not for %s? ", suspect, current->question);
                   fgets(question, 80, stdin);
                   free(current->question);
                   current->question = strdup(question);
                   break;
               }
   ```

2. Clues from valgrind

   When we ran the code through valgrind and added a single suspect, the program allocated 13 times, but only released 12 times. It told us that there were 19 bytes of data left on the heap when the program ended.

   - How many pieces of data were left on the heap?

     - There is one piece of data.

   - What was the piece of data left on the heap?

     - The string "Loretta Bransworth". It's 18 characters with a string terminator.

   - Which line or lines of code caused the leak?

     - The create() functions themselves don't cause leaks because they didn't on the first pass, so it must be this strdup() line:

     - ```C
       current->question = strdup(question);
       ```

   - How do we plug the leak?

     - If current->question is already pointing to something on the heap, free that before allocating a new question:
     - free(current->question);
     - current->question = strdup(question);



Now that we have added the fix to the code, it's time to run the code through valgrind again.

```bash
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_2
$ valgrind --leak-check=full ./spies
==7591== Memcheck, a memory error detector
==7591== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==7591== Using Valgrind-3.18.1 and LibVEX; rerun with -h for copyright info
==7591== Command: ./spies
==7591== 
Does suspect have a mustache? (y/n): n
Loretta Barnsworth? (y/n): n
Who's the suspect? Hanna
Give me a question that is TRUE for Hanna
 but not for Loretta Barnsworth? Has a facial scar
Run again? (y/n): y
Does suspect have a mustache? (y/n): n
Has a facial scar
? (y/n): y
Hanna
? (y/n): y
SUSPECT IDENTIFIED
Run again? (y/n): n
==7591== 
==7591== HEAP SUMMARY:
==7591==     in use at exit: 0 bytes in 0 blocks
==7591==   total heap usage: 13 allocs, 13 frees, 2,277 bytes allocated
==7591== 
==7591== All heap blocks were freed -- no leaks are possible
==7591== 
==7591== For lists of detected and suppressed errors, rerun with: -s
==7591== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

chan@CMA:~/C_Programming/HFC/chapter_6/exercise_2$ 

```

The leak is fixed.

- Spot when leaks happen.
- Identify the location where they happen.
- Check to make sure the leak is fixed.



Q: valgrind said the leaked memory was created on line 46, but the leak was fixed on a completely different line. How come?

A: The "Loretta..." data was put onto the heap on line 46, but the leak happened when the variable pointing to it (current->question) was reassigned without freeing it. Leaks don't happen when data is created; they happen when the program loses all references to the data.



Q: How does valgrind intercept calls to malloc() and free()?

A: The `malloc()` and `free()` functions are contained in the C Standard Library. But valgrind contains a library with its own versions of `malloc()` and `free()`. When you run a program with valgrind, your program will be using valgrind's functions, rather than the ones in the C Standard Library.



Q: Why doesn't the compiler always include debug information when it compiles code?

A: Because debug information will make your executable larger and it may also make your program slightly slower.