### Chapter 7 - Advanced Functions

The extern keyword in C is used to declare a variable or function and specify that its actual definition is located elsewhere, not within the same block where it is declared. This can be in a different file or above/below in the same file.

When you declare a variable with extern, you're telling the compiler that this variable will be defined somewhere else in the program. This allows you to use that variable in different source files.

For example, if you have a variable int count that you want to use in multiple .c files, you can declare it in one file like this:

```C
 int count = 0;
```


And then in any other file where you want to use count, you would declare it with extern:

```C
extern int count;
```


This tells the compiler that count is an integer variable that's defined in another file. The linker then takes care of making sure all the extern declarations point to the correct location when the program is linked.



####  Every function name is a pointer to the function...

The name of a function really is a way of referring to the piece of code. And that's just what a pointer is: a way of referring to something in memory.

- That's why, in C, function names are also pointer variables.

- When you create a function called `go_to_warp_speed(int speed)`, you are also creating a pointer variable called `go_to_warp_speed` that contains the address of the function.

- ```C
  int go_to_warp_speed(int speed){
      dilithium_crystals(ENGAGE);
      warp = speed;
      reactor_core(c, 125000 * speed, PI);
      clutch(ENGAGE);
      brake(ENGAGE);
      return 0;
  }
  ```

- Whenever you create a function, you also create a function pointer with the same name.

- | STACK                        |
  | ---------------------------- |
  | HEAP                         |
  | GLOBALS                      |
  | CONSTANTS "go_to_warp_speed" |
  | CODE                         |

- ```C
  go_to_warp_speed(4);
  ```

- When you call the function, you are using the function pointer.



#### but there's no function data type

Usually, it's pretty easy to declare pointers in C. If you have a data type like int, you just need to add an asterisk to the end of the data type name, and you declare a pointer with `int *`. Unfortunately, C doesn't have a function data type, so you can't declare a function pointer with anything like `function *`.

```C
int *a;
```

This declares an int pointer.

```C
function *f;
```

but this won't declare a function pointer.



#### Why doesn't C have a function data type?

C doesn't have a function data type because there's not just one type of function.

- When you create a function, you can vary a lot of things, such as the return type or the list of parameters it takes.
- That combination of things is what defines the type of the function.

```C
int go_to_warp_speed(int speed){
    ...
}
```

```C
char **album_names(char *artist, int year){
    ...
}
```

There are many different types of functions. These two functions above are different types because they have different return types and parameters.



#### How to create function pointers

Say you want to create a pointer variable that can store the address of each of the functions.

```C
int (*warp_fn) (int);

warp_fn = go_to_warp_speed; <- // This will create a variable called warp_fn that can store the address of the go_to_warp_speed() function.
    
warp_fn(4); <- // This is just like calling go_to_warp_speed(4)
    
    
char ** (*names_fn) (char*, int);
names_fn = album_names; <- // This will create a variable called names_fn that can store the address of the album_names() function.
    
char **results = names_fn("Sacha Distel", 1972);
```



We need to tell C the return type and the parameter types the function will take. But once we have declared a function pointer variable, you can use it like any other variable. You can assign values to it, you can add it to arrays and you can also pass it to functions.



Q: What does `char **` mean? Is it a typing error?

A: `char **` is a pointer normally used to point to an array of strings.



**Exercises are in `HFCExercises.md`. **

| Return type | (*Pointer variable) | (Param types) |
| ----------- | ------------------- | ------------- |
| char**      | (*names_fn)         | (char*, int)  |



`(*names_fn)` - This is the name of the variable we are declaring.



Q: If function pointers are just pointers, why don't you need to prefix them with a * when you call the function?

A: You can. In the program in `HFCExercises.md`, instead of writing `match(ADS[i])`, you could have written `(*match)(ADS[i])`.



Q: And could I have used & to get the address of a method?

A: Yes. Instead of `find(sports_or_workout)`, you could have written `find(&sports_or_workout)` in the exercise `HFCExercises.md`.



Q: Then why didn't I?

A: Because it makes the code easier to read. If you skip the * and &, C will still understand what you're saying.



#### Get it sorted with the C Standard Library

Lots of programs need to sort data. If the data is something simple like a set of numbers, then sorting is pretty easy. Numbers have their own natural order. But it's not so easy with other types of data.



**How could a sort function sort any type of data at all?**



#### Use function pointers to set the order

The C Standard Library has a sort function that accepts a pointer to a comparator function which will be used to decide if one piece of data is **the same as**, **less than** or **greater than** another piece of data.



This is what the `qsort()` function looks like:

```C
qsort(void *array, <- // This is a pointer to 										an array.
     size_t length, <- // This is the length of 									the array.
     size_t item_size, <- // This is the size 					of each element in the array.
     int(*compar)(const void *, const void *));

// (*compar) - This is a pointer to a function that compares two items in the array.
// const void * - Remember, a void* pointer can point to anything.
```



- The `qsort()` function compares pairs of values over and over again, and if they are in the wrong order, the computer will switch them.
- And that's what the comparator function is for. It will tell `qsort()` which order a pair of elements should be in. 
- It does this by returning three different values.
- If the first value is greater than the second value, it should return a positive number.
- If the first value is less than the second value, it should return a negative number.
- If the two values are equal, it should return zero.



Let's say we have an array of integers and we want to sort them in increasing order.

```C
int scores[] = {543, 323, 32, 554, 11, 3, 112};
```



If you look at the **signature** of the comparator function that `qsort()` needs, it takes two **void pointers** given by `void *`.

A void pointer can store the address of **any kind of data**, but you always need to cast it to something more specific before you can use it.



**A void pointer `void *` can store a pointer to anything.**



The `qsort()` function works by comparing pairs of elements in the array and then placing them in the correct order. It compares the values by calling the comparator function that you give it.

```C
int compare_scores(const void* score_a, const void* score_b){
    ...
}
```



- Values are always passed to the function as pointers, so the first thing we need to do is get the integer values from the pointers.

- ```C
  int a = *(int*)score_a;
  int b = *(int*)score_b;
  ```

- We need to cast the void pointer to an integer pointer.

- The first * then gets the int stored at address score_b.

- Then we need to return a positive, negative or zero value depending on whether `a` is greater than, less than, or equal to `b`.

- For integers, that's pretty easy to do - we just subtract one number from the other.

- ```C
  return a - b;
  
  // If a > b, this is positive;
  // If a < b, this is negative;
  // If a and b are equal, this is zero
  ```

- And this is how we ask `qsort()` to sort the array

- ```C
  qsort(scores, 7, sizeof(int), compare_scores);
  ```

  

#### Why use `*(int*)score_a`?

The function `compare_scores` needs to compare two `int` values.

1. **Casting the Void Pointer:** `score_a` is of type `const void *`, which is a generic pointer type in C. Since `void *` does not point to any specific type, it must be cast to the appropriate type before dereferencing. In this case, we cast it to `int *`.

   ```C
   (int *)score_a
   ```

   This tells the compiler that `score_a` is actually pointing to an `int`.

2. **Dereferencing the Pointer:** After casting `score_a` to `int *`, we then dereference it to get the `int` value it points to:

   ```C
   *(int *)score_a
   ```

   This `*` operator is used to access the value at the address stored in the pointer. This effectively converts `score_a` from a pointer to a void type into an `int` value.



```C
int compare_scores(const void *score_a, const void *score_b){
    int a = *(int*)score_a;
    int b = *(int*)score_b;
    return a - b;
}

qsort(scores, 7, sizeof(int), compare_scores);
```



#### How it works

- `score_a` and `score_b`: These are pointers to `int` values but they are passed as `const void *` for generality as required by `qsort`.
- `Casting and Dereferencing`: By casing `score_a` and `score_b` to `int *` and then dereferencing them, you obtain the actual `int` values that need to be compared.
- `Returning the Result`: The function returns the difference between `a` and `b`. This is a common way to write a comparison function in C.

**Refer to the example exercise in `HFCExercises.md`.**



#### `strcmp`

The `strcmp` function in C starts comparing the first characters of both strings. If they are equal, it moves on to the next characters, and so on. This process continues until it finds a pair of characters that differ or until it reaches the end of strings.

- If the two strings are equal, `strcmp` returns 0.
- If the first string is lexicographically greater than the second, `strcmp` returns a positive integer.
- If the first string is lexicographically less than the second, `strcmp` returns a negative integer.

```C
#include <stdio.h>
#include <string.h>

int main() {
    char *str1 = "Hello";
    char *str2 = "World";
    char *str3 = "Hello";

    printf("strcmp(str1, str2): %d\n", strcmp(str1, str2));
    printf("strcmp(str1, str3): %d\n", strcmp(str1, str3));

    return 0;
}
```

Code Execution

```bash
chan@CMA:~/C_Programming/test$ ./final
strcmp(str1, str2): -15
strcmp(str1,str3): 0

```

In this example, "Hello" and "World" both have 5 characters, but "Hello" is considered lexicographically less than "World". This is because the ASCII value of 'H'(72) is less than the ASCII value of 'W'(87).

