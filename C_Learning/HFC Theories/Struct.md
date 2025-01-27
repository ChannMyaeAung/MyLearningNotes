#### Struct

Create your own structured data types with a `struct`.

- If you have a set of data that you need to bundle together into a single thing, then you can use a `struct`.

- The word `struct` is short for `structured data type`.

- A `struct` will let you take all of those different pieces of data into the code and wrap them up into one large new data type.

  - ```C
    struct fish{
        const char *name;
        const char *species;
        int teeth;
        int age;
    }
    ```

- It's a little bit like an array except:

  - It's fixed length.
  - The pieces of data inside the `struct` are given names.

- We can create pieces of data that use it:

  - ```C
    struct fish snappy = {"Snappy", "Piranha", 69, 4}
    ```

  - `struct fish` is the data type.

  - `snappy` is the variable name.

  - `"Snappy"` is the name.

  - `"Piranha"` is the species.

  - `69` is the number of teeth.

  - `4`is Snappy's age.



Q: What's that `const char` thing again?

A: `const char *` is used for strings that you don't want to change. That means it's often used to record string literals.



Q: Does this `struct` store the string?

A: In this case, no. The `struct` here just stores a pointer to a string. That means it's just recording an address, and thee string lives somewhere else in memory.



Q: But you can store the whole string in there if you want?

A: Yes, if you define a `char` array in the string, like `char name[20]`;

#### Read a struct's fields with the "." operator

Even though a struct stores fields like an array, the only way to access them is by name.

```C
struct fish{
    const char *name;
    const char *species;
    int teeth;
    int age;
}

struct fish snappy = {"Snappy", "piranha", 69, 4}

printf("Name = %s\n", snappy.name);
//This will return the string "Snappy".
// Name = Snappy
```



- One of the great things about data passing around inside `struct`s is that you can change contents of your `struct` without having to change the functions that use it.

- For example, let's say you want to add an extra field to `fish`:

  ```C
  struct fish{
      const char *name;
      const char *species;
      int teeth;
      int age;
      int favorite_music;
  }
  ```

  - All the `catalog()` and `label()` which are the functions that uses the `struct fish` which can be found in `HFCExercises.md` have been told is they're going to be handed a `fish`. They don't know (and don't care) that the `fish` now contains more data, so long as it has all the fields they need.
  - That means that `struct`s don't just make your code easier to read, they also make it better able to cope with change.

Q: is a `struct` just an array?

A: No but like an array, it groups a number of pieces of data together.



Q: An array variable is just a pointer to the array. Is a `struct` variable a pointer to a `struct`?

A: No, a `struct` variable is a name for the `struct` itself.



Q: Are `structs` like classes in other languages?

A: They're similar but it's not so easy to add methods to `struct`s.



#### Structs In Memory Up Close

- The assignment copies the pointers to strings, not the strings themselves.

- When you assign one `struct` to another, the contents of the `struct` will be copied.

- But if that includes pointers, the assignment will just copy the pointer values.

  ```C
  struct fish{
      const char *name;
      const char *species;
      int teeth;
      int age;
  };
  
  struct fish snappy = {"Snappy", "Piranha", 69, 4};
  struct fish gnasher = snappy;
  
  // gnasher and snappy both point to the same strings.
  ```

  - That means the `name` and `species` fields of `gnasher` and `snappy` both point to the same strings.

- When we define a new variable, the computer will need to create some space in memory for an instance of the `struct`.

- That space in memory will need to be big enough to contain all of the fields within the `struct`.

- The computer will create a brand-new copy of the `struct` when we assign a `struct` to another variable.

- That means it will need to allocate another piece of memory of the same size, then copy over each of the fields.

- When we are assigning `struct` variables, we are telling the computer to copy data.



#### Can yhou put one struct inside another?

When we define a `struct`, we are actually creating a new data type.

If a `struct` creates a data type from existing data types, that means we can also **create `structs` from other `structs`.**

```C
struct preferences{
    const char *food;
    float exercise_hours;
};

struct fish{
    const char *name;
    const char *species;
    int teeth;
    int age;
    struct preferences care;
}
```

- This is a `struct` inside a `struct`. This is called nesting.
- Our new field is called "care", but it will contain fields defined by the "preferences" `struct`.
- The above code tells the computer one `struct` will contain another `struct`.

```C
struct fish snappy = {"Snappy", "Piranha", 69, 4, {"Meat", 7.5}};
// "Meat" - The value for care.food
// 7.5 - The value for care.exercise_hours
// {"Meat", 7.5} - struct data for the care field.
```

- Once we've combined `struct` together, we can access the fields using a chain of "." operators:

  ```C
  printf("Snappy likes to eat %s", snappy.care.food);
  printf("Snappy likes to exercise for %f hours", snappy.care.exercise_hours);
  ```


#### Typedef

C allows you to create an **alias** for any `struct` that you create. If you add the word **typedef** before the `struct` keyword and a **type name** after the closing brace, you can call the new type whatever you like:

Before:

```C
struct cell_phone{
    int cell_no;
    const char *wallpaper;
    float minutes_of_charge;
};

struct cell_phone p = {5557879, "sinatra.png", 1.35};
```



After:

```C
typedef struct cell_phone{
    int cell_no;
    const char *wallpaper;
    float minutes_of_charge;
} phone;

phone p = {5557879, "sinatra.png", 1.35};

```



- `typedef` means you are going to give the `struct` type a new name.

- `phone` will become an alias for `struct cell_phone`.

- Now, when the compiler sees "phone", it will treat it like `struct cell_phone`.

- `typedef` can shorten your code and make it easier to read.

- If you use `typedef` to create an alias for a `struct`, you will need to decide what your alias will be.

- The alias is just the name of your type that means there are two names to think about: the name of the `struct(struct cell_phone)` and the name of the `type(phone)`.

- Why have two names? You usually don't need both. The compiler is quite happy for you to skip the `struct` name like this:

  ```C
  typedef struct{
      int cell_no;
      const char *wallpaper;
      float minutes_of_charge;
  } phone;
  phone p = {5557879, "sinatra.png", 1.35};
  ```

  



#### Bullet Points

- A `struct` is a data type made from a sequence of other data types.
- `struct`s are fixed length.
- `struct` fields are accessed by name, using `<struct>.<field name>` syntax(aka dot notation).
- `struct` fields are stored in memory in the same order they appear in the code.
- You can nest `struct`s.
- `typedef` creates an alias for a data type.
- If you use `typedef` with a `struct`, then you can skip giving the `struct` a name.



Q: Do `struct`  fields get placed next to each other in memory?

A: Sometimes there are small gaps between the fields.



Q: Why is that?

A: The computer likes data to fit inside the word boundaries. SO, if a computer uses 32-bit words, it won't want a `short` , say, to be split over a 32-bit boundary.



Q: So it would leave a gap and start the `short` in the next 32-bit word?

A: Yes.



Q: Does that mean each field takes up a whole word?

A: No, the computer leaves gaps only to prevent fields from splitting across word boundaries. If it can fit several fields into a single word, it will.



Q: Why does the computer care so much about word boundaries?

A: it will read complete words from the memory. If a field was split across more than one word, the CPU would have to read several locations and somehow stitch the value together. That'd be slow.



Q: In languages like Java, if I assign an object to a variable, it doesn't copy the object, it just copies a reference. Why is it different in C?

A: In C, all assignments copy data. If you want to copy a reference to a piece of data, you should assign a pointer.



Q: I'm really confused about `struct` names. What's the `struct` name and what's the alias?

A: The `struct` name is the word that follows the `struct` keyword. If you write `struct peter_parker{...}`, then the name is `peter_parker` and when you create variables, you would say `struct peter_parker x`. Sometimes you don't want to keep using the `struct` keyword when you declare variables, so `typedef` allows you to create a single word alias. In `typedef struct peter_parker{...} spider_man;`, `spider_man` is the alias.



Q: What's an anonymous `struct`?

A: One without a name, so `typedef struct {...} spider_man;` has an alias of `spider_man` but no name. Most of the time, if you create an alias, you don't need a name.



#### how do you update a struct?

A `struct` is really jsut a bundle of variables, grouped together and treated like a single piece of data.

We can change the fileds of a `struct` just like any other variable.

```C
fish snappy = {"Snappy", "piranha", 69, 4};
printf("Hello %s\n", snappy.name);
snappy.teeth = 68;
```



If we take a look at this code:

```C
#include <stdio.h>

typedef struct{
    const char *name;
    const char *species;
    int age;
} turtle;

void happy_birthday(turtle t){
    t.age = t.age + 1;
    printf("Happy Birthday %s! You are now %i years old!\n");
}

int main(){
    turtle myrtle = {"Myrtle", "Leatherback sea turtle", 99};
    happy_birthday(myrtle);
    printf("%s's age is now %i\n", myrtle.name, myrtle.age);
    return 0;
}
```



When we run:

```bash
chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ make all
Compiling the main file...
gcc -c main.c 
Compiling the final turtle exe file...
gcc main.o functions.o -o turtle

chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ ./turtle
Happy Birthday Myrtle! You are now 100 years old!
Myrtle's age is now 99

```



- Even though the `age` was updated by the function, when the code returned to the main function. the `age` seemed to reset itself.

- The code is cloning the turtle. When we assign a `struct`, its values get copied to the new `struct`.

- ```C
  happy_birthday(myrtle);
  ```

  - myrtle - This is the turtle that we are passing to the function. The myrtle `struct` will be copied to this parameter.

- In C, parameters are passed to functions by **value**. That means that when we call a function, the values we pass into it are assigned to the parameters.

- So in this code, it's almost as if we had written something like this:

  ```C
  turtle t = myrtle;
  ```

- When we assign `structs` in C, the values are copied.

- When we call the function, the parameter `t` will contain a copy of the `myrtle struct`.

- It's as if the function has a clone of the original turtle.

- So, the code inside the function does update the age of the turtle, but it's a different turtle.

- What happens when the function returns?

- The `t` parameter disappears, and the rest of the code in `main()` uses `myrtle struct`. But the value of `myrtle` was never changed by the code. It was always a completely separate piece of data.



#### So what do we do if we want to pass a `struct` to a function that needs to update it?

##### We need a pointer to the `struct`

- When we pass a variable to the `scanf()` function, we couldn't pass the variable itself to `scanf()`; we have to pass a pointer:

  ```C
  scanf("%f", &length_of_run);
  ```

  

- If we tell the `scanf()` function where the variable lives in memory, then the function will be able to update the data stored at the place in memory, which means it can update the variable.

- We can just do the same with `struct`.

- ```C
  void happy_birthday(turtle *t){
     (*t).age = (*t).age + 1;
      printf("Happy Birthday %s! You are now %i years old!\n", (*t).name, (*t).age);
  }
  happy_birthday(&myrtle);
  ```

- The parentheses are really important because `(*t).age` and `*t.age` are very different.



- `(*t).age != *t.age`. 

- `(*t).age`. If t is a pointer to a turtle `struct`, then this is the age of the turtle. (I am the age of the turtle pointed to by t.)

- `*t.age` . If t is a pointer to a turtle `struct`, then this expression is wrong. (I am the contents of the memory location given by `t.age`).

- So the expression `*t.age` is really the same as `*(t.age)`. It means "the contents of the memory location given by `t.age`". But `t.age` isn't a memory location.

- The reason `(*t).age` and `*t.age` are not the smae is due to the precedence of the operators in C.

  - `(*t).age` first dereferences the pointer `t` to access the `turtle struct` and then accesses the `age` field of that `struct`.
  - `*t.age` tried to access the `age` field of the pointer `t` (which doesn't exist because `t` is a pointer, not a `struct`), and then tries to dereference the result, which is not valid and will likely cause a compile error.

- In C, the `.` operator has higher precedence than the `*` operator, so `*t.age` is interpreted as `*(t.age)`, not `(*t).age`.

- To avoid confusion, C provides the `->` operator for accessing the members of a struct through a pointer, so instead of writing `(*t).age` , you can write `t->age`, which is clearer and easier to read.

- `t->age` means `(*t).age`. (The `age` field in the `struct` that `t` points to).

  ```C
  void happy_birthday(turtle *a){
      a->age = a->age + 1;
      printf("Happy Birthday %s! You are now %i years old!\n", a->name, a->age);
  }
  ```

  

- By passing a pointer to the `struct`, we allowed the function to update the original data rather than taking a local copy.

Q: Why are values copied to parameter variables?

A: The computer will pass values to a function by assigning values to the function's parameters. And all assignments copy values.



#### Bullet Points

- When you call a function, the values are copied to the parameter variables.
- You can create pointers to `struct`s, just like any other type.
- The `->` notation cuts down on parentheses and makes the code more readable.