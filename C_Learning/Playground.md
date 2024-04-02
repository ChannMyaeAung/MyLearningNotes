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

