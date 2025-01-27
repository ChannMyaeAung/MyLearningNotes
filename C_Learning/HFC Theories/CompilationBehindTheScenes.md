#### Compilation Behind the scenes

1. **Preprocessing: fix the source**: The first thing the compiler needs to do is fix the source code. It needs to add in any extra header files it's been told about using the `#include` directive. It might also need to expand or skip over some sections of the program. 

2. **Compilation: translate into assembly:** The C programming language isn't low level enough for the computer to understand. The computer only really understands very low-level machine code instructions and the first step to generate machine code is convert the C source code into **assembly language symbols**.

   - Assembly language describes the individual instructions the central processor will have to follow when running the program.
   - The C compiler has a whole set of recipes for each of the different parts of the C language.
   - These recipes will tell the compiler to convert an `if` statement or a function call into a sequence of assembly language instructions. 
   - But even assembly isn't low level enough for the computer. That's why it needs..

3. **Assembly: generate the object code:** The compiler will need to assemble the symbol codes into machine or object code. This is the actual binary code that will be executed by the circuits inside the CPU.

   ```css
   10010101 00100101 11010101 01011100
   ```

   After all, we've taken the original C source code and converted it into the 1s and 0s that the computer's circuits need.

   - If we give the computer several files to compile for a program, the compiler will generate a piece of object code for each source file.
   - But in order for these separate object files to form a single executable program, one more thing has to occur.

   

4. **Linking: put it all together:** Once we have all of the separate pieces of object code, we need to fit them together like jigsaw pieces to form executable program. The compiler will connect the code in one piece of object code that calls a function in another piece of object code.

   - Linking will also make sure that the program is able to call library code properly.
   - The program will be written out into the executable program file using a format that is supported by the OS.
   - The file format is important because it will allow the OS to load the program into memory and make it run.

**So how do we tell `gcc` that we want to make one executable program from several separate source files?**

```bash
gcc message_hider.c encrypt.c -o message_hider
```

