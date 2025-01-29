# Function Pointers

- In C, a **function pointer** is a variable that stores the address of a function. 
- This allows functions to be passed as arguments to other functions, enabling callback mechanisms and facilitating flexible code structures. 
- Function pointers are particularly useful for implementing callback functions, where a function is passed as an argument to another function to be called later. 



**Declaring a Function Pointer:**

- To declare a function pointer, we specify the return type and parameter types of the function it will point to. 
- For example, to declare a pointer to a function that takes two integers and returns an integer:

```c
int (*operation)(int, int);
```

Here, `operation` is a pointer to a function that accepts two `int` parameters and returns an `int`.

**Assigning a Function to a Pointer:**

We can assign the address of a function to a function pointer as follows:

```c
int add(int a, int b) {
    return a + b;
}

operation = &add; // The '&' is optional; 'operation = add;' is also valid.
```

**Using a Function Pointer:**

Once assigned, we can call the function through the pointer:

```c
int result = operation(5, 3); // Calls 'add(5, 3)' through the function pointer.
```

Function pointers are powerful tools in C, enabling dynamic function calls and facilitating patterns like callbacks and event handlers. 



Regarding implementing a feature similar to JavaScript's `Array.prototype.map()` in C, it's important to note that C does not have built-in support for higher-order functions or array methods like JavaScript. However, we can achieve similar functionality using function pointers.

**Implementing a Map Function in C:**

We can create a function that applies a given function to each element of an array and stores the results in another array. Here's an example:

```c
#include <stdio.h>

#define SIZE 5

// Function to apply to each element
int square(int x) {
    return x * x;
}

// Map function
void map(int (*func)(int), int *input, int *output, int size) {
    for (int i = 0; i < size; i++) {
        output[i] = func(input[i]);
    }
}

int main() {
    int numbers[SIZE] = {1, 2, 3, 4, 5};
    int results[SIZE];

    // Apply 'square' function to each element of 'numbers' array
    map(square, numbers, results, SIZE);

    // Print results
    for (int i = 0; i < SIZE; i++) {
        printf("%d ", results[i]);
    }

    return 0;
}
```

**Explanation:**

- **Function Pointer Parameter:** The `map` function takes a function pointer `func` as its first parameter. This pointer should point to a function that takes an `int` and returns an `int`.
- **Input and Output Arrays:** The `map` function also takes pointers to the input and output arrays, as well as the size of the arrays.
- **Applying the Function:** Within the `map` function, a loop iterates over each element of the input array, applies the `func` function to it, and stores the result in the output array.

```sh
chan@CMA:~/C_Programming/test$ ./final
1 4 9 16 25 
```

