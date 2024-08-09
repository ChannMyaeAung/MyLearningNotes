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
```

`practice.c` (main)

```C
```

`Output`

```sh
```

