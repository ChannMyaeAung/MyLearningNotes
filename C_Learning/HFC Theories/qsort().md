#### `qsort()`

The `qsort` function in C is used to sort items in an array. It takes 4 parameters.

1. `base`: A pointer to the first element of the array to be sorted.
2. `nitems`: The number of elements in the array.
3. `size`: The size in bytes of each element in the array. This is typically calculated as `sizeof(type)`, where `type` is the data type of the elements.
4. `compar`: A function pointer to a comparison function. This function takes two pointers (pointing to two elements to be compared) and returns an integer less than, equal to, or greater than zero if the first argument is considered to be respectively less than, equal to, or greater than the second.

```C
int scores[] = {543, 323, 32, 554, 11, 3, 112};

qsort(scores, 7, sizeof(int), compare_scores_desc);

char *names[] = {"Karen", "Mark", "Brett", "Molly"};

qsort(names, 4, sizeof(char *), compare_names);
```



#### Sorting Explanation

- < 0 if x should go before y.
- 0 if x is equivalent to y.
- `> 0` if x should go after y. 

```C
int a[] = {8, 7, 2, 4, 6, 3, 5, 1, 9, 0};
```



Considering the above array:

```C
return x - y;
```

Let's say `x` = 2 and `y` = 7;

x - y = 2 - 7 = - 5 which is less than zero so x should go before y which means in ascending order. (2...7)



```C
return y - x;
```

Now same as above `x` = 2 and `y` = 7;

y - x = 7 - 2 = 5 which is greater than zero so x should go after y which means in descending order. (7....2)

