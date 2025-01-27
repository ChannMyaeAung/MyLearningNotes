#### Three-card monte

```C
#include <stdio.h>

int main(){
    char *cards = "JQK";
    char a_card = cards[2];
    cards[2] = cards[1];
    cards[1] = cards[0];
    cards[0] = cards[2];
    cards[2] = cards[1];
    cards[1] = a_card;
    puts(cards);
    return 0;
}
```

The above code will result in an error because the string literals can never be updated.

A variable that points to a string literal can't be used to change the contents of the string:

```C
char *cards = "JQK"; // This variable can't modify this string
```

But if you create an array from a string literal, then you can modify it:

```C
char cards[] = "JQK";
```

1. The computer loads the string literal.
   - when the computer loads the program into memory, it puts all of the constant values into the constant memory block. This section of memory is read-only.
2. The program creates the cards variable on the stack.
3. The cards variable is set to the address of "JQK".
   - The cards variable will contain the address of the string literal "JQK".
4. The computer  tries to change the string.
   - It cannot change because it's read-only.

#### If you're going to change a string, make a copy

If you create a copy of the string in an area of memory that's not read-only, there won't be a problem if you try to change the letters it contains.

We can make a copy just by creating the string as a new array.

```C
char cards[] = "JQK";
// cards is not just a pointer. cards is now an array.
```

In the old code, cards was just a pointer. In the new code, it's an array. By declaring an array called cards and then set it to a string literal, the cards array will be a completely new copy. The variable isn't just pointing at the string literal. It's a brand-new array that contains a fresh copy of the string literal.

```C
int my_function{
    char cards[] = "JQK";
    // cards is an array
    // There is no array size given, so you have to set it to something immediately.
}

// The following two functions are equivalent.
void stack_deck(char cards[]){
    ...
}

void stack_deck(char *cards){
    ...
}

```

With our new code `char cards[] = "JQK"`,

1. The computer loads the string literal.
2. The program creates a new array on the stack.
3. The program initializes the array.
   - As well as allocating the space, the program will also copy the contents of the string literal "JQK" into the stack memory.

**Difference:** the original code used a pointer to point to a read-only string literal. But if you initialize an array with a string literal, you then have a copy of the letters and you can change them as much as you like.

```C
#include <stdio.h>

int main(void){
    char cards[] = "JQK";
    char a_card = cards[2];
    cards[2] = cards[1];
    cards[1] = cards[0];
    cards[0] = cards[2];
    cards[2] = cards[1];
    cards[1] = a_card;
    puts(cards);

    return 0;
}

// Output: QKJ
```

The `cards` variable now points to a string in an unprotected section of memory, so we are free to modify its contents.

**Tips: **

One way to avoid this problem in the future is to never write code that sets a simple char pointer to a string literal value like:

```C
char *s = "Some string";
```

There's nothing wrong with setting a pointer to a string literal - the problems only happen when you try to modify a string literal. Instead, if you want to set a pointer to a literal, always make sure you use the `const` keyword.

```C
const char *s = "some string";
```

That way if the compiler sees some code that tries to modify the string, it will give you a compile error:

```C
s[0] = 'S'; 
```

##### Bullet points

- if you see * in a variable declaration, it means the variable will be a pointer.
- String literals are stored in read-only memory.
- Why are string literals stored in read-only memory? Because they are designed to be constant. If you write a function to print "Hello World", you don't want some other part of the program modifying the "Hello World" string literal.
- If you want to modify a string, you need to make a copy in a new array.
- You can declare a `char` pointer as `const char *` to prevent the code from using it to modify a string. The `const` modifier means that the compiler will complain if you try to modify an array with that particular variable.
- When the program is complied, all the references to array variables are replaced with the addresses of the array. So the truth is that the array variable won't exist in the final executable. That's OK because the array variable will never be needed to point anywhere else.