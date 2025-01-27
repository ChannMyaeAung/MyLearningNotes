A small tool does one task and does it well. OS like Linux are mostly made up of hundreds and hundreds of small tools.

If one small part of your program needs to convert data from one format to another, that's the perfect kind of task for a small tool.

- The OS controls how data gets into and out of the Standard Input and Output.
- If you run a program from the command prompt or terminal, the operating system will send all of the keystrokes from the keyboard into the Standard Input.
- If the OS reads any data from the Standard Output, by default it will send that data to the display.
- The `scanf()` and `printf()` functions just read and write Standard Input and the Standard Output.
- You can redirect the Standard Input and Standard Output with `<` and `>`so that they read and write data somewhere else such as to and from files.

- By default, the Standard Error is sent to the display.



**`fprintf()` prints to a data stream.**

`printf()` is just a version of a more general function called `fprintf()`. When you call `printf()`, it actually calls `fprintf()`.

```C
printf("I like Turtles!");

fprintf(stdout, "I like Turtles!"); // This will send data to the data stream

// These two calls are equivalent.
```



The `fprintf()` function allows you to choose where you want to send text to. You can tell `fprintf()` to send text to `stdout`(the Standard Output) or `stderr`(the Standard Error).

- `fscanf()` can be used to read the Standard input which is just like `scanf()` but you can specify the data steam.



`>` redirects the Standard Output. But `2>` redirects the Standard Error.

```bash
$ ./hello 2> errors.txt
```



**Bullet Points**

- The `printf()` function sends data to the Standard Output.
- The Standard Output goes to the display by default.
- You can redirect the Standard Output to a file using `>` on the command line.
- You can redirect the Standard Input to read a file by using `<` on the command line.
- You can redirect the Standard Error using `2>`.
- The Standard Error is reserved for outputting error messages.
- `scanf()` reads data from the Standard Input.
- The Standard Input reads data from the keyboard by default.

In C and many other programming languages, including C++, Java, and Python, the concept of "truthiness" differs from JavaScript.

In JavaScript, certain values are considered "falsy" and others are considered "truthy" when evaluated in a conditional context. For example, the number `0`, empty strings `""` or `''`, `null`, `undefined`, and `NaN` are considered falsy, while all other values are considered truthy.

However, in C and other languages mentioned above, the condition inside a conditional statement is typically evaluated based on whether the expression evaluates to zero (false) or non-zero (true). So, in C, the value `0` is considered false in a conditional context, while any non-zero value is considered true.

For example, in C:

```c
int num = 0;
if (num) {
    printf("This won't be printed because num is 0\n");
} else {
    printf("This will be printed because num is 0\n");
}
```

In the above code, the `if` condition evaluates `num`, and since `num` is `0`, which is considered false in C, the code inside the `else` block will be executed, printing "This will be printed because num is 0".



#### Tips for Designing Small Tools

Small tools should follow these design principles:

- They can read data from the Standard Input.
- They can display data on the Standard Output.
- They deal with text data rather than obscure binary formats.
- They each perform one simple task.





Q: Why is it important that small tools use the Standard Input and Standard Output?

A: Because it makes it easier to connect them with pipes.



Q: Why does that matter?

A: Small tools usually don't solve an entire problem on their own, just a small technical problem like converting from one format to another. But if you can combine them together, then you can solve large problems.



Q: What is a pipe, actually?

A: The exact details depend on the OS. Pipes might be made from sections of memory or temporary files. The important thing is that they accept data in one end and send the data out of the other in sequence.



Q: So if two programs are piped together, does the first program have to finish running before the second program can start?

A: No. Both of the programs will run at the same time; as output is produced by the first program, it can be consumed by the second program.



Q: Why do small tools use text?

A: It's the most open format. If a small tool uses text, it means that any other programmer can easily read and understand the output just by using a text editor. Binary formats are normally obscure and hard to understand.



Q: Can I connect several programs together with pipes?

A: Yes, just add more `|` between each program name. A series of connected processes is called a pipeline.



Q: If several processes are connected together with pipes and then I use `>` and `<` to redirect the Standard Input and Output, which processes will have their input and output redirected?

A: The `<` will send a file's content to the first process in the pipeline. The `>` will capture the Standard Output from the last process in the pipeline.



##### Bullet Points

- If you want to perform a different task, consider writing a separate small tool.
- Design tools to work with Standard Input and Standard Output.
- Small tools normally read and write text data.

`fscanf()` and `scanf()` returns the number of items successfully read.

#### `fprintf()` and `fscanf()`

- `fprintf()`: This function is used to write formatted output to a file. It takes at least two arguments:

  1. A pointer to a `FILE` object that identifies the stream where the content is to be written. This can be `stdout` (standard output), `stderr` (standard error), or a file opened using `fopen()`.

  2. A format string that specifies how subsequent arguments are converted for output. It can contain ordinary characters (which are copied directly to the output) and conversion specifiers (beginning with %) which are replaced by the values of the subsequent arguments.

  3. Zero or more additional arguments supplying values to be printed. Their type should match the corresponding format specifier in the format string.

     ```C
     fprintf(FILE *stream, "%s\n", line);
     ```

- `fscanf()`: This function is used to read formatted input from a file. It also takes at least two arguments:

  1. A pointer to a `FILE` object that identifies the stream from which characters are read. This can be `stdin` (standard input) or a file opened using `fopen()`.

  2. A format string that specifies how to read the input.

  3. A variable number of arguments corresponding to the conversion specifiers in the format string. These arguments are pointers to memory locations where the values read from the input stream will be stored, according to the format string.

     ```C
     fscanf(FILE *stream, "%s\n", line)
     ```

  In both functions, the return value indicates the number of items successfully formatted or read and stored. If an error occurs or the end-of-file (EOF) is reached before any data is read, `EOF` is returned.

#### Roll your own data streams

When a program runs, the OS gives it three file data streams: the Standard Input, the Standard Output and the Standard Error. But sometimes you need to create other data streams on the fly.

The good news is that the operating system doesn't limit you to the ones you are dealt when the program starts. You can roll your own as the program runs.



**Each data stream is represented by a pointer to a file** and you can create  a new data stream using the `fopen()` function:

```C
FILE *in_file = fopen("input.txt", "r");
// This will create a data stream to read from a file.
// fopen(filename, mode);
FILE *out_file = fopen("output.txt", "w");
```

The mode is:

"w" = write, "r" = read and "a" = append.

Once you have created a data stream, you can print to it using `fprintf()`. `fscanf()` to read from a file.

```C
fprintf(out_file, "Don't wear %s with %s", "red", "green");

fscanf(in_file, "%79[^\n]\n", sentence);
```



Finally when you're finished with a data stream, you need to close it. The truth is that all data streams are automatically closed when the program ends but it's still a good idea to always close the data stream yourself:

```C
fclose(in_file);
fclose(out_file);
```



Q: How many data streams can I have?

A: It depends on the OS, but usually a process can have up to 256. The key thing is there's a limited number of them, so make sure you close them when you're done using them.



Q: Why is FILE in uppercase?

A: It's historic. FILE used to be defined using a macro. Macros are usually given uppercase names.



#### Command-Line Options

Any program we write is going to need options. If we create a chat program, it's going to need preferences. If we write a game, a user will want to change the shape of the blood spots. Similarly, if we are writing a command-line tool, we are probably going to need to add command-line options.

Command-line options are the little switches we often see with command-line tools:

```bash
ps -ae 
// Display all the processes, including thier environments.

tail -f logfile.out
// Display the end of the file, but wait for new data to be added to the end of the file.
```



#### Let the library do the work for you

Many programs use command-line options. So, there's a special library function you can use to make dealing with them a little easier and it's called `getopt()`. and each time you call it, it returns the next option it finds on the command line.

```bash
rocket_to -e 4 -a Brasilia Tokyo London

-e = engines
4 = use four engines
-a = awesomeness
```



This program needs one option that will take a value (-e = engines) and another that is simply on or off (-a). You can handle these options by calling `getopt()` in a loop like this:

The `unistd.h` header is not actually part of the standard C library. Instead it gives your programs access to some of the POSIX libraries. POSIX was an attempt to create a common set of functions for use across all popular OS.

```C
// We will need to include this header.
#include <unistd.h>

// "ae:" means "The a option is valid; so is the e option"
// ":" means that the e option needs an argument
while((ch = getopt(argc, argv, "ae:")) != EOF){
    // The code to handle each option goes here.
    switch(ch){
        case 'e':
            engine_count = optarg;
    };
}
	// These final two lines make sure we skip past the options we read.
	argc -= optind;
	argv += optind;
	// optind stores the number of strings read from the command line to get past the options.

	
```



The string `ae:` tells the `getopt()` function that a and e are valid options. The e is followed by a colon to tell `getopt()` that the `-e` needs to be followed by an extra argument. `getopt()` will point to that argument with the `optarg` variable.

When the loop finishes, you tweak `argv` and `argc` variables to skip past all of the options and get to the main command-line arguments. That will make your `argv` array look like this:

Barasilia - This is `argv[0]`.

Tokyo - This is `argv[1]`.

London - This is `argv[2]`.

After processing the arguments, the 0th argument will no longer be the program name. `argv[0]` will instead point to the first command-line argument that follows the options.

##### optarg

`optarg` is a global variable that is part of the `getopt` function in the `unistd.h` library in C.

When `getopt` is called to parse command-line options and arguments, if it encounters an option that takes an argument, it sets `optarg` to point to the argument value.

For example, if you run your program with `-d delivery`, `optarg` would point to the string "delivery".

`optarg` is only valid after `getopt` has been called and returned a character that requires an argument. It's undefined at other times.

```bash
./order_pizza -d now
// now is optarg
```
