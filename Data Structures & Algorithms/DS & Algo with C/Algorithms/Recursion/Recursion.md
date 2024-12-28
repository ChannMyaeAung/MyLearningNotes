## Recursion

- Recursion is when a function calls itself.
- Some programming languages (particularly functional programming languages like Scheme, ML, or Haskell use recursion as a basic tool for implementing algorithms that in other languages would typically be expressed using iteration (loops)).
- Procedural languages like C tend to emphasize iteration over recursion, but can support recursion as well.

`practice.h`

```C
void printRangeIterative(int start, int stop);
void printRangeRecursive(int start, int stop);
void printRangeRecursiveReversed(int start, int stop);
void printRangeRecursiveSplit(int start, int stop);
```

`functions.c`

```C
void printRangeIterative(int start, int stop)
{
    for (int i = start; i < stop; i++)
    {
        printf("%d\n", i);
    }
}
void printRangeRecursive(int start, int stop)
{
    if (start < stop)
    {
        printf("%d\n", start);
        printRangeRecursive(start + 1, stop);
    }
}
void printRangeRecursiveReversed(int start, int stop)
{
    if (start < stop)
    {
        printRangeRecursiveReversed(start + 1, stop);
        printf("%d\n", start);
    }
}
void printRangeRecursiveSplit(int start, int stop)
{
    int mid;

    if (start < stop)
    {
        mid = (start + stop) / 2;
        printRangeRecursiveSplit(start, mid);
        printf("%d\n", mid);
        printRangeRecursiveSplit(mid + 1, stop);
    }
}
```

`practice.c`

```C
#define Noisy(x) (puts(#x), x)

int main(int argc, char *argv[])
{
    if (argc != 1)
    {
        fprintf(stderr, "Usage: %s\n", argv[0]);
        return 1;
    }

    Noisy(printRangeIterative(0, 10));
    Noisy(printRangeRecursive(0, 10));
    Noisy(printRangeRecursiveReversed(0, 10));
    Noisy(printRangeRecursiveSplit(0, 10));
    return 0;
}
```

`Output`

```shell
chan@CMA:~/C_Programming/practice$ ./practice
printRangeIterative(0, 10)
0
1
2
3
4
5
6
7
8
9
printRangeRecursive(0, 10)
0
1
2
3
4
5
6
7
8
9
printRangeRecursiveReversed(0, 10)
9
8
7
6
5
4
3
2
1
0
printRangeRecursiveSplit(0, 10)
0
1
2
3
4
5
6
7
8
9

```

- The function `printRangeRecusive` is an example of solving a problem using a divide and conquer approach.
- When we hit the bottom, the call stack will look something like this:

```C
printRangeRecursive(0, 10)
    printRangeRecursive(1, 10)
    printRangeRecursive(2, 10)
    printRangeRecursive(3, 10)
    printRangeRecursive(4, 10)
    printRangeRecursive(5, 10)
    printRangeRecursive(6, 10)
    printRangeRecursive(7, 10)
    printRangeRecursive(8, 10)
    printRangeRecursive(9, 10)
    printRangeRecursive(10, 10)
```

- This works because each call to `printRangeRecursive` gets its own parameters and its own variables separate from the others, even the ones that are still in prograss.
- So each will print out `start` and then call another copy in to print `start+1` etc.
- In the last call, we finally fail the test `start<stop`, so the function exits, then its parent exits, and so on until we unwind all the calls on the stack back to the first one.
- In `printRangeRecursiveReversed`, the calling pattern is exactly the same, but now instead of printing `start` on the way down, we print `start` on the way back, after making the recursive call.
  - This means that in `printRecursiveReversed(0, 10)`, 0 is printed only after the results of `printRangeRecursiveReversed(1, 10)`, which gives us the countdown effect.
- `printRecursiveSplit` takes a much more aggressive approach to dividing up the problem, it splits a range [0, 10] as two ranges [0, 5) and [6, 10) separated by a midpoint 5.

```C
printRangeRecursiveSplit(0, 10)
    printRangeRecursiveSplit(0, 5)
    printRangeRecursiveSplit(0, 2)
    printRangeRecursiveSplit(0, 1)
    printRangeRecursiveSplit(0, 0)
    printRangeRecursiveSplit(1, 1)
    printRangeRecursiveSplit(2, 2)
    printRangeRecursiveSplit(3, 5)
    printRangeRecursiveSplit(3, 4)
    printRangeRecursiveSplit(3, 3)
    printRangeRecursiveSplit(4, 4)
    printRangeRecursiveSplit(5, 5)
    printRangeRecursiveSplit(6, 10)   
    ... etc.
```

- the computation has the structure of a tree instead of a list, so it is not so obvious how one might rewrite this procedure as a loop.

### Blowing out the stack

- Blowing out the stack is what happens when a recursion is too deep.
- Typically, the OS puts a hard limit on how big the stack can grow, on the assumption that any program that grows the stack too much has gone insane and needs to be killed before it does more damage.
- One of the ways this can happen is if we forget the base case, but it can also happen if we just try to use a recursive function to do too much.
- For example, if we call `printRangeRecursive(0, 1000000)`, we will probably get a segmentation fault after the first 100, 000 numbers or so.
- For this reason, it's best to try to avoid linear recursions like the one in `printRangeRecursive`, where the depth of the recursion is proportional to the number of things we are doing.
- Much safer are even splits like `printRangeRecursiveSplit`, since the depth of the stack will now be only logarithmic in the number of things we are doing.
- Fortunately, linear recursions are often **tail-recursive**, where the recursive call is the last thing the recursive function does.

### Failure to make progress

Sometimes we end up blowing out the stack because we though we were recursing on a smaller instance of the problem, but in fact we weren't.

```C
void printRangeRecursiveSplitBad(int start, int stop){
    int mid;
    if(start == stop){
        print("%d\n", start);
    }else{
        mid = (start + stop) / 2;
        printRangeRecursiveSplitBad(start, mid);
        printRangeRecursiveSplitBad(mid, stop);
    }
}
```

- This will get stack on as simple a call as `printRangeRecursiveSplitBad(0, 1)`.
- It will set `mid` to 0.
- While the recursive call to `printRangeRecursiveSplitBad(0, 0)` will work just fine, the recursive call to `printRangeRecursiveSplitBad(0, 1)` will put us back where we started, giving us infinite recursion.



### Tail-recursion and iteration

- **Tail recursion** is when a recursive function calls itself only once, and as the last thing it does.
- The `printRangeRecursive` function is an example of a tail-recursive function.

```C
void printRangeRecursive(int start, int stop){
    if(start < stop){
        printf("%d\n", start);
        printRangeRecursive(start + 1, stop);
    }
}
```

- The nice thing about tail-recursive functions is that we can always translate them directly into iterative functions.
- The reason is that when we do the tail call, we are effectively replacing the current copy of the function with a new copy with new arguments.
- So rather than keeping around the old zombie parent copy which has no purpose other than to wait for the child to return and then return itself, we can reuse it by assigning new values to its arguments and jumping back to the top of the function.