### Chapter 6 - Data Structures and Dynamic memory

- To store a flexible amount of data, you need something more extensible than an array, you need a linked list.

- Linked lists are like chains of data.

- A linked list is an example of an abstract data structure. 

- It's called an abstract data structure because a linked list is general: it can be used to store a lot of different kinds of data.

- A linked list stores a piece of data and a link to another piece of data.

- In a linked list, as long as you know where the list starts, you can travel along the list of links, from one of piece of data to the next, until you reach the end of the list.

- Page 309 in HFC book for visual representation.

- Linked lists allow inserts: inserting data is very quick which is another advantage linked lists have over arrays.

- If you wanted to insert a value into the middle of an array, you would have to shuffle all the pieces of data that follow it along by one:

  | Amity | Craggy | Isla Nublar | Shutter |
  | ----- | ------ | ----------- | ------- |

  - This is an array.
  - Let's say we have an array with 3 items, which are Amity, Craggy, and Shutter.
  - If we wanted to insert an extra value after Craggy Island, we'd have to move the other values along one space.
  - And because an array is fixed length, we'd lose Shutter Island.

- So linked lists allow us to store a variable amount of data and they make it simple to add more data.



#### How do we create a linked list in C?

Create a recursive structre.

Each one of the `structs` in the list will need to connect to the one next to it.

A `struct` that contains a link to another `struct` of the same type is called a recursive structure.

- Recursive structures contain pointers to other structures of the same type.

- So if you have a flight schedule for the list of islands that you're going to visit, you can use a recursive structure for each island.

  ```C
  typedef struct island{
      char *name;
      char *opens;
      char *closes;
      struct island *next;
  } island;
  ```

  - You'll record these details for each island.
  - For each island, you'll also record the next island.
  - You must give the `struct` a name.
  - You'll use strings for the name and opening times.
  - You store a **pointer** to the next island in the `struct`.
  - If you use the `typedef` command, you can normally skip giving the `struct` a proper name. 
  - But in recursive structure, you need to include a pointer to the same type.
  - C syntax won't let you use the `typedef` alias, so you need to give the `struct` a proper name.
  - We store a link from one `struct` to the next with a **pointer**.
  - That way the island data will contain the address of the next island that we're going to visit.
  - So, whenever our code is at one island, it'll always be able to hop over to the next island.



#### Create islands in C...

Once we have defined an island data type, we can create the first set of islands like this:

```C
island amity = {"Amity", "09:00", "17:00", NULL};
island craggy = {"Craggy", "09:00", "17:00", NULL};
island isla_nublar = {"Isla Nublar", "09:00", "17:00", NULL};
island shutter = {"Shutter", "09:00", "17:00", NULL};
```



- In C, NULL actually has the value 0 but it's set aside specially to set pointers to 0.

- Once we've created each island, we can then connect them together:

  ```C
  amity.next = &craggy;
  craggy.next = &isla_nublar;
  isla_nublar.next = &shutter;
  ```

- We have to be careful to set the next field in each island to the address of the next island.

- We'll use `struct` variables for each of the islands.

- But what if we want to insert an excursion to Skull Island between Isla Nublar and Shutter Island?



#### Inserting values into the list

We can insert islands by changing the values of the pointers between islands.

```C
island skull = {"Skull", "09:00", "17:00", NULL};

isla_nublar.next = &skull;
skull.next = &shutter;
```



**Exercises are in `HFCExercises.md`.**



Q: Other languages, like Java, have linked lists built in. Does C have any data structures?

A: C doesn't really come with any data structures built in. You have to create them yourself.



Q: What if I want to use the 700th item in a really long list? Do I have to start at the first item and then read all the way through?

A: Yes, you do.



Q: That's not very good. I thought a linked list was better than an array.

A: You shouldn't think of data structures as being better or worse. They are either appropriate or inappropriate for what you want to use them for.



Q: So if I want a data structure that lets me insert things quickly, I need a linked list but if I want a direct access I might use an array?

A: Exactly.



Q: You've shown a `struct` that contains a pointer to another `struct`. Can a `struct` contain a whole recursive `struct` inside itself?

A: No.



Q: Why not?

A: C needs to know the exact amount of space a `struct` will occupy in memory. If it allowed full recursive copies of the same `struct`, then one piece of data would be a different size than another.





Each island `struct` needed its own variable. 

If we wanted the code to store more than four islands, we would need another local variable. That's fine if we know how much data we need to store at compile time,But quite often programs don't know how much storage they need until runtime. 

- If we are writing a web browser, for instance, we won;t know how much data we'll need to store a web page until we read the web page.
- So C programs need some way to tell the OS that they need a little extra storage at the moment they need it.



#### Program need dynamic storage

##### Use the heap for dynamic storage

- Most of the memory we've been using so far has been in the **stack**.
- The stack is the area of memory that's used for local variables.
- Each piece of data is stored in a variable and each variable disappears as soon as you leave its function.
- It's harder to get more storage on the stack at runtime and that's where the **heap** comes in.
- The **heap** is the place where a program stores data that will need to be available longer term.
- It won't automatically get cleared away, so that means it's the perfect place to store data structures like our linked list.
- We can think of heap storage as being a bit like reserving a locker in a locker room.



##### First, get your memory with malloc()

- We tell the malloc() function exactly how much memory we need and it asks the OS to set that much memory aside in the heap.

- The malloc() function then returns a **pointer** to the new heap space, a bit like getting a key to the locker.

- It allows you access to the memory, and it can also be used to keep track of the storage locker that's been allocated.

  | Stack     |
  | --------- |
  | Heap      |
  | Globals   |
  | Constants |
  | Code      |

- The malloc() function will give you a pointer to the space in the heap.



#### Give the memory back when you're done

When we use the stack, we don't need to worry about returning memory; it all happened automatically.

- Every time we leave a function, the local storage is freed from the stack.

The heap is different. When we've asked for space on the heap, it will never be available for anything else until we tell the C Standard Library that we are finished with it.

- If we keep asking for more and more heap space, our program will quickly start to develop memory leaks.
- A memory leak happens when a program asks for more and more memory without releasing the memory it no longer needs.
- Memory leaks are among the most common bugs in C programs and they can be really hard to track down.
- The heap has only a fixed amount of storage available, so be sure you use it wisely.

#### Free memory by calling the free() function

-  The malloc() function allocates space and gives us a pointer to it.
-  We'll need to use this pointer to access the data and then when we're finished with the storage, we need to release the memory using the free() function.
-  It's a bit like handing your locker key back to the attendant so that the locker can be reused.
-  Every time some part of our code requests heap storage with the malloc() function, there should be some part of our code that hands the storage back with the free() function.
-  When our program stops running, all of its heap storage will be released automatically, but it's always good practice to explicitly call free() on every piece of dynamic memory we've created.



#### Ask for memory with malloc()

The function that asks for memory is called malloc() for memory allocation.

- malloc() taks a single parameter: the number of bytes you need.

- Most of the time, you probably don't know exactly how much memory you need in bytes, so malloc() is almost always used with an operator called `sizeof`, like this:

  ```C
  #include <stdlib.h>
  // We need to include the stdlib.h header file to pick up the malloc() and free() functions.
  
  ...
  malloc(sizeof(island));
  // This means, "Give me enough space to store an island struct"
  ```

- `sizeof` tells us how many bytes a particular data type occupies on our system.

- It might be a `struct`, or it could be some base data type, like `int` or `double`.

- The malloc() function sets aside a chunk of memory for us, then returns a pointer containing the start address.

- malloc() actually returns a general-purpose pointer, with type `void*`.

  ```C
  island *p = malloc(sizeof(island));
  
  // This means "Create enough space for an island, and store the address in variable p."
  ```



#### ...and free up the memory with free()

- Once we have created the memory on the heap, we can use it for as long as we like.

- But once we've finished, we need to release the memory using the free() function.

- free() needs to be given the address of the memory that malloc() created.

  ```C
  free(p);
  // This means "Release the memory you allocated from heap address p."
  ```



**Key takeaway: if you allocated memory with malloc() in one part of your program, you should always release it later with the free() function.**

#### `strdup()` function

- The `strdup()` function can reproduce a complete copy of the string somewhere on the heap:
- The `strdup()` function works out how long the string is, and then calls the malloc() function to allocate the correct number of characters on the heap.
- It then copies each of the characters to the new space on the heap.
- That means that `strdup()` always create space **on the heap**. It can't create space on the stack because that's for local variables, and local variables get cleared away too often.
- But because `strdup()` puts new strings on the heap, that means we must **always remember to release their storage with the free() function.**



**Dynamic memory allocation lets us create the memory we need at RUNTIME. And they way we access dynamic heap memory is with malloc() and free().**



Q: Why is the heap called the heap?

A: Because the computer doesn't automatically organize it. It's just a big heap of data.



Q: What's a garbage collection?

A: Some languages track when you allocate data on a heap and then when you're no longer using the data, they free the data from the heap.



Q: Why doesn't C contain garbage collection?

A: C is quite an old language; when it was invented, most languages didn't do automatic garbage collection.



Q: I understand why I needed to copy the name of the island in the example in `HFCExercises.md`. Why didn't I need to copy the `opens` and `closes` values?

A: The `opens` and `closes` values are set to string literals. String literals can't be updated, so it doesn't matter if several data items refer to the same string.



Q: Does `strdup()` actually call the `malloc()` function?

A: It will depend on how the C standard library is implemented, but most of the time, yes.



Q: Do I need to free all my data before the program ends?A: You don't have to; the OS will clear away all of the memory when the program exits. But it's good practice to always explicitly free anything you've created.



#### Bullet Points

- Dynamic data structures allow you to store a variable number of data items.
- A linked list is a data structure that allows you to easily insert items.
- Dynamic data structures are normally defined in C with recursive `struct`s.
- A recursive `struct` contains one or more pointers to a  similar `struct`.
- The stack is used for local variables and is managed by the computer.
- The heap is used for long-term storage. You allocate space with `malloc()`.
- The `sizeof` operator will tell you how much space a `struct` needs.
- Data will stay on the heap until you release it with `free()`.
- `valgrind` checks for memory leaks.
- `valgrind` works by intercepting the calls to `malloc()` and `free()`.
- When a program stops running, `valgrind` prints details of what's left on the heap.
- If you compile your code with debug information, `valgrind` can give you more information.
- If you run your program several times, you can narrow the search for the leak.
- `valgrind` can tell you which lines of code in your source put the data on the heap.
- `valgrind` can be used to check that you've fixed a leak.
- A linked list is more extensible than an array.
- Data can be inserted easily into a linked list.
- A link list is a dynamic data structure.
- Dynamic data structures use recursive structs.
- Recursive structs contain one or more links to similar data.
- `malloc()` allocates memory on the heap.
- `free()` releases memory on the heap.
- Unlike the stack, heap memory is not automatically released. 
- The stack is used for local variables.
- `strdup()` will create a copy of a string on the heap.
- A memory leak is allocated memory you can no longer access.
- `valgrind` can help you track down memory leaks.

---

