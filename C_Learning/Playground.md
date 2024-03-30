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



