#### How do we tell `gcc` to save the object code in a file? And how do we then get the compiler to link the object files together?

---

##### First, compile the source into object files

You want object code for each of the source files, and you can do that by typing this command:

```bash
gcc -c *.c
//This will create object code for every file.
// The OS will replace *.c with all the C filenames.
```



The `*.c` will match every C file in the current directory, and the `-c` will tell the compiler that you want to create an object file for each source file, but you don't want to link them together into a full executable program.



##### Then, link them together

Now that you have a set of object files, you can link them together with a simple compile command. But instead of giving the compiler the names of the C source files, you tell the names of the object files:

```bash
gcc *.o -o launch

```

The compiler is smart enough to recognize the files as object files rather than source files, so it will skip most of the compilation steps and just link them together into an executable program called `launch`.

So, if you change just one of the files, you'll only need to recompile that single file and then relink the program.

```bash
// This will recreate the thruster.o file.
// This is the only file that's changed.
gcc -c thruster.c

gcc *.o -o launch
//This will link everything together
```

Even though you have to type two commands, you are saving a lot of time.



#### Automate your builds with the make tool

You can compile your applications really quickly in `gcc` as long as you keep track of which files have changed. That's a tricky thing to do, but it's also pretty straightforward to automate.

```bash
thruster.c ---> thruster.o
// If the thruster.c file is newer, you need to recompile.
// If the thruster.o file is newer, you don't need to recompile.
```

`make` is a tool that can run the compile command for you. The `make` tool will check the timestamps of the source files and the generated files, and then it will only recompile the files if things have gotten out of date.

But before you can do all these things, you need to tell `make` about your source code. It needs to know the details of which files depend on which files. And it also needs to be told exactly how you want to build the code.



#### What does `make` need to know?

Every file that `make` compiles is called a target. A target is any file that is generated from some other files.

For every target, `make` needs to be told two things:

1. **The dependencies:** Which files the target is going to be generated from.
2. **The recipe:** The set of instructions it needs to run to generate the file.

Together, the dependencies and the recipe form a rule. A rule tells `make` all it needs to know to create the target file.



#### How make works

Let's say you want to compile `thruster.c` into some object code in `thruster.o`. What are the dependencies and what's the recipe?

```code
thruster.c --> thruster.o
```

- The `thruster.o` file is called the target, because it's the file you want to generate.

- `thruster.c` is a dependency because it's a file the compiler will need in order to create `thruster.o`. And what will the recipe be? That's the compile command to convert `thruster.c` into `thruster.o`.

  ```code
  gcc -c thruster.c
  ```

- If you tell the `make` tool about the dependencies and the recipe, you can leave it to `make` to decide when it needs to recompile `thruster.o`.

- Once you build the `thruster.o` file, you're going to use it to create the `launch` program. That means the `launch` file can also be set up as a target, because it's a file you want to generate. The dependency files for `launch` are all of the `.o` object files. The recipe is this command:

  ```bash
  gcc *.o -o launch
  ```

- Once `make` has been given the details of all of the dependencies and rules, all you have to do is tell it to create the `launch` file. `make` will work out the details.

**But how do you tell `make` about the dependencies and recipes?**

All of the details about the targets, dependencies and recipes need to be stored in a file called either `makefile` or `Makefile`.

**Page 240 in HFC for visualization.**

1. The `launch` program is made from the `launch.o` and `thruster.o` files.
2. The `launch.o` is compiled from `launch.c` and `launch.h` and also from `thruster.h`.
3. `thruster.o` is compiled from `thruster.h` and `thruster.c.`

The `launch` program is made by linking the `launch.o` and `thruster.o` files. Those files are compiled from their matching C and header files, but the `launch.o` file also depends on the `thruster.h` file because it contains code that will need to call a function in the `thruster` code.

```bash
launch.o: launch.c launch.h thruster.h
	gcc -c launch.c // this is the recipe.
// The recipes must begin with a tab character.

thruster.o: thruster.h thruster.c
	gcc -c thruster.c

launch: launch.o thruster.o
	gcc launch.o thruster.o -o launch
```



#### Bullet Points

- It can take a long time to compile a large number of files.

- You can speed up compilation time by storing object code in *.o files. 

  ```bash
  gcc -c my_program.c
  //This command compiles my_program.c into my_program.o without linking it to other object files.
  ```

- You can then link the objects files together to create the final executable.

- The `gcc` can compile programs from object files as well as source files.

- The `make` tool can be used to automate your builds.

- `make` knows about the dependencies between files, so it can compile just the files that change.

- `make` needs to be told about your build with a makefile.

- Be careful formatting your makefile: don't forget to indent lines with tabs instead of spaces.

- `chars` are  numbers.

- Use `shorts` for small whole numbers. Use `ints` for most whole numbers. Use `longs` for really big whole numbers. Use `float` for most floating points. Use `double` for really precise floating points.

- Split function declaration from definitions. Put declarations in a header file.

- `#include <>` for library headers. `#include ""` for local headers.

- Save object code into files to speed up your builds.

- Use `make` to manage your builds.