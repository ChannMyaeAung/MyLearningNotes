### `strcspn()`

The `strcspn` function in C is part of the C standard library, declared in the `<string.h>` header file. It computes the length of the initial segment of the first string (`str1`) which consists entirely of characters not in the second string (`str2`). In other words, it searches for the first occurrence of any character from `str2` in `str1` and returns the number of characters in `str1` before this first occurrence.

Here's the prototype for `strcspn`:

```C
size_t strcspn(const char *str1, const char *str2);
```

#### Detailed Explanation

- **Parameters:**
  - `const char *str1`: A pointer to the null-terminated string to be scanned.
  - `const char *str2`: A pointer to the null-terminated string containing the characters to match.
- **Return Value:**
  - The function returns the number of characters in the initial segment of `str1` which are not in `str2`. If `str1` does not contain any character from `str2`, the function returns the length of `str1`.

#### Example

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

#### Explanation of the Example

- `str1` is `"hello, world"`.
- `str2` is `"o,"`.

The function `strcspn` will scan `str1` and stop at the first occurrence of any character that appears in `str2`.

- The first character in `str1` that matches any character in `str2` is `o`, which is at index 4.

So, the output of this program will be:

```C
The length of the initial segment of str1 not containing any character from str2 is: 4
```

#### Use Cases

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

#### Detailed Steps:

1. **User Input**:
   - Assume the user types "John Doe" and presses Enter.
   - `fgets` will store `"John Doe\n"` in the `name` array.
2. **Using `strcspn` to Find Newline**:
   - `strcspn(name, "\n")` finds the index of the newline character.
   - For the input `"John Doe\n"`, `strcspn` returns `8` because the newline character is at the 8th position (0-based index).
3. **Replacing Newline with Null Terminator**:
   - `name[8] = '\0';` changes the newline character to a null terminator.
   - Now `name` contains `"John Doe"` without the newline character.

#### Summary:

In this code, `strcspn` is used to determine the position of the newline character that `fgets` adds at the end of the input. By replacing this newline with a null terminator, the code ensures that the `name` string is correctly terminated, allowing accurate calculation of its length and proper handling of the string in subsequent operations.

---



### Fibonacci Sequence

```C
#include <stdio.h>

void printFibonacci(int n){
    int t1 = 0, t2 = 1, nextTerm;
    
    for(int i = 1; i <= n; i++){
        if(i == n){
            printf("%d\n", t1); // Print the last term without a comma
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

## Small C Projects



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



### Circle Circumference

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



### Number Guessing Game

```C
#include <stdio.h>
#include <time.h>

void numberGuessingGame(){
    int number = 0;
    int guess = 0;
    int attempts = 0;
    srand(time(0));
    number = rand() % 10 + 1;
    
    printf("Welcome to the Number Guessing Game!\n");
    printf("I have selected a number between 1 and 10. Try to guess it!\n");
    
    do{
        printf("Enter your guess: ");
        scanf("%d", &guess);
        attempts++;
        
        if(guess > number){
            printf("Too high! Try again.\n");
        }else if(guess < number){
            printf("Too low! Try again.\n");
        }else{
            printf("Congratulations! You guessed the number in %d attempts.\n", attempts);
        }
        
    }while(guess != number);
}
```

Code Exeuction:

```sh
chan@CMA:~/C_Programming/test
$ ./final

Welcome to the Number Guessing Game!
I have selected a number between 1 and 10. Try to guess it!
Enter your guess: 9
Too high! Try again.
Enter your guess: 7
Too high! Try again.
Enter your guess: 4
Congratuations! You guessed the number in 3 attempts.

```

Added features (allowed attempts)

```C
int allowed_attempts = 0;

do{
    printf("Enter your guess: ");
    scanf("%d", &guess);
    attempts++;
    allowed_attempts++;
    
    if(allowed_attempts == 3){
        printf("Sorry, you've exceeded the maximum number of attempts. The number was %d.\n", number);
        return 1;
    }
    ...
}while(guess !=number);
```

Code Execution:

```sh
chan@CMA:~/C_Programming/test
$ ./final

Welcome to the Number Guessing Game!
I have selected a number between 1 and 10. Try to guess it!
Enter your guess: 1
Too low! Try again.
Enter your guess: 2
Too low! Try again.
Enter your guess: 3
Sorry, you've exceeded the maximum number of attempts. The number was 5.
```



---



### Quiz App

```C
void quizApp(){
    int score = 0;
    int answer;
    printf("\nWelcome to the Quiz App!\n");
    printf("Answer the following questions:\n");

    // Question 1
    printf("\n1. What is the capital of France?\n");
    printf("1. Berlin\n2. Madrid\n3. Paris\n4. Rome\n");
    printf("Enter your answer: ");
    scanf("%d", &answer);
    if (answer == 3) {
        printf("Correct!\n");
        score++;
    } else {
        printf("Wrong! The correct answer is Paris.\n");
    }

    // Question 2
    printf("\n2. Who wrote 'To Kill a Mockingbird'?\n");
    printf("1. Harper Lee\n2. Mark Twain\n3. J.K. Rowling\n4. Ernest Hemingway\n");
    printf("Enter your answer: ");
    scanf("%d", &answer);
    if (answer == 1) {
        printf("Correct!\n");
        score++;
    } else {
        printf("Wrong! The correct answer is Harper Lee.\n");
    }

    // Question 3
    printf("\n3. What is the largest planet in our Solar System?\n");
    printf("1. Earth\n2. Mars\n3. Jupiter\n4. Saturn\n");
    printf("Enter your answer: ");
    scanf("%d", &answer);
    if (answer == 3) {
        printf("Correct!\n");
        score++;
    } else {
        printf("Wrong! The correct answer is Jupiter.\n");
    }

    printf("\nYou scored %d out of 3.\n", score);
    
}
```

Code Execution:

```sh
chan@CMA:~/C_Programming/practice
$ ./practice
Welcome to the Quiz App!
Answer the following questions:

1. What is the capital of France?
1. Berlin
2. Madrid
3. Paris
4. Rome
Enter your answer: 1
Wrong! The correct answer is Paris.

2. Who wrote 'To Kill a Mockingbird'?
1. Harper Lee
2. Mark Twain
3. J.K. Rowling
4. Ernest Hemingway
Enter your answer: 2
Wrong! The correct answer is Harper Lee.

3. What is the largest planet in our Solar System?
1. Earth
2. Mars
3. Jupiter
4. Saturn
Enter your answer: 3
Correct!
You scored 1 out of 3.

```

#### Better Approach

`functions.c`

```C
int getValidAnswer(int answer)
{
    while (true)
    {
        printf("Enter your answer: ");
        scanf("%d", &answer);
        if (answer >= 1 && answer <= 4)
        {
            break;
        }
        printf("Please enter a number between 1 and 4.\n");
    }
    return answer;
}

int quizApp()
{
    int score = 0;
    int answer = 0;

    printf("Welcome to the Quiz App!\n");
    printf("Answer the following questions!\n");

    // Question 1
    printf("\na. What is the capital of France?\n");
    printf("1. Berlin\n2. Paris\n3. Madrid\n4. Rome\n");
    answer = getValidAnswer(answer);
    if (answer == 2)
    {
        score++;
        printf("Correct!\n");
    }
    else
    {
        printf("Incorrect. The correct answer is 2. Paris.\n");
    }

    // Question 2
    printf("\nb. What is largest planet in our solar system?\n");
    printf("1. Earth\n2. Mars\n3. Jupitar\n4. Saturn\n");
    answer = getValidAnswer(answer);
    if (answer == 3)
    {
        score++;
        printf("Correct!\n");
    }
    else
    {
        printf("Incorrect. The correct answer is 2. Paris.\n");
    }

    // Question 3
    printf("\nc. Who wrote 'TO Kill a Mockingbird'?\n");
    printf("1. Harper Lee\n2. Mark Twain\n3. J.K. Rowling\n4. Ernest Hemingway\n");
    answer = getValidAnswer(answer);
    if (answer == 1)
    {
        score++;
        printf("Correct!");
    }
    else
    {
        printf("Incorrect. The correct answer is 1. Harper Lee.\n");
    }

    while (answer <= 0 || answer > 4)
    {
        printf("Please enter a number between 1 and 4: ");
        scanf("%d", &answer);
    }

    return score;
}
```

`practice.c (main)`

```C
int main()
{
    printf("You score %d out of 3.\n", quizApp());
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Welcome to the Quiz App!
Answer the following questions!

a. What is the capital of France?
1. Berlin
2. Paris
3. Madrid
4. Rome
Enter your answer: -1
Please enter a number between 1 and 4.
Enter your answer: 0
Please enter a number between 1 and 4.
Enter your answer: 2
Correct!

b. What is largest planet in our solar system?
1. Earth
2. Mars
3. Jupitar
4. Saturn
Enter your answer: 2
Incorrect. The correct answer is 2. Paris.

c. Who wrote 'TO Kill a Mockingbird'?
1. Harper Lee
2. Mark Twain
3. J.K. Rowling
4. Ernest Hemingway
Enter your answer: 1
Correct!You score 2 out of 3.
```



### Two in one program

`main.c`

```C
int main(){
    int choice;
    
    printf("Choose an option:\n");
    printf("1. Play Number Guessing Game\n");
    printf("2. Take a QUizz\n");
    printf("3. Exit\n");
    printf("Enter your choice: ");
    scanf("%d", &choice);
    
    switch(choice){
        case 1:
            numberGuessingGame();
            break;
        case 2:
            quizApp();
            break;
        case 3:
            printf("Goodbye!\n");
            break;
        default:
            printf("Invalid choice! Please select 1, 2, or 3.\n");
            break;
    }
    return 0;
}
```

Code Execution:

```sh
chan@CMA:~/C_Programming/practice
$ ./practice

Choose an option:
1. Play Number Guessing Game
2. Take a Quiz
3. Exit
Enter your choice: 3
Goodbye!

chan@CMA:~/C_Programming/practice
$ ./practice

Choose an option:
1. Play Number Guessing Game
2. Take a Quiz
3. Exit
Enter your choice: 1
Welcome to the Number Guessing Game!
I have selected a number between 1 and 10. Try to guess it!
Enter your guess: 3
Too high! Try again.
Enter your guess: 2
Too high! Try again.
Enter your guess: 1
Sorry, you've exceeded the maximum number of attempts. The number was 1.
chan@CMA:~/C_Programming/practice
$ ./practice

Choose an option:
1. Play Number Guessing Game
2. Take a Quiz
3. Exit
Enter your choice: 2
Welcome to the Quiz App!
Answer the following questions:

1. What is the capital of France?
1. Berlin
2. Madrid
3. Paris
4. Rome
Enter your answer: 3
Correct!

2. Who wrote 'To Kill a Mockingbird'?
1. Harper Lee
2. Mark Twain
3. J.K. Rowling
4. Ernest Hemingway
Enter your answer: 1
Correct!

3. What is the largest planet in our Solar System?
1. Earth
2. Mars
3. Jupiter
4. Saturn
Enter your answer: 3
Correct!
You scored 3 out of 3.
```

### Hypotenuse

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



### Temperature conversion

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



### Calculator

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



### String Length

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

### Rock, Paper, Scissors

`functions.c`

```C
void print_menu()
{
    printf("Rock, Paper, Scissors Game\n");
    printf("1. Rock\n");
    printf("2. Paper\n");
    printf("3; Scissors\n");
    printf("Enter your choice (1-3):");
}
int get_user_choice()
{
    // Get the input from the user and read it
    int choice;
    scanf("%d", &choice);

    // If the choice is less than 1 or greater than 3, keep prompting the user until they enter a valid choice
    while (choice < 1 || choice > 3)
    {
        printf("Invalid choice. Please enter a number between 1 and 3.\n");
        scanf("%d", &choice);
    }
    return choice;
}
// Function to get the computer's choice
int get_computer_choice()
{
    // Randomize the choice from 1 to 3
    return (rand() % 3) + 1;
}
void determine_winner(int user_choice, int computer_choice)
{
    const char *choices[] = {"Rock", "Paper", "Scissors"};

    printf("You chose %s.\n", choices[user_choice - 1]);
    printf("The computer chose %s.\n", choices[computer_choice - 1]);

    if (user_choice == computer_choice)
    {
        printf("It's a tie!\n");
    }
    else if ((user_choice == 1 && computer_choice == 3) || (user_choice == 2 && computer_choice == 1) || (user_choice == 3 && computer_choice == 2))
    {
        printf("You win!\n");
    }
    else
    {
        printf("You lose!\n");
    }
}
```

`practice.h`

```C
// Functions.c
void print_menu();
int get_user_choice();
int get_computer_choice();
void determine_winner(int user_choice, int computer_choice);
// Functions_2.c
```



`main.c`

```C
int main()
{
    int user_choice;
    int computer_choice;
    char play_again;

    // Seed the random number generator
    srand(time(NULL));

    // Keep playing until the user press any key other than 'y' or 'Y'
    do
    {
        // Print the menu and get the user's choice
        print_menu();
        user_choice = get_user_choice();

        // Get the computer's choice
        computer_choice = get_computer_choice();

        // Determine the winner
        determine_winner(user_choice, computer_choice);

        // Ask if the user wants to play again
        printf("Do you want to play again? (y/n): ");
        scanf(" %c", &play_again);
    } while (play_again == 'y' || play_again == 'Y');

    // Print a message to thank the user for playing
    printf("Thanks for playing!\n");
    return 0;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Rock, Paper, Scissors Game
1. Rock
2. Paper
3; Scissors
Enter your choice (1-3):0
Invalid choice. Please enter a number between 1 and 3.
1
You chose Rock.
The computer chose Scissors.
You win!
Do you want to play again? (y/n): y
Rock, Paper, Scissors Game
1. Rock
2. Paper
3; Scissors
Enter your choice (1-3):2
You chose Paper.
The computer chose Scissors.
You lose!
Do you want to play again? (y/n): y
Rock, Paper, Scissors Game
1. Rock
2. Paper
3; Scissors
Enter your choice (1-3):2
You chose Paper.
The computer chose Paper.
It's a tie!
Do you want to play again? (y/n): n
Thanks for playing!
chan@CMA:~/C_Programming/practice$ 


```



### 2D Arrays in C

- An array, where each element is an entire array
- Useful if we need a matrix, grid, or table of data

`main.c`

```C
int main(){
    int numbers[2][3] = {{1,2,3}, {4,5,6}};
    
    // Setting each value in a 2D array
    // Same as above
    numbers[0][0] = 1;
    numbers[0][1] = 2;
    numbers[0][2] = 3;
    numbers[1][0] = 4;
    numbers[1][1] = 5;
    numbers[1][2] = 6;
    
    for(int i = 0; i < 2; i++){
        for(int j = 0; j < 3; j++){
            printf("%d", numbers[i][j]);
        }
        // New line for each row
        printf("\n");
    }
    return 0;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
1 2 3 
4 5 6 
```



#### Making our 2D array demonstration code a bit more flexible

`main.c`

```C
int main()
{
    int numbers[2][3];

    // Calculate the number of rows in the array
    int rows = sizeof(numbers) / sizeof(numbers[0]);
    
  	// Cal;culate the number of columns in the array
    int columns = sizeof(numbers[0]) / sizeof(numbers[0][0]);

    printf("rows: %d\n", rows);
    printf("columns: %d\n", columns);

    numbers[0][0] = 1;
    numbers[0][1] = 2;
    numbers[0][2] = 3;
    numbers[1][0] = 4;
    numbers[1][1] = 5;
    numbers[1][2] = 6;
	
    // Nested for loop to iterate over the 2D array
    for (int i = 0; i < rows; i++)
    {
        for (int j = 0; j < columns; j++)
        {
            // Print each element of the array
            printf("%d ", numbers[i][j]);
        }
        // print a newline for each row
        printf("\n");
    }
    return 0;
}
```

- The way nested for loops work is that according to the above example, if i = 0, we are going to calculate `j` `columns` times (in this case 3 times);
- So it's going to be like this:

```
// i = 0 , j = 0
// i = 0, j = 1
// i = 0, j = 2

// i = 1, j = 0
// i = 1, j = 1
// i = 1, j = 2
```

#### Code Explanation

```C 
int numbers[2][3];
```



- This declares a 2D array named `numbers` with 2 rows and 3 columns.

```C
int rows = sizeof(numbers) / sizeof(numbers[0]);
```

- `sizeof(numbers)` gives the total size in bytes of the entire 2D array.
- `sizeof(numbers[0])` gives the size in bytes of the first row of the array.
- Dividing the total size of the array by the size of one row gives the number of rows.

```C
int columns = sizeof(numbers[0]) / sizeof(numbers[0][0]);
```

- `sizeof(numbers[0])` gives the size in bytes of the first row of the array.
- `sizeof(numbers[0][0])` gives the size in bytes of the first element of the first row.
- Dividing the size of one row by the size of one element gives the number of columns.

### Detailed Breakdown

1. **Calculating Rows:**
   - `sizeof(numbers)` returns the total size of the array. For a 2x3 array of `int` (assuming `int` is 4 bytes), this would be `2 * 3 * 4 = 24` bytes.
   - `sizeof(numbers[0])` returns the size of the first row. For a row with 3 `int` elements, this would be `3 * 4 = 12` bytes.
   - Dividing these values gives `24 / 12 = 2`, which is the number of rows.
2. **Calculating Columns:**
   - `sizeof(numbers[0])` returns the size of the first row, which is `12` bytes.
   - `sizeof(numbers[0][0])` returns the size of a single `int` element, which is `4` bytes.
   - Dividing these values gives `12 / 4 = 3`, which is the number of columns.

### Summary

- The number of rows is calculated by dividing the total size of the array by the size of one row.
- The number of columns is calculated by dividing the size of one row by the size of one element.

This method is useful because it dynamically calculates the dimensions of the array, making the code more flexible and less error-prone.



### Tic-Tac-Toe

`main.c`

```C

int main(int argc, char *argv[])
{
    (void)argc;
    (void)argv;

    char winner = ' '; // Variable to store the winner ('X', 'O', or ' ' for no winner).
    char response;     // Variable to store the player's response to play again.

    do
    {
        winner = ' ';   // Reset the winner for a new game
        response = ' '; // Reset the response for a new game
        // reset the game board
        resetBoard();
        // continue until there's a winner or no free spaces left.
        while (winner == ' ' && checkFreeSpaces() != 0)
        {
            // Display the current state of the game board.
            printBoard();

            // Execute the player's move.
            playerMove();
            // Check if the player has won.
            winner = checkWinner();
            if (winner != ' ' || checkFreeSpaces() == 0)
            {
                break; // Exit the loop if there's a winner or no free spaces.
            }

            // Execute the computer's move.
            computerMove();
            // Check if the computer has won.
            winner = checkWinner();
            if (winner != ' ' || checkFreeSpaces() == 0)
            {
                // Exit the loop if there's a winner or no free spaces.
                break;
            }
        }

        // Display the final state of the game board.
        printBoard();
        // Announce the winner of the game.
        printWinner(winner);

        // Prompt the player to play again.
        printf("\nWould you like to play again? (Y/N): \n");

        // Read the player's response.
        scanf(" %c", &response);
        // Convert the response to uppercase
        response = toupper(response);
    } while (response == 'Y'); // Continue if the player wants to play again.

    return 0;
}
```

`practice.h`

```C
void error(char *msg);

void resetBoard();
void printBoard();
int checkFreeSpaces();

void playerMove();

void computerMove();

char checkWinner();

void printWinner(char winner);

```

`functions.c`

```C
// The game board, a 3x3 grid for Tic-Tac-Toe
char board[3][3];
// Constant representing the player's marker
const char PLAYER = 'X';
// Constant representing the computer's marker.
const char COMPUTER = 'O';

void error(char *msg)
{
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(EXIT_FAILURE);
}

void resetBoard()
{
    // Initialize the game board by setting all positions to empty (' ').
    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 3; j++)
        {
            board[i][j] = ' ';
        }
    };
}
void printBoard()
{
    // Print the current state of the game board in a formatted manner.
    printf(" %c | %c | %c ", board[0][0], board[0][1], board[0][2]);
    printf("\n---|---|---\n");
    printf(" %c | %c | %c ", board[1][0], board[1][1], board[1][2]);
    printf("\n---|---|---\n");
    printf(" %c | %c | %c ", board[2][0], board[2][1], board[2][2]);
    printf("\n---|---|---\n");
    printf("\n");
}
int checkFreeSpaces()
{
    // Count and return the number of empty spaces left on the board.
    int freeSpaces = 9;
    for (int i = 0; i < 3; i++)
    {
        for (int j = 0; j < 3; j++)
        {
            if (board[i][j] != ' ')
            {
                freeSpaces--;
            }
        }
    }
    return freeSpaces;
}

void playerMove()
{
    int x;
    int y;

    // Prompt the player to enter the row and column for their move.
    do
    {
        printf("Enter row #(1-3): ");
        scanf("%d", &x);
        x--;

        printf("Enter column #(1-3): ");
        scanf("%d", &y);
        y--;

        if (board[x][y] != ' ')
        {
            printf("Invalid move\n");
        }
        else
        {
            board[x][y] = PLAYER;
            break;
        }
    } while (board[x][y] != ' ');
}

void computerMove()
{
    // Create a seed based on current time
    srand(time(0));

    int x;
    int y;

    // Check if there are any free spaces left.
    if (checkFreeSpaces() > 0)
    {
        do
        {
            // Generate random coordinates for the computer's move.
            x = rand() % 3;
            y = rand() % 3;
        } while (board[x][y] != ' ');

        board[x][y] = COMPUTER;
    }
    else
    {
        // If no free spaces, declare a tie.
        printWinner(' ');
    }
}

char checkWinner()
{
    // check rows
    for (int i = 0; i < 3; i++)
    {
        if (board[i][0] == board[i][1] && board[i][0] == board[i][2] && board[i][0] != ' ')
        {
            return board[i][0];
        }
    }

    // check columns
    for (int i = 0; i < 3; i++)
    {
        if (board[0][i] == board[1][i] && board[0][i] == board[2][i] && board[0][i] != ' ')
        {
            return board[0][i];
        }
    }

    // check diagonals
    if (board[0][0] == board[1][1] && board[0][0] == board[2][2] && board[0][0] != ' ')
    {
        return board[0][0];
    }
    if (board[0][2] == board[1][1] && board[0][2] == board[2][0] && board[0][2] != ' ')
    {
        return board[0][2];
    }

    return ' ';
}

void printWinner(char winner)
{
    if (winner == PLAYER)
    {
        printf("YOU WIN!\n");
    }
    else if (winner == COMPUTER)
    {
        printf("YOU LOSE!");
    }
    else
    {
        printf("IT'S A TIE!");
    }
}

```

`Code Execution:`

`computer wins`

```sh
   |   |   
---|---|---
   |   |   
---|---|---
   |   |   
---|---|---

Enter row #(1-3): 1
Enter column #(1-3): 3
   |   | X 
---|---|---
   |   |   
---|---|---
 O |   |   
---|---|---

Enter row #(1-3): 2
Enter column #(1-3): 3
   |   | X 
---|---|---
 O |   | X 
---|---|---
 O |   |   
---|---|---

Enter row #(1-3): 2
Enter column #(1-3): 1
Invalid move
Enter row #(1-3): 2
Enter column #(1-3): 2
   |   | X 
---|---|---
 O | X | X 
---|---|---
 O |   | O 
---|---|---

Enter row #(1-3): 1
Enter column #(1-3): 2
   | X | X 
---|---|---
 O | X | X 
---|---|---
 O | O | O 
---|---|---

YOU LOSE!
Would you like to play again? (Y/N): 
N
chan@CMA:~/C_Programming/test$ 

```

`player wins`

```sh
chan@CMA:~/C_Programming/test$ ./final
   |   |   
---|---|---
   |   |   
---|---|---
   |   |   
---|---|---

Enter row #(1-3): 1
Enter column #(1-3): 3
   |   | X 
---|---|---
   |   |   
---|---|---
   |   | O 
---|---|---

Enter row #(1-3): 2
Enter column #(1-3): 1
 O |   | X 
---|---|---
 X |   |   
---|---|---
   |   | O 
---|---|---

Enter row #(1-3): 3
Enter column #(1-3): 1
 O | O | X 
---|---|---
 X |   |   
---|---|---
 X |   | O 
---|---|---

Enter row #(1-3): 2
Enter column #(1-3): 2
 O | O | X 
---|---|---
 X | X |   
---|---|---
 X |   | O 
---|---|---

YOU WIN!

Would you like to play again? (Y/N): 
Y

```

`draw`

```sh
  |   |   
---|---|---
   |   |   
---|---|---
   |   |   
---|---|---

Enter row #(1-3): 2
Enter column #(1-3): 3
   |   |   
---|---|---
 O |   | X 
---|---|---
   |   |   
---|---|---

Enter row #(1-3): 1
Enter column #(1-3): 1
 X |   |   
---|---|---
 O | O | X 
---|---|---
   |   |   
---|---|---

Enter row #(1-3): 1
Enter column #(1-3): 2
 X | X | O 
---|---|---
 O | O | X 
---|---|---
   |   |   
---|---|---

Enter row #(1-3): 3
Enter column #(1-3): 3
 X | X | O 
---|---|---
 O | O | X 
---|---|---
   | O | X 
---|---|---

Enter row #(1-3): 1
Enter column #(1-3): 2
Invalid move
Enter row #(1-3): 3
Enter column #(1-3): 1
 X | X | O 
---|---|---
 O | O | X 
---|---|---
 X | O | X 
---|---|---

IT'S A TIE!
Would you like to play again? (Y/N): 
Y

```

---

### Leap Year Program

A leap year (in the Gregorian calendar) occurs:

- In every year that is evenly divisible by 4.
- Unless the year is evenly divisible by 100, in which case it's only a leap year if the year is also evenly divisible by 400.

Some examples:

- 1997 was not a leap year as it's not divisible by 4.
- 1900 was not a leap year as it's not divisible by 400.
- 2000 was a leap year!



1. **A year is a leap year if it is divisible by 4**:
   - This is the basic rule for determining leap years.
2. **However, if the year is also divisible by 100, it is not a leap year**:
   - This is an exception to the rule above.
3. **But if the year is divisible by 400, it is a leap year**:
   - This is an exception to the exception.

Using the concepts above, we're going to develop a program that determines whether a given year is a leap year of not.

`functions.c`

```C
#include <stdbool.h>
bool leap_year(int year)
{
    if (year % 4 == 0)
    {
        if (year % 100 == 0)
        {
            if (year % 400 == 0)
            {
                return true; // leap year
            }
            else
            {
                return false; // not leap year
            }
        }
        else
        {
            return true; // leap year
        }
    }
    else
    {
        return false; // not leap year
    }
}
```

`practice.h`

```C
#include <stdbool.h>

bool leap_year(int year);
```

`main.c`

```C
int main()
{
    int year;
    printf("Enter a year: ");
    scanf("%d", &year);

    if (leap_year(year))
    {
        printf("%d is a leap year.\n", year);
    }
    else
    {
        printf("%d is not a leap year.\n", year);
    }
    return EXIT_SUCCESS;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter a year: 1997
1997 is not a leap year.
chan@CMA:~/C_Programming/practice$ ./practice
Enter a year: 2000
2000 is a leap year.
chan@CMA:~/C_Programming/practice$ ./practice
Enter a year: 1900
1900 is not a leap year.
chan@CMA:~/C_Programming/practice$ 

```

---

### Sum of Squares or Square of sum?

Find the difference between the square of the sum and the sum of the squares of the first N natural numbers.

The square of the sum of the first ten natural numbers is (1 + 2 + ... + 10)² = 55² = 3025.

The sum of the squares of the first ten natural numbers is 1² + 2² + ... + 10² = 385.

Hence the difference between the square of the sum of the first ten natural numbers and the sum of the squares of the first ten natural numbers is 3025 - 385 = 2640.

#### Traditional approach (for loop)

`functions.c`

```C
unsigned int sum_of_squares(unsigned int number){
    unsigned int result = 0;
    for(unsigned int i = 1; i <= number; i++){
        result += i * i;
    }
    return result;
}
// This algorithm iterates from 1 to number, summing the squares of each integer. The time complexity is O(n).


unsigned int square_of_sum(unsigned int number){
    unsigned int sum = 0;
    for(unsigned int i = 1; i <= number; i++){
        sum += i;
    }
    return sum * sum;
}
// THis algorithm iterates from 1 to number, summing the integers and then squaring the result. The time complexity is O(n).

unsigned int difference_of_squares(unsigned int number){
    return square_of_sum(number) - sum_of_squares(number);
}

// This algorithm simply calls the previous two functions and subtracts their results. The time complexity is O(n) due to the calls to square_of_sum and sum_of_square.
```

`practice.h`

```C
#ifndef PRACTICE_H
#define PRACTICE_H
// Functions.c

unsigned int sum_of_squares(unsigned int number);
unsigned int square_of_sum(unsigned int number);
unsigned int difference_of_squares(unsigned int number);
// Functions_2.c
#endif // PRACTICE_H
```

The `#ifndef`, `#define`, and `#endif` directives are used to prevent multiple inclusions of the same header file, which can cause errors and increase compilation time. This is known as an "include guard."

Here's a step-by-step explanation:

1. **`#ifndef PRACTICE_H`**: This checks if `PRACTICE_H` has not been defined yet. If it hasn't, the code between `#ifndef` and `#endif` will be included.
2. **`#define PRACTICE_H`**: This defines `PRACTICE_H`, so the next time this header file is included, the `#ifndef PRACTICE_H` check will fail, preventing the contents of the header file from being included again.
3. **`#endif`**: This ends the conditional inclusion started by `#ifndef`.

This mechanism ensures that the contents of the header file are included only once, even if the header file is included multiple times in different source files or within the same source file.

Here is the `practice.h`file with the include guard:

```C
\#ifndef PRACTICE_H

\#define PRACTICE_H

void error(char *msg);

unsigned int sum_of_squares(unsigned int number);

unsigned int square_of_sum(unsigned int number);

unsigned int difference_of_squares(unsigned int number);

\#endif // PRACTICE_H
```

Without these include guards, if `practice.h` were included multiple times, the compiler would see multiple definitions of the same functions and variables, leading to errors.

`practice.c`(main file)

```C
int main(){
    int n;
    printf("Enter a term: ");
    scanf("%d", &n);
    printf("Sum of squares: %u\n", sum_of_squares(n));
    printf("Square of sum: %u\n", difference_of_squares(n));
    printf("Diffs of squares: %u\n", difference_of_squares(n));
    return 0;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ make all
clang -std=c18 -Wall -Wextra -g  -c practice.c -o ./obj/practice.o 
clang -std=c18 -Wall -Wextra -g  -c functions.c -o ./obj/functions.o 
clang -std=c18 -Wall -Wextra -g  -c functions_2.c -o ./obj/functions_2.o
ar -rcs ./libs/libfunctions.a ./obj/functions.o ./obj/functions_2.o
clang -std=c18 ./obj/practice.o -L./libs -lfunctions -o practice -lpthread -lm -lssl -lcrypto
chan@CMA:~/C_Programming/practice$ ./practice
Enter a term: 10
Sum of squares: 385
Square of sum: 3025
Diffs of squares: 2640

```



#### Best approach (O(1))

For large values of `n`, these algorithms can be optimized using mathematical formulas:

`functions.c`

```C
// S = n(n+1)(2n+1) / 6
unsigned int sum_of_squares(unsigned int number){
    return (number * (number + 1) * (2 * number + 1)) / 6;
}

// S = n(n+1) / 2
unsigned int square_of_sum(unsigned int number){
    unsigned int sum = (number * (number + 1)) / 2;
    return sum * sum;
}

unsigned int difference_of_squares(unsigned int number){
    return square_of_sum(number) - sum_of_squares(number);
}
```

##### 1. Sum of Squares

```C
unsigned int sum_of_squares(unsigned int number)
{
    return (number * (number + 1) * (2 * number + 1)) / 6;
}
```

**Formula**: S = n(n+1)(2n+1) / 6

**Explanation**:

- This formula comes from the sum of squares of the first `n` natural numbers.
- It can be derived using methods from calculus or algebraic manipulation.
- For example, for n=3:
  - S<sub>1</sub>= 1<sup>2</sup>+2<sup>2</sup>+3<sup>2</sup> = 1 + 4 + 9 = 14
  - Using the formula:
  - S<sub>1</sub> = 3 * 4 * 7 / 6 = 84 / 6 = 14

##### 2. Square of the Sum

```C
unsigned int square_of_sum(unsigned int number)
{
    unsigned int sum = (number * (number + 1)) / 2;
    return sum * sum;
}
```

**Formula**: S = n(n+1) / 2

**Explanation**:

- This formula calculates the sum of the first `n` natural numbers and then squares the result.
- For example, for n=3:
  - S<sub>2</sub> = 1 + 2 + 3 = 6
  - (S<sub>2</sub>)<sup>2</sup> = 6<sup>2</sup> = 36
  - Using the formula:
  - (3 * 4 / 2)<sup>2</sup> = (6)<sup>2</sup> = 36

These formulas are efficient for large values of `n` as they avoid the need for iteration, reducing the time complexity from O(n) to O(1).



`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ make all
clang -std=c18 -Wall -Wextra -g  -c functions.c -o ./obj/functions.o 
ar -rcs ./libs/libfunctions.a ./obj/functions.o ./obj/functions_2.o
clang -std=c18 ./obj/practice.o -L./libs -lfunctions -o practice -lpthread -lm -lssl -lcrypto
chan@CMA:~/C_Programming/practice$ ./practice
Enter a term: 10
Sum of squares: 385
Square of sum: 3025
Diffs of squares: 2640
chan@CMA:~/C_Programming/practice$ 

```

---

### Factorial 

The factorial of a non-negative integer ( n ) is the product of all positive integers less than or equal to ( n ). 

By definition, the factorial of 0 is 1.

For example, if the number is 4, then the factorial of 4 will be calculated in a way:

4 = 4 * 3 * 2 * 1

#### Iterative approach

`functions.c`

```C
int factorial_iterative(int n){
    int result = 1;
    for(int i = 1; i <= n; i++){
        result *= i;
    }
    return result;
}
```

`practice.c`(main file)

```C
int main()
{
    int terms;
    printf("Enter the number of terms: ");
    scanf("%d", &terms);
    printf("Factorial of %d (iterative) is %d\n", terms, factorial_iterative(terms));
    return 0;
}
```

`Code Execution`

```
chan@CMA:~/C_Programming/practice$ ./practice
Enter the number of terms: 4
Factorial of 4 (iterative) is 24

```



#### Recursive approach

`functions.c`

```C
int factorial_recursive(int n){
    if(n == 0 || n == 1){
        return 1;
    }else{
        return n * factorial_recursive(n - 1);
    }
}
```

`practice.c`(main file)

```C
int main(){
    int terms;
    printf("Enter the number of terms: ");
    scanf("%d", &terms);
    printf("Factorial of %d (recursive) is %d\n", terms, factorial_recursive(terms));
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ make all
clang -std=c18 -Wall -Wextra -g  -c practice.c -o ./obj/practice.o 
clang -std=c18 ./obj/practice.o -L./libs -lfunctions -o practice -lpthread -lm -lssl -lcrypto

chan@CMA:~/C_Programming/practice$ ./practice
Enter the number of terms: 4
Factorial of 4 (recursive) is 24

```

---

### Number of grains of wheat on a chessboard

#### Instructions

Calculate the number of grains of wheat on a chessboard given that the number on each square doubles.

There once was a wise servant who saved the life of a prince. The king promised to pay whatever the servant could dream up. Knowing that the king loved chess, the servant told the king he would like to have grains of wheat. One grain on the first square of a chess board, with **the number of grains doubling on each successive square**.

There are 64 squares on a chessboard (where square 1 has one grain, square 2 has two grains, and so on).

Write code that shows:

- how many grains were on a given square, and
- the total number of grains on the chessboard



#### Solutions

Let's consider the number of grains on each square according to the question:

Square 1: 1

Square 2: 2

Square 3: 4

Square 4: 8

Square 5: 16

...

Square 64: 9223372036854775807

#### Maths Formula Approach

`functions.c`

```C
uint64_t grains_on_square(int square)
{
    return (uint64_t)pow(2, square - 1);
}

uint64_t total_grains_on_chessboard()
{
    uint64_t total_grains = 0;
    for (int i = 1; i <= 64; i++)
    {
        total_grains += grains_on_square(i);
    }
    return total_grains;
}
```

- `uint64_t` is a data type defined in the C standard library header `<stdint.h>`.
- It represents an unsigned 64-bit integer.
- This type is guaranteed to be 64 bits wide on all platforms, providing a consistent way to handle large integer values across different systems.
- **Unsigned**: It can only represent non-negative numbers (0 and positive integers)
- **64-bit**: It can represent values from 0 to 2<sup>64</sup>-1, which is 0 to 18,446,744,073,709,551,615.

`practice.c` (main file)

```C
int main()
{
    int square = 10; // Example square number
    printf("Grains on square %d: %lu\n", square, grains_on_square(square));
    printf("Total grains on chessboard: %lu\n", total_grains_on_chessboard());
    return 0;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Grains on square 10: 512
Total grains on chessboard: 18446744073709551615

```



#### Bitwise Operation Approach

`functions.c`

```C
uint64_t grains_on_square(int square)
{
    return (uint64_t)1 << (square - 1);
}

uint64_t total_grains_on_chessboard()
{
    // 3 different approach same results
    // ((uint64_t)1 << 64) - 1 would overflow, hence 2^64 - 1 is achieved by -1
    return ((uint64_t)1 << 63) - 1 + ((uint64_t)1 << 63);
    // Or 
    return ~(uint64_t)0;
    // Or
    uint64_t total_grains = 0;
    for (int i = 1; i <= 64; i++) {
        total_grains += grains_on_square(i);
    }
    return total_grains;
}

```

- Uses `bitwise left shift` to calculate the number of grains on a specific square.
- It is efficient because left-shifting a 1 by `(square - 1)` bits directly computes 2<sup>(square - 1)</sup>.
- `total_grains_on_chessboard()`: Uses the property of bit-wise operations to calculate 2<sup>64</sup> - 1 directly, which is correct for calculating the sum of grains on all 64 squares.
- `((uint64_t)1 << 63) - 1` : In C, the behavior of shifting a value by an amount equal to or greater than the width of the type is undefined. For a 64-bit integer, shifting by 64 or more bits is not allowed and will result in undefined behavior. That's why we cannot use `((uint64_t)1 << 64)`.
  - `((uint64_t)1 << 63)` shifts the number 1 left by 63 bits, resulting in 2<sup>63</sup>.
  - Shifting by 64 bits or more is undefined because it exceeds the bit width of the type.

`practice.c` (main file)

```C
int main()
{
    int square = 10; // Example square number
    printf("Grains on square %d: %lu\n", square, grains_on_square(square));
    printf("Total grains on chessboard: %lu\n", total_grains_on_chessboard());
    return 0;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Grains on square 10: 512
Total grains on chessboard: 18446744073709551615


```

---



### Compute Pay Program

#### Instruction

Write a program for your pay computation in which regular hours, up to 40 hours, are paid at the regular hourly rate. Any hours worked beyond 40 are paid at a rate of 1.5 times the regular hourly rate. 

Example Output

```sh
EnterHours: 45
EnterRate: 10
Result
Pay: 475.0
```



`functions.c`

```C
float compute_pay(float hours, float rate)
{
    float regular_pay = hours * rate;
    float overtime_hours = hours - 40;
    float overtime_pay = (40 * rate) + (1.5 * rate * (overtime_hours));

    if (hours > 40)
    {
        return overtime_pay;
    }
    else
    {
        return regular_pay;
    }
}
```

`practice.c` (main)

```C
int main()
{
    float hours;
    float rate;
    float pay;
    printf("Enter Hours: ");
    scanf("%f", &hours);

    printf("Enter Rate: ");
    scanf("%f", &rate);

    pay = compute_pay(hours, rate);

    printf("Pay: %.1f\n", pay);
    return 0;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter Hours: 45
Enter Rate: 10
Pay: 475.0

```

---

### Filter Even numbers from a list

#### Instructions

Write a function that takes a list of numbers and returns a new list containing only the even numbers from the original list.

#### Solution

- **`filter_even_numbers` function**:
  - Takes an input array `arr`, its size `size`, and a pointer to `result_size` to store the size of the result array.
  - Allocates memory for the result array using `malloc`.
  - Iterates through the input array and adds even numbers to the result array.
  - Updates the `result_size` with the number of even numbers found.
  - Optionally reallocates memory to fit the exact number of even numbers using `realloc`.
  - Returns the result array.
- **`main` function**:
  - Defines the original array and its size.
  - Calls `filter_even_numbers` to get the array of even numbers and its size.
  - Prints the result array.
  - Frees the allocated memory for the result array.

`functions.c`

```C

int *filter_even_numbers(int *arr, int size, int *result_size)
{
    // Allocate memory for the result array (maximum possible size is the same as input array)
    int *result = (int *)malloc(size * sizeof(int));

    if (result == NULL)
    {
        // Handle memory allocation failure
        *result_size = 0;
        return NULL;
    }

    // Initialize index for the result array
    int result_index = 0;
    for (int i = 0; i < size; i++)
    {
        if (arr[i] % 2 == 0)
        {
            result[result_index++] = arr[i];
        }
    }

    // Update the result size with the number of even numbers found
    *result_size = result_index;

    // Reallocate memory to fit the exact number of even numbers
    result = (int *)realloc(result, result_index * sizeof(int));
    if (result == NULL & result_index > 0)
    {
        // Handle memory reallocation failure
        *result_size = 0;
        return NULL;
    }
    return result;
}
```

- **`malloc(size * sizeof(int))`**: Allocates a block of memory large enough to hold `size`integers. The `sizeof(int)` ensures that the correct amount of memory is allocated based on the size of the `int` type on the system.

- **`(int *)`**: Casts the `void *` returned by `malloc` to an `int *`, which is a pointer to an integer. This cast is necessary because `malloc` returns a generic pointer (`void *`), and you need to specify that this memory will be used to store integers.

- The `realloc` function in C is  used to resize a previously allocated block of memory. It takes two parameters:

  1. **Pointer to the previously allocated memory block**: This is the memory block that we want to resize.
  2. **New size in bytes**: This is the new size for the memory block.

- ```C
  void *realloc(void *ptr, size_t size);
  ```

  - `ptr`: A pointer to the memory block previously allocated with `malloc`, `calloc`, or `realloc`. If `ptr` is NULL, `realloc` behaves like `malloc`.
  - `size`: The new size for the memory block in bytes. If `size` is zero and `ptr` is not `NULL`, the memory block is freed.

`practice.c`(Main)

```C
int main()
{
    int original_arr[] = {1, 3, 5, 2, 4, 8, 10};
    int size = sizeof(original_arr) / sizeof(original_arr[0]);

    // variable to store the size of the result array
    int result_size;
    
    // call the function to filter even numbers from the array
    int *result = filter_even_numbers(original_arr, size, &result_size);

    // Check if the result array is not NULL (memory allocation was successful)
    if (result != NULL)
    {
        // Print the result array
        for (int i = 0; i < result_size; i++)
        {
            if(i == result_size - 1){
                // If it is the last element
                // put a new line character
                printf("%d\n", result[i]);
            }else{
                // if not, put a comma
                printf("%d, ", result[i]);
            }
        }
        printf("\n");

        // Free the allocated memory
        free(result);
    }
    else
    {
        printf("Memory allocation failed.\n");
    }
    return 0;
}
```

Let's break down the connection between `result_size` and how it's used when passing it as an argument to the `filter_even_numbers` function in `main.c`.

#### `filter_even_numbers` Function

In the function `filter_even_numbers`, we have the following signature:

```C
int *filter_even_numbers(int *arr, int size, int *result_size);
```

- `arr`: Pointer to the input array.
- `size`: Size of the input array.
- `result_size`: Pointer to an integer where the size of the result array (number of even numbers) will be stored.

Within this function, `result_size` is used to store the number of even numbers found in the input array. This variable is modified inside the function to reflect the count of even numbers. Since it's a pointer, the changes made inside the function will be reflected in the variable passed from the caller (i.e., in `main.c`).

#### `main.c` Usage

In `main.c`, `result_size` is declared as an `int` variable:

```C
int result_size;
```

This variable is then passed to the `filter_even_numbers` function by its address:

```C
int *result = filter_even_numbers(original_arr, size, &result_size);
```

Here, `&result_size` passes the address of `result_size` to the function, allowing `filter_even_numbers` to modify its value.

#### Detailed Steps and Connection

1. **Variable Declaration in `main.c`**:

   ```C
   int result_size;
   ```

   - `result_size` is declared to store the size of the array of even numbers.

2. **Passing `result_size` to `filter_even_numbers`**:

   ```C
   int *result = filter_even_numbers(original_arr, size, &result_size);
   ```

   - The address of `result_size` is passed to `filter_even_numbers`.

3. **Inside `filter_even_numbers`**:

   ```C
   int *filter_even_numbers(int *arr, int size, int *result_size)
   {
       // Various operations...
       
       // Update the result size with the number of even numbers found
       *result_size = result_index;
   
       // Reallocate memory to fit the exact number of even numbers
       result = (int *)realloc(result, result_index * sizeof(int));
   
       // Various operations...
   }
   ```

   - The function updates `*result_size` to the number of even numbers found (`result_index`).
   - This change is made directly to the memory location of `result_size` in `main.c` because `result_size` is passed by reference.

4. **Back in `main.c`**:

   ```C
   if (result != NULL)
   {
       // Print the result array
       for (int i = 0; i < result_size; i++)
       {
           printf("%d ", result[i]);
       }
       printf("\n");
   
       // Free the allocated memory
       free(result);
   }
   ```

   - After the function call, `result_size` in `main.c` contains the number of even numbers found.
   - This value is used in the loop to print the elements of the result array.

#### Summary

- `result_size` is declared in `main.c` to store the size of the filtered array.
- The address of `result_size` is passed to `filter_even_numbers`.
- `filter_even_numbers` updates the value at the address of `result_size` with the count of even numbers.
- This updated value is then used in `main.c` to iterate over and print the result array.

By passing the address of `result_size`, changes made within `filter_even_numbers` are reflected in the original variable in `main.c`, enabling the main function to know how many even numbers were found and stored in the result array.

`Code Execution`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
2, 4, 8, 10, chan@CMA:~/C_Programming/practice$ make all
clang -std=c18 -Wall -Wextra -g  -c practice.c -o ./obj/practice.o 
clang -std=c18 ./obj/practice.o -L./libs -lfunctions -o practice -lpthread -lm -lssl -lcrypto
chan@CMA:~/C_Programming/practice$ ./practice
2, 4, 8, 10

```

### Color Code Program

`functions.c`

```C
const char *color_names[] = {
    "BLACK", "BROWN", "RED", "ORANGE", "YELLOW",
    "GREEN", "BLUE", "VIOLET", "GREY", "WHITE"};

// Function to get the numerical value of a color
int color_code(const char *color)
{
    for (int i = 0; i < 10; i++)
    {
        if (strcasecmp(color_names[i], color) == 0)
        {
            return i;
        }
    }
    return -1; // Return -1 if color is not found
}
```

`practice.c` (main)

```C
int main()
{
    char input[10];
    printf("Enter the color: ");
    scanf("%9s", input); // %9s to avoid buffer overflow
    int code = color_code(input);
    if (code != -1)
    {
        printf("The color code for %s is %d.\n", input, code);
    }
    else
    {
        printf("Color not found.\n");
    }
    return 0;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/test$ ./final
Enter the color: white
The color code for white is 9.
chan@CMA:~/C_Programming/test$ ./final
Enter the color: black
The color code for black is 0.
chan@CMA:~/C_Programming/test$ ./final
Enter the color: grey
The color code for grey is 8.
```



### Collatz Conjecture

#### Instructions

The Collatz Conjecture or 3x+1 problem can be summarized as follows:

Take any positive integer n. If n is even, divide n by 2 to get n / 2. If n is odd, multiply n by 3 and add 1 to get 3n + 1. Repeat the process indefinitely. The conjecture states that no matter which number you start with, you will always reach 1 eventually.

Given a number n, return the number of steps required to reach 1.

#### Examples

Starting with n = 12, the steps would be as follows:

1. 12
2. 6
3. 3
4. 10
5. 5
6. 16
7. 8
8. 4
9. 2
10. 1

Resulting in 9 steps. So for input n = 12, the return value would be 9.



`functions.c`

```C
void error(char *msg){
    fprintf(stderr, "%s", msg);
    exit(EXIT_FAILURE);
}

int steps(int start){
    if(start <= 0){
        error("value cannot be less than or equal to 0.\n");
    }
    int count = 0;
    while(start != 1){
        // Print the current value
        printf("%d\n", start);
        if(start % 2 == 0){
            start /= 2;
            count++;
        }else{
            start = 3 * start + 1;
            count++;
        }
    }
    // print the final value
    printf("%d\n", start);
    return count;
}
```

`practice.c` (main)

```C
int main(){
    int n;
    printf("Enter a number: ");
    scanf("%d", &n);
    printf("According to Collatz Conjecture, \n");
    printf("The number of steps for %d to reach 1 is %d\n", n, steps(n));
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter a number: -1
According to Collatz Conjecture, 
value cannot be less than or equal to 0.
chan@CMA:~/C_Programming/practice$ ./practice
Enter a number: 12
According to Collatz Conjecture, 
12
6
3
10
5
16
8
4
2
1
The number of steps for 12 to reach 1 is 9
chan@CMA:~/C_Programming/practice$ 

```

---

### Queen Attack Program

#### Instructions
Given the position of two queens on a chess board, indicate whether or not they are positioned so that they can attack each other.

In the game of chess, a queen can attack pieces which are on the same row, column, or diagonal.

A chessboard can be represented by an 8 by 8 array.

So if you are told the white queen is at c5 (zero-indexed at column 2, row 3) and the black queen at f2 (zero-indexed at column 5, row 6), then you know that the set-up is like so:

 ```
   a b c d e f g h
 8 _ _ _ _ _ _ _ _ 8
 7 _ _ _ _ _ _ _ _ 7
 6 _ _ _ _ _ _ _ _ 6
 5 _ _ W _ _ _ _ _ 5
 4 _ _ _ _ _ _ _ _ 4
 3 _ _ _ _ _ _ _ _ 3
 2 _ _ _ _ _ B _ _ 2
 1 _ _ _ _ _ _ _ _ 1
   a b c d e f g h
 ```

You are also able to answer whether the queens can attack each other. In this case, that answer would be yes, they can, because both pieces share a diagonal.

#### Solution

To determine if two queens can attack each other on a chessboard, we need to check if they share the same row, column, or diagonal.

##### Step-by-Step Breakdown:

1. **Same Row:** The queens are on the same row if their row indices are equal.
2. **Same Column:** The queens are on the same column if their column indices are equal.
3. **Same Diagonal:** The queens are on the same diagonal if the absolute difference between their row indices equals the absolute difference between their column indices.

##### Detailed Breakdown on Diagonals

- In chess, a queen can attack another piece if they are on the same diagonal. 
- To determine if two queens are on the same diagonal, we need to check if the absolute difference between their row indices is equal to the absolute difference between their column indices.

###### Explanation:
**Diagonal Movement:** A queen moves diagonally by changing both its row and column by the same amount. For example, moving from (row 3, column 2) to (row 6, column 5) involves changing the row by 3 and the column by 3.
**Absolute Difference:** The absolute difference ensures that we are considering the magnitude of the change, regardless of direction (positive or negative).

###### Mathematical Justification:

If two positions (r1, c1) and (r2, c2) are on the same diagonal, then the difference in rows `|r1 - r2|` must be equal to the difference in columns `|c1 - c2|`.
###### Example:

Consider two queens at positions `(3, 2)`and `(6, 5)`:

- Row difference: `|3 - 6| = 3`
- Column difference: `|2 - 5| = 3`
- Since both differences are equal, the queens are on the same diagonal.

##### Code Implementation

`practice.h`

```C
typedef enum{
    CAN_NOT_ATTACK,
    CAN_ATTACK,
    INVALID_POSITION
} attack_status_t;

typedef struct{
    uint8_t row;
    uint8_t column;
}position_t;

attack_status_t can_attack(position_t queen_1, position_t queen_2);
```



`functions.c`

```C
attack_status_t can_attack(position_t queen_1, position_t queen_2){
    // Invalid position check: If any row or column is out of bounds (>= 8)
    if (queen_1.row >= 8 || queen_1.column >= 8 || queen_2.row >= 8 || queen_2.column >= 8)
    {
        return INVALID_POSITION;
    }

    // Same position check
    if (queen_1.row == queen_2.row && queen_1.column == queen_2.column)
    {
        return INVALID_POSITION;
    }

    // Check if they can attack each other:
    // 1. Same row
    if (queen_1.row == queen_2.row)
    {
        return CAN_ATTACK;
    }

    // 2. Same column
    if (queen_1.column == queen_2.column)
    {
        return CAN_ATTACK;
    }

    // 3. Same diagonal
    if (abs(queen_1.row - queen_2.row) == abs(queen_1.column - queen_2.column))
    {
        return CAN_ATTACK;
    }

    // If none of the conditions are met, they cannot attack each other
    return CAN_NOT_ATTACK;
}
```

`practice.c (Main file)`

```C
int main()
{
    position_t white_queen = {3, 2}; // c5
    position_t black_queen = {6, 5}; // f2

    attack_status_t result = can_attack(white_queen, black_queen);

    if (result == CAN_ATTACK)
    {
        printf("The queens can attack each other.\n");
    }
    else if (result == CAN_NOT_ATTACK)
    {
        printf("The queens cannot attack each other.\n");
    }
    else
    {
        printf("Invalid position.\n");
    }
    return 0;
}
```

`Output`

```C
chan@CMA:~/C_Programming/practice$ ./practice
The queens can attack each other.
```

###### Better solution (Includes user interaction)

`functions.c`

```C
void print_board(position_t queen_1, position_t queen_2)
{
    printf("  a b c d e f g h\n");
    for (int row = 7; row >= 0; row--)
    {
        printf("%d ", row + 1);
        for (int col = 0; col < 8; col++)
        {
            if (queen_1.row == row && queen_1.column == col)
            {
                printf("W "); // White Queen
            }
            else if (queen_2.row == row && queen_2.column == col)
            {
                printf("B "); // Black Queen
            }
            else
            {
                printf("_ ");
            }
        }
        printf("%d\n", row + 1);
    }
    printf("  a b c d e f g h\n");
}

attack_status_t can_attack(position_t queen_1, position_t queen_2)
{
    // Invalid position check: If any row or column is out of bounds (>= 8)
    if (queen_1.row >= 8 || queen_1.column >= 8 || queen_2.row >= 8 || queen_2.column >= 8)
    {
        return INVALID_POSITION;
    }

    // Same position check
    if (queen_1.row == queen_2.row && queen_1.column == queen_2.column)
    {
        return INVALID_POSITION;
    }

    // Check if they can attack each other:
    // 1. Same row
    if (queen_1.row == queen_2.row)
    {
        return CAN_ATTACK;
    }

    // 2. Same column
    if (queen_1.column == queen_2.column)
    {
        return CAN_ATTACK;
    }

    // 3. Same diagonal
    if (abs(queen_1.row - queen_2.row) == abs(queen_1.column - queen_2.column))
    {
        return CAN_ATTACK;
    }

    // If none of the conditions are met, they cannot attack each other
    return CAN_NOT_ATTACK;
}
```



`practice.c`(Main)

```C
int main()
{
    position_t white_queen, black_queen;
    char column;
    int row;

    // Get the position of the White Queen
    printf("Enter the position of the White Queen (e.g., c5): ");
    scanf(" %c%d", &column, &row);
    white_queen.column = column - 'a';
    white_queen.row = row - 1;

    // Get the position of the Black Queen
    printf("Enter the position of the Black Queen (e.g., f2): ");
    scanf(" %c%d", &column, &row);
    black_queen.column = column - 'a';
    black_queen.row = row - 1;

    // Display the board with the queens
    print_board(white_queen, black_queen);

    // Determine if the queens can attack each other
    attack_status_t result = can_attack(white_queen, black_queen);

    if (result == CAN_ATTACK)
    {
        printf("The queens can attack each other.\n");
    }
    else if (result == CAN_NOT_ATTACK)
    {
        printf("The queens cannot attack each other.\n");
    }
    else
    {
        printf("Invalid position.\n");
    }

    return 0;
}

```

**Explanation:**

**Conversion Logic**

- Why `column - 'a'` and `row - 1`]?

- **`column - 'a'`**: This converts the column character (e.g., 'a', 'b', 'c', etc.) to a zero-based index. For example, 'a' becomes 0, 'b' becomes 1, and so on. This is necessary because array indices in C are zero-based.
- **`row - 1`**: This converts the row number (which is 1-based as entered by the user) to a zero-based index. For example, row 1 becomes 0, row 2 becomes 1, and so on. This is also necessary for zero-based array indexing.

- The column is converted using `column - 'a'`, which should correctly map 'a' to 0, 'b' to 1, ..., 'h' to 7.
- The row is converted using `row - 1`, which should correctly map 1 to 0, 2 to 1, ..., 8 to 7.

1. **Variable Declarations**: Declares variables for the positions of the white and black queens (`white_queen`, `black_queen`), and temporary variables for reading input (`column`, `row`).
2. **Get White Queen Position**:
   - Prompts the user to enter the position of the white queen.
   - Reads the input using `scanf` and converts the column from a character to an integer index (`column - 'a'`) and the row to a zero-based index (`row - 1`).
3. **Get Black Queen Position**:
   - Prompts the user to enter the position of the black queen.
   - Reads the input and converts it similarly to the white queen's position.
4. **Print the Board**: Calls `print_board` to display the current positions of the queens on the board.
5. **Check Attack Possibility**:
   - Calls `can_attack` to determine if the queens can attack each other.
   - Prints the result based on the return value of `can_attack`:
     - `CAN_ATTACK`: Prints that the queens can attack each other.
     - `CAN_NOT_ATTACK`: Prints that the queens cannot attack each other.
     - `INVALID_POSITION`: Prints that the position is invalid.
6. **Return**: Returns 0 to indicate successful execution.

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter the position of the White Queen (e.g., c5): c5
Enter the position of the Black Queen (e.g., f2): f2
  a b c d e f g h
8 _ _ _ _ _ _ _ _ 8
7 _ _ _ _ _ _ _ _ 7
6 _ _ _ _ _ _ _ _ 6
5 _ _ W _ _ _ _ _ 5
4 _ _ _ _ _ _ _ _ 4
3 _ _ _ _ _ _ _ _ 3
2 _ _ _ _ _ B _ _ 2
1 _ _ _ _ _ _ _ _ 1
  a b c d e f g h
The queens can attack each other.

chan@CMA:~/C_Programming/practice$ ./practice
Enter the position of the White Queen (e.g., c5): c3
Enter the position of the Black Queen (e.g., f2): f5
  a b c d e f g h
8 _ _ _ _ _ _ _ _ 8
7 _ _ _ _ _ _ _ _ 7
6 _ _ _ _ _ _ _ _ 6
5 _ _ _ _ _ B _ _ 5
4 _ _ _ _ _ _ _ _ 4
3 _ _ W _ _ _ _ _ 3
2 _ _ _ _ _ _ _ _ 2
1 _ _ _ _ _ _ _ _ 1
  a b c d e f g h
The queens cannot attack each other.

chan@CMA:~/C_Programming/practice$ ./practice
Enter the position of the White Queen (e.g., c5): c14
Enter the position of the Black Queen (e.g., f2): f45
  a b c d e f g h
8 _ _ _ _ _ _ _ _ 8
7 _ _ _ _ _ _ _ _ 7
6 _ _ _ _ _ _ _ _ 6
5 _ _ _ _ _ _ _ _ 5
4 _ _ _ _ _ _ _ _ 4
3 _ _ _ _ _ _ _ _ 3
2 _ _ _ _ _ _ _ _ 2
1 _ _ _ _ _ _ _ _ 1
  a b c d e f g h
Invalid position.
chan@CMA:~/C_Programming/practice$ 

```

---

### Darts

#### Instructions
Write a function that returns the earned points in a single toss of a Darts game.

Darts is a game where players throw darts at a target.

In our particular instance of the game, the target rewards 4 different amounts of points, depending on where the dart lands:

- If the dart lands outside the target, player earns no points (0 points).

- If the dart lands in the outer circle of the target, player earns 1 point.

- If the dart lands in the middle circle of the target, player earns 5 points.

- If the dart lands in the inner circle of the target, player earns 10 points.


The outer circle has a radius of 10 units (this is equivalent to the total radius for the entire target), the middle circle a radius of 5 units, and the inner circle a radius of 1. Of course, they are all centered at the same point — that is, the circles are concentric defined by the coordinates (0, 0).

Write a function that given a point in the target (defined by its Cartesian coordinates x and y, where x and y are real), returns the correct amount earned by a dart landing at that point.



#### Solution

`practice.h`

```C
int dart_score(double x, double y);
```

`functions.c`

```C
int dart_score(double x, double y)
{
    // Calculate the distance from the center of the dartboard (0, 0)
    double distance = sqrt(pow(x, 2) + pow(y, 2));

    // Determine the score based on the distance
    if (distance > 10.0)
    {
        return 0; // Outside the target
    }
    else if (distance > 5.0)
    {
        return 1; // Outer circle
    }
    else if (distance > 1.0)
    {
        return 5; // Middle circle
    }
    else
    {
        return 10; // Inner circle
    }
}
```

`practice.c` (main)

```C
int main()
{
    double x;
    double y;
    printf("Enter the x coordinate: ");
    scanf("%lf", &x);
    printf("Enter the y coordinate: ");
    scanf("%lf", &y);

    double score = dart_score(x, y);
    printf("Your score is %.1lf\n", score);
    return 0;
}

```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter the x coordinate: 2
Enter the y coordinate: 2
Your score is 5.0
chan@CMA:~/C_Programming/practice$ ./practice
Enter the x coordinate: 5
Enter the y coordinate: 6
Your score is 1.0
chan@CMA:~/C_Programming/practice$ ./practice
Enter the x coordinate: 0
Enter the y coordinate: 1
Your score is 10.0
```

**Explanation**

- The function `dart_score` takes two `double` parameters, `x` and `y`, which represent the coordinates of where the dart lands on the board.
- It first calculates the distance from the origin `(0, 0)` using the Pythagorean theorem: `distance = sqrt(x * x + y * y)`.
- The function then checks the distance to determine which circle the dart landed in and returns the corresponding score:
  - If the distance is greater than 10, the dart is outside the target, so the function returns `0`.
  - If the distance is greater than 5 but less than or equal to 10, the dart is in the outer circle, and the function returns `1`.
  - If the distance is greater than 1 but less than or equal to 5, the dart is in the middle circle, and the function returns `5`.
  - If the distance is less than or equal to 1, the dart is in the inner circle, and the function returns `10`.

**Illustration**

![img](https://lh7-rt.googleusercontent.com/docsz/AD_4nXdDlzZTuApTOYuD13MaYw1iTH1Ce35buFnBJJ8o5oIga8cXaiT-vuVRSRd2ZSrepki-k-uRFXLOv1kGqiPmFkYa5iNCtfLLTZF9cdoFQwepZWk1tzW2QXmDA8Fix1zW35itjVxxj66q_3MirvNtUsDEEcw?key=X0QCPkupdQGzHXrrQEfXhw)

#### 2nd Solution (Better Approach)

`practice.h`

```C
typedef struct
{
    float x;
    float y;
} coordinate_t;

uint8_t dart_score(coordinate_t position);
```

`functions.c`

```C
uint8_t dart_score(coordinate_t position)
{
    // Calculate the distance from the target (0, 0)
    float distance = hypot(position.x, position.y);

    if (distance <= 1.0F)
    {
        return 10; // Inner circle
    }
    else if (distance <= 5.0F)
    {
        return 5; // Middle circle
    }
    else if (distance <= 10.0F)
    {
        return 1; // Outer circle
    }
    else
    {
        return 0; // Outside the target
    }
}
```

`practice.c`

```C
int main()
{
    float x;
    float y;
    printf("Enter the x coordinate: ");
    scanf("%f", &x);
    printf("Enter the y coordinate: ");
    scanf("%f", &y);

    coordinate_t position = {x, y};
    uint8_t score = dart_score(position);
    printf("Your score is %u\n", score);
    return 0;
}

```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter the x coordinate: 0
Enter the y coordinate: 1
Your score is 10
chan@CMA:~/C_Programming/practice$ ./practice
Enter the x coordinate: 3
Enter the y coordinate: 4
Your score is 5
chan@CMA:~/C_Programming/practice$ ./practice
Enter the x coordinate: 6
Enter the y coordinate: 7
Your score is 1
```

In C, the `F` suffix is used to indicate that a literal is of type `float`. Without the `F` suffix, the literal is treated as a `double` by default. Using `float` literals can be important for performance and precision reasons, especially when working with functions or variables that are explicitly of type `float`.

```C
float distance = 1.0F; // 'distance' is a float
double distance = 1.0; // 'distance' is a double
```

In our code, if we are calculating the distance using `float` variables, we should use `float` literals to avoid unnecessary type conversions and to maintain precision.



#### Additional Things to Note

- Using `uint8_t` for the return type of the `dart_score`function is appropriate if the score values are guaranteed to be small and non-negative, as it saves memory and clearly indicates the range of possible values. 

- However, if you anticipate that the score might need to handle larger values or if there's a possibility of negative values in the future, using `int` might be more flexible.

- In this specific case, since the scores are small and non-negative (0, 1, 5, 10), `uint8_t` is a good choice. 

- It makes the code more efficient in terms of memory usage and clearly communicates that the values are within the range of 0 to 255.

---

### Resistor Color Duo

#### Instructions
If you want to build something using a Raspberry Pi, you'll probably use resistors. For this exercise, you need to know two things about them:

Each resistor has a resistance value.
Resistors are small - so small in fact that if you printed the resistance value on them, it would be hard to read.
To get around this problem, manufacturers print color-coded bands onto the resistors to denote their resistance values. Each band has a position and a numeric value.

The first 2 bands of a resistor have a simple encoding scheme: each color maps to a single number. For example, if they printed a brown band (value 1) followed by a green band (value 5), it would translate to the number 15.

In this exercise you are going to create a helpful program so that you don't have to remember the values of the bands. The program will take color names as input and output a two digit number, even if the input is more than two colors!

The band colors are encoded as follows:

black: 0
brown: 1
red: 2
orange: 3
yellow: 4
green: 5
blue: 6
violet: 7
grey: 8
white: 9
From the example above: brown-green should return 15, and brown-green-violet should return 15 too, ignoring the third color.



#### First Solution

`practice.h`

```C
extern const char *color_codes[];

int get_color_value(char *color);
```

`functions.c`

```C
const char *color_codes[] = {"black", "brown", "red", "orange", "yellow", "green", "blue", "violet", "grey", "white"};

int get_color_value(char *color)
{
    // Iterate through the color_codes array to find the color
    for (int i = 0; i < 10; i++)
    {
        if (strcasecmp(color, color_codes[i]) == 0)
        {
            return i;
        }
    }
    // Return -1 if the color is not found
    return -1;
}
```

`practice.c`

```C
int main()
{
    // Define variables
    char input[30];
    char color1[10], color2[10];

    // Get the input as a single string
    printf("Enter the colors (e.g, brown-green): ");
    scanf("%s", input);

    // Split the input by the hyphen
    sscanf(input, "%[^-]-%[^-]", color1, color2);

    // Convert the first two colors to their respective values
    int value1 = get_color_value(color1);
    int value2 = get_color_value(color2);

    // Check if the input colors are valid
    if (value1 == -1 || value2 == -1)
    {
        printf("Invalid color entered.\n");
        return 1;
    }

    // Calculate the two-digit number;
    int result = value1 * 10 + value2;

    // Print the result
    printf("The resistor value is: %d\n", result);
    return 0;
}

```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter the colors (e.g, brown-green): brown-green
The resistor value is: 15

chan@CMA:~/C_Programming/practice$ ./practice
Enter the colors (e.g, brown-green): brown-green-violet
The resistor value is: 15

chan@CMA:~/C_Programming/practice$ ./practice
Enter the colors (e.g, brown-green): Brown-Green
The resistor value is: 15
```

#### Additional Things to Note

`sscanf(input, "%[^-]-%[^-]", color1, color2);` which captures the first two color names separated by hyphens and ignores any additional colors. This ensures that only the first two colors are processed, and any extra colors are ignored.

-  The format string `"%[^-]-%[^-]"` used in `sscanf` is a way to parse a string by splitting it based on a delimiter, in this case, the hyphen (`-`).

##### Explanation of `"%[^-]-%[^-]"`

1. **`%[^-]`**:
   - The `%` character indicates the start of a format specifier.
   - The `[` character introduces a scanset, which is a set of characters to be matched.
   - The `^` character inside the scanset negates the set, meaning it will match any character except those in the set.
   - The `-` character inside the scanset specifies that the hyphen character should not be matched.
   - Therefore, `%[^-]` matches any sequence of characters up to, but not including, the first hyphen.
2. **`-`**:
   - This matches the literal hyphen character in the input string.
3. **`%[^-]`**:
   - This is another scanset that matches any sequence of characters up to, but not including, the next hyphen.

##### Putting It All Together

- `"%[^-]-%[^-]` tells `sscanf` to:
  1. Read characters into the first variable (`color1`) until a hyphen is encountered.
  2. Skip the hyphen.
  3. Read characters into the second variable (`color2`) until another hyphen is encountered or the end of the string is reached.

##### Example

Given the input string `"brown-green-violet"`:

- `%[^-]` will match `"brown"` and store it in `color1`.
- The hyphen `-` will be skipped.
- `%[^-]` will match `"green"` and store it in `color2`.

The third color `"violet"` will be ignored because the format string only specifies two scansets.



**More about `delimiter` can be found  in `essentials.md`.**



##### Explanation of `sscanf`

- In C, the `sscanf` function is used to read formatted input from a string. 
- It is similar to `scanf`, but instead of reading from standard input, it reads from a provided string.

`Syntax`

```C
int sscanf(const char *str, const char *format, ...);
```

`Parameters`

- `str`: The input string from which to read the data.
- `format`: The format string that specifies how to interpret the input.
- `...`: The additional arguments that point to the variables where the parsed values will be stored.

`Return Value`

- The function returns the number of input items successfully matched and assigned. 
- If the input does not match the format, it returns `EOF`.

`Example`

```C
#include <stdio.h>

int main() {
    char input[] = "brown green";
    char color1[10], color2[10];

    // Use sscanf to parse the input string
    int result = sscanf(input, "%s %s", color1, color2);

    if (result == 2) {
        printf("Color 1: %s\n", color1);
        printf("Color 2: %s\n", color2);
    } else {
        printf("Failed to parse input\n");
    }

    return 0;
}
```

`Explanation`

- The `input` string contains the colors "brown" and "green".
- The `sscanf` function is used to parse these colors into the `color1`and `color2` variables.
- The format string `"%s %s"` specifies that two strings separated by a space should be read.
- The `result` variable will contain the number of successfully parsed items, which should be 2 in this case.

#### Second Solution (Better Approach)

`practice.h`

```C
#define ERROR_VALUE ((resistor_band_t) - 1)
typedef enum{
    BLACK = 0,
    BROWN,
    RED,
    ORANGE,
    YELLOW,
    GREEN,
    BLUE,
    VIOLET,
    GREY,
    WHITE
} resistor_band_t;

// Struct to map color names to resistor_band_t values
typedef struct{
    const char *name;
    resistor_band_t value;
}color_map_t;

uint16_t color_code(const resistor_band_t *bands);

resistor_band_t lookup_color_value(const char *color);
```

`functions.c`

```C
color_map_t color_map[] = {
    {"black", BLACK},
    {"brown", BROWN},
    {"red", RED},
    {"orange", ORANGE},
    {"yellow", YELLOW},
    {"green", GREEN},
    {"blue", BLUE},
    {"violet", VIOLET},
    {"grey", GREY},
    {"white", WHITE},
    {NULL, ERROR_VALUE}
};

// Function to print the duo color value
uint16_t color_code(const resistor_band_t *bands){
    return bands[0] * 10 + bands[1];
}

resistor_band_t lookup_color_value(const char *color){
    // Iterate till the name of the struct is equal to NULL (the last value)
    for(int i = 0; color_map[i].name != NULL; i++){
        if(strcasecmp(color, color_map[i].name) == 0){
            return color_map[i].value;
        }
    }
    return ERROR_VALUE;
}
```

`practice.c`

```C
int main(){
    char input[30];
    char color1[10], color2[10];
    resistor_band_t bands[2];
    
    printf("Enter the colors (e.g, brown-green): ");
    scanf("%s", input);
    
    // Read up to two color codes separated by a hyphen
    sscanf(input, "%[^-]-%[^-]", color1, color2);
    
    // Look up resistor_band_t values for the colors
    bands[0] = lookup_color_values(color1);
    bands[1] = lookup_color_values(color2);
    
    if(bands[0] == ERROR_VALUE || bands[1] == ERROR_VALUE){
        fprintf(stderr, "Invalid color name.\n");
        return EXIT_FAILURE;
    }
    
    // Calculate the resistor value
    uint16_t resistor_value = color_code(bands);
    
    printf("The resistor value is %u\n", resistor_value);
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/test$ ./final
Enter the colors (e.g, brown-green): brown-green
The resistor value is 15
    
chan@CMA:~/C_Programming/test$ ./final
Enter the colors (e.g, brown-green): brown-green-violet
The resistor value is 15
    
chan@CMA:~/C_Programming/test$ ./final
Enter the colors (e.g, brown-green): black-white
The resistor value is 9
    
chan@CMA:~/C_Programming/test$ ./final
Enter the colors (e.g, brown-green): Brown-Green
The resistor value is 15
    
chan@CMA:~/C_Programming/test$ ./final
Enter the colors (e.g, brown-green): Brown
Invalid color name.
```

#### Explanation

- The `color_map_t` struct maps color names to their corresponding `resistor_band_t`values.
- The `color_map` array holds the mappings, ending with a sentinel value.
- The `lookup_color_value` function searches the `color_map` array for the given color name and returns the corresponding `resistor_band_t` value.
- The `main` function prompts the user for input, parses the first two colors, looks up their `resistor_band_t` values using `lookup_color_value`, and then calls `color_code` to get the color code.
- The result is printed to the console.

---

### Hamming Program

#### Instructions

Calculate the Hamming Distance between two DNA strands.

Your body is made up of cells that contain DNA. Those cells regularly wear out and need replacing, which they achieve by dividing into daughter cells. In fact, the average human body experiences about 10 quadrillion cell divisions in a lifetime!

When cells divide, their DNA replicates too. Sometimes during this process mistakes happen and single pieces of DNA get encoded with the incorrect information. If we compare two strands of DNA and count the differences between them we can see how many mistakes occurred. This is known as the "Hamming Distance".

We read DNA using the letters C,A,G and T. Two strands might look like this:

```
GAGCCTACTAACGGGAT
CATCGTAATGACGGCCT
^ ^ ^  ^ ^    ^^
```

They have 7 differences, and therefore the Hamming Distance is 7.

The Hamming Distance is useful for lots of things in science, not just biology, so it's a nice phrase to be familiar with :)

#### Implementation notes

The Hamming distance is only defined for sequences of equal length, so an attempt to calculate it between sequences of different lengths should not work.



#### Solution

`practice.h`

```C
int compute(const char *lhs, const char *rhs);
```

`functions.c`

```C
int compute(const char *lhs, const char *rhs)
{
    int hamming_distance = 0;

    // Check if the lengths are equal
    if (strlen(lhs) != strlen(rhs))
    {
        return -1; //
    }

    // Calculate the Hamming distance
    for (int i = 0; lhs[i] != '\0'; i++)
    {
        if (lhs[i] != rhs[i])
        {
            hamming_distance++;
        }
    }

    return hamming_distance;
}
```

`practice.c`

```C
int main()
{
    const char *strand1 = "GAGCCTACTAACGGGAT";
    const char *strand2 = "CATCGTAATGACGGCCT";

    int distance = compute(strand1, strand2);

    if (distance == -1)
    {
        printf("Error: DNA strands must be of equal length.\n");
    }
    else
    {
        printf("Hamming Distance: %d\n", distance);
    }

    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Hamming Distance: 7

```



#### Solution (Better and Optimized Solution)

`practice.h`

```C
#define ERROR_CODE -1
int computer(const char *lhs, const char *rhs);
```



`functions.c`

```C
#include <string.h>

int computer(const char *lhs, const char *rhs){
    int hamming_distance = 0;
    int i = 0;
    for(i = 0; lhs[i] != '\0' && rhs[i] != '\0'; i++){
        if(lhs[i] != rhs[i]){
            hamming_distance++;
        }
    }
    
    if(lhs[i] != '\0' || rhs[i] != '\0'){
        return ERROR_CODE;
    }
}
```



---

## Space Age

### Instructions

Given an age in seconds, calculate how old someone would be on:

- Mercury: orbital period 0.2408467 Earth years
- Venus: orbital period 0.61519726 Earth years
- Earth: orbital period 1.0 Earth years, 365.25 Earth days, or 31557600 seconds
- Mars: orbital period 1.8808158 Earth years
- Jupiter: orbital period 11.862615 Earth years
- Saturn: orbital period 29.447498 Earth years
- Uranus: orbital period 84.016846 Earth years
- Neptune: orbital period 164.79132 Earth years

So if you were told someone were 1,000,000,000 seconds old, you should be able to say that they're 31.69 Earth-years old.

If you're wondering why Pluto didn't make the cut, go watch [this YouTube video](https://www.youtube.com/watch?v=Z_2gbGXzFbs).

Note: The actual length of one complete orbit of the Earth around the sun is closer to 365.256 days (1 sidereal year). The Gregorian calendar has, on average, 365.2425 days. While not entirely accurate, 365.25 is the value used in this exercise. See [Year on Wikipedia](https://en.wikipedia.org/wiki/Year#Summary) for more ways to measure a year.



### Solution

`practice.h`

```C
#define EARTH_YEAR_SECONDS 31557600.0
#define ERROR_CODE -1

typedef enum planet
{
    MERCURY,
    VENUS,
    EARTH,
    MARS,
    JUPITER,
    SATURN,
    URANUS,
    NEPTUNE,
} planet_t;

float age(planet_t planet, int64_t seconds);

```

- This header file defines an enumeration `planet_t` for the planets in the solar system.
- It also defines two constants: `EARTH_YEAR_SECONDS` representing the number of seconds in an Earth year, and `ERROR_CODE`for error handling.

`functions.c`

```C
const double orbital_periods[] = {
    [MERCURY] = 0.2408467,
    [VENUS] = 0.61519726,
    [EARTH] = 1.0,
    [MARS] = 1.8808158,
    [JUPITER] = 11.862615,
    [SATURN] = 29.447498,
    [URANUS] = 84.016846,
    [NEPTUNE] = 164.79132};

float age(planet_t planet, int64_t seconds)
{
    if ((int)planet < 0)
    {
        return ERROR_CODE;
    }

    return seconds / (EARTH_YEAR_SECONDS * orbital_periods[planet]);
}
```

- The array `orbital_periods` holds the orbital periods of the planets in the solar system relative to Earth's orbital period (1.0). The array is indexed using enumerators like `MERCURY`, `VENUS`, etc.
- Casting `planet` to `(int)` ensures the comparison is valid and checks if `planet` is a negative integer. Removing the cast to `(int)` will result in segmentation fault.
- Array indexing in C automatically converts the index to an integer type.
- `orbital_periods[planet]` works as long as `planet` is a valid index within the bounds of the `orbital_periods` array.

`practice.c`

```C
int main()
{
    int64_t age_in_seconds;

    // Example input: 1 billion seconds
    age_in_seconds = 1000000000;

    printf("Age in seconds: %.0f\n", age_in_seconds);
    printf("Age on Mercury: %.2f years\n", age(MERCURY, age_in_seconds));
    printf("Age on Venus: %.2f years\n", age(VENUS, age_in_seconds));
    printf("Age on Earth: %.2f years\n", age(EARTH, age_in_seconds));
    printf("Age on Mars: %.2f years\n", age(MARS, age_in_seconds));
    printf("Age on Jupiter: %.2f years\n", age(JUPITER, age_in_seconds));
    printf("Age on Uranus: %.2f years\n", age(URANUS, age_in_seconds));
    printf("Age on Neptune: %.2f years\n", age(NEPTUNE, age_in_seconds));
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Age in seconds: 1000000000
Age on Mercury: 131.57 years
Age on Venus: 51.51 years
Age on Earth: 31.69 years
Age on Mars: 16.85 years
Age on Jupiter: 2.67 years
Age on Uranus: 0.38 years
Age on Neptune: 0.19 years
```



#### Better or Optimized Solution

`practice.h`

```C
enum{
    ERROR_CODe = -1
};

typedef enum planet
{
    MERCURY,
    VENUS,
    EARTH,
    MARS,
    JUPITER,
    SATURN,
    URANUS,
    NEPTUNE,
} planet_t;

float age(planet_t planet, int64_t seconds);
```

- Here, we choose to define it as an enumerator in an unnamed `enum` instead.
- Macros are a known source of issues, so if you can avoid them or reduce their usage it can make your code easier to work with.

`functions.c`

```C
#define EARTH_YEAR_SECONDS 31557600.0f

static const float orbital_periods[] = {
    [MERCURY] = 0.2408467f,
    [VENUS] = 0.61519726f,
    [EARTH] = 1.0f,
    [MARS] = 1.8808158f,
    [JUPITER] = 11.862615f,
    [SATURN] = 29.447498f,
    [URANUS] = 84.016846f,
    [NEPTUNE] = 164.79132f};

float age(planet_t planet, int64_t seconds)
{
    if ((int)planet < 0 || (int)planet >= (int)(sizeof(orbital_periods) / sizeof(orbital_periods[0])))
    {
        return ERROR_CODE;
    }

    return seconds / (EARTH_YEAR_SECONDS * orbital_periods[planet]);
}
```

##### First Modification:

You've defined `EARTH_YEAR_SECONDS` in the `.h` file.

That's perfectly fine if you consider it part of the interface of your code and if you want it to be used by other `.h` or `.c` files. But macros are tricky, they don't obey the usual scope and precedence rules, if the files that `#include` your `.h` file use that symbol themselves that will result in a compilation error.

And as a warning: If this were a library, your users would eventually start to use this macro, depend on it and then you'd have to support it.

In my view it's rather an implementation detail that others should not have to worry about. I'd suggest moving the macro to the `.c` file. That hides it from other files and communicates to the reader that it's not part of the interface.

##### Second Modification

You could consider defining it as `static const`. That would give the array *internal linkage*, it would essentially hide the array from other translation units and avoid conflicts with global variables of that name in other `.c` files.
This is one of the differences between C and C++: In C++ the `const` alone would give the array internal linkage, in C you need to write `static` explicitly.

##### Third Modification

Lines 13-15 handle a negative value as an error. Great, that's what the tests require.

But what about the other end, if the value of `planet` is too big? I'd suggest handling that as an error as well.

`practice.c`

```C
int main()
{
    int64_t age_in_seconds;

    // Example input: 1 billion seconds
    age_in_seconds = 1000000000;

    printf("Age in seconds: %.0ld\n", age_in_seconds);
    printf("Age on Mercury: %.2f years\n", age(MERCURY, age_in_seconds));
    printf("Age on Venus: %.2f years\n", age(VENUS, age_in_seconds));
    printf("Age on Earth: %.2f years\n", age(EARTH, age_in_seconds));
    printf("Age on Mars: %.2f years\n", age(MARS, age_in_seconds));
    printf("Age on Jupiter: %.2f years\n", age(JUPITER, age_in_seconds));
    printf("Age on Uranus: %.2f years\n", age(URANUS, age_in_seconds));
    printf("Age on Neptune: %.2f years\n", age(NEPTUNE, age_in_seconds));
    return 0;
}

```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Age in seconds: 1000000000
Age on Mercury: 131.57 years
Age on Venus: 51.51 years
Age on Earth: 31.69 years
Age on Mars: 16.85 years
Age on Jupiter: 2.67 years
Age on Uranus: 0.38 years
Age on Neptune: 0.19 years

```



---

# RNA Transcription

## Introduction

You work for a bioengineering company that specializes in developing therapeutic solutions.

Your team has just been given a new project to develop a targeted therapy for a rare type of cancer.

Note

It's all very complicated, but the basic idea is that sometimes people's bodies produce too much of a given protein. That can cause all sorts of havoc.

But if you can create a very specific molecule (called a micro-RNA), it can prevent the protein from being produced.

This technique is called [RNA Interference](https://admin.acceleratingscience.com/ask-a-scientist/what-is-rnai/).

## Instructions

Your task is determine the RNA complement of a given DNA sequence.

Both DNA and RNA strands are a sequence of nucleotides.

The four nucleotides found in DNA are adenine (**A**), cytosine (**C**), guanine (**G**) and thymine (**T**).

The four nucleotides found in RNA are adenine (**A**), cytosine (**C**), guanine (**G**) and uracil (**U**).

Given a DNA strand, its transcribed RNA strand is formed by replacing each nucleotide with its complement:

- `G` -> `C`
- `C` -> `G`
- `T` -> `A`
- `A` -> `U`



## Solution

`practice.h`

```C
char *to_rna(const char *dna);
```

`functions,c`

```C
char *to_rna(const char *dna)
{
    // Get the length of the input DNA strand
    size_t length = strlen(dna);

    // Allocate memory for the RNA strand (same as DNA)
    char *rna = (char *)malloc(length + 1); // +1 for the null terminator

    // Check if memory allocation was successful
    if (rna == NULL)
    {
        return NULL;
    }

    // Iterate through each nucleotide in the DNA strand
    for (size_t i = 0; i < length; i++)
    {
        switch (dna[i])
        {
        case 'G':
            rna[i] = 'C';
            break;
        case 'C':
            rna[i] = 'G';
            break;
        case 'T':
            rna[i] = 'A';
            break;
        case 'A':
            rna[i] = 'U';
            break;
        default:
            // Free allocated memory
            free(rna);
            // Return NULL if an invalid character is found
            return NULL;
        }
    }

    // Null-terminate the RNA string
    rna[length] = '\0';
    return rna;
}
```

`practice.c`

```C
int main()
{
    // Example DNA sequence
    const char *dna_seq = "GATTACA";

    // Convert the DNA to RNA
    char *rna_seq = to_rna(dna_seq);

    // Check if the conversion is successful
    if (rna_seq != NULL)
    {
        printf("RNA Complement: %s\n", rna_seq);
        // Free the allocated memory for the RNA sequence
        free(rna_seq);
    }
    else
    {
        printf("Invalid DNA sequence or memory allocation failed.\n");
    }
    return 0;
}

```

`Output`

```C
chan@CMA:~/C_Programming/practice$ ./practice
RNA Complement: CUAAUGU
```

### Explanation:

`to_rna` is a normal function that returns a pointer to a `char`. It is not a function pointer or a pointer function.

1. **Memory Allocation**:
   We use `malloc` to allocate enough memory for the RNA string, which is the same length as the DNA string plus 1 for the null terminator (`'\0'`).
2. **Transcription Process**:
   We loop through each character in the DNA string and convert it to the corresponding RNA nucleotide using a `switch` statement.
3. **Error Handling**:
   If an invalid character is found (not 'G', 'C', 'T', or 'A'), the function will return `NULL`, indicating an error. Memory is freed in this case to avoid memory leaks.
4. **Null-Termination**:
   After the conversion, we append the null terminator (`'\0'`) to the RNA string to make it a valid C string.
5. **Example Execution**: For the DNA sequence `"GATTACA"`, the RNA sequence will be `"CUAAUGU"`.

---



## Binary

### Instructions

Convert a binary number, represented as a string (e.g. '101010'), to its decimal equivalent using first principles.

Implement binary to decimal conversion. Given a binary input string, your program should produce a decimal output. The program should handle invalid inputs.

### Note

- Implement the conversion yourself. Do not use something else to perform the conversion for you.

### About Binary (Base-2)

Decimal is a base-10 system.

A number 23 in base 10 notation can be understood as a linear combination of powers of 10:

- The rightmost digit gets multiplied by 10^0 = 1
- The next number gets multiplied by 10^1 = 10
- ...
- The *n*th number gets multiplied by 10^*(n-1)*.
- All these values are summed.

So: `23 => 2*10^1 + 3*10^0 => 2*10 + 3*1 = 23 base 10`

Binary is similar, but uses powers of 2 rather than powers of 10.

So: `101 => 1*2^2 + 0*2^1 + 1*2^0 => 1*4 + 0*2 + 1*1 => 4 + 1 => 5 base 10`.



### Solution

`practice.h`

```C
enum{
    ERROR_CODE = -1
};

int convert(const char *input);
```

`functions.c`

```C
int convert(const char *input){
    int num_length = strlen(input);
    int result = 0;
    int i = 0;
    for(i = num_length - 1; i >= 0; i--){
        if(input[i] == '1'){
            result += pow(2, num_length - 1 - i);
        }
        if(input[i] != '0' && input[i] != '1'){
            return ERROR_CODE;
        }
    }
    return result;
}
```

`practice.c` (Main)

```C
int main(){
    char num[100];
    int result = 0;
    
    while(1){
        printf("Enter the binary number for coversion to the decimal: ");
    	scanf("%99s", num);
        result = convert(num);
        if(result != ERROR_CODE){
            break;
        }else{
            printf("Invalid binary number. Please try again.\n");
        }
    }
    
    printf("Result: %d\n", result);
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 101
Result: 5

chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 111
Result: 7

chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 100101
Result: 37

chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 234
Invalid binary number. Please try again.
Enter the binary number for conversion to the decimal: 401
Invalid binary number. Please try again.
Enter the binary number for conversion to the decimal: 104
Invalid binary number. Please try again.
Enter the binary number for conversion to the decimal: 102
Invalid binary number. Please try again.
Enter the binary number for conversion to the decimal: 110
Result: 6
```

#### Explanation for the Solution 1

The expression `pow(2, num_length - 1 - i)` is used to calculate the power of 2 for the corresponding binary digit in the string. Let's break it down step-by-step:

1. **Binary Representation**: In a binary number, each digit (bit) represents an increasing power of 2, starting from the rightmost bit (least significant bit) to the leftmost bit (most significant bit).
2. **Indexing**:
   - `num_length` is the length of the input string.
   - `i`is the current index in the loop, which starts from the last character of the string (`num_length - 1`) and decrements to 0.
3. **Power Calculation**:
   - `num_length - 1 - i` calculates the position of the bit from the right (least significant bit).
   - For example, if `num_length`, and `i`is 3 (the leftmost bit), `num_length - 1 - i` would be `4 - 1 - 3 = 0`, which means the leftmost bit is at position 0 from the right.
   - As `i` decreases, `num_length - 1 - i` increases, representing the increasing powers of 2 from right to left.
4. **Using `pow` Function**:
   - `pow(2, num_length - 1 - i)` computes `2` raised to the power of `(num_length - 1 - i)`.
   - This gives the value of the bit at position `i` in the binary number.
5. This loop iterates over each bit of the binary number from right to left.
6. If the bit is `1`, it adds the corresponding power of 2 to the `result`.

#### Example Breakdown:

Consider the binary number `1101`:

`num_length` = 4
**Loop iterations:**

- `i = 3`: `input[3]` is `1`, `num_length - 1 - i is 0, pow(2, 0)` is `1`.
- `i = 2`: `input[2]` is `0`, `num_length - 1 - i` is `1`, `pow(2, 1)`  is  `2`.
- `i = 1`: `input[1]` is `1`, `num_length - 1 - i` is `2`, `pow(2, 2)` is `4`.
- `i = 0`: `input[0]` is `1`, `num_length - 1 - i` is `3`, `pow(2, 3)` is `8`.
- **Final Calculation:**
  The decimal value is calculated as:
  `1 * 8 + 1 * 4 + 0 * 2 + 1 * 1 = 8 + 4 + 0 + 1 = 13`



However, the use of `pow` function returns a `double`, which is not ideal for integer calculations. Instead, use bitwise operations for better performance and accuracy.



#### Solution 2 with bitwise operators

`practice.h`

```C
enum
{
    ERROR_CODE = -1
};

int convert(const char *input);

```

`functions.c`

```C
int convert(const char *input){
    int num_length = strlen(input);
    int result = 0;
    int i = 0;
    for(i = 0; i < num_length; i++){
        if(input[i] == '1'){
            result = (result << 1) | 1;
        }else if(input[i] == '0'){
            return = result << 1;
        }else{
            return ERROR_CODE;
        }
    }
}
```

`practice.c`

```C
int main()
{
    char num[100];
    int result = 0;
    while (1)
    {
        printf("Enter the binary number for conversion to the decimal: ");
        scanf("%99s", num);

        result = convert(num);
        if (result != ERROR_CODE)
        {
            break;
        }
        else
        {
            printf("Invalid binary number. Please try again.\n");
        }
    }

    printf("Result: %d\n", result);
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 101
Result: 5
chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 111
Result: 7
chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 234
Invalid binary number. Please try again.
Enter the binary number for conversion to the decimal: 112
Invalid binary number. Please try again.
Enter the binary number for conversion to the decimal: 4312
Invalid binary number. Please try again.
Enter the binary number for conversion to the decimal: 1131
Invalid binary number. Please try again.
Enter the binary number for conversion to the decimal: 100101
Result: 37
```

#### Explanation for the Solution 2

- The `convert` function takes a string of binary digits (`'0'` and `'1'`) and converts it into an integer. If the string contains any character other than `'0'` or `'1'`, it returns an error code (`ERROR_CODE`).

##### Left Shift Operator (`<<`)

- The left shift operator (`<<`) shifts all bits in a number to the left by a specified number of positions.
- Each shift to the left is equivalent to multiplying the number by 2.
- Example:
  - `5 << 1` shifts the bits of `5` (binary `101`) to the left, resulting in `10` (binary `1010`).
  - `10 << 1` shifts the bits of `10` (binary `1010`) to the left, resulting in `20` (binary `10100`).

- **If the character is `'1'`:**
  - `result = (result << 1) | 1;`
  - The left shift operator (`<<`) shifts all bits in `result` one position to the left. This is equivalent to multiplying `result` by 2.
  - The bitwise OR operator (`|`) sets the least significant bit to 1.
  - Example: If `result` is `5` (binary `101`), `result << 1` becomes `10` (binary `1010`). Then, `10 | 1` becomes `11` (binary `1011`).
- **If the character is `'0'`:**
  - `result = result << 1;`
  - The left shift operator (`<<`) shifts all bits in `result` one position to the left. This is equivalent to multiplying `result` by 2.
  - Example: If `result` is `5` (binary `101`), `result << 1` becomes `10` (binary `1010`).
- **If the character is neither `'1'` nor `'0'`:**
  - `return ERROR_CODE;`
  - The function returns an error code.

#### Example Conversion

Let's walk through an example conversion of the binary string `"1011"`:

1. Initial `result = 0`

2. First character `'1'`: `result = (0 << 1) | 1 = 1`

   ```
   0 << 1 = 0 0 0 0
   1      = 0 0 0 1
   -----------------
   XOR    = 0 0 0 1
   
   decimal = 1
   ```

   

3. Second character `'0'`: `result = 1 << 1 = 2`

   ```
   1 << 1 = 0 0 1 0
   
   decimal value = 2
   ```

   

4. Third character `'1'`: `result = (2 << 1) | 1 = 5`

   ```
   2 << 1 = 0 1 0 0 
   1      = 0 0 0 1
   ----------------
   XOR	   = 0 1 0 1
   
   decimal value = 5;
   ```

   

5. Fourth character `'1'`: `result = (5 << 1) | 1 = 11`

   ```
   5 << 1 = 1 0 1 0
   1      = 0 0 0 1
   -----------------
   XOR    = 1 0 1 1
   
   decimal value = 11
   ```

   

Final result is `11`, which is the integer representation of the binary string `"1011"`.

```sh
chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 1011
Result: 11
```

**Why 10 | 1 becomes 11?**

- The bitwise OR operator (`|`) compares each bit of two numbers and returns a new number whose bits are set to `1` if either of the corresponding bits of the operands is `1`.

Let's look at the binary representation of the numbers involved:

- `10` in binary is `1010`
- `1`in binary is `0001`

When we perform the bitwise OR operation, we compare each bit of `10` and `1`:

```
  1010  (10 in binary)
| 0001  (1 in binary)
------
  1011  (result in binary)
```

#### Step-by-Step Breakdown

1. First bit (from the right):
   - `0 | 1` = `1`
2. Second bit:
   - `1 | 0` = `1`
3. Third bit:
   - `0 | 0` = `0`
4. Fourth bit:
   - `1 | 0` = `1`

So, `1010 | 0001` results in `1011`.

#### Conversion to Decimal

The binary number `1011` converts to the decimal number `11`:

- `1 * 2^3` = `8`
- `0 * 2^2`)= `0`
- `1 * 2^1` = `2`
- `1 * 2^0` = `1`

Adding these values together: `8 + 0 + 2 + 1 = 11`.

- `5 << 1` results in `10` (binary `1010`).
- `10 | 1` results in `11` (binary `1011`).
- When we print the result using `%d`, it is displayed as a decimal number `11`.

Therefore, `10 | 1` results in `11`.



#### The Best Solution (Based on the feedback, code review from a mentor on "Exercism" platform)

##### **What the mentor said about the above solution:**

Clear and simple. Well done!

I love that you've replaced the macro `INVALID` from the initial stub with an enumerator of an unnamed `enum`. That gets the job done without involving the preprocessor.

I just have some small suggestions:

------

The parameter is named `input`, the variable in line 4 is named `num_len`.

*Personally* I'm not a big fan of generic names like `input`. From the point of view of the function any parameter (that is not an OUT-variable) is "input". That doesn't tell the reader anything about the meaning or purpose of the parameter. How about something like `num` or `binary`?

`num_len` represents the length of the `input`. You could consider choosing a name that makes it more obvious how the two variables are related. If you'd rename `input` to `num` *OR* `num_len` to `input_len` I think that would make the code a tiny bit easier to understand.

But naming is subjective to a large degree. If you like the names and think they are easy to understand, keep them.

------

I recommend defining variables as late as possible and in the narrowest possible scope. Often that turns unintended uses into compilation errors (which is a good thing). It also reduces the mental burden of the reader because they don't have to remember the type and value of a variable for several lines.

Since `i` is not used outside the loop you could define it in the first part of the `for` construct in line 7 where you assign a value to it. Then you wouldn't need line 6 anymore and the code would become a tiny bit shorter.

------

Line 4 calls `strlen()` to determine the length of the string.

`strlen()` computes its result by traversing the string until it finds the terminating null-byte. That means lines 4 and 7-15 traverse the `input` twice.
IMHO that's fine, the `input` will probably be not that long and it won't make a bit difference whether you traverse a string once or twice.

But if you're looking for potential optimizations: The loop in line 7 could loop until the current character `input[i]` is the terminating null-byte. Then you wouldn't need line 4.

------

Lines 9 and 11 calculate the new value of the `result` with a bitwise left-shift operation and a bitwise `OR`. Nothing wrong with that.

*Personally* I would rather keep the calculation on the arithmetic level, use a multiplication and an addition, and trust my compiler that it will generate optimized instructions.
You can see that in the [compiler explorer](https://compiler-explorer.com/z/YEqeEKxf7).

But if you like these bitwise operations and think they are easy to understand, keep them.

`practice.h`

```C
enum{
    ERROR_CODe = -1
};

int convert(const char *binary);
```

`functions.c`

```C
int convert(const char *binary){
    int result = 0;
    for(int i = 0; binary[i] != '\0'; i++){
        if(binary[i] == '1'){
            result = result * 2 + 1;
        }else if(binary[i] == '0'){
            result *= 2;
        }else{
            return ERROR_CODE;
        }
    }
    return result;
}
```

`practice.c`

```c
int main()
{
    char num[100];
    int result = 0;
    while (1)
    {
        printf("Enter the binary number for conversion to the decimal: ");
        scanf("%99s", num);

        result = convert(num);
        if (result != ERROR_CODE)
        {
            break;
        }
        else
        {
            printf("Invalid binary number. Please try again.\n");
        }
    }

    printf("Result: %d\n", result);
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 1011
Result: 11

chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 101
Result: 5

chan@CMA:~/C_Programming/test$ ./final
Enter the binary number for conversion to the decimal: 231
Invalid binary number. Please try again.
Enter the binary number for conversion to the decimal: 1101
Result: 13
```

`Explanation`

###### Iteration Process

Let's assume the input binary string is `"1011"`. We'll go through each character of the string and see how `result` is updated in each iteration.

1. **Initialization**:
   - `result` is initialized to `0`.
2. **First Iteration (`i = 0`)**:
   - `num[0]` is `'1'`.
   - Since `num[0]` is `'1'`, the code executes `result = result * 2 + 1`.
   - Calculation: `result = 0 * 2 + 1 = 1`.
   - `result` is now `1`.
3. **Second Iteration (`i = 1`)**:
   - `num[1]` is `'0'`.
   - Since `num[1]` is `'0'`, the code executes `result *= 2`.
   - Calculation: `result = 1 * 2 = 2`.
   - `result`is now `2`.
4. **Third Iteration (`i = 2`)**:
   - `num[2]` is `'1'`.
   - Since `num[2]` is `'1'`, the code executes `result = result * 2 + 1`.
   - Calculation: `result = 2 * 2 + 1 = 5`.
   - `result` is now `5`.
5. **Fourth Iteration (`i = 3`)**:
   - `num[3]` is `'1'`.
   - Since `num[3]` is `'1'`, the code executes `result = result * 2 + 1`.
   - Calculation: `result = 5 * 2 + 1 = 11`.
   - `result` is now `11`.
6. **End of Loop**:
   - The loop terminates because `num[4]`is the null-byte (`'\0'`).

###### Final Result

- The function returns `result`, which is `11`.

###### Summary of Iterations

| Iteration | `num[i]` | Operation                 | Calculation      | `result` |
| --------- | -------- | ------------------------- | ---------------- | -------- |
| 0         | '1'      | `result = result * 2 + 1` | `0 * 2 + 1 = 1`  | 1        |
| 1         | '0'      | `result *= 2`             | `1 * 2 = 2`      | 2        |
| 2         | '1'      | `result = result * 2 + 1` | `2 * 2 + 1 = 5`  | 5        |
| 3         | '1'      | `result = result * 2 + 1` | `5 * 2 + 1 = 11` | 11       |

###### Conclusion

The function correctly converts the binary string `"1011"` to its decimal equivalent `11` by iterating through each character of the string and updating the `result`] accordingly. If any character other than `'0'` or `'1'` is encountered, the function returns `ERROR_CODE`.

---



## Eliud's Eggs

### Instruction

Your friend Eliud inherited a farm from her grandma Tigist. Her granny was an inventor and had a tendency to build things in an overly complicated manner. The chicken coop has a digital display showing an encoded number representing the positions of all eggs that could be picked up.

Eliud is asking you to write a program that shows the actual number of eggs in the coop.

The position information encoding is calculated as follows:

1. Scan the potential egg-laying spots and mark down a `1` for an existing egg or a `0` for an empty spot.
2. Convert the number from binary to decimal.
3. Show the result on the display.

**Example 1**:

```
Chicken Coop:
 _ _ _ _ _ _ _
|E| |E|E| | |E|

Resulting Binary:
 1 0 1 1 0 0 1

Decimal number on the display:
89

Actual eggs in the coop:
4
```

**Example 2**:

```
Chicken Coop:
 _ _ _ _ _ _ _ _
| | | |E| | | | |

Resulting Binary:
 0 0 0 1 0 0 0 0

Decimal number on the display:
16

Actual eggs in the coop:
1
```

Your task is to count the number of 1 bits in the binary representation of a number.



### Solution

`practice.h`

```C
int eggs_count(int encoded_num);
```

`functions.c`

```C
int eggs_count(int encoded_num){
    int count = 0;
    
    // Loop thru each bit of the number
    while(encoded_num > 0){
        // Check if the least significant bit is 1
        if(encoded_num & 1){
            count++;
        }
        
        //Shift the number to the right by 1 bit
        encoded_num >>= 1;
    }
    return count;
}
```

`practice.c`

```C
int main()
{
    int encoded_num = 0;
    printf("Enter the encoded number: ");
    scanf("%d", &encoded_num);

    printf("Actual eggs in the coop: %d\n", eggs_count(encoded_num));

    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter the encoded number: 89
Actual eggs in the coop: 4

chan@CMA:~/C_Programming/practice$ ./practice
Enter the encoded number: 16
Actual eggs in the coop: 1

chan@CMA:~/C_Programming/practice$ ./practice
Enter the encoded number: -1
Actual eggs in the coop: 0

chan@CMA:~/C_Programming/practice$ ./practice
Enter the encoded number: -4
Actual eggs in the coop: 0

chan@CMA:~/C_Programming/practice$ ./practice
Enter the encoded number: -5
Actual eggs in the coop: 0

chan@CMA:~/C_Programming/practice$ ./practice
Enter the encoded number: 0
Actual eggs in the coop: 0

chan@CMA:~/C_Programming/practice$ ./practice
Enter the encoded number: 1
Actual eggs in the coop: 1

```

#### Things to Note

The bitwise AND operation compares each bit of two numbers and returns a new number whose bits are set to `1` only if both corresponding bits of the operands are `1`. For example:

- `5` in binary is `0101`
- `3` in binary is `0011`
- `5 & 3` results in `0001` (which is `1` in decimal)

- `while (encoded_num > 0)`: Loops as long as `encoded_num` is greater than 0.
  - **Check Least Significant Bit:**
    - `if (encoded_num & 1)`: Uses the bitwise AND operator to check if the least significant bit of `encoded_num` is 1. If it is, the condition is true.
    - `count++`: Increments the `count`variable by 1 if the least significant bit is 1.
  - **Shift Right:**
    - `encoded_num >>= 1;`: Shifts `encoded_num`]to the right by 1 bit, effectively dividing it by 2 and discarding the least significant bit.

##### Example Looping

- Let's use `13` as an example input (`encoded_num = 13`), which is `1101` in binary.

  1. **Initialization**:
     - `encoded_num = 13` (binary `1101`)
     - `count = 0`
  2. **First Iteration**:
     - `encoded_num & 1` checks if the least significant bit is `1`(which it is, since `1101 & 0001 = 0001`).
     - `count`is incremented to `1`.
     - `encoded_num` is right-shifted by 1 bit, resulting in `110` (which is `6` in decimal).
  3. **Second Iteration**:
     - `encoded_num & 1` checks if the least significant bit is `1`(which it is not, since `0110 & 0001 = 0000`).
     - `count` remains `1`.
     - `encoded_num`is right-shifted by 1 bit, resulting in `11` (which is `3` in decimal).
  4. **Third Iteration**:
     - `encoded_num & 1`checks if the least significant bit is `1` (which it is, since `0011 & 0001 = 0001`).
     - `count`is incremented to `2`.
     - `encoded_num` is right-shifted by 1 bit, resulting in `1` (which is `1` in decimal).
  5. **Fourth Iteration**:
     - `encoded_num & 1`checks if the least significant bit is `1` (which it is, since `0001 & 0001 = 0001`).
     - `count` is incremented to `3`.
     - `encoded_num` is right-shifted by 1 bit, resulting in `0`.
  6. **Termination**:
     - The loop terminates because `encoded_num` is now `0`.

  The function returns `3`, which is the number of `1`s in the binary representation of `13`.

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter the encoded number: 13
Actual eggs in the coop: 3

```



### 2nd Solution (Optimized)

- The second solution uses Brian Kernighan's algorithm.
- This algorithm works by repeatedly clearing the lowest set bit and incrementing the count until the number becomes zero.

#### What a mentor on exercism said about my solution

"Hi Chan,

Great code! This code runs in *O(bits)*, where *bits* is the number of bits needed to represent `encoded_num` (the position of the left-most set bit).

There is a trick to have this check take *O(set_bits)*, where *set_bits* is the number of bits set to 1.

So for example, `0b1000000000000000` would require 16 iterations with your method, and only one with this alternative method.

What you want is Brian Kernighan's (of K&R fame!) [algorithm to count bits](https://yuminlee2.medium.com/brian-kernighans-algorithm-count-set-bits-in-a-number-18ab05edca93#1844) :)"

##### Brain Kernighan's algorithm

- The idea is to **subtract** `n` **by 1** to **invert all the bits after the rightmost set bit** of `n` (including the rightmost set bit). 
- Then **use** `n & (n-1)` **expression** to **clear the rightmost set bit**. The number of times we clear the rightmost set bit until the number becomes zero will be the set bit count.

`practice.h`

```C
int eggs_count(int encoded_num);
```

`functions.c`

```C
int eggs_count(int encoded_num){
    int count = 0;
    // Loop thru each bit of the number
    while(encoded_num > 0){
        encoded_num &= (encoded_num - 1);
        count++;
    }
    return count;
}
```

`practice.c`

```C
int main()
{
    int encoded_num = 0;
    printf("Enter the encoded number: ");
    scanf("%d", &encoded_num);

    printf("Actual eggs in the coop: %d\n", eggs_count(encoded_num));

    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter the encoded number: 13
Actual eggs in the coop: 3
```

#### Explanation

- "Clearing the lowest set bit" means turning the rightmost `1` bit in the binary representation of the number to `0`.

#### How It Works

1. **Binary Representation**: Consider the binary representation of the number.
2. **Subtracting 1**: Subtracting `1`from the number flips all the bits after the rightmost `1` bit, including the rightmost `1` bit itself.
3. **Bitwise AND**: Performing a bitwise AND operation between the original number and the result of the subtraction clears the rightmost `1` bit.

#### Example

Let's take an example with `encoded_num = 13` (binary `1101`):

1. **Initial State**:
   - `encoded_num = 13` (binary `1101`)
2. **First Iteration**:
   - `encoded_num - 1 = 12` (binary `1100`)
   - `encoded_num &= (encoded_num - 1)`:
     - `1101 & 1100 = 1100`
   - `encoded_num` becomes `12` (binary `1100`)
3. **Second Iteration**:
   - `encoded_num - 1 = 11` (binary `1011`)
   - `encoded_num &= (encoded_num - 1)`:
     - `1100 & 1011 = 1000`
   - `encoded_num` becomes `8` (binary `1000`)
4. **Third Iteration**:
   - `encoded_num - 1 = 7` (binary `0111`)
   - `encoded_num &= (encoded_num - 1)`:
     - `1000 & 0111 = 0000`
   - `encoded_num` becomes `0` (binary `0000`)

After each iteration, the rightmost `1` bit is cleared, and the loop continues until `encoded_num` becomes `0`, resulting `count` equal to `3` in this case.

---



## Two Fer

### Introduction

In some English accents, when you say "two for" quickly, it sounds like "two fer". Two-for-one is a way of saying that if you buy one, you also get one for free. So the phrase "two-fer" often implies a two-for-one offer.

Imagine a bakery that has a holiday offer where you can buy two cookies for the price of one ("two-fer one!"). You take the offer and (very generously) decide to give the extra cookie to someone else in the queue.

Your task is to determine what you will say as you give away the extra cookie.

If you know the person's name (e.g. if they're named Do-yun), then you will say:

```text
One for Do-yun, one for me.
```

If you don't know the person's name, you will say *you* instead.

```text
One for you, one for me.
```

Here are some examples:

| Name   | Dialogue                    |
| :----- | :-------------------------- |
| Alice  | One for Alice, one for me.  |
| Bohdan | One for Bohdan, one for me. |
|        | One for you, one for me.    |
| Zaphod | One for Zaphod, one for me. |



### Solution

`practice.h`

```C
void two_fer(char *buffer, const char *name);
```

`functions.c`

```C
void two_fer(char *buffer, const char *name)
{
    if (name == NULL || strlen(name) == 0)
    {
        sprintf(buffer, "One for you, one for me.\n");
    }
    else
    {
        sprintf(buffer, "One for %s, one for me.\n", name);
    }
}
```

`practice.c`

```C
int main()
{
    char name[80];

    // buffer to store the result of two_fer
    char buffer[100];
    printf("Enter the name of the person: ");

    fgets(name, sizeof(name), stdin);

    // Remove newline character from fgets
    name[strcspn(name, "\n")] = '\0';

    two_fer(buffer, name);
    printf("%s", buffer);
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter the name of the person: 
One for you, one for me.
chan@CMA:~/C_Programming/practice$ ./practice
Enter the name of the person: Do-yun
One for Do-yun, one for me.
```



### Solution 2 (Better and Modified)

#### What my mentor said

Hi Chan!

The code looks perfect as it is, congrats!

Using `sprintf()` is frowned upon, as nothing prevents you from writing beyond the buffer limits, and `snprintf()` is preferred. For that, you'd need the size of the buffer, though, which you are not being given, so there's nothing you can do here (just be aware of this when writing your own code!).

`practice.h`

```C
void two_fer(char *buffer, size_t buffer_size, const char *name);
```

`functions.c`

```C
void two_fer(char *buffer, size_t buffer_size, const char *name){
    if(name == NULL || strlen(name) == 0){
        // Use snprintf to ensure we do not write beyong the buffer limits
        snprintf(buffer, buffer_size, "One for you, one for me.\n");
    }else{
        snprintf(buffer, buffer_size, "One for %s, one for me.\n", name);
    }
}
```

`practice.c`

```C
int main(){
    char name[80];
    char buffer[80];
    
    printf("Enter the name of the person: ");
    fgets(name, sizeof(name), stdin);
    
    name[strcspn(name, "\n")] = '\0';
    
    two_fer(buffer, sizeof(buffer), name);
    printf("%s", buffer);
}
```



```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter the name of the person: Do-yun
One for Do-yun, one for me.
chan@CMA:~/C_Programming/practice$ ./practice
Enter the name of the person: 
One for you, one for me.
```



---



## Raindrops

### Instruction

Raindrops is a slightly more complex version of the FizzBuzz challenge, a classic interview question.

Your task is to convert a number into its corresponding raindrop sounds.

If a given number:

- is divisible by 3, add "Pling" to the result.
- is divisible by 5, add "Plang" to the result.
- is divisible by 7, add "Plong" to the result.
- **is not** divisible by 3, 5, or 7, the result should be the number as a string.

### Examples

- 28 is divisible by 7, but not 3 or 5, so the result would be `"Plong"`.
- 30 is divisible by 3 and 5, but not 7, so the result would be `"PlingPlang"`.
- 34 is not divisible by 3, 5, or 7, so the result would be `"34"`.

```
A common way to test if one number is evenly divisible by another is to compare the remainder or modulus to zero. Most languages provide operators or functions for one (or both) of these.
```



### Solution

`practice.h`

```C
void convert(char result[], int drops);
```

`functions.c`

```C
void convert(char result[], int drops){
    // Initialize the result string to an empty string
    result[0] = '\0';
    
    // Check divisibility and concatenate the appropriate string
    if(drop % 3 == 0){
        strcat(result, "Pling");
    }
    if(drop % 5 == 0){
        strcat(result, "Plang");
    }
    if(drop % 7 == 0){
        strcat(result, "Plong");
    }
    
    // If result is still empty, the number is not divisible by 3, 5, or 7
    if(!result[0]){
        
        // Convert the number to string
        sprintf(result, "%d", drops);
    }
}
```

`practice.c`

```C
int main()
{
    char result[50];

    // Test cases
    int numbers[] = {28, 30, 34, 105, 1};
    int size = sizeof(numbers) / sizeof(numbers[0]);

    for (int i = 0; i < size; i++)
    {
        convert(result, numbers[i]);
        printf("The raindrop sound for %d is: %s\n", numbers[i], result);
    }
    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
The raindrop sound for 28 is: Plong
The raindrop sound for 30 is: PlingPlang
The raindrop sound for 34 is: 34
The raindrop sound for 105 is: PlingPlangPlong
The raindrop sound for 1 is: 1
```

#### Explanation:

**`convert(char result[], int drops)`**: This function takes a `result` string and an integer `drops`, and converts `drops` into its corresponding raindrop sounds.

- If the number is divisible by 3, the function adds `"Pling"` to the result string.
- If it's divisible by 5, it adds `"Plang"`.
- If it's divisible by 7, it adds `"Plong"`.
- If the number isn't divisible by any of these, it converts the number to a string and stores it in `result`.

**`strcat`**: This function is used to concatenate strings. It appends the given raindrop sound to the `result` string.

**`sprintf`**: If the number is not divisible by 3, 5, or 7, the number itself is converted to a string using `sprintf`.

---

## D & D Character

### Introduction

After weeks of anticipation, you and your friends get together for your very first game of [Dungeons & Dragons](https://en.wikipedia.org/wiki/Dungeons_%26_Dragons) (D&D). Since this is the first session of the game, each player has to generate a character to play with. The character's abilities are determined by rolling 6-sided dice, but where *are* the dice? With a shock, you realize that your friends are waiting for *you* to produce the dice; after all it was your idea to play D&D! Panicking, you realize you forgot to bring the dice, which would mean no D&D game. As you have some basic coding skills, you quickly come up with a solution: you'll write a program to simulate dice rolls.

### Instructions

For a game of [Dungeons & Dragons](https://en.wikipedia.org/wiki/Dungeons_%26_Dragons), each player starts by generating a character they can play with. This character has, among other things, six abilities; strength, dexterity, constitution, intelligence, wisdom and charisma. These six abilities have scores that are determined randomly. You do this by rolling four 6-sided dice and recording the sum of the largest three dice. You do this six times, once for each ability.

Your character's initial hitpoints are 10 + your character's constitution modifier. You find your character's constitution modifier by subtracting 10 from your character's constitution, divide by 2 and round down.

Write a random character generator that follows the above rules.

For example, the six throws of four dice may look like:

- 5, 3, 1, 6: You discard the 1 and sum 5 + 3 + 6 = 14, which you assign to strength.
- 3, 2, 5, 3: You discard the 2 and sum 3 + 5 + 3 = 11, which you assign to dexterity.
- 1, 1, 1, 1: You discard the 1 and sum 1 + 1 + 1 = 3, which you assign to constitution.
- 2, 1, 6, 6: You discard the 1 and sum 2 + 6 + 6 = 14, which you assign to intelligence.
- 3, 5, 3, 4: You discard the 3 and sum 5 + 3 + 4 = 12, which you assign to wisdom.
- 6, 6, 6, 6: You discard the 6 and sum 6 + 6 + 6 = 18, which you assign to charisma.

Because constitution is 3, the constitution modifier is -4 and the hitpoints are 6.

Note

Most programming languages feature (pseudo-)random generators, but few programming languages are designed to roll dice. One such language is [Troll](https://di.ku.dk/Ansatte/?pure=da%2Fpublications%2Ftroll-a-language-for-specifying-dicerolls(84a45ff0-068b-11df-825d-000ea68e967b)%2Fexport.html).



### Solution

`practice.h`

```C
typedef struct {
    int strength;
    int dexterity;
    int constitution;
    int intelligence;
    int wisdom;
    int charisma;
    int hitpoints;
} dnd_character_t;

int ability(void);
int modifier(int score);
dnd_character_t make_dnd_character(void);
```

`functions.c`

```C
// Functions to generate an abilty score by rolling 4 dices
int ability(void){
    int dice[4];
    int sum = 0, min = 7; // 7 is higher than the max roll (6)
    
    // Roll 4 dice
    for(int i = 0; i < 4; i++){
        // Generate random number between 1 and 6
        dice[i] = rand() % 6 + 1; 
        sum += dice[i];
        
        // Track the smallest dice and set that to "min"
        if(dice[i] < min){
            min = dice[i];
        }
    }
    
    // Discard the smallest dice by subtracting it from the total sum
    return sum - min;
}

int modifier(int score){
    return floor((score - 10) / 2.0);
}

dnd_character_t make_dnd_character(void){
    dnd_character_t character;
    
    // Generate scores for each ability
    character.strength = ability();
    printf("\n");
    character.dexterity = ability();
    printf("\n");
    character.constitution = ability();
    printf("\n");
    character.intelligence = ability();
    printf("\n");
    character.wisdom = ability();
    printf("\n");
    character.charisma = ability();
    printf("\n");
  
    // Calculate hitpoints (10 + constitution)
    character.hitpoints = 10 + modifier(character.constitution);
    
    return character;
}
```

- **`dice[4]`**: Array to store the results of rolling four dice.

- **`sum`**: Variable to store the sum of all dice rolls.

- **`min`**: Variable to store the smallest dice roll, initialized to 7 (since the maximum roll is 6).

- The loop iterates four times, simulating the roll of four dice.

- `rand() % 6 + 1` generates a random number between 1 and 6.

- `sum += dice[i]` adds the current dice roll to the sum.

- `if (dice[i] < min)`updates the `min` variable if the current dice roll is smaller than the current `min`.

- Why `sum - min` Yields the Sum Excluding the Smallest Roll

  - **Sum of All Rolls**: `sum`contains the total of all four dice rolls.
  - **Smallest Roll**: `min` contains the smallest value among the four dice rolls.

  - By subtracting the smallest roll (`min`) from the total sum (`sum`), we effectively get the sum of the remaining three dice rolls. 

- Consider the dice rolls: `2, 5, 4, 2`

  - **Sum**: `2 + 5 + 4 + 2 = 13`
  - **Smallest Roll (`min`)**: `2`
  - **Sum Excluding the Smallest Roll**: `13 - 2 = 11`

`practice.c`

```C
int main(){
    // Seed the random number generator
    srand(time(NULL));
    
    // Generate a random character
    dnd_character_t character = make_dnd_character();
    
    // Output the character's stats
    printf("Strength: %d\n", character.strength);
    printf("Dexterity: %d\n", character.dexterity);
    printf("Constitution: %d\n", character.constitution);
    printf("Intelligence: %d\n", character.intelligence);
    printf("Wisdom: %d\n", character.wisdom);
    printf("Charisma: %d\n", character.charisma);
    printf("Hitpoints: %d\n", character.hitpoints);

    return 0;
}
```

#### Explanation:

- **`ability()`**: Rolls four 6-sided dice, sums the highest three rolls, and returns the result.

- **`modifier()`**: Calculates the ability modifier by using the formula `(score - 10) / 2` and returns the rounded down result.

- **`make_dnd_character()`**: This function generates all six ability scores using the `ability()` function, calculates the hitpoints based on the constitution modifier, and returns a `dnd_character_t` structure.

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
dice[0] = 6
dice[1] = 4
dice[2] = 2
dice[3] = 3

dice[0] = 1
dice[1] = 1
dice[2] = 2
dice[3] = 4

dice[0] = 6
dice[1] = 2
dice[2] = 1
dice[3] = 4

dice[0] = 5
dice[1] = 2
dice[2] = 2
dice[3] = 5

dice[0] = 1
dice[1] = 3
dice[2] = 3
dice[3] = 4

dice[0] = 5
dice[1] = 3
dice[2] = 6
dice[3] = 1

Strength: 13
Dexterity: 7
Constitution: 12
Intelligence: 12
Wisdom: 10
Charisma: 14
Hitpoints: 11
```

#### Additional Note

The purpose of calling `srand(time(NULL))` in the `main` function is to seed the random number generator, which is used by `rand()` to produce pseudo-random numbers.

Here’s why it’s important:

- **`rand()`** generates a sequence of pseudo-random numbers based on an initial value called the "seed." If the seed is the same every time the program runs, `rand()` will produce the same sequence of numbers.
- **`srand(time(NULL))`** seeds the random number generator with the current time (in seconds). Since time constantly changes, it ensures that the sequence of numbers generated by `rand()` is different each time we run the program.

Without `srand(time(NULL))`, the program would always generate the same "random" results each time we run it, because `rand()` would start with the same default seed.

Example:

- Without `srand(time(NULL))`, we might get the same ability scores every time we run the program.
- With `srand(time(NULL))`, the ability scores will be different on each run, making the character generation truly random.

In summary, `srand(time(NULL))` makes sure that the random numbers are different each time the program is executed.



### Optimized Solution (Modified the previous solution)

#### What my mentor said regarding the solution

"Not really, your solution is the standard one. Maybe the floor could be optimized to only be used when score - 10 is negative and is odd, since you don't need it when it is positive(regular div already rounds to 0), but the branch could be counterproductive.

Also `rand() % 6` may not be uniform if 6 is not a divisor of RAND_MAX."

`practice.h`

```C
typedef struct
{
    int strength;
    int dexterity;
    int constitution;
    int intelligence;
    int wisdom;
    int charisma;
    int hitpoints;
} dnd_character_t;
int ability(void);
int modifier(int score);
dnd_character_t make_dnd_character(void);
```

`functions.c`

```C
int ability(void){
    int dice[4];
    int sum = 0, min = 7;
    
    for(int i = 0; i < 4; i++){
        int roll;
        do{
            roll = rand();
        }while (roll >= RAND_MAX - (RAND_MAX % 6));
        dice[i] = rand() % 6 + 1;
        sum += dice[i];
        if(dice[i] < min){
            min = dice[i];
        }
    }
    return sum - min;
}

int modifier(int score){
    int diff = score - 10;
    if(diff < 0 && diff % 2 != 0){
        return floor(diff / 2.0);
    }
    return diff / 2;
    // or use bitwise shift for division by 2
    // often more efficient because it directly manipulates the bits
    // return diff >> 1;
}

dnd_character_t make_dnd_character(void)
{
    dnd_character_t character;

    character.strength = ability();
    character.dexterity = ability();
    character.constitution = ability();
    character.intelligence = ability();
    character.wisdom = ability();
    character.charisma = ability();

    character.hitpoints = 10 + modifier(character.constitution);

    return character;
}
```

`practice.c`

```C
int main()
{
    srand(time(NULL));

    dnd_character_t character = make_dnd_character();

    printf("Strength: %d\n", character.strength);
    printf("Dexterity: %d\n", character.dexterity);
    printf("Constitution: %d\n", character.constitution);
    printf("Intelligence: %d\n", character.intelligence);
    printf("Wisdom: %d\n", character.wisdom);
    printf("Charisma: %d\n", character.charisma);
    printf("Hitpoints: %d\n", character.hitpoints);
    return 0;
}
```

1. **Uniform Distribution**: 

   - Uniform distribution is a type of probability distribution in which all outcomes are equally likely. 
   - In the context of random number generation, this means that each possible value within a specified range has an equal chance of being selected.
   - **Key Characteristics of Uniform Distribution:**
     1. **Equal Probability**: Every value within the range has the same probability of occurring.
     2. **Range**: The distribution is defined over a specific range, and values outside this range have a probability of zero.
     3. **Discrete vs. Continuous**: Uniform distribution can be either discrete (e.g., rolling a die) or continuous (e.g., generating a random floating-point number between 0 and 1).
   - Uniform distribution is crucial in random number generation to ensure fairness and unbiased results. 
   - If the distribution is not uniform, some values will be more likely to occur than others, leading to biased outcomes.
   - When using functions like `rand()` in C, special care must be taken to ensure uniform distribution, especially when using the modulo operation (`%`). 
   - The modulo operation can introduce bias if the range of possible values is not a multiple of the divisor.

   

2. **Improved Random Number Generation**:

   - The `ability` function now uses a loop to discard values that would cause bias. This ensures that the random numbers generated are uniformly distributed.

   - The loop continues to generate a random number until it gets a value that is within the range `[0, RAND_MAX - (RAND_MAX % 6))`. This ensures that the modulo operation does not introduce bias.

   - **Purpose of the Loop:** 

     - The purpose of the `do-while` loop is to ensure that the random number generated by `rand()` is uniformly distributed when using the modulo operation (`% 6`). 

     - This is necessary because `rand()` generates numbers in the range `[0, RAND_MAX]`, and if `RAND_MAX` is not a multiple of 6, using `rand() % 6` directly can introduce bias.

     - `RAND_MAX`: The maximum value returned by `rand()`.

     - `RAND_MAX % 6`: The remainder when `RAND_MAX` is divided by 6.

     - `RAND_MAX - (RAND_MAX % 6)`: The largest multiple of 6 that is less than or equal to `RAND_MAX`.

     - Let's assume `RAND_MAX` is 32767 (which is common in many implementations).

       - `RAND_MAX = 32767`
       - `RAND_MAX % 6 = 32767 % 6 = 5`
       - `RAND_MAX - (RAND_MAX % 6) = 32767 - 5 = 32762`

       This means the valid range for `roll` is `[0, 32761]`. If `roll` is 32762 or higher, it is discarded and a new random number is generated.

       Let's walk through an example with a few iterations of the loop.

       1. **First Iteration**:
          - `roll = rand() = 32760` (within the valid range)
          - Condition: `32760 >= 32762` is `false`
          - Exit the loop
       2. **Second Iteration**:
          - `roll = rand() = 32763` (outside the valid range)
          - Condition: `32763 >= 32762` is `true`
          - Continue the loop
       3. **Third Iteration**:
          - `roll = rand() = 32761` (within the valid range)
          - Condition: `32761 >= 32762` is `false`
          - Exit the loop

     

   - The `srand(time(NULL))` function call seeds the random number generator with the current time, ensuring that the sequence of random numbers generated by `rand()` is different each time the program runs. 
   - However, it does not ensure uniform distribution by itself. The uniform distribution is about ensuring that each possible outcome within a specified range has an equal probability of occurring.

   

3. **Optimized `modifier` Function**:

   - The `modifier` function now checks if `(score - 10)` is negative and odd. If so, it uses `floor`to round down. Otherwise, it uses integer division.
   - This optimization reduces the use of `floor` to only when necessary, potentially improving performance.





---

## Perfect Numbers

### Instructions

Determine if a number is perfect, abundant, or deficient based on Nicomachus' (60 - 120 CE) classification scheme for positive integers.

The Greek mathematician [Nicomachus](https://en.wikipedia.org/wiki/Nicomachus) devised a classification scheme for positive integers, identifying each as belonging uniquely to the categories of **perfect**, **abundant**, or **deficient** based on their [aliquot sum](https://en.wikipedia.org/wiki/Aliquot_sum). The aliquot sum is defined as the sum of the factors of a number not including the number itself. For example, the aliquot sum of 15 is (1 + 3 + 5) = 9

- **Perfect: aliquot sum = number**
- 6 is a perfect number because (1 + 2 + 3) = 6
  - 28 is a perfect number because (1 + 2 + 4 + 7 + 14) = 28

- **Abundant: aliquot sum > number**
- 12 is an abundant number because (1 + 2 + 3 + 4 + 6) = 16
  - 24 is an abundant number because (1 + 2 + 3 + 4 + 6 + 8 + 12) = 36

- **Deficient: aliquot sum < number**
  - 8 is a deficient number because (1 + 2 + 4) = 7
  - Prime numbers are deficient

Implement a way to determine whether a given number is **perfect**. Depending on your language track, you may also need to implement a way to determine whether a given number is **abundant** or **deficient**.

------

### Source

[Taken from Chapter 2 of Functional Thinking by Neal Ford.![The link opens in a new window or tab](https://assets.exercism.org/assets/icons/external-link-49b422d65245eaf4cb29a072e85d73b5c6b8aa04.svg)](https://www.oreilly.com/library/view/functional-thinking/9781449365509/)



### Solution

- To determine whether a given number is perfect, abundant, or deficient, we need to calculate the aliquot sum (the sum of the divisors of the number excluding the number itself) and compare it to the number.

#### Steps:

1. Calculate the sum of divisors of the number, excluding the number itself.
2. Compare the aliquot sum with the number:
   - If aliquot sum equals the number, it's perfect.
   - If aliquot sum is greater than the number, it's abundant.
   - If aliquot sum is less than the number, it's deficient.

`practice.h`

```C
typedef enum
{
    PERFECT_NUMBER = 1,
    ABUNDANT_NUMBER = 2,
    DEFICIENT_NUMBER = 3,
    ERROR = -1,
} kind;

kind classify_number(int number);
```

`functions.c`

```C
kind classify_number(int number) {
    if (number <= 0) {
        return ERROR;  // Aliquot sums are only defined for positive integers.
    }

    int aliquot_sum = 0;

    // Calculate aliquot sum (sum of divisors, excluding the number itself)
    for (int i = 1; i <= number / 2; i++) {
        if (number % i == 0) {
            aliquot_sum += i;
        }
    }

    // Classify the number based on its aliquot sum
    if (aliquot_sum == number) {
        return PERFECT_NUMBER;
    } else if (aliquot_sum > number) {
        return ABUNDANT_NUMBER;
    } else {
        return DEFICIENT_NUMBER;
    }
}

```

- The `for` loop iterates from 1 to `number / 2`. 
- This is because a proper divisor of a number `n` is any positive integer less than `n` that divides `n` without leaving a remainder. 
- The largest proper divisor of `n` is `n / 2`.
- For example, the proper divisors of 28 are 1, 2, 4, 7, and 14. The largest proper divisor is 14, which is `28 / 2`.
- **Efficiency**: By iterating only up to `number / 2`, the function avoids unnecessary checks and improves efficiency. Checking beyond `number / 2` would not yield any new proper divisors.

`practice.c`

```C
int main() {
    int number;
    printf("Enter a positive integer: ");
    scanf("%d", &number);

    kind result = classify_number(number);

    switch (result) {
        case PERFECT_NUMBER:
            printf("%d is a perfect number.\n", number);
            break;
        case ABUNDANT_NUMBER:
            printf("%d is an abundant number.\n", number);
            break;
        case DEFICIENT_NUMBER:
            printf("%d is a deficient number.\n", number);
            break;
        case ERROR:
        default:
            printf("Error: Please enter a positive integer.\n");
            break;
    }

    return 0;
}
```

`Output`

```sh
chan@CMA:~/C_Programming/practice$ ./practice
Enter a positive integer: 6
6 is a perfect number.

chan@CMA:~/C_Programming/practice$ ./practice
Enter a positive integer: 28
28 is a perfect number.

chan@CMA:~/C_Programming/practice$ ./practice
Enter a positive integer: 12
12 is an abundant number.

chan@CMA:~/C_Programming/practice$ ./practice
Enter a positive integer: 24
24 is an abundant number.

chan@CMA:~/C_Programming/practice$ ./practice
Enter a positive integer: 8
8 is a deficient number.

chan@CMA:~/C_Programming/practice$ ./practice
Enter a positive integer: 0
Error: Please enter a positive integer.

chan@CMA:~/C_Programming/practice$ ./practice
Enter a positive integer: -1
Error: Please enter a positive integer.
```

---



## High Scores Program

### Instructions

Manage a game player's High Score list.

Your task is to build a high-score component of the classic Frogger game, one of the highest selling and most addictive games of all time, and a classic of the arcade era. Your task is to write methods that return the highest score from the list, the last added score and the three highest scores.

The functions in this exercise accept an array containing one or more numbers, each representing one 'game score'.

The player's game scores can be read from

| `scores`                 |
| :----------------------- |
| `scores[0]`              |
| `scores[1]`              |
| ...                      |
| `scores[scores_len - 1]` |

`personal_top_three()` should write the player's top scores to

| `output`    |
| :---------- |
| `output[0]` |
| ...         |

------

### Source

Tribute to the eighties' arcade game Frogger



### Solution

`practice.h`

```C
```

`functions.c`

```C
```

`practice.c`

```C
```

