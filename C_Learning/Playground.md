### `strcspn()`

The `strcspn` function in C is part of the C standard library, declared in the `<string.h>` header file. It computes the length of the initial segment of the first string (`str1`) which consists entirely of characters not in the second string (`str2`). In other words, it searches for the first occurrence of any character from `str2` in `str1` and returns the number of characters in `str1` before this first occurrence.

Here's the prototype for `strcspn`:

```C
size_t strcspn(const char *str1, const char *str2);
```

### Detailed Explanation

- **Parameters:**
  - `const char *str1`: A pointer to the null-terminated string to be scanned.
  - `const char *str2`: A pointer to the null-terminated string containing the characters to match.
- **Return Value:**
  - The function returns the number of characters in the initial segment of `str1` which are not in `str2`. If `str1` does not contain any character from `str2`, the function returns the length of `str1`.

### Example

```C
#include <stdio.h>
#include <string.h>

int main() {
    const char *str1 = "hello, world";
    const char *str2 = "o,";
    
    size_t len = strcspn(str1, str2);
    
    printf("The length of the initial segment of str1 not containing any character from str2 is: %zu\n", len);
    
    return 0;
}
```

### Explanation of the Example

- `str1` is `"hello, world"`.
- `str2` is `"o,"`.

The function `strcspn` will scan `str1` and stop at the first occurrence of any character that appears in `str2`.

- The first character in `str1` that matches any character in `str2` is `o`, which is at index 4.

So, the output of this program will be:

```C
The length of the initial segment of str1 not containing any character from str2 is: 4
```

### Use Cases

- **Parsing strings**: When you need to find the length of a segment in a string that does not include certain delimiter characters.
- **Validation**: Checking for the presence of any invalid characters in an input string up to a certain point.

`strcspn` is a useful function for various string processing tasks where identifying non-matching segments is required.

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"

int name_length(char *name);

int main()
{
    char name[25];
    printf("Enter your name: ");
    fgets(name, 25, stdin);
    name[strcspn(name, "\n")] = '\0';
    printf("Your name is %d characters long.\n", name_length(name));
    return 0;
}

int name_length(char *name)
{
    int i = 0;
    while (name[i] != '\0')
    {
        i++;
    }
    return i;
} 
```

In this code, the `strcspn` function is used to remove the newline character (`'\n'`) that `fgets` includes when the user presses Enter. Here's a step-by-step explanation of what `strcspn` does in this context:

1. **Reading User Input**:

   ```C
   fgets(name, 25, stdin);
   ```

   This line reads up to 24 characters from the standard input (stdin) and stores them in the `name` array. The `fgets` function also stores a newline character (`'\n'`) at the end of the input if there is space available in the buffer.

2. **Removing the Newline Character**:

   ```C
   name[strcspn(name, "\n")] = '\0';
   ```

   - `strcspn(name, "\n")` calculates the length of the initial segment of `name` that does not contain the newline character (`'\n'`).
   - If a newline character is found in the `name` string, `strcspn` returns the index of this newline character.
   - This index is then used to place a null terminator (`'\0'`) at that position in the `name` string, effectively removing the newline character and terminating the string.

Here is a more detailed breakdown of the steps in context:

### Detailed Steps:

1. **User Input**:
   - Assume the user types "John Doe" and presses Enter.
   - `fgets` will store `"John Doe\n"` in the `name` array.
2. **Using `strcspn` to Find Newline**:
   - `strcspn(name, "\n")` finds the index of the newline character.
   - For the input `"John Doe\n"`, `strcspn` returns `8` because the newline character is at the 8th position (0-based index).
3. **Replacing Newline with Null Terminator**:
   - `name[8] = '\0';` changes the newline character to a null terminator.
   - Now `name` contains `"John Doe"` without the newline character.

### Summary:

In this code, `strcspn` is used to determine the position of the newline character that `fgets` adds at the end of the input. By replacing this newline with a null terminator, the code ensures that the `name` string is correctly terminated, allowing accurate calculation of its length and proper handling of the string in subsequent operations.

---



### Fibonacci Sequence

```C
#include <stdio.h>

void printFibonacci(int n){
    int t1 = 0, t2 = 1, nextTerm;
    
    for(int i = 1; i <= n; i++){
        if(i == n){
            printf("%d", t1); // Print the last term without a comma
        } else{
            printf("%d, ", t1); // Print the terms with a comma
        }
        nextTerm = t1 + t2;
        t1 = t2;
        t2 = nextTerm;
    }
}

int main(){
    int n;
    
    printf("Enter the number of terms: ");
    scanf("%d", &n);
    
    printFibonacci(n);
    
    return 0;
}
```

### Code Breakdown:

1. **Function Declaration**:

   ```C
   void printFibonacci(int n) {
   ```

   - The function `printFibonacci` takes an integer `n` as an argument, which represents the number of terms in the Fibonacci sequence to be printed.
   - The return type is `void` because the function does not return any value; it only prints the sequence.

2. **Variable Initialization**:

   ```C
   int t1 = 0, t2 = 1, nextTerm;
   ```

   - `t1` and `t2` are initialized to the first two terms of the Fibonacci sequence (0 and 1, respectively).
   - `nextTerm` will be used to store the next term in the sequence.

3. **Print Statement**:

   ```C
   printf("Fibonacci Sequence: ");
   ```

   - This prints the label "Fibonacci Sequence: " to the console.

4. **For Loop**:

   ```C
   for (int i = 1; i <= n; ++i) {
   ```

   - The loop runs from `i = 1` to `i = n`, iterating `n` times to print `n` terms of the sequence.

5. **Conditional Printing**:

   ```C
   if (i == n) {
       printf("%d", t1); // Print the last term without a comma
   } else {
       printf("%d, ", t1); // Print the term with a comma
   }
   ```

   - Inside the loop, there is a conditional check to determine if the current term is the last one (`i == n`).
   - If it is the last term, it prints `t1` without a comma.
   - Otherwise, it prints `t1` followed by a comma and a space.

6. **Update Terms**:

   ```C
   nextTerm = t1 + t2;
   t1 = t2;
   t2 = nextTerm;
   ```

   - These lines update the values of `t1` and `t2` for the next iteration.
   - `nextTerm` is calculated as the sum of `t1` and `t2`.
   - `t1` is then updated to the value of `t2`.
   - `t2` is updated to the value of `nextTerm`.

---



### Calculator

```C
#include <stdio.h>

int main(){
    double num1;
    double num2;
    char math_operation = '\0';
    double result = 0.0;
    
    printf("Enter num1: ");
    scanf("%lf", &num1);
    
    printf("Enter num2: ");
    scanf("%lf", &num2);
    
    while(math_operation != '+' && math_operation != '-' && math_operation != '*' && math_operation != '/'){
        printf("Enter the math_operation: (+, -, *, /): ");
        scanf(" %c", &math_operation); // space before %c to consume any newline character left in the input buffer.
    }
    
    switch(math_operation){
        case '+':
            result = num1 + num2;
            break;
        case '-':
            result = num1 - num2;
            break;
        case '*':
            result = num1 * num2;
            break;
        case '/':
            result = num1 / num2;
            break;
        default:
            printf("Invaid math_operation\n");
            break;
    }
    printf("Result: %.2lf\n", result);
    return 0;
}
```

​	Code Execution:

```sh
Enter num1: 4
Enter num2: 5
Enter math_operation: (+, -, *, /): /
Result: 0.80
```



#### Circle Circumference

```c
#include <stdio.h>

int main(){
    const double PI = 3.14159;
    double radius;
    double circumference;
    double area;
    
    printf("Enter the radius of a circle: \n");
    scanf("%lf", &radius);
    
    circumference = 2 * PI * radius;
    area = PI * radius * radius;
    
    printf("circumference: %lf\n", circumference);
    printf("area: %lf\n", area);
    
    return 0;
}
```



---

#### Hypotenuse

```c
#include <stdio.h>
#include <math.h>

int main(){
    double A;
    double B;
    double C;
    
    printf("Enter side A: \n");
    scanf("%lf", &A);
    
    printf("Enter side B: \n");
    scanf("%lf", &B);
    
    C = sqrt(A*A + B*B);
    
    printf("C: %lf\n", C);
    
    return 0;
}
```



---



#### Temperature conversion

```c
#include <stdio.h>
#include <ctype.h>

int main(void)[
    char unit;
    float temp;
    
    printf("Is the temperature in (F) or (C)?: \n");
    scanf("%c", &unit);
    
    unit = toupper(unit);
    
    if(unit == 'C'){
        printf("Enter the temperature in Celsius: \n");
        scanf("%f", &temp);
        temp = (temp * 9/5) + 32;
        printf("The temp in Fahrenheit is: %.2f\n", temp);
    }else if(unit == 'F'){
        printf("Enter the temperature in Fahrenheit: \n");
        scanf("%f", &temp);
        temp = ((temp-32) * 5) / 9;
        printf("The temperature in Celsius is: %.2f\n", temp);
    }else{
        printf("Invalid input\n");
    }
    return 0;
]
```



---



#### Calculator

```c
#include <stdio.h>
#include <ctype.h>

int main(void){
    char operator;
    double num1;
    double num2;
    double result;
    
    // Constantly prompting until the input is double type.
    while(1){
        printf("Enter an operator (+ - * /): \n");
        scanf("%c", &operator);
        
        //Clear the input buffer
        while(getchar() != '\n');
        //Clear the input buffer unitl newline character is encountered.
        
        //Check if the input operator is valid
        if(operator == '+' || operator == '-' || operator == '*' || operator == '/'){
            break;
            // Exit the loop if a valid operator is entered.
        }else{
            printf("Invalid operator. Please try again.\n");
        }
    }
    
    printf("Enter number 1: \n");
    scanf("%lf". &num1);
    
    printf("Enter number 2: \n");
    scanf("%lf", &num2);
    
    switch(operator){
        case '+':
            result = num1 + num2;
            printf("The result: %.3lf\n", result);
            break;
        case '-':
            result = num1 - num2;
            printf("The result: %3lf\n", result);
            break;
        case '*':
            result = num1 * num2;
            printf("The result: %3lf\n", result);
            break;
        case '/':
            result = num1 / num2;
            printf("The result: %3lf\n", result);
            break;
    }
    
    return 0;
}
```

---



#### String Length

```C
#include <stdio.h>

int main(void){
    char name[25];
    puts("Name: ");
    scanf("%s", name);
    
    int n = 0;
    while(name[n] != '\0')
        n++;
    printf("%i\n", n);
    
    return 0;
}
```

---

