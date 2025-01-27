#### Make your functions stretchy

The `printf()` function has one really cool feature that we have used: it can take a **variable number of arguments**.

```C
printf("%i bottles of beer on the wall, %i bottles of beer\n", 99, 99);

// You can pass the printf() as many arguments as you need to print.
```



#### Variadic Functions

A function that takes a variable number of parameters is called a **variadic function**. 

- The C Standard Library contains a set of **macros** that can help you create your own variadic functions.

- To see how they work, we'll create a function that can print out series of `ints`:

  ```C
  print_ints(3, 79, 101, 32);
  
  // 3 - Number of ints to print
  // 79, 101, 32 - The ints that need to be printed
  ```

```C
#include <stdarg.h>

void print_ints(int args, ...){
    va_list ap;
    va_start(ap, args);
    // va_start says where the variable arguments start. The variable arguments will start after the args parameter.
    int i;
    
    // This will loop through all of the other arguments.
    for(i = 0; i < args; i++){ // args contains a count of how many variables there are.
        printf("argument: %i\n", va_arg(ap, int));
    }
    va_end(ap);
}
```

Let's break it down and take a look at it, step by step

1. **Include the `stdarg.h` header**. 

   All the code to handle variadic functions is in `stdard.h`, so we need to make sure we include it.

2. **Tell our function there's more to come**.

   An ellipsis "...", in C, after the argument of a function means there are more arguments to come.

3. **Create a va_list**.

   A `va_list` will be used to store the extra arguments that are passed to our function.

4. **Say where the variable arguments start**.

   C needs to be told the name of the **last fixed argument**. In the case of our function, that'll be the `args` parameter.

5. **Then read off the variable arguments, one at a time**.

   Now our arguments are all stored in the `va_list`, we can read them with `va_arg`. `va_arg`takes two values: the `va_list` and the **type** of the next argument. In our case, all of the arguments are `ints`.

6. **Finally...end the list**.

   After we've finished reading all of the arguments, we need to tell C that we're finished. We do that with the `va_end` macro.

7. **Now we can call our function**.

   Once the function is complete, we can call it:

   ```C
   print_ints(3, 79, 101, 32);
   
   // This will print out 79, 101, and 32 values.
   ```

