### Chapter 8 - Static and dynamic libraries

#### Hot-swappable code



#### Explanation of Encrypt function and checksum function

encrypt.c

```C
#include "encrypt.h"

void encrypt(*message){
    while(*message){
        *message = *message ^ 31;
        message++;
    }
}
```

#### What it does:

- The `encrypt` function performs a simple bitwise XOR encryption on a string.
- It iterates through each character in the `message` string and applies the XOR operation with the value `31`.

#### How it works:

1. **Pointer to the message**: The function takes a pointer to a character array (`char *message`), which is the string to be encrypted.
2. **Loop through the string**: The `while (*message)` loop continues until it reaches the null terminator (`'\0'`) of the string.
3. **Bitwise XOR operation**: Inside the loop, the statement `*message = *message ^ 31;` takes the current character, performs a bitwise XOR with `31`, and stores the result back in the same character position.
4. **Move to the next character**: The `message++` statement moves the pointer to the next character in the string.

#### Example:

If the original message is "abc", the ASCII values are 97, 98, and 99. After the XOR operation with 31, the values become:

- 'a' (97) ^ 31 = 126 (ASCII for '~')
- 'b' (98) ^ 31 = 125 (ASCII for '}')
- 'c' (99) ^ 31 = 124 (ASCII for '|')

So, "abc" would be encrypted to "~}|".



checksum.c

```C
#include "checksum.h"

int checksum(char *message){
    int c = 0;
    while(*message){
        c += c ^ (int)(*message);
        message++;
    }
    return c;
}
```

#### What it does:

- The `checksum` function calculates a checksum value for the string using a combination of addition and bitwise XOR operations.

#### How it works:

1. **Pointer to the message**: The function takes a pointer to a character array (`char *message`), which is the string to calculate the checksum for.

2. **Initialize checksum**: An integer variable `c` is initialized to `0`.

3. **Loop through the string**: The `while (*message)` loop continues until it reaches the null terminator (`'\0'`) of the string.

4. Calculate checksum

   : Inside the loop, the statement 

   ```
   c += c ^ (int)(*message);
   ```

    does the following:

   - It converts the current character to its integer (ASCII) value.
   - It performs a bitwise XOR operation between the current checksum value `c` and the integer value of the character.
   - It adds the result of the XOR operation to `c`.

5. **Move to the next character**: The `message++` statement moves the pointer to the next character in the string.

6. **Return checksum**: The function returns the final checksum value `c`.

#### Example:

For the string "abc":

- 'a' (97): `c += c ^ 97` when `c` is 0 -> `c = 0 + (0 ^ 97) = 97`
- 'b' (98): `c += c ^ 98` when `c` is 97 -> `c = 97 + (97 ^ 98) = 97 + 3 = 100`
- 'c' (99): `c += c ^ 99` when `c` is 100 -> `c = 100 + (100 ^ 99) = 100 + 7 = 107`



```markdown
   01100001  (97)
^  01100010  (98)
-----------
   00000011  (3)

```



### Bit-by-Bit Explanation

1. The first bit (leftmost) of both numbers is `0`. XOR of `0` and `0` is `0`.
2. The second bit of both numbers is `1`. XOR of `1` and `1` is `0`.
3. The third bit of both numbers is `1`. XOR of `1` and `1` is `0`.
4. The fourth bit of both numbers is `0`. XOR of `0` and `0` is `0`.
5. The fifth bit of both numbers is `0`. XOR of `0` and `0` is `0`.
6. The sixth bit of the first number is `0` and the second number is `1`. XOR of `0` and `1` is `1`.
7. The seventh bit of the first number is `1` and the second number is `0`. XOR of `1` and `0` is `1`.
8. The eighth bit of both numbers is `1`. XOR of `1` and `1` is `0`.

Combining these results, we get `00000011` in binary, which is `3` in decimal.

So, the XOR operation `97 ^ 98` results in `3`.

So, the checksum for "abc" would be 107.



#### Angle brackets are for standard headers

##### Where are the standard header directories?

So, if you include headers using angle brackets, where does the compiler go searching for the header files? 

You'll need to check the documentation that came with your compiler, but typically on a Unix-style system like the Mac or a Linux machine, the compiler will look for the files under these directories:

`/usr/local/include`

`/usr/include`

It will check `/usr/local/include` first.  `/usr/local/include` is often used for header files for third-party libraries.

`/usr/include` is normally used for OS headerfiles.



#### Sharing .h header files

1. **Store them in a standard directory.**

   If you copy your header files into one of the standard directories like `/usr/local/include`, you can include them in your source code using angle brackets.

   ```C
   #include <encrypt.h>
   
   // You can use angle brackets if your header files are in a standard directory.
   ```

2. **Put the full pathname in your include statement.**

   If you want to store your header files somewhere else, such as  `/my_header_files`, you can add the directory name to your include statement.

   ```C
   #include "/my_header_files/encrypt.h"
   ```

3. **You can tell the compiler where to find them.**

   The final option is to tell the compiler where it can find your header files. You can do this with the `-I` option on `gcc`:

   ```bash
   gcc -I/my_header_files test_code.c ... -o test_code
   
   // This tells the compiler to look in /my_header_files as well as the standard directories.
   ```

   - The `-I` option tells the `gcc` compiler that there's another place where it can find header files. It will still search in all the standard places, but first it will check the directory names in the `-I` option.



#### Share .o object files by using the full pathname

We can always put our .o object files into some sort of shared directory. Once we have done that, we can then jut add the full path to the object files when we're compiling the program that uses them:

```makefile
gcc -I/my_header_files test_code.c /my_object_files/encrypt.o /my_object_files/checksum.o -o test_code
```



- Using the full pathname to the object files means we don't need a separate copy for each C project.
- /my_object_files is like a central store for our project files.



Makefile

```makefile
all: final

final: object_files/encrypt.o object_files/checksum.o object_files/main.o
	@echo "Compiling the final exe file..."
	gcc object_files/encrypt.o object_files/checksum.o object_files/main.o -o final

main.o: main.c
	@echo "Compiling the main file..."
	gcc -I/header_files -c main.c -o object_files/main.o

encrypt.o: encrypt.c 
	@echo "Compiling the encrypt file..."
	gcc -I/header_files -c encrypt.c -o object_files/encrypt.o 

checksum.o: checksum.c 
	@echo "Compiling the checksum file..."
	gcc -I/header_files -c checksum.c -o object_files/checksum.o

clean: 
	@echo "Removing everything except the source files..."
	@rm object_files/encrypt.o object_files/checksum.o object_files/main.o final

```

header_files

- `encrypt.h`
- `checksum.h`

object_files

- `main.o`
- `encrypt.o`
- `checksum.o`

encrypt.c

```C
#include "../header_files/encrypt.h"

void encrypt(char *message){
    while(*message){
        *message = *message ^ 31;
        message++;
    }
}
```

checksum.c

```C
#include "../header_files/checksum.h"

int checksum(char *message){
    int c = 0;
    while(*message){
        c += c ^ (int)(*message);
        message++;
    }
    return c;
}
```



If we compile our code with the full pathname to the object files we want to use, then all our C programs can share the same `encrypt.o` and `checksum.o` files.

```markdown
Hmm.. That's OK if I just have one or two object files to share, but what if I have alot of object files? I wonder if there's some way of telling the compiler about a bunch of them...
```



**Yes, if we create an archive of object files, we can tell the compiler about a whole set of object files all at once.**

An archive is just a bunch of object files wrapped up into a single file. By creating a single archive file of all of your security code, you can make it a lot easier to share the code between projects.



#### An archive contains .o files

Ever used a .zip or .tar file? Then you know how easy it is to create a file that contains other files. That's exactly what a .a archive file is: **a file containing other files.**



```bash
chan@CMA:~$ 
cd /usr/local/lib

chan@CMA:/usr/local/lib
$ ls

libcs50.a  libcs50.so  libcs50.so.11  libcs50.so.11.0.2  python3.10

chan@CMA:/usr/local/lib
$ nm libcs50.a

libcs50.o:
0000000000000000 b allocations
                 U atexit
                 U __ctype_b_loc
                 U __errno_location
                 U fgetc
                 U free
00000000000003ce T get_char
00000000000004ee T get_double
00000000000006fd T get_float
00000000000008fd T get_int
0000000000000aa3 T get_long
0000000000000c3d T get_long_long
0000000000000000 T get_string
                 U __isoc99_sscanf
                 U realloc
0000000000000e3a t setup
                 U setvbuf
                 U __stack_chk_fail
                 U stdin
                 U stdout
                 U strcspn
0000000000000008 b strings
                 U strlen
                 U strtod
                 U strtof
                 U strtol
                 U strtoll
0000000000000dd7 t teardown
                 U ungetc
                 U vprintf
chan@CMA:/usr/local/lib$ 

```



The `nm` command lists the **names** that are stored inside the archive.





#### Create an archive with the `ar` command

The `archive comand (ar)` will store a set of object files in an archive file:

```makefile
ar -rcs libhfsecurity.a encrypt.o checksum.o
```

- `-rcs` - The `r` means the .a file will be updated if it already exists. The `c` means that the archive will be created without any feedback. The `s` tells `ar` to create an index at the start of the .a file.
- `libhfsecurity.a` - This is the name of the .a file to create.
- The last two files are the files that will be stored in the archive.

Did you notice that all of the .a files have names like `lib<something>.a`? That's the standard way of naming archives. The names begin with `lib` because they are `static libraries`. 

```markdown
**Make sure you always name your archives lib<something>.a.
If you don't name them this way, your compiler will have problems tracking them down.
```

#### then store the .a in a library directory

Once you have an archive, you can store it in a library directory. Which library directory should you store it in? It's up to you, but you have a couple of choices:

1. **You can put your .a file in a standard directory like `/usr/local/lib`.**

   Some coders like to install archives into a standard directory once they are sure it's working. On Linux, on Mac and in Cygwin, the `/usr/local/lib` directory is a good choice because that's the directory set aside for your own local custom libraries.

2. **Put the .a file in some other directory.**

   If you are still developing your code, or if you don't feel comfortable installing your code in a system directory, you can always create your own library directory. For example, `/my_lib`.

   - On most machines, you need to be an administrator to put files in `/usr/local/lib`.

#### Finally, compile your other programs

The whole point of creating a library archive was so you could use it with other programs. If you've installed your archive in a standard directory, you can compile your code using the `-l` switch:

```bash
gcc test_code.c -lhfsecurity -o test_code
```

- `test_code.c` - Remember to list your source files before your `-l` libraries.
- `-lhfsecurity` - hfsecurity tells the compiler to look for an archive called `libhfsecurity.a`.
- If you're using several archives, you can set several `-l` options.
- Do you need an `-I` option? It depends on where you put your headers.

Can you see why it's so important to name your archive `lib<someting>.a`?

```markdown
The name that follows the -l option needs to match part of the archive name. So if your archive is called libawesome.a, you can compile your program with the -lawesome switch.
```

But what if you put your archive somewhere else, like `/my_lib`? In that case, you will need to use the `-L` option to say which directories to search:

```bash
gcc test_code.c -L/my_lib -lhfsecurity -o test_code
```



#### Chapter - 8 Bullet points

- Headers in angle brackets (< >) are read from the standard directories.
- Examples of standard header directories are `/usr/include` and `C:/MinGW/include`.
- A library archive contains several object files.
- You can create an archive with `ar -rcs libarchive.a file0.o file1.o...`.
- Library archive names should begin `lib` and end `.a`.
- If you need to link to an archive called `libfred.a`, use `-lfred`.
- The `-L` flag should appear after the source files in the `gcc` command.



Q: How do I know what the standard library directories are on my machine?

A: You need to check the documentation for your compiler. On most Unix-style machines, the library directories include `/usr/lib` and `/usr/local/lib`.



Q: When I try to put a library archive into my `/usr/lib` directory, it won't let me. Why is that?

A: Almost certainly security. Many OS will prevent you from writing files to the standard directories in case you accidentally break one of the existing libraries.



Q: is the `ar` format the same on all systems?

A: No. Different platforms can have slightly different archive formats. And the object code the archive contains will be completely different for different OS.



Q: If I've created a library archive, can I see what's inside it?

A: Yes. `ar -t <filename>` will list the contents of the archive.



Q: Are the object files in the archive linked together like an executable?

A: No. The object files are stored in the archive as distinct files.



Q: Can I put any kind of file in a library archive?

A: No. The `ar` command will check the file type before including it.



Q: Can I extract a single object file from an archivee?

A: Yes. To extract the `encrypt.o` file from `libhfsecurity.a` , use `ar -x libhfsecurity.a encrypt.o`.



Q: Why is it called "static" linking?

A: Because it can't change once it's been done. When two files are linked together statically, it's like mixing coffee with milk: you can't separate them afterwards.



An object file might need to call a function that's stored in some other file. The Linker links together the point in one file where the function call is made to the point in another file where the function lives.



```markd
We've already seen that we can build programs using different pieces of object code. We've created .o files and .a archives and we've linked them together.
but once they're linked, we can't change them as they are static.
Once we've created a single executable file from those separate pieces of object code, we really have no way of changing any of the ingredients without rebuilding the whole program.
```



#### Dynamic linking happens at runtime

The reason we can't change the different pieces of object code in an executable file is because they are all contained in a single file. They were statically linked together when the program was compiled.

But if our program wasn't just a single file - if our program was made up of lots of separate files that only joined together when the program was run- we would avoid the problem.



```markdo
The trick, then, is to find a way of storing pices of object code in separate files and then dynamically linking them together only when the program runs.
```



#### Dynamic libraries are object files on steroids

- Dynamic libraries are similar to those .o object files but they are not quite the same.
- Like an archive file, a dynamic library can be built from several .o object files, but unlike an archive, the object files are properly linked together in a dynamic library to form a single piece of object code.
- A dynamic library contains extra information that the OS will need to link the library to other things.
- At the heart of a dynamic library is a single piece of object code.
- The library is built from one or more .o files.



#### How do we create our own dynamic libraries

#### First, create an object file

If we're going to convert the `hfcal.c` code into a dynamic library, then we need to begin by compiling it into a .o object file, like this:

**The exercise is in `HFCExercises.md`.**

```makefille
gcc -I/header_files -fPIC -c hfcal.c -o hfcal.o
```

- `-I/header_files` - The `hfcal.h` header is in `./header_files`.
- `-c` - means "Don't link the code".
- `-fPIC`- This tells `gcc` that we want to create `position-independent code`.
- Position-independent code can be moved around in memory.
- Some OS and processors need to build libraries from position-independent code so that they can decide at runtime where they want to load it in memory.
- The truth is that on most systems we don't need to specify this option.

```markd
What is position-independent code?

Position-independent code is code that doesn't mind where the computer loads it into memory. Imagine you had a dynamic library that expected to find the value of some piece of global data 500 bytes away from where the library is loaded. Bad things would happen if the OS decided to load the library somewhere else in memory. If the compiler is told to create position-independent code, it will avoid problems like this.

Some OS, like Windows, use a technique called memory mapping when loading dynamic libraries, which means all code is effectively position-independent. If you compile your code on Windows, you might find that gcc will give you a warning that the -fPIC option is not needed.
```



#### What you call your dynamic library depends on your platform

- Dynamic libraries are available on most OS and they all work in pretty much the same way but what they're called can vary a lot.
- On Windows, dynamic libraries are usually called `dynamic link libraries` and they have the extension `.dll`.
- On Linux and Unix, they're `shared object files (.so)`.
- On Mac, they're just called `dynamic libraries (.dylib)`.

```makefile
gcc -shared hflocal.o -o /libs/libhfcal.so
```

- `C:\libs\hfcal.dll` - MinGW on Windows
- `/libs/libhfcal.dll.a` - Cygwin on Windows
- `/libs/libhfcal.dylib` - Mac



```markdo
The -shared option tells gcc that we want to convert a .o object file into a dynamic library.
When the compiler creates the dynamic library, it will store the name of the library inside the file.
So, if you create a library called libhfcal.so on a Linux machine, the libhfcal.so file will remember that its library name is hfcal.
Why is that important? It means that if we compile a library with one name, we can't just rename the file afterwards.
If we need to rename a library, recompile it with the new name.
```



#### Compiling the elliptical program from `HFCExercises.md`

- Once we've created the dynamic library, we can use it just like a static library. 

```makefile
gcc -I./header_files -c elliptical.c -o elliptical.o
gcc elliptical.o -L./libs -lhfcal -o elliptical
```

- Even though these are the same commands we would use if `hfcal` were a static archive, the compile will work differently.
- Because the library is dynamic, the compiler won't include the library code into the executable file. Instead, it will insert some placeholder code that will track down the library and link to it at runtime.



If we want to store it in standard libraries directory,

```makefile
gcc -shared hfcal.o -o /libs/libhfcal.so

gcc -I\include -c elliptical.c -o elliptical.o

gcc elliptical.o -L\libs -lhfcal -o elliptical
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

  ```bash
  chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
  $ export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:./libs
  chan@CMA:~/C_Programming/HFC/chapter_8/exercise_3
  $ ./elliptical
  Weight: 115.20 lbs
  Distance: 11.30 miles
  Calories burned: 1028.39 cal
  ```

  

  This command will add `./libs` to the list of directories that the runtime linker searches. The `:$LD_LIBRARY_PATH` part ensures that the linker will still search all the standard locations as well.

- If we want to avoid having to set `LD_LIBRARY_PATH` every time, we can add `-Wl,-rpath,./libs` option when we link `elliptical` .

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

  

- This will embed `./libs` as a runtime search path in the `elliptical` executable itself.



#### Bullet Points - Chapter 8

- Dynamic libraries are linked to programs at run time.
- Dynamic libraries are created from one or more object files.
- The `-shared` compiler option creates a dynamic library.
- Dynamic libraries have different names on different systems.
- The `ar` command creates a library archive of object files.
- `# include <>` looks in standard directories such as `/usr/include`.
- `gcc -shared` converts object files into dynamic libraries.
- Dynamic libraries have `.so`, `.dylib`, `.dll`, or `.dll.a` extensions.
- `-l<name>` links to a file in standard directories such as `/usr/lib`.
- `-I<name>` adds a directory to the list of standard include directories.
- `-L<name>` adds a directory to the list of standard library directories.
- Library archives have names like `libsomething.a`.
- Library archives are statically linked.



Q: Why are dynamic libraries so different on different OS?

A: OS like to optimize the way they load dynamic libraries, so they've each evolved different requirements for dynamic libraries.



Q: I tried to change the name of my library by renaming the file, but the compiler couldn't find it anymore. Why not?

A: When the compiler creates a dynamic library, it stores the name of the library inside the file. If you rename the file, it will then have the wrong name inside the file and will get confused. If you want to change its name, you should recompile the library.



Q: Why doesn't Linux just store library pathnames in executables? That way, you wouldn't need to set `LD_LIBRARY_PATH`?

A: It was a design choice. By not storing the pathname, it gives you a lot more control over which version of a library a program can use--- which is great when you're developing new libraries.



Q: Which is better? Static or dynamic linking?

A: It depends. Static linking means you get small, fast executable file that is easier to move from machine to machine. Dynamic linking means that you can configure the program at run time more.



Q: If different programs use the same dynamic library, does it get loaded more than once? Or is it shared in memory?

A: That depends on the OS. Some OS will load separate copies for each process. Others load shared copies to save memory.



Q: Are dynamic libraries the best way of configuring an application?

A: Usually, it's simpler to use configuration files. But if you're going to connect to some external device, you'd normally need separate dynamic libraries to act as drivers.

In C, both `strstr` and `strchr` are functions used to search within strings, but they have different purposes and behaviors:

### `strstr`

- **Prototype**: `char *strstr(const char *haystack, const char *needle);`
- **Purpose**: Finds the first occurrence of the substring `needle` in the string `haystack`.
- **Returns**: A pointer to the beginning of the found substring in `haystack`, or `NULL` if the substring is not found.

**Example**:

```C
#include <stdio.h>
#include <string.h>

int main() {
    const char *haystack = "hello world";
    const char *needle = "world";
    char *result = strstr(haystack, needle);

    if (result) {
        printf("Found: %s\n", result);  // Output: "Found: world"
    } else {
        printf("Not found.\n");
    }

    return 0;
}
```

### `strchr`

- **Prototype**: `char *strchr(const char *str, int c);`
- **Purpose**: Finds the first occurrence of the character `c` in the string `str`.
- **Returns**: A pointer to the first occurrence of the character in the string, or `NULL` if the character is not found.

**Example**:

```C
#include <stdio.h>
#include <string.h>

int main() {
    const char *str = "hello world";
    char ch = 'o';
    char *result = strchr(str, ch);

    if (result) {
        printf("Found: %s\n", result);  // Output: "Found: o world"
    } else {
        printf("Not found.\n");
    }

    return 0;
}
```

### Key Differences

1. **Search Type**:
   - `strstr`: Searches for a substring.
   - `strchr`: Searches for a single character.
2. **Return Value**:
   - `strstr`: Returns a pointer to the beginning of the found substring or `NULL` if the substring is not found.
   - `strchr`: Returns a pointer to the first occurrence of the character or `NULL` if the character is not found.
3. **Input Parameters**:
   - `strstr`: Takes two `const char *` parameters (strings).
   - `strchr`: Takes a `const char *` (string) and an `int` (character, which is internally converted to `char`).
4. **Common Use Cases**:
   - `strstr`: Used when you need to find a specific sequence of characters (substring) within another string.
   - `strchr`: Used when you need to find the position of a specific character within a string.

Both functions are part of the C standard library and can be found in the `<string.h>` header.



### Static vs. Dynamic Libraries in C

In C programming, libraries are used to share code across multiple programs. There are two primary types of libraries: static libraries and dynamic libraries. Here’s a detailed comparison of the two:

### Static Libraries

1. **Definition**:
   - A static library is a collection of object files that are linked into the program at compile-time. The code from the static library becomes part of the executable.
2. **File Extension**:
   - Typically has a `.a` extension on Unix/Linux (e.g., `libmylib.a`).
3. **Linking**:
   - Linked at compile-time. The linker copies the code from the static library into the executable.
4. **Advantages**:
   - **Portability**: The executable is self-contained and can run on any system without needing the library files.
   - **Performance**: Since all code is included in the executable, there’s no need to load external libraries at runtime.
5. **Disadvantages**:
   - **Size**: Executables can become very large because they include all the library code.
   - **Updates**: If a library is updated, each program that uses it must be recompiled.

### Dynamic Libraries

1. **Definition**:
   - A dynamic library (also known as a shared library) is a collection of object files that are linked into the program at runtime. The library code is not included in the executable but is loaded as needed.
2. **File Extension**:
   - Typically has a `.so` extension on Unix/Linux (e.g., `libmylib.so`).
3. **Linking**:
   - Linked at runtime. The operating system loads the dynamic library when the executable is run.
4. **Advantages**:
   - **Smaller Executables**: The executable doesn’t include the library code, so it’s smaller.
   - **Easy Updates**: Updating the library does not require recompiling the executables that use it.
5. **Disadvantages**:
   - **Dependency Management**: The correct version of the dynamic library must be present on the system.
   - **Performance**: Loading the library at runtime can introduce some overhead.

### Creating a Static Library in C

To create a static library, you need one or more object files. Here’s how you can create and use a static library:

1. **Compile Source Files to Object Files**:

   ```bash
   gcc -c file1.c
   gcc -c file2.c
   ```

2. **Create the Static Library**:

   ```bash
   ar rcs libmylib.a file1.o file2.o
   ```

3. **Link the Static Library to Your Program**:

   ```bash
   gcc -o myprogram main.c -L. -lmylib
   ```

In this example:

- `file1.c` and `file2.c` are compiled into object files `file1.o` and `file2.o`.
- These object files are archived into a static library `libmylib.a` using the `ar` command.
- The program `main.c` is compiled and linked with the static library `libmylib.a`.

### Summary

- **Static Libraries**: Linked at compile-time, larger executable size, easier to manage for distribution, but harder to update.
- **Dynamic Libraries**: Linked at runtime, smaller executable size, easier to update, but requires careful management of dependencies.

