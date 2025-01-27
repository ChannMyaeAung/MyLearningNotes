#### Arrays

An array variable can be used as a pointer to the first element in an array. That means you can read the first element of the array either by using the brackets notation or using the * operator like this

The index is just the number that's added to the pointer to find the location of the element.



```C
int drinks[] = {4,2,3};
printf("1st order: %i drinks\n", drinks[0]);
printf("1st order: %i drinks\n", *drinks);
// The above two lines of code are equivalent.
// drinks[0] == *drinks

printf("3rd order: %i drinks\n", drinks[2]);
printf("3rd order: %i drinks\n", *(drinks + 2));
```

#### Array of arrays vs array of pointers

An array of arrays is used to stored a sequence of strings.

An array of pointers is actually a list of memory addresses stored in an array. It's very useful if you want to quickly create a list of string literals.

```C
char *names_for_dog[] = {"Browser", "Bonza", "Snodgrass"};
// This is an array that stores pointers.
// There will be one pointer pointing at each string literal.
```



You can access the array of pointers just like you accessed the array of arrays.

#### Why pointers have types

Pointer types exist so that the compiler knows how much to adjust the pointer arithmetic.

For instance, if you add 1 to a char pointer, the pointer will point to the very next memory address because a char occupies 1 byte of memory.

**int** usually takes 4 bytes of space, so if you add 1 to an int pointer, the complied code will actually add 4 to the memory address.

```C
int nums[] = {1,2,3};
printf("nums is at %p\n", nums);
printf("nums + 1 is at %p\n", nums + 1);

// nums is at 0x7ffdca81cabc
// nums + 1 is at 0x7ffdca81cac0

// nums + 1 is 4 bytes away from nums.
// these addresses are printed in hex format.

int arr[5] = {1,2,3,4,5};
printf("result: %p %p %p\n", arr, arr + 1, arr + 2);

// result: 0x7fff82772940 0x7fff82772944 0x7fff82772948
// 4 bytes away from each other.
```



##### Bullet Points

- Array variables can be used as pointers
- but array variables are not quite the same
- Array variables are not pointers themselves. They are just a special type of variable that can be used as pointers in certain contexts thanks to the implicit conversion provided by the C language.
- Array variables have a fixed size and cannot be reassigned to point to a different memory location unlike pointers.
- *sizeof* is different for array and pointer variables.
- Array variables can't point to anything else.
- Passing an array variable to a pointer decays it.
- You can pass array variables as arguments to functions that expect pointers. The array variable decays into a pointer to its first element when passed to the function and the function has no way to determine the size of the original array.
- Arrays start at zero because of pointer arithmetic.
- You can perform pointer arithmetic on array variables just like you would with regular pointers. For example, `arr+1` points to the second element of the array `arr`.
- You can dereference an array variable using * operator, which accesses the value at the memory address it points to. For example, `*arr` gives you the value of the first element of the array.
- Pointer variables have types so they can adjust pointer arithmetic.