## Abstract data types

- Abstract data types are data types whose implementation is not visible to their user.
- From the outside, all the user knows about an ADT is what  operations can be performed on it and what those operations are supposed to do.
- ADTs have an outside and an inside.
  - The outside is called the **interface**. It consists of the minimal set of type and function declarations needed to use the ADT.
  - The inside is called the **implementation**. It consists of type and function definitions, and sometimes auxiliary data or helper functions, that are not visible to users of the ADT.
- This separation between interface and implementation is called an **abstraction barrier**, and allows the implementation to change without affecting the rest of the program.
- What joins the implementation to the interface is an **abstraction function**.
  - This is a function (in the mathematical sense) that takes any state of the implementation and trims off any irrelevant details to leave behind an idealized pictures of what the data type is doing.
  - For example, a linked list implementation translates to a sequence abstract data type by forgetting about the pointers used to hook up the elements and just keeping the sequence of elements themselves.
  - To exclude bad states of implementation (for example, a singly-linked list that loops back on itself instead of having a terminating null pointer), we may have a **representation invaraint**, which is just some property of the implementation that is always true.
    - Representation invariant are also useful for detecting when we've bungled our implementation, and a good debugging strategy for misbehaving abstract data type implementations is often to look for the first point at which they violated some property that we thought was an invariant. 

### A sequence type

```C
void seq_print(Sequence s, int limit){
    int i;
    
    for(i = seq_next(s); i < limit; i = seq_next(s)){
        printf("%d\n", i);
    }
}
```

- `seq_print` doesn't need to know anything at all about what a `Sequence` is or how `seq_next` works in order to print out all the values in the sequence until it hits one greater than or equal to limit. 
- This is a good thing. It means that we can use with any implementation of `Sequence` we like, and we don't have to change it if `Sequence` or `seq_next` changes.



### Interface

In C, the interface of an abstract data type will usually be declared in a header file, which is included both in the file that implements the ADT (so that the compiler can check that the declarations match up with the actual definition in the implementation).

```C
// stack.h
#ifndef STACK_H
#define STACK_H

// Forward declaration of the Stack structure (opaque pointer)
typedef struct Stack Stack;

// Function to create a new stack
Stack* createStack();

// Function to push an element onto the stack
void push(Stack* stack, int data);

// Function to pop an element from the stack
int pop(Stack* stack);

// Function to peek at the top element without removing it
int peek(Stack* stack);

// Function to check if the stack is empty
int isEmpty(Stack* stack);

// Function to destroy the stack and free memory
void destroyStack(Stack* stack);

#endif // STACK_H
```



### Implementation

The implementation of an ADT in C is typically contained in one (or sometimes more than one) `.c` file. This file can be compiled and linked into any program that needs to use the ADT.

```C
// stack.c
#include <stdio.h>
#include <stdlib.h>
#include "stack.h"

// Definition of the node structure
typedef struct Node {
    int data;
    struct Node* next;
} Node;

// Definition of the Stack structure
struct Stack {
    Node* top;
};

// Function to create a new stack
Stack* createStack() {
    Stack* stack = (Stack*) malloc(sizeof(Stack));
    if (!stack) {
        printf("Memory allocation failed.\n");
        exit(EXIT_FAILURE);
    }
    stack->top = NULL;
    return stack;
}

// Function to push an element onto the stack
void push(Stack* stack, int data) {
    if (!stack) return;
    Node* newNode = (Node*) malloc(sizeof(Node));
    if (!newNode) {
        printf("Memory allocation failed.\n");
        exit(EXIT_FAILURE);
    }
    newNode->data = data;
    newNode->next = stack->top;
    stack->top = newNode;
    printf("Pushed %d onto the stack.\n", data);
}

// Function to pop an element from the stack
int pop(Stack* stack) {
    if (!stack || !stack->top) {
        printf("Stack Underflow! Cannot pop.\n");
        exit(EXIT_FAILURE);
    }
    Node* temp = stack->top;
    int popped = temp->data;
    stack->top = temp->next;
    free(temp);
    printf("Popped %d from the stack.\n", popped);
    return popped;
}

// Function to peek at the top element without removing it
int peek(Stack* stack) {
    if (!stack || !stack->top) {
        printf("Stack is empty! Nothing to peek.\n");
        exit(EXIT_FAILURE);
    }
    return stack->top->data;
}

// Function to check if the stack is empty
int isEmpty(Stack* stack) {
    if (!stack) return 1;
    return (stack->top == NULL);
}

// Function to destroy the stack and free memory
void destroyStack(Stack* stack) {
    if (!stack) return;
    while (!isEmpty(stack)) {
        pop(stack);
    }
    free(stack);
    printf("Stack destroyed.\n");
}

```



### Using the ADT (Main Program)

```C
// main.c
#include <stdio.h>
#include "stack.h"

int main() {
    // Create a new stack
    Stack* stack = createStack();
    
    // Push elements onto the stack
    push(stack, 10);
    push(stack, 20);
    push(stack, 30);
    
    // Peek at the top element
    printf("Top element is %d\n", peek(stack)); // Output: 30
    
    // Pop elements from the stack
    pop(stack); // Output: Popped 30 from the stack.
    pop(stack); // Output: Popped 20 from the stack.
    
    // Check if stack is empty
    if (isEmpty(stack)) {
        printf("Stack is empty.\n");
    } else {
        printf("Stack is not empty.\n");
    }
    
    // Destroy the stack
    destroyStack(stack); // Output: Popped 10 from the stack.
                          //         Stack destroyed.
    
    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Pushed 10 onto the stack.
Pushed 20 onto the stack.
Pushed 30 onto the stack.
Top element is 30
Popped 30 from the stack.
Popped 20 from the stack.
Stack is not empty
Popped 10 from the stack.
Stack destroyed.
```

### Benefits of Using ADTs

1. Abstraction:
   - **Focus on Interface:** Users interact with ADTs through their defined operations without needing to understand their internal workings.
2. Modularity:
   - **Separate Implementation:** Changes to the ADT's implementation do not affect code that uses the ADT, as long as the interface remains consistent.
3. Reusability:
   - **Reusable Components:** Once an ADT is implemented, it can be reused across different projects and programs.
4. Maintainability:
   - **Easier Maintenance:** Bugs and enhancements can be addressed within the ADT's implementation without altering the user's code.
5. Data Hiding and Encapsulation:
   - **Protect Data Integrity:** By hiding internal data structures, ADTs prevent unintended interference, ensuring that data is accessed and modified only through defined operations.

------

### Summary

- **Abstract Data Types (ADTs)** are **theoretical constructs** that define data structures by their **operations** and **behaviors** rather than their implementation.
- **Benefits of ADTs:**
  - Promote **abstraction**, **modularity**, **reusability**, **maintainability**, and **data hiding**.
- **Common ADTs:** Stack, Queue, Linked List, Tree.
- **Implementing ADTs in C:**
  - Use **structures** to define data layouts.
  - Use **functions** to define operations.
  - Utilize **header files** (`.h`) for declaring interfaces.
  - Employ **opaque pointers** to hide implementation details and enforce encapsulation.
- **Example Implementation:** A Stack ADT using a singly linked list, demonstrating how to separate interface from implementation and use opaque pointers for data hiding.

**Key Takeaways:**

- **Understand the Interface:** Know what operations an ADT should support based on its definition and use cases.
- **Separate Interface and Implementation:** Use header files and source files to define interfaces and implement functionalities separately.
- **Encapsulate Data:** Hide internal data structures from users to protect data integrity and allow implementation flexibility.