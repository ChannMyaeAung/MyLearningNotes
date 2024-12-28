## Stacks

- A **Stack** is a linear data structure that follows the **Last In, First Out (LIFO)** principle. Think of it like a stack of plates: we add (push) plates to the top and remove (pop) plates from the top.
- When we pop, we get the last item we pushed.
- A Stack is an abstract data type that supports operations `push` (insert a new element on top of the stack) and `pop` (remove and return the element at the top of the stack).
- Below is an example of a simple linked-list implementation of a stack.

### Stack Operations

1. **Push:** Add an element to the top of the stack.
2. **Pop:** Remove the top element from the stack.
3. **Peek (Top):** View the top element without removing it.
4. **IsEmpty:** Check if the stack is empty.



### C Code Example #1

#### Program Code

`main.c`

```C
#include <stdio.h>
#include <stdlib.h>

// Definition of a stack node
struct StackNode {
    int data;
    struct StackNode* next;
};

typedef struct StackNode SN;

// Function to create a new stack node
SN* createNode(int data) {
    struct StackNode* newNode = (SN*) malloc(sizeof(SN));
    if (!newNode) { // Check for malloc failure
        printf("Memory allocation error\n");
        exit(1);
    }
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Push operation: Add an element to the stack
void push(SN** top_ref, int new_data) {
    SN* new_node = createNode(new_data);
    new_node->next = *top_ref; // Link the old stack to the new node
    *top_ref = new_node;       // Update the top pointer
    printf("Pushed %d to stack\n", new_data);
}

// Pop operation: Remove an element from the stack
int pop(SN** top_ref) {
    if (*top_ref == NULL) {
        printf("Stack Underflow\n");
        exit(1);
    }
    SN* temp = *top_ref;
    int popped = temp->data;
    *top_ref = temp->next; // Move the top pointer to the next node
    free(temp);            // Free memory
    return popped;
}

// Peek operation: Get the top element without removing it
int peek(SN* top) {
    if (top == NULL) {
        printf("Stack is empty\n");
        exit(1);
    }
    return top->data;
}

// Check if the stack is empty
int isEmpty(SN* top) {
    return (top == NULL);
}

// Function to print the stack
void printStack(SN* top) {
    printf("Stack: ");
    while (top != NULL) {
        printf("%d -> ", top->data);
        top = top->next;
    }
    printf("NULL\n");
}

// Driver program to test the stack operations
int main() {
    SN* stack = NULL; // Initialize stack as empty

    push(&stack, 10);
    push(&stack, 20);
    push(&stack, 30);

    printStack(stack); // Expected Output: 30 -> 20 -> 10 -> NULL

    printf("Top element is %d\n", peek(stack)); // Expected Output: 30

    printf("Popped element: %d\n", pop(&stack)); // Expected Output: 30
    printStack(stack); // Expected Output: 20 -> 10 -> NULL

    printf("Popped element: %d\n", pop(&stack)); // Expected Output: 20
    printStack(stack); // Expected Output: 10 -> NULL

    printf("Is stack empty? %s\n", isEmpty(stack) ? "Yes" : "No"); // Expected Output: No

    printf("Popped element: %d\n", pop(&stack)); // Expected Output: 10
    printStack(stack); // Expected Output: NULL

    printf("Is stack empty? %s\n", isEmpty(stack) ? "Yes" : "No"); // Expected Output: Yes

    return 0;
}

```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Pushed 10 to stack
Pushed 20 to stack
Pushed 30 to stack
Stack: 30 -> 20 -> 10 -> NULL
Top element is 30
Popped element: 30
Stack: 20 -> 10 -> NULL
Popped element: 20
Stack: 10 -> NULL
Is stack empty? No
Popped element: 10
Stack: NULL
Is stack empty? Yes

```



### C Code Example # 2

#### Program Code

`hello.h`

```C
// Definition of a stack element
struct elt {
    struct elt *next; // Pointer to the next element in the stack
    int value;        // Integer value stored in the stack element
};

// Typedef for the stack, which is a pointer to the top element
typedef struct elt *Stack;

// Macro representing an empty stack (NULL pointer)
#define STACK_EMPTY (0)

// Function prototypes
void stackPush(Stack *s, int value);
int stackEmpty(const Stack *s);
int stackPop(Stack *s);
void stackPrint(const Stack *s);
```

`hello.c`

```C
void stackPush(Stack *s, int value)
{
    struct elt *e; // Allocate memory for a new stack element

    e = malloc(sizeof(struct elt));
    assert(e); // Ensure memory allocation was successful

    // Initialize the new element with the given value
    e->value = value;
    e->next = *s;// Point the new element to the current top of the stack
    *s = e; // Update the stack pointer to point to the new element
}


int stackEmpty(const Stack *s)
{
    // Check if the stack pointer is NULL (empty stack)
    return (*s == 0);
}


int stackPop(Stack *s)
{
    int ret;
    struct elt *e;

    // Ensure the stack is not empty before popping
    assert(!stackEmpty(s));
    
    // Retrieve the value from the top element
    ret = (*s)->value;

    // Remove the top element from the stack
    e = *s;
    *s = e->next;

    free(e);
    
    // Return the popped value
    return ret;
}


void stackPrint(const Stack *s)
{
    struct elt *e;
    
    // Iterate through the stack elements
    for (e = *s; e != 0; e = e->next)
    {
        printf("%d ", e->value);
    }
    
    // Move to the next line after printing the stack
    putchar('\n');
}
```

`main.c`

```C
int main()
{
    int i;
    Stack s;

    // Initialize the stack to be empty
    s = STACK_EMPTY;

    // Push integers 0 to 4 onto the stack
    for (i = 0; i < 5; i++)
    {
        printf("push %d\n", i);
        stackPush(&s, i);
        stackPrint(&s);
    }

    // Pop elements from the stack until it is empty
    while (!stackEmpty(&s))
    {
        printf("pop gets %d\n", stackPop(&s));
        stackPrint(&s);
    }

    return 0;
}
```

#### Step-by-Step Walkthrough

- The loop runs with `i` from `0` to `4`.

- In each iteration:

  - **Print Push Message**: `"push i"`
  - Push Value: `stackPush(&s, i);`
    - `stackPush` adds the value `i` to the top of the stack `s`.
  - Print Stack State: `stackPrint(&s);`
    - `stackPrint` displays the current contents of the stack.

- **Detailed Operations in Each Iteration**

  - **Iteration 1 (`i = 0`):**
    - Push `0` onto the stack.
    - Stack becomes: `0`
  - **Iteration 2 (`i = 1`):**
    - Push `1` onto the stack.
    - Stack becomes: `1 0`
  - **Iteration 3 (`i = 2`):**
    - Push `2` onto the stack.
    - Stack becomes: `2 1 0`
  - **Iteration 4 (`i = 3`):**
    - Push `3` onto the stack.
    - Stack becomes: `3 2 1 0`
  - **Iteration 5 (`i = 4`):**
    - Push `4` onto the stack.
    - Stack becomes: `4 3 2 1 0`

  **Note**: The stack is implemented as a linked list where each new element becomes the new head of the list, resulting in a LIFO (Last-In-First-Out) order.

**Popping Elements from the Stack**

1. **While Loop to Pop Values**

   ```C
   while (!stackEmpty(&s))
   
   {
   
     printf("pop gets %d\n", stackPop(&s));
   
     stackPrint(&s);
   
   }
   ```

   

   - The loop continues as long as the stack is not empty.
   - In each iteration:
     - Pop Value:`stackPop(&s);`
       - `stackPop` removes and returns the value from the top of the stack `s`.
     - **Print Pop Message**: `"pop gets value"`
     - Print Stack State: `stackPrint(&s);`
       - `stackPrint` displays the current contents of the stack after popping.

2. **Detailed Operations in Each Iteration**

   - **Iteration 1:**
     - Pop the top value `4` from the stack.
     - Stack becomes: `3 2 1 0`
     - Output: `"pop gets 4"`
   - **Iteration 2:**
     - Pop the top value `3` from the stack.
     - Stack becomes: `2 1 0`
     - Output: `"pop gets 3"`
   - **Iteration 3:**
     - Pop the top value `2` from the stack.
     - Stack becomes: `1 0`
     - Output: `"pop gets 2"`
   - **Iteration 4:**
     - Pop the top value `1` from the stack.
     - Stack becomes: `0`
     - Output: `"pop gets 1"`
   - **Iteration 5:**
     - Pop the top value `0` from the stack.
     - Stack becomes: empty
     - Output: `"pop gets 0"`

3. **End of Program**

   - Once the stack is empty, the condition `!stackEmpty(&s)` becomes false, and the loop exits.
   - The program reaches the `return 0;` statement and terminates.

**Stack Using Linked List**

- The stack is implemented using a singly linked list where each node (`struct elt`) contains an integer `value` and a pointer `next` to the next element.

#### **Functions Explanation**

**1. `stackPush(Stack *s, int value)`**

- **Purpose**: Adds a new element with the given `value` to the top of the stack.
- **Steps**:
  - Allocate memory for a new element.
  - Set the new element's `value` to the given `value`.
  - Point the new element's `next` to the current top of the stack (`*s`).
  - Update the stack pointer `*s` to point to the new element.

**2. `int stackPop(Stack *s)`**

- **Purpose**: Removes and returns the value from the top element of the stack.

- **Ensure Stack is Not Empty:**

  - `assert(!stackEmpty(s));`
  - Ensures that the stack is not empty before attempting to pop an element.

  - **Retrieve the Value:**
    - `ret = (*s)->value;`
    - Retrieves the value from the top element of the stack.
    - `(*s)` dereferences the stack pointer to get the top element. `(*s)->value` accesses the `value` field of the top element and stores it in the variable `ret`.
  - **Remove the Top Element:**
    - `e = *s;`
    - Stores the pointer to the top element in `e`.
    - `*s = e->next;`
    - Updates the stack pointer to point to the next element, effectively removing the top element.
    - `e = *s;` stores the pointer to the top element in the variable `e`.
    - `*s = e->next;` updates the stack pointer to point to the next element in the stack, effectively removing the top element.
  - **Free Memory:**
    - `free(e);`
    - Frees the memory allocated for the removed element.
  - **Return the Value:**
    - `return ret;`
    - Returns the value retrieved from the top element.

- **Visualization**:

  - Let's visualize the process with an example. Suppose we have the following stack:

  - ```css
    Top -> [4] -> [3] -> [2] -> [1] -> [0] -> NULL
    ```

  - We want to pop the top element (`4`) from the stack.

  - **Initial State**

    - **Stack Pointer (`s`):** Points to the top element (`4`).
    - **Top Element (`*s`):** `[4]`
    - **Next Element (`(*s)->next`):** `[3]`

    ```css
    s -> [4] -> [3] -> [2] -> [1] -> [0] -> NULL
    ```

  - **Retrieve the Value from the Top Element**

    ```C
    ret = (*s)->value;
    ```

    - `(*s)` points to `[4]`.
    - `(*s)->value` is `4`.
    - `ret` is set to `4`.

  - **Remove the Top Element from the Stack**:

    ```C
    e = *s;
    *s = e->next;
    ```

    - `e = *s;` stores the pointer to `[4]` in `e`.
    - `*s = e->next;` updates `*s` to point to `[3]`.

  - **State After Removal:**

    ```css
    e -> [4] -> [3] -> [2] -> [1] -> [0] -> NULL
    s -> [3] -> [2] -> [1] -> [0] -> NULL
    ```

  - **Free the Memory Allocated for the Removed Element**

    ```C
    free(e);
    ```

    - Frees the memory allocated for `[4]`.

  - **State After Freeing Memory:**

    ```css
    s -> [3] -> [2] -> [1] -> [0] -> NULL
    ```

  - **Return the Popped Value**:

    ```C
    return ret;
    ```

    - Returns `4`.

  - **Final State**:

    After popping the top element (`4`), the stack looks like this:

    ```css
    Top -> [3] -> [2] -> [1] -> [0] -> NULL
    ```

    - The stack pointer (`s`) now points to the new top element (`3`).
    - The value `4` has been returned by the `stackPop` function.


**3. `int stackEmpty(const Stack *s)`**

- **Purpose**: Checks if the stack is empty.
- **Returns**: `1` (true) if the stack is empty (`*s == 0`), `0` (false) otherwise.

**4. `void stackPrint(const Stack *s)`**

- **Purpose**: Prints the current contents of the stack from top to bottom.
- **Steps**:
  - Iterate through the linked list starting from the top of the stack (`*s`).
  - In each iteration, print the `value` of the current element.
  - After printing all elements, move to a new line.

#### Program Output

```shell
chan@CMA:~/C_Programming/test$ ./final
push 0
0 
push 1
1 0 
push 2
2 1 0 
push 3
3 2 1 0 
push 4
4 3 2 1 0 
pop gets 4
3 2 1 0 
pop gets 3
2 1 0 
pop gets 2
1 0 
pop gets 1
0 
pop gets 0

```



### C Code Example #3

#### Program Code

`practice.h`

```C
typedef struct
{
    int *collection;
    int capacity;
    int size;
} Stack;

Stack *create_stack(int capacity);
void destroy_stack(Stack *stack);
bool is_full(Stack *stack);
bool is_empty(Stack *stack);
bool pop(Stack *stack, int *item);
bool push(Stack *stack, int item);
bool peek(Stack *stack, int *item);
```

`functions.c`

```C
Stack *create_stack(int capacity)
{
    if (capacity <= 0)
    {
        return NULL;
    }

    Stack *stack = malloc(sizeof(Stack));
    if (stack == NULL)
    {
        return NULL;
    }
    stack->collection = malloc(sizeof(int) * capacity);
    if (stack->collection == NULL)
    {
        free(stack);
        return NULL;
    }

    stack->capacity = capacity;
    stack->size = 0;

    return stack;
}

void destroy_stack(Stack *stack)
{
    free(stack->collection);
    free(stack);
}

bool is_full(Stack *stack)
{
    return stack->capacity == stack->size;
}

bool is_empty(Stack *stack)
{
    return stack->size == 0;
}

bool pop(Stack *stack, int *item)
{
    if (is_empty(stack))
    {
        return false;
    }
    stack->size--;
    *item = stack->collection[stack->size];
    return true;
}

bool push(Stack *stack, int item)
{
    if (is_full(stack))
    {
        return false;
    }
    stack->collection[stack->size++] = item;
    return true;
}

bool peek(Stack *stack, int *item)
{
    if (is_empty(stack))
    {
        return false;
    }
    *item = stack->collection[stack->size - 1];
    return true;
}
```



`practice.c`

```C
int main()
{
    Stack *stack = create_stack(5);

    if (stack == NULL)
    {
        fprintf(stderr, "Failed to create stack\n");
        return 1;
    }

    if (is_empty(stack))
    {
        printf("Stack is empty\n");
    }
    push(stack, 2);
    printf("Stack size: %d\n", stack->size);
    push(stack, 9);
    push(stack, 4);
    push(stack, 7);
    push(stack, 8);

    printf("Stack size: %d\n", stack->size);
    if (is_full(stack))
    {
        printf("Stack is full\n");
    }
    bool try_push = push(stack, 5);
    if (!try_push)
    {
        printf("Push failed\n");
    }

    int peek_val = 0;
    peek(stack, &peek_val);
    printf("Peek value: %d\n", peek_val);

    int pop_val = 0;
    for (int i = 0; i < 5; i++)
    {
        pop(stack, &pop_val);
        printf("Popped value: %d\n", pop_val);
    }

    bool try_pop = pop(stack, &pop_val);
    if (try_pop == false)
    {
        printf("Pop failed\n");
    }

    bool try_peek = peek(stack, &peek_val);
    if (try_peek == false)
    {
        printf("Peek failed\n");
    }

    destroy_stack(stack);
    return 0;
}
```

#### Program Output

```shell
chan@CMA:~/C_Programming/practice$ ./practice
Stack is empty
Stack size: 1
Stack size: 5
Stack is full
Push failed
Peek value: 8
Popped value: 8
Popped value: 7
Popped value: 4
Popped value: 9
Popped value: 2
Pop failed
Peek failed
```



### Real World Applications

#### 1. Function Call Management (Call Stack)

**Description:** Every time a function is called in a program, information about that function (like its local variables and return address) is stored on a **call stack**. When the function finishes execution, its information is popped off the stack.

**Real-World Analogy:** Imagine a stack of plates:

- **Push:** Adding a new plate on top when we call a function.
- **Pop:** Removing the top plate when the function returns.

**Use Case Example:**

- **Programming Languages:** Languages like C, Java, and Python use a call stack to manage function calls and returns efficiently.

**Visualization:**

```less
Top of Stack
-------------
| Function A |
-------------
| Function B |
-------------
| Function C |
-------------
Bottom of Stack
```

#### 2. Expression Evaluation

**Description:** Stacks are used to evaluate expressions and convert them between different notations (infix, postfix, prefix).

**Real-World Analogy:** Think of processing operations in a calculator:

- Operations are stored temporarily on a stack to manage the order of execution.

**Use Case Example:**

- **Compilers:** During the parsing phase, stacks help evaluate and convert arithmetic expressions.

**Visualization:**

Evaluating `3 + 4 * 2 / (1 - 5)^2^3` using a stack to handle operator precedence and parentheses.

#### 3. Undo Mechanisms in Applications

**Description:** Applications like text editors and graphic design tools use stacks to keep track of user actions, allowing users to undo operations in the reverse order they were performed.

**Real-World Analogy:** A stack of actions:

- **Push:** Each action (like typing a character) is pushed onto the stack.
- **Pop:** Undoing an action pops the latest action from the stack.

**Use Case Example:**

- **Microsoft Word:** Pressing `Ctrl + Z` undoes the last action by popping it from the undo stack.

**Visualization:**

```less
Undo Stack
-------------
| Action 3   |  <-- Last action to undo
-------------
| Action 2   |
-------------
| Action 1   |
-------------
```

#### 4. Browser Navigation

**Description:** Browsers use stacks to manage the history of visited pages, enabling the "Back" and "Forward" buttons functionality.

**Real-World Analogy:** Navigating back through a stack of web pages you've visited.

**Use Case Example:**

- **Web Browsers:** Each visited URL is pushed onto the back stack. When we click "Back," the top URL is popped off and displayed.

**Visualization:**

```less
Back Stack
-------------
| Page 3     |  <-- Current page
-------------
| Page 2     |
-------------
| Page 1     |
-------------
```

#### 5. Syntax Parsing

**Description:** Stacks are used in parsing the syntax of programming languages, ensuring that constructs like parentheses, brackets, and braces are properly opened and closed.

**Real-World Analogy:** Checking balanced parentheses in mathematical expressions using a stack.

**Use Case Example:**

- **Compilers and Interpreters:** Validating the correct nesting of symbols in code.

**Visualization:**

Validating the expression `(a + b) * (c + d)` using a stack to match parentheses.