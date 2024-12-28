#### Chapter 4 Exercises

##### Exercise 1

```C
#include <stdio.h>

float total = 0.0;
short count = 0;
short tax_percent = 6;

float add_with_tax(float f);
// Function declaration

int main(){
    float val;
    printf("Price of the item: ");
    while(scanf("%f", &val) == 1){
        printf("Total so far: .2%f\n", total);
        printf("Price of the item: ");
        
    }
    printf("\nFinal Total: %.2f\n", total);
    printf("Number of items: %hi\n", count);
    return 0;
}

// Function to caculate tax based on tax percent and return the total.
float add_with_tax(float f){
    float tax_rate = 1 + tax_percent / 100.0;
    total = total + (f * tax_rate);
    count++;
    return total;
}
```



**Code Breakdown:**

1. We declared some global variables, `total` for the total of all prices entered, `count` for the number of items, and `tax_percent` is the tax rate.
2. The `add_with_tax` function takes a price as an argument. It calculates the tax_rate based on `tax_percent`, adds the tax to the price, adds the result to the total, increments the count of items and returns the new total.
3. The `main()` function starts by declaring a variable `val` to hold the price of the current item.
4. It then enters a loop where it prompts the user to enter a price, reads the price into `val`, adds `val` to the total using `add_with_tax`, and prints the new total. This loop continues until the user enters something that isn't a number.
5. Once the loop ends, the function prints the final total and the number of items.



```bash
chan@CMA:~/C_Programming/HFC$ ./test
Price of item: 25
Total so far: 26.50
Price of item: 24
Total so far: 51.94
Price of item: 34
Total so far: 87.98
Price of item: 45
Total so far: 135.68
Price of item: 


Final total: 135.68
Number of items: 4

```



**Code Execution:**

1. The user enters 25.

2. The `scanf` function reads the input into `val`. `val` is now 25.

3. The `add_with_tax` function is called with `val` as an argument. Inside `add_with_tax`, the tax rate is calculated as `1 + 6 / 100.0 = 1.06`.

4. The price including tax is calculated as `25 * 1.06 = 26.5`. This is added to `total` which was initially 0, so `total` is now 26.5. The count of items is incremented from 0 to 1.

5. The program then goes back to the start of the loop and prompts the user to enter another price. If the user enters a non-number, the loop ends and the final total and number of items are printed.

6. Now the total is `26.5`. The price `24` was added and `24* 1.06 = 25.44`. Then Total is calculated `total = 26.5 + 25.44 = 51.94`. The count is incremented from 1 to 2.

   

#### Compilers don't like surprises

What happened when we don't declared our function before the main function?. 

1. If the compiler see this line of code `printf("Total so far: %.2f\n", add_with_tax(val));`, the compiler figures that it will find out more about the function later in the source file.
2. The compiler needs to know what data type the function will return otherwise, it makes an assumption.
3. When it reaches the code for the actual function, it returns a "conflicting" types for 'add_with_tax' error. The compiler thinks it has two functions with the same name. One function is the real one in the file. The other is the one that the compiler assumed would return an `int`.



#### Split the declaration from the definition

We can avoid the compiler making assumptions by explicitly telling it what functions it should expect. When we tell the compiler about a function, it's called function declaration.

```C
float add_with_tax();
// A declaration has no body code. 
// It just ends with a ; (semicolon)
// The declaration tells the compiler what return value to expect
```



Even better than that, C allows you to take that whole set of declarations out of your code and put them in a header file.

#### Creating our first header file

1. Create a new file with a .h extension.

   `totaller.h`

   ```C
   float add_with_tax(float f);
   ```

2. Include the header file in our main program.

   ```C
   #include <stdio.h>
   #include "totaller.h"
   ```



- The reason why we should surround it with double quotes rather than angle brackets is that when the compiler sees an `include` line with angle brackets, it assumes it will find the header file somewhere off in the directories where the library code lives. 
- But our header file is in the same directory as our `.c` file. 
- By wrapping the header filename in quotes, we are telling the compiler to look for a local file.
- When the compiler reads the `#include` in the code, it will read the contents of the header file, just as if it had been typed into the code.
- `#include` is a preprocessor instruction.



Data types are different sizes on different platforms.

```C
#include <stdio.h>
#include <limits.h>
// This contains the values for the integer types like int and char.
#include <float.h>
// This contains the vlaues for floats and doubles.

int main(){
    printf("The value of INT_MAX is %i\n", INT_MAX);
    printf("The value of INT_MIN is %i\n", INT_MIN);
    printf("An int takes %zu bytes\n", sizeof(int));
    
    printf("The value of FLT_MAX is %f\n", FLT_MAX);
    printf("The value of FLT_MIN is %f\n", FLT_MIN);
    printf("A float takes %zu bytes\n", sizeof(float));
    return 0;
}
```



When you compile and run this code

```bash
The value of INT_MAX is 2147483647
The value of INT_MIN is -2147483648
An int takes 4 bytes
The value of FLT_MAX is 340282346638528859811704183484516925440.000000
The value of FLT_MIN is 0.000000
A float takes 4 bytes

```

`CHAR(chars)`, `DBL(double)`, `SHRT(shorts)`, or `LNG(longs)`

