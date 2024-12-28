## Object-Oriented Programming with C

- In some circumstances, it make sense to design data structures or functions to work on any type that supplies the right operations.
- To make this work, we will attach the functions for manipulating an object to the object itself.
- This is the central idea behind object-oriented programming.

`practice.h`

- Objects in this system have three methods: `clone`, which makes a copy of the object, `print`, which sends a representation of the object to `stdout`, and `destroy`, which frees the object.
- Each of these methods takes the object itself as its first argument `self`, since C provides no other mechanism to tell these functions which object to work on.

```C
struct object
{
    const struct methods *methods;
};

typedef struct object Object;

struct methods
{
    Object *(*clone)(const Object *self);
    void (*print)(const Object *self);
    void (*destroy)(Object *self);
};

Object *intObjectCreate(int value);
Object *stringObjectCreate(const char *value);

struct intObject
{
    struct methods *methods;
    int value;
};

struct stringObject
{
    struct methods *methods;
    char *value;
};

static void printInt(const Object *s);
static Object *cloneInt(const Object *s);
static void destroyInt(Object *s);

static struct methods intObjectMethods = {
    cloneInt,
    printInt,
    destroyInt};

static void printInt(const Object *self)
{
    printf("%d", ((struct intObject *)self)->value);
}

static Object *cloneInt(const Object *self)
{
    return intObjectCreate(((struct intObject *)self)->value);
}

static void destroyInt(Object *self)
{
    free(self);
}

static void printString(const Object *s);
static Object *cloneString(const Object *s);
static void destroyString(Object *s);

static struct methods stringObjectMethods = {
    cloneString,
    printString,
    destroyString};

static void printString(const Object *self)
{
    printf("%s", ((struct stringObject *)self)->value);
}

static Object *cloneString(const Object *self)
{
    return stringObjectCreate(((struct stringObject *)self)->value);
}

static void destroyString(Object *self)
{
    free(((struct stringObject *)self)->value);
    free(self);
}

struct elt
{
    struct elt *next;
    Object *value;
};

typedef struct elt *Stack;

Stack *stackCreate(void);

void stackDestroy(Stack *s);
void stackPush(Stack *s, Object *value);

Object *stackPop(Stack *s);

int stackNotEmpty(const Stack *s);

void stackPrint(const Stack *s);
```

`functions.c`

- Internally, this stack will use the `clone` method to ensure that it gets its own copy of anything pushed onto the stack in `stackPush` to protect against the caller later destroying or modifying the object being pushed.
- The `print` method to print objects in `stackPrint`, and the `destroy` method to clean up in `stackDestroy`.

```C
Object *intObjectCreate(int value)
{
    struct intObject *self = malloc(sizeof(struct intObject));
    assert(self);

    self->methods = &intObjectMethods;
    self->value = value;

    return (Object *)self;
}

Object *stringObjectCreate(const char *value)
{
    struct stringObject *self = malloc(sizeof(struct stringObject));
    assert(self);

    self->methods = &stringObjectMethods;
    self->value = strdup(value);
    return (Object *)self;
}

Stack *stackCreate(void)
{
    Stack *s = malloc(sizeof(Stack));
    assert(s);

    *s = 0;
    return s;
}

void stackDestroy(Stack *s)
{
    Object *o;
    while (stackNotEmpty(s))
    {
        o = stackPop(s);
        o->methods->destroy(o);
    }
    free(s);
}
void stackPush(Stack *s, Object *value)
{
    struct elt *e = malloc(sizeof(struct elt));
    e->next = *s;
    e->value = value->methods->clone(value);

    // set the stack pointer to point to the new element which put it to the top of the stack
    *s = e;
}

Object *stackPop(Stack *s)
{
    // save the current top element in e
    struct elt *e = *s;

    // save the value of the top element in ret
    Object *ret = e->value;

    // set the stack pointer to point to the next element of the top element
    *s = e->next;

    // free the top element,effectively getting rid of the top element
    free(e);

    // return popped value
    return ret;
}

int stackNotEmpty(const Stack *s)
{
    return *s != 0;
}

void stackPrint(const Stack *s)
{
    for (struct elt *e = *s; e; e = e->next)
    {
        e->value->methods->print(e->value);
        putchar(' ');
    }
    putchar('\n');
}
```

- A messy line in C like `e->value->methods->print(e->value)` would look in C++ like `e->value.print()`.

`practice.c`

```C
#define N (3)
int main(int argc, char *argv[])
{
    char str[] = "hi";
    Object *o;

    int n = N;
    if (argc >= 2)
    {
        n = atoi(argv[1]);
    }

    Stack *s = stackCreate();

    for (int i = 0; i < n; i++)
    {
        // push a string onto the stack
        // creates a string obj, pushes it onto the stack and then destroy the original obj
        str[0] = 'a' + i;
        o = stringObjectCreate(str);
        stackPush(s, o);
        o->methods->destroy(o);
        stackPrint(s);

        // push an int onto the stack
        o = intObjectCreate(i);
        stackPush(s, o);
        o->methods->destroy(o);
        stackPrint(s);
    }

    while (stackNotEmpty(s))
    {
        // pops each obj from the stack, prints it within square brackets, destroys it, and displays the current state of the stack
        o = stackPop(s);
        putchar('[');
        o->methods->print(o);
        o->methods->destroy(o);
        fputs("] ", stdout);
        stackPrint(s);
    }

    stackDestroy(s);
    return 0;
}
```

- Why `o->methods->destroy(o);` is necessary after each push operation:
  - **The Lifecycle of Objects in the Stack**
    1. **Creation:**
       - We create an object (`o`) using either `intObjectCreate` or `stringObjectCreate`. This allocates memory for the object and initializes its fields.
    2. **Pushing onto the Stack:**
       - When we call `stackPush(s, o);`, the stack performs the following:
         - **Cloning the Object:** It uses the `clone` method (`o->methods->clone(o)`) to create a **duplicate** of the object.
         - **Storing the Clone:** The cloned object is what gets stored in the stack's top node.
       - **Original Object (`o`):** After cloning, the original object (`o`) is **no longer needed** by the stack since the stack has its own copy.
    3. **Destroying the Original Object:**
       - To prevent **memory leaks**, we must **free** the memory allocated for the original object once it's no longer needed.
       - This is achieved by calling `o->methods->destroy(o);`, which deallocates the memory used by `o`.

#### Output

```shell
chan@CMA:~/C_Programming/practice$ make valgrind
valgrind --leak-check=full ./practice 
==24481== Memcheck, a memory error detector
==24481== Copyright (C) 2002-2022, and GNU GPL'd, by Julian Seward et al.
==24481== Using Valgrind-3.22.0 and LibVEX; rerun with -h for copyright info
==24481== Command: ./practice
==24481== 
ai 
0 ai 
bi 0 ai 
1 bi 0 ai 
ci 1 bi 0 ai 
2 ci 1 bi 0 ai 
[2] ci 1 bi 0 ai 
[ci] 1 bi 0 ai 
[1] bi 0 ai 
[bi] 0 ai 
[0] ai 
[ai] 
==24481== 
==24481== HEAP SUMMARY:
==24481==     in use at exit: 0 bytes in 0 blocks
==24481==   total heap usage: 26 allocs, 26 frees, 1,338 bytes allocated
==24481== 
==24481== All heap blocks were freed -- no leaks are possible
==24481== 
==24481== For lists of detected and suppressed errors, rerun with: -s
==24481== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

