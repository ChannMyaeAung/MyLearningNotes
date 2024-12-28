#### Exercise - 3

`./header_files/hfcal.h`

```C
void display_calories(float weight, float distance, float coeff);
```

``hfcal.c`

```C
#include <stdio.h>
#include "./header_files/hfcal.h"

void display_calories(float weight, float distance, float coeff){
    printf("Weight: %3.2f lbs\n", weight);
    printf("Distance: %3.2f miles\n", distance);
    printf("Calories burned: %4.2f cal\n", coeff * weight * distance);
}
```



`elliptical.c`

```C
#include <stdio.h>
#include "./header_files/hfcal.h"

int main(){
    display_calories(115.2, 11.3, 0.79);
    return 0;
}
```

- The test user weighs 115.2 pounds, and has done 11.3 miles on the elliptical.
- For this machines, the coefficient is 0.79.



Makefile

```makefile
all: elliptical

elliptical: elliptical.o libhfcal.a
	gcc elliptical.o -L./libs -lhfcal -o elliptical
	
elliptical.o: elliptical.c ./header_files/hfcal.h
	gcc -I./header_files -c elliptical.c -o elliptical.o
   
hfcal.o: hfcal.c ./header_files/hfcal.h
	gcc -I./header_files -c hfcal.c -o hfcal.o

libhfcal.a: hfcal.o
	ar -rcs ./libs/libhfcal.a hfcal.o
```

- When creating `hfcal.o`, the `hfcal.c` program needs to know where the header file is. That's why we have to specify the correct pathnames for it. In this case since I am saving the header files in a directory called `header_files` so I have to specify it like `./header_files/`.
- `-c` means "just create the object file; don't link it."
- `libhfcal.a`: `./libs/` means the archive needs to go into the `./libs` directory. The directory must be created first otherwise the compiler will complain. `libhfcal.a` The library needs to be named `lib... .a`. So, the correct command would be `./libs/libhfcal.a`.
- When creating the final executable called `elliptical`, we have to include the necessary object files, in this case `elliptical.o`, followed by `-L./libs` which tells the compiler where the library is stored, followed by `-lhfcal` which tells the compiler to look for `libhfcal.a` and then followed by the output command and the final executable name, in this case `-o elliptical`.
- We are building the program using `elliptical.o` and the library.



Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
$ make all
ar -rcs ./libs/libhfcal.a hfcal.o
gcc elliptical.o -L./libs -lhfcal -o elliptical

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
$ ./elliptical
Weight: 115.20 lbs
Distance: 11.30 miles
Calories burned: 1028.39 cal

```

But what if we want to output it in kilograms and kilometres.





#### Compiling the elliptical program with dynamic libraries

- Once we've created the dynamic library, we can use it just like a static library. 

```makefile
gcc -shared hfcal.o -o ./libs/libhfcal.so
gcc -I./header_files -c elliptical.c -o elliptical.o
gcc elliptical.o -L./libs -lhfcal -o elliptical
```

- Even though these are the same commands we would use if `hfcal` were a static archive, the compile will work differently.
- Because the library is dynamic, the compiler won't include the library code into the executable file. Instead, it will insert some placeholder code that will track down the library and link to it at runtime.



Makefile

```makefile
all: elliptical

elliptical: elliptical.o libhfcal.so  
	gcc elliptical.o -L./libs -lhfcal -Wl,-rpath,./libs -o elliptical 
	
elliptical.o: elliptical.c ./header_files/hfcal.h
	gcc -I./header_files -c elliptical.c -o elliptical.o

hfcal.o: hfcal.c ./header_files/hfcal.h
	gcc -I./header_files -c hfcal.c -o hfcal.o

libhfcal.so: hfcal.o 
	gcc -shared hfcal.o -o ./libs/libhfcal.so
```



**Important:**

- When we link against a shared library, the runtime linker needs to be able to find that library at runtime. 

- By default, it looks in several standard locations (like `/lib` and `/usr/lib`) but it won't look in your `./libs` directory.

- We can tell the runtime linker to look in `./libs` by setting the `LD_LIBRARY_PATH` environment variable to include `./libs`. 

  ```makeflie
  export LD_LIBRARY_PATH=./libs:$LD_LIBRARY_PATH
  ./elliptical
  ```

  This command will add `./libs` to the list of directories that the runtime linker searches. The `:$LD_LIBRARY_PATH` part ensures that the linker will still search all the standard locations as well.

- If we want to avoid having to set `LD_LIBRARY_PATH` every time, we can add `-Wl,-rpath,./libs` option when we link `elliptical` .

- This will embed `./libs` as a runtime search path in the `elliptical` executable itself.

- There's no need to do this if the library is somewhere standard, like `/usr/lib`.



Code Execution: 

Before adding `-Wl,-rpath,./libs`, it is resulting in error. 

```bash
chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
$ make all
gcc -shared hfcal.o -o ./libs/libhfcal.so
gcc elliptical.o -L./libs -lhfcal -o elliptical

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
$ ./elliptical
./elliptical: error while loading shared libraries: libhfcal.so: cannot open shared object file: No such file or directory

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
$ make all
gcc -shared hfcal.o -o ./libs/libhfcal.so
gcc elliptical.o -L./libs -lhfcal -Wl,-rpath,./libs -o elliptical 

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
$ ./elliptical
Weight: 115.20 lbs
Distance: 11.30 miles
Calories burned: 1028.39 cal

```

Another way

```bash
chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
$ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:./libs
chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3$ ./elliptical
Weight: 115.20 lbs
Distance: 11.30 miles
Calories burned: 1028.39 cal
```



What if we want to ship our gym equipment to the UK and we have to display it in kilograms and kilometres.

`hfcal_UK.c`

```C
#include <stdio.h>
#include "./header_files/hfcal.h"

void display_calories_UK(float weight, float distance, float coeff)
{
    printf("Weight: %3.2f kg\n", weight / 2.2046);
    printf("Distance: %3.2f km\n", distance * 1.609344);
    printf("Calories burned: %4.2f cal\n", coeff * weight * distance);
}
```

`hfcal.h`

```C
void display_calories(float weight, float distance, float coeff);
void display_calories_UK(float weight, float distance, float coeff);
```



`elliptical.c`

```C
#include <stdio.h>
#include "./header_files/hfcal.h"

int main()
{
    puts("US elliptical:");
    display_calories(115.2, 11.3, 0.79);
    puts("UK elliptical:");
    display_calories_UK(115.2, 11.3, 0.79);
    return 0;
}
```

Makefile

```makefile
all: elliptical

elliptical: elliptical.o libhfcal.so  
	gcc elliptical.o -L./libs -lhfcal -Wl,-rpath,./libs -o elliptical 
	
elliptical.o: elliptical.c ./header_files/hfcal.h
	gcc -I./header_files -c elliptical.c -o elliptical.o

hfcal.o: hfcal.c ./header_files/hfcal.h
	gcc -I./header_files -c hfcal.c -o hfcal.o

hfcal_UK.o: hfcal_UK.c ./header_files/hfcal.h 
	gcc -I./header_files -c hfcal_UK.c -o hfcal_UK.o 

libhfcal.so: hfcal.o hfcal_UK.o
	gcc -shared hfcal.o hfcal_UK.o -o ./libs/libhfcal.so
```



Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
$ make all

gcc -I./header_files -c hfcal_UK.c -o hfcal_UK.o 
gcc -shared hfcal.o hfcal_UK.o -o ./libs/libhfcal.so
gcc elliptical.o -L./libs -lhfcal -Wl,-rpath,./libs -o elliptical 

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
$ ./elliptical

US elliptical:
Weight: 115.20 lbs
Distance: 11.30 miles
Calories burned: 1028.39 cal
UK elliptical:
Weight: 52.25 kg
Distance: 18.19 km
Calories burned: 1028.39 cal

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3$ 

```



### Suggestions for Improvement

1. **Directory Structure**: It's a good practice to place your object files in a separate directory (e.g., `obj`) and your source files in another (e.g., `src`). This keeps your project organized.

2. **Compiler Flags**: Consider adding compiler flags for warnings and debugging. For example:

   ```makefile
   CFLAGS = -Wall -Wextra -g
   ```

3. **Phony Targets**: Add `.PHONY` to indicate non-file targets to avoid conflicts with files named `all` or `clean`:

   ```makefile
   .PHONY: all clean
   ```

4. **Clean Target**: Add a `clean` target to remove compiled files and the executable:

   ```makefile
   clean:
       rm -f *.o ./libs/libfunctions.a practice
   ```

5. **Library Path**: Use a variable for the library path to make it easier to change:

   ```makefile
   LIBDIR = ./libs
   ```

### Updated Makefile

Here’s an updated version of your Makefile with these improvements:

```makefile
CC = gcc
CFLAGS = -Wall -Wextra -g
LIBDIR = ./libs

all: practice

practice: practice.o $(LIBDIR)/libfunctions.a
	$(CC) practice.o -L$(LIBDIR) -lfunctions -o practice

practice.o: practice.c practice.h
	$(CC) $(CFLAGS) -c practice.c

functions.o: functions.c practice.h
	$(CC) $(CFLAGS) -c functions.c

functions_2.o: functions_2.c practice.h
	$(CC) $(CFLAGS) -c functions_2.c

$(LIBDIR)/libfunctions.a: functions.o functions_2.o
	mkdir -p $(LIBDIR)
	ar -rcs $(LIBDIR)/libfunctions.a functions.o functions_2.o

.PHONY: clean
clean:
	rm -f *.o $(LIBDIR)/libfunctions.a practice
```

### Explanation

1. **Variables**: Using `CC` for the compiler and `CFLAGS` for compiler flags makes it easy to update these settings.
2. **Library Path**: The `LIBDIR` variable centralizes the library path, making it easy to change.
3. **Phony Target**: `.PHONY: clean` ensures the `clean` target is always run when called.
4. **Clean Target**: The `clean` target removes object files, the library, and the executable.



