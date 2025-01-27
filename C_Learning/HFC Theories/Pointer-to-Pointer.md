#### pointer-to-pointer

In C, a pointer is a variable that holds the memory address of another variable. A pointer-to-pointer, denoted by two asterisks(**), is a form of multiple indirection, or a chain of pointers. Essentially, a pointer-to-pointer is a pointer that points to another pointer.

```C
int x = 5; // An integer
int *p = &x; // A pointer that holds the address of x
int **q = &p; // A pointer-to-pointer that holds the address of p
```

In this example, `p` points to `x`, and `q` points to `p`. So, `q` is a pointer-to-pointer.

**Now why do we use a double asterisk or a pointer-to-pointer?**

One common use of pointer-to-pointer is in functions that need to modify the content of a pointer passed as arguments.

- In C, everything is passed by value, including pointers. This means that if you pass a pointer to a function, the function works with a copy of the pointer, not the original pointer itself.
- If you want to change the original pointer, you need to pass a pointer to the pointer, i.e. , a pointer-to-pointer.

```C
void change_pointer(int **p){
    int y = 10;
    *p = &y;
}

int main(){
    int x = 5,
    int *p = &x;
    printf("Before calling change_pointer: %d\n", *p); // Before modification, this will print 5.
    change_pointer(&p);
    printf("After calling change_pointer: %d\n", *p); // After modification, this will print 10
    return 0;
}
```

In this example:

1. In the `change_pointer` function:
   - It takes a pointer to a pointer to an integer (`int **p`) as its parameter.
   - Inside the function, a local integer variable `y` is declared and assigned the value 10.
   - Then, the pointer `p` is updated to point to the address of `y` using the address-of operator (`&`). This means that `p` now holds the address of the local variable `y`.
2. In the `main` function:
   - An integer variable `x` is declared and assigned the value 5.
   - A pointer `p` is declared and initialized to point to the address of `x`.
   - The value of `p` is printed before and after the function call to `change_pointer`.
   - After the function call, the value of `p` has been changed to point to the address of the local variable `y` within the `change_pointer` function.

So, `p` is a pointer to an integer (`int *`), and `&p` is a pointer to a pointer to an integer (`int **`). By passing `&p` to the `change_pointer` function, we allow the function to modify the value of `p`, effectively changing what it points to.