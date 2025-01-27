## Chapter 2 - Memory and pointers

### Pointers

**Pointer** - A pointer is just the address of a piece of data in memory.

- Pointers help us do both these things: avoid copies and share data.

- In C, the presence of an asterisk (`*`) in a function declaration indicates that the function returns a pointer, while its absence means the function returns a value directly.

Every time you declare a variable, the computer creates space for it somewhere in memory. If you declare a variable inside a function like main(), the computer will store it in a section of memory called the **stack**. If a variable is declared outside any function, it will be stored in the **globals** section of memory.

- **&** to find out the memory address of the variable.

  ```C
  printf("x is stored at %p\n", &x);
  
  // x is stored at 0x7ffdd281e4b4
  ```

- The address of the variable tells you where to find the variable in memory. That's why an address is also called a **pointer** because it **points** to the variable in memory.

Q: Why are local variables stored in the stack and globals stored somewhere else?

A: Local and global variables are used differently. You will only ever get one copy of a global variable, but if you write a function that calls itself, you might get very many instances of the same local variable.

- Pointers make it easier to share memory, to let functions share memory. The data created by one function can be modified by another function, so long as it knows where to find it in memory.

Three things you need to know in order to use pointers to read and write data.

1. Get the address of a variable.

   - A pointer variable is just a variable that stores a memory address. When declaring a pointer variable, you need to say what kind of data is stored at the address it will point to:

     ```C
     int x = 4;
     int *address_of_x = &x;
     ```

2. Read the contents of an address.

   - When you have  a memory address, you will want to read the data that's stored there with * operator:

     ```C
     int value_stored = *address_of_x;
     // This will read the contents at the memory address given by address_of_x. This will be set to 4: the value originally stored in the x variable.
     ```

   - Notes: **The & operator takes a piece of data and tells you where it's stored while the * operator takes an address and tells you what's stored there.**

   - Because pointers are sometimes called references, the * operator is said to dereference a pointer. (dereferencing in details below)

3. Change the contents of an address.

   - If you have a pointer variable and want to change the data at the address where the variable's pointing, you can just use * operator again but on the **left side** of an assignment:

     ```C
     *address_of_x = 99;
     ```

     

```C
int x = 10; //Declare an integer x
int *p; //Declare a pointer p;
p = &x; //Store the address of x in p
printf("%d", *p); // Print the value at address stored in p (which is x)

//This will output 10 because *p gives the value at the address stored in p which is the value of x.
```

#### Summary Bullet Points

- Variables are allocated storage in memory.
- Local variables live in the stack.
- Global variables live in the globals section.
- Pointers are just variables that store memory addresses.
- The & operator finds the address of a variable.
- The * operator can read the contents of a memory address.
- The * operator can also set the contents of a memory address.
- An array variable can be used as a pointer. 
- The array variable points to the first element in the array.
- If you declare an array argument to a function, it will be treated as a pointer.
- The **sizeof** operator returns the space taken by a piece of data. It returns the size of the pointer, which is typically 4 or 8 bytes depending on the system's architecture, not the size of the original array.
- You can also call **sizeof** for a data type such as **sizeof(int)**.
- **sizeof(a pointer)** returns 4 on 32-bit OS and 8 on 64-bit. On 32-bit OS, a memory address is stored as a 32-bit number. That's why it's called a 32-bit system. 32 bits == 4 bytes. That's why a 64-bit system uses 8 bytes to store an address.
- **sizeof** is not a function but it's an operator.
- An operator is compiled to a sequence of instructions by the compiler. But if the code calls a function, it has to jump to a separate piece of code.
- If I create a pointer variable, does the pointer variable live in memory? Yes. A pointer variable is just a variable storing a number. You can find the address of a pointer variable using the & operator.

---

### Memory memorizer

**Stack:** A section of memory used for local variable storage. Every time you call a function, all of the function's local variables get created on the stack. It's called the stack because it's like a stack of plates: variables get added to the stack when you enter a function, and get taken off the stack when you leave. The stack actually works upside down, it starts at the top of memory and grows downward.

**Heap:** The heap is for dynamic memory: pieces of data that get created when the program is running and then hang around a long time.

**Globals:** A global variable is a variable that lives outside all of the functions and is visible to all of them. Globals get created when the program first runs and you can update them freely. But that's unlike...

**Constants:** Created when the program first runs and they are stored in read-only memory. E.g things like string literals that you'll need when the program is running but you'll never want them to change.

**Code:** Finally the code segment. A lot of OS place the code right down in the lowest memory addresses. The code segment is also read-only. The is the part of the memory where the actual assembled code gets loaded.

---