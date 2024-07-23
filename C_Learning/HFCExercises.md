## Chapter 1 - Exercises

#### Card Counter

```C
#include <stdio.h>
#include <stdlib.h>

int main(void){
    char card_name[3];
    int count = 0;
    while(card_name[0] != 'X'){
        puts("Enter the card name: ");
        scanf("%2s", card_name);
        int val = 0;
        
        switch(card_name[0]){
            case 'K':
            case 'Q':
            case 'J':
                val = 10;
                break;
            case 'A':
                val = 11;
                break;
            case 'X':
                continue;
                // Break won't break us out of the loop becase we're inside a switch statement. We need a continue to go back and check the loop condition again.
            default:
                val = atoi(card_name);
                if(val < 1 || val > 10){
                    puts("I don't understand that value!\n");
                    continue;
                    // You need another continue here because you want to keep looping.
                }
        }
        
        if(val > 2 && val < 7){
            count++;
        }else if(val == 10){
            count--;
        }
        printf("Current count: %i\n", count);
    }
    return 0;
}
```



---



## Chapter 2 - Exercises



#### The Mating Game

```C
#include <stdio.h>

int main(){
    int contestants[] = {1,2,3};
    int *choice = contestants;
    contestants[0] = 2;
    contestants[1] = contestants[2];
    contestants[2] = *choice;
    printf("I'm going to pick contestant number %i\n", contestants[2]); 
    // I am going to pick contestant number 2.
    
    char s[] = "How cool is it?";
    char *t = s;
    printf("Size of s: %ld\n", sizeof(s)); 
    // Size of s: 16 (\0 is counted as well.)
    // This is the size of the s array.
    printf("Size of t: %ld\n", sizeof(t));
    // Size of t: 8 
    // This is the t pointer. sizeof is 4 or 8.
    
    return 0;
}
```



#### Pointers

```C
#include <stdio.h>

void printAge(int *pAge);

int main(){
    int age = 24;
    int *pAge = NULL; // a good practice to assign NULL if declaring a pointer
    pAge = &age;
    
    printAge(pAge);
}

void printAge(int *pAge){
    printf("You are %d years old\n", *pAge);
}
```



#### Arrays

```C
#include <stdio.h>

void skip(char *msg);

int main(){
    // defining a pointer variable
    char *msg_from_amy = "Don't call me";
    //passing the address of the above string
    skip(msg_from_amy);
    // call me
    return 0;
}

void skip(char *msg){ // holds the address of the string
    puts(msg + 6); 
    // adds 6 to the memory address stored in msg
}
```

---



### Chapter 2.5

#### Create an array of arrays

```C
#include <stdio.h>
#include <string.h>

char tracks[][80] = {
        "I left my heart in Harvard Med School",
        "Newark, Newark - a wonderful town",
        "Dancing with a Dork",
        "From here to maternity",
        "The girl from Iwo Jima",
    };

void find_track(char search_for[]);

int main(){
    printf("Result: %s\n", tracks[4]);
    // Result: The girl from Iwo Jima
    
    printf("Result: %c\n", tracks[4][6]);
    // Result: r
    
    char search_for[80];
    printf("Search for: ");
    fgets(search_for,80, stdin);
    
    // Remove the newline character if it exists
    if(search_for[strlen(search_for) - 1] == '\n'){
        search_for[strlen(search_for) - 1] == '\0';
    }
    
    find_track(search_for);
    
    return 0;
}

void find_track(char search_for[]){
    int i;
    for(i = 0; i < 5; i++){
        if(strstr(tracks[i], search_for)){
            printf("Track %i: '%s'\n", i, tracks[i]);
        }
    }
}
```



---



### Chapter 3

**Pocket Code**

```C
#include <stdio.h>

int main(){
    float latitude;
    float longitude;
    char info[80];
    int started = 0;
    
    puts("data[");
    // We are using scanf() to enter more than one piece of data.
    // While there're 3 values entered, we keep looping the loop.
    while(scanf("%f,%f, 79[^\n]", &latitude, &longitude, info) == 3){
        // 79[^\n] means "give me every character up to the end of the line"
        // scanf() always uses pointers.
        // The scnaf() function returns the number of values it was able to read.
        
        if(started){
            printf(",\n");
            // Display a comma only if you've already displayed a previous line.
        }else{
            started = 1;
            // Once the loop has started, you can set "started" to 1 which is true.
        }
        printf("{latitude: %f, longitude: %f, info: '%s'\n}", latitude, longitude, info);
    }
    // We don't need & here because printf() is using the values, not the addresses of the numbers.
    puts("]");
    return 0;
}
```



```terminal
>./geo2json
data=[
42.363, -71.098, Speed = 21
{latitude: 42.363, longitude: -71.098, info: 'Speed = 21'}44.324, -72.453, Speed = 23
,
{latitude: 44.324, longitude: -72.453, info: 'Speed = 23'}
]
>
```



One problem is the input and the output are all mixed up together and instead of reading and writing files, the program is currently reading data from the keyboard and writing it to the display. Also, there's a lot of data. If you are writing a small tool, you don't want to type in the data; you want to get large amounts of data by reading a file.

We really don't want the output on the screen. We need it in a file so we can use it with the mapping application.

Here is how the program should work.

**For visual representation, refer to page 148 on HFC Book.**

1. Take the GPS from the bike and download the data. It creates a file called `gpsdata.csv` with one line of data for every location.
2. The `geo2json` tool needs to read the contents of the `gpsdata.csv` line by line.
3. And then write that data in JSON format into a file called `output.json`.
4. The web page that contains the map application reads the `output.json` file. It displays all of the locations on the map.

- Tools that read data line by line, process it and write it out again are called filters. If you have a Unix machine or you've installed Cygwin on Windows, you already have a few filter tools installed.
- **head:** This tool displays the first few lines of a file.
- **tail:** This filter displays the lines at the end of a file.
- **sed:** The stream editor lets you do things like search and replace text.



**Redirect the Standard Input with `<` and the Standard Output with `>`.**

```terminal
> ./geo2json < gpsdata.csv > output.json
```



```bash
$ cat > gpsdata.csv
42.987,-72.3345,Speed=21

$ ./hello < gpsdata.csv
data=[
{latitude: 42.987000, longitude: -72.334503, info: 'Speed=21'}]

$ ./hello < gpsdata.csv > output.json
$ cat output.json
data=[
{latitude: 42.987000, longitude: -72.334503, info: 'Speed=21'}]


```



Let's add error-checking code.

```C
#include <stdio.h>

int main(void){
    float latitude;
    float longitude;
    char info[80];
    int started = 0;
    
    puts("data[");
    while(scanf("%f, %f, %79[^\n]", &latitude, &longitude, info) == 3){
        
        if(latitude < -90.00 || latitude > 90.00){
            printf("Invalid latitude: %f\n", latitude);
            return 2;
        }
        
        if(longitude < -180.00 || longitude > 180.00){
            printf("Invalid longitude: %f\n", longitude);
            return 2;
        }
        
        if(started){
            printf(",\n");
        }else{
            started = 1;
        }
        
        printf("{latitude: %f, longitude: %f, info: '%s'}", latitude, longitude, info);
        
    }
    puts("]");
    return 0;
}
```



```bash
chan@CMA:~/C_Programming/brocode$ ./hello
data=[
180.99, 240.987, London
Invalid latitude: 180.990005

chan@CMA:~/C_Programming/brocode$ cat > gpsdata.csv
180.99, 240.987, London

chan@CMA:~/C_Programming/brocode$ ls
gpsdata.csv  hello  hello.c  output.json

chan@CMA:~/C_Programming/brocode$ cat gpsdata.csv
180.99, 240.987, London

chan@CMA:~/C_Programming/brocode$ man cat
chan@CMA:~/C_Programming/brocode$ ./hello < gpsdata.csv
data=[
Invalid latitude: 180.990005

chan@CMA:~/C_Programming/brocode$ ./hello < gpsdata.csv > output.json

chan@CMA:~/C_Programming/brocode$ cat output.json
data=[
Invalid latitude: 180.990005

```



As you can see the error message is not displayed on the screen since we are redirecting the standard output to `output.json` file. The error message is redirected and displayed or written inside the `output.json` file.

So, the program ended silently and you never saw what the problem was.

But how can you still display error messages if you are redirecting the output?

On Mac, Linux, or Cygwin on Windows:

```bash
$ echo $?
2
```

Command Prompt on Windows:

```bash
C: \> echo %ERRORLEVEL%
2
```



Test Drive:

```bash
chan@CMA:~/C_Programming/brocode$ ./hello < gpsdata.csv > output.json 

chan@CMA:~/C_Programming/brocode$ echo $?
2

```



If we replace the `printf()` with the `fprintf()`, the code should now work in exactly the same way, except the error messages should appear on the Standard Error instead of the Standard Output.

```C
if(lattitude < -90.00 || latitude > 90.00){
    fprintf(stderr, "Invalid latitude: %f\n", latitude);
    return 2;
}
if(longitude < -180.00 || longitude > 180.00){
    fprintf(stderr, "Invalid longitude: %f\n", longitude);
    return 2;
}

// We need to specify stderr as the first parameter.
```



```bash
chan@CMA:~/C_Programming/brocode$ ./hello < gpsdata.csv > output.json 
Invalid latitude: 180.990005

```



#### Top Secret Exercise

```C
#include <stdio.h>

int main(){
    char word[10];
    int i = 0;
    // while scanf successfully reads the input, run this loop.
    while(scanf("%9s", word) == 1){
        i = i + 1;
        if(i % 2){
            fprintf(stdout, "%s", word);
        }else{
            fprintf(stderr, "%s", word);
        }
    }
    
   	return 0;
}
```



```bash
$ chan@CMA:~/C_Programming/HFC$ ./test > message.txt 2> message2.txt
THE
SUBMARINE
WILL
NOT 
ARRIVE 
TODAY

chan@CMA:~/C_Programming/HFC$ cat message.txt
THE
WILL
ARRIVE

chan@CMA:~/C_Programming/HFC$ cat message2.txt
SUBMARINE
NOT
TODAY

```



In the case of first input which is "THE", i = 0 + 1 = 1 and since 1 % 2 == 1, 1 is a non-zero value which means success. So the **if** block will be run and "THE" will be redirected to the Standard Output which in this case is to `message.txt`. In the case of "SUBMARINE", i = 1 + 1 = 2 and since 2 % 2 == 0, 0 is a zero value which evaluates to false and "SUBMARINE" will be redirected to the Standard Error which in this case is `message2.txt`.

**while `0` signifies success in the context of the `main()` function return value, it represents false in the context of conditional statements.**



Let's try to use our geo2json program above for something a little more complex which is displaying the information that falls inside the Bermuda Rectangle instead of just displaying data on a map.

That means only data that matches these conditions:

For visual representation, refer to page 168 in HFC book.

```C
((latitude > 26) && (latitude < 34))

((longitude > -76) && (longitude < -64))
```



We will not change the geo2json tool because we just want it to do just one task. If you make the program do something more complex, you'll cause problems for your users who expect the tool to keep working in exactly the same way.

#### A different task needs a different tool

We'll have two tools: a new Bermuda tool that filters out data that is outside the Bermuda Rectangle and then our original geo2json tool that will convert the remaining data for the map.



**For visualization, refers to page 170 in HFC Book.**

This is how we'll connect the programs together:

1. We'll feed all of out data into the Bermuda tool. This data includes events inside and outside the Bermuda Rectangle.
2. The Bermuda tool will only pass on data that falls inside the Bermuda Rectangle.
3. So, we'll only pass Bermuda Rectangle data to geo2.json.
4. geo2.json will work exactly the same as before.
5. We will product a map containing only Bermuda Rectangle data.

By splitting the problem down into two tasks, we'll be able to leave our geo2json untouched. But how will we connect our two tools together. 

#### Connect our input and output with a pipe

But now we will connect the **Standard Output** of the Bermuda tool to the **Standard Input** of the `geo2json` .



The `|` symbol is a pipe that connects the Standard Output of one process to the Standard Input of another process.

**Page 171 for visualization**.

```bash
$ bermuda | geo2json

// The operating system will run both programs at the same time.
// The output of bermuda will become the input of geo2json.
```



#### The Bermuda Tool

- It will read through a set of GPS data, line by line and send data to the Standard Output.
- But it won't send every piece of data to the Standard Output, just the lines that are inside the Bermuda Rectangle.
- The Bermuda tool will always output data in the same CSV format used to store GPS data.

```C
// Read the latitude, longitude, and other data for each line:
// if the latitude is between 26 and 34, then:
// if the longitude is between -64 and -76, then:
// display the latitude, longitude and other data.
```



##### bermuda.c

```C
#include <stdio.h>

int main(void){
    float latitude;
    float longitude;
    char info[80];
    
    while(scanf("%f, %f, %79[^\n]", &latitude, &longitude, info)){
        if((latitude > 26) && (latitude < 34)){
            if((longitude > -76) && (longitude < -64)){
                printf("%f, %f, '%s'\n", latitude, longitude, info);
            }
        }
    }
    return 0;
}
```



##### test.c (geo2json according to what I referred above)

```C
#include <stdio.h>

int main(void){
    float latitude;
    float longitude;
    char info[80];
    
    int started = 0;
    
    puts("data=[");
    
    while(scanf("%f, %f, %79[^\n]", &latitude, &longitude, info)){
        if((latitude < -90.00) || (latitude > 90.00)){
            fprintf(stderr, "Invalid latitude: %f\n", latitude);
            return 2;
        }
        if((longitude < -180.00) || (longitude > 180.00)){
            fprintf(stderr, "Invalid longitude: %f\n", longitude);
            return 2;
        }
        
        if(started){
            printf(",\n");
        }else{
            started = 1;
        }
        
        printf("{latitude: %f, longitude: %f, info: '%s'}", latitude, longitude, info);
    }
    
    puts("]");
    
 	return 0;
}
```



```bash
chan@CMA:~/C_Programming/HFC$ 
(./bermuda | ./test) < spooky.csv > output.json

chan@CMA:~/C_Programming/HFC$ cat output.json
data=[
{latitude: 28.987000, longitude: -66.897003, info: 'Bermuda Rectangle'}]

```



When you connect the two programs together, you can treat them as a single program.

- The `bermuda` tool filters out the events we want to ignore.

- `|` is the pipe that connects the processes.

- The `test.c` or `geo2json` as the book has referred to will convert the events to JSON format.

- `spooky.csv` is the file containing all the events.

  ```bash
  chan@CMA:~/C_Programming/HFC$ 
  ./test < spooky.csv
  data=[
  {latitude: 28.987000, longitude: -66.897003, info: 'Bermuda Rectangle'},
  {latitude: 43.987999, longitude: -44.896999, info: 'London'}]
  
  ```

  

#### Segmentation Fault(core dumped)

The segmentation fault is likely due to the program trying to access memory that it shouldn't.

`perror` is a function in the C standard library that prints a descriptive error message to the standard error output (`stderr`).The message is based on the current value of the `errno` variable which is set by many library functions when they encounter errors.

The `perror` function takes a single argument which is a string that is printed before the error message, followed by a colon and a space. This argument can be used to provide context about what operation was being attempted when the error occured.

For example, if you try to open a file that doesn't exist, `fopen` will return `NULL` and set `errno` to a value representing an error. You can then call `perror` to print a message like "Error opening file: No such file or directory."



```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(){
    char line[80];
    FILE *in = fopen("spooky.csv", "r");
    // This will print an error message and exit the program if the file cannot be opened, preventing segmentation fault.
    if(in == NULL){
        perror("Error opening file");
        return 1;
    }
    FILE *file1 = fopen("ufos.csv", "w");
    FILE *file2 = fopen("disappearances.csv", "w");
    FILE *file3 = fopen("others.csv", "w");
    while(fscanf(in, "%79[^\n]\n", line) == 1){
        if(strstr(line, "UFO")){
            fprintf(file1, "%s\n", line);
        }else if(strstr(line, "Disappearance")){
            fprintf(file2, "%s\n", line);
        }else{
            fprintf(file3, "%s\n", line);
        }
    }
    fclose(file1);
    fclose(file2);
    fclose(file3);
    return 0;
}
```



```bash
chan@CMA:~/C_Programming/HFC$ ./categorize 

chan@CMA:~/C_Programming/HFC$ ls
bermuda     categorize.c        message.txt  secret.txt  test.c
bermuda.c   disappearances.csv  others.csv   spooky.csv  ufos.csv
categorize  message2.txt        output.json  test

chan@CMA:~/C_Programming/HFC$ cat disappearances.csv

chan@CMA:~/C_Programming/HFC$ cat others.csv
28.987, -66.897, Bermuda Rectangle
43.988, -44.897, London

chan@CMA:~/C_Programming/HFC$ cat ufos.csv
chan@CMA:~/C_Programming/HFC$ 
```



The program will read the `spooky.csv` file and split up the data, line by line into three other files -

`ufos.csv`, `disappearances.csv` and `others.csv`.

But what if a user wanted to split up the data differently? What if he wanted to search for different words or write to different files? Could he do it without needing to recompile the program each time?



#### There's more to main()

The thing is any program we write will need to give the user the ability to change the way it works. If it's a GUI program, you will probably need to give its preferences. If it is a command-line program like our `categorize` tool, it will need to give the user the ability to pass it command-line arguments.

```bash
$ ./categorize mermaid mermaid.csv Elvis elvises.csv the_rest.csv
```



`mermaid` - this is the first word to filter for.

`mermaid.csv` - All of the mermaid data will be stored in this file.

`Elvis` - This means you want to check for Elvis

`elvises.csv` - All the Elvis sightings will be stored here.

`the_rest.csv`- Everything else goes into this file.



But how do we read command-line arguments from **within the program**?

```C
int main(int argc, char *argv[]){
    ...Do stuff...
}
// argc = argument count, arvg = argument vecotr
// argc value is a count of the number of elements in the array. In other words, it is the number of arguments passed into the program from the command line, including the name of the program.

// argv is the argument of vector. It is an array of strings(character arrays) that hold each of the command line arguments. argv[0] is the name of the program, argv[1] is the first argument and so on.
```



The main function can read the command-line arguments as an array of strings. Because C doesn't really have strings built-in, it reads them as an array of character pointers to strings. 

```bash
./categorize mermaid mermaid.csv Elvis elvises.csv the_rest.csv
```

The `argc` would be 6.

`./categorize` - The first argument is actually the name of the program being run as it was run by the user. This is argv[0]. That means the first proper command-line argument is `argv[1]`.

`mermaid` - This is argv[1]

`mermaid.csv` - This is argv[2]

`Elvis` - This is argv[3]

`elvises.csv` - This is argv[4]

`the_rest.csv` - This is argv[5]

Command-line arguments really give your program a lot more flexibility.

**This is a modified version of the categorize program that can read the keywords to search for and the files to use form the command line.**

```C
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char *argv[]){
    char line[80];
    
    if(argc != 6){
        fprintf(stderr, "You need to give 5 arguments");
        return 1;
    }
    
    FILE *in = fopen("spooky.csv", "r");
    
    //This will print an error message and exit the program if the file cannot be opened, preventing segmentation fault.
    if(in == NULL){
        perror("Error opening file");
        return 1;
    }
    
    FILE *file1 = fopen(argv[2], "w");
    FILE *file2 = fopen(argv[4], "w");
    FILE *file3 = fopen(argv[5], "w");
    
    while(fscanf(in, "%79[^\n]\n", line) == 1){
        if(strstr(line, argv[1])){
            fprintf(file1, "%s\n", line);
        } else if(strstr(line, argv[3])){
            fprintf(file2, "%s\n", line);
        }else{
            fprintf(file3, "%s\n", line);
        }
    }
    fclose(in);
    fclose(file1);
    fclose(file2);
    fclose(file3);
    return 0;
}
```



```bash
chan@CMA:~/C_Programming/HFC/chapter3_datastreams
$ make categorize
cc     categorize.c   -o categorize

chan@CMA:~/C_Programming/HFC/chapter3_datastreams
$ cat spooky.csv
30.685163,-68.137207,Type=Yeti
28.304380,-74.575195,Type=UFO
29.132971,-71.136475,Type=Ship
28.343065,-62.753906,Type=Elvis
27.868217,-68.005371,Type=Goatsucker
30.496017,-73.333740,Type=Disappearance
26.224447,-71.477051,Type=UFO
29.401320,-66.027832,Type=Ship
37.879536,-69.477539,Type=Elvis
22.705256,-68.192139,Type=Elvis
27.166695,-87.484131,Type=Elvis

chan@CMA:~/C_Programming/HFC/chapter3_datastreams
$ ./categorize mermaid mermaid.csv Elvis elvises.csv the_rest.csv

chan@CMA:~/C_Programming/HFC/chapter3_datastreams
$ cat mermaid.csv

chan@CMA:~/C_Programming/HFC/chapter3_datastreams
$ cat elvises.csv
28.343065,-62.753906,Type=Elvis
37.879536,-69.477539,Type=Elvis
22.705256,-68.192139,Type=Elvis
27.166695,-87.484131,Type=Elvis

chan@CMA:~/C_Programming/HFC/chapter3_datastreams
$ cat the_rest.csv
30.685163,-68.137207,Type=Yeti
28.304380,-74.575195,Type=UFO
29.132971,-71.136475,Type=Ship
27.868217,-68.005371,Type=Goatsucker
30.496017,-73.333740,Type=Disappearance
26.224447,-71.477051,Type=UFO
29.401320,-66.027832,Type=Ship
chan@CMA:~/C_Programming/HFC/chapter3_datastreams$ 


```



```bash
chan@CMA:~/C_Programming/HFC/chapter3_datastreams
$ ./categorize UFO aliens.csv Elvis elvises.csv the_rest.csv

chan@CMA:~/C_Programming/HFC/chapter3_datastreams
$ cat aliens.csv
28.304380,-74.575195,Type=UFO
26.224447,-71.477051,Type=UFO

chan@CMA:~/C_Programming/HFC/chapter3_datastreams
$ cat elvises.csv
28.343065,-62.753906,Type=Elvis
37.879536,-69.477539,Type=Elvis
22.705256,-68.192139,Type=Elvis
27.166695,-87.484131,Type=Elvis

chan@CMA:~/C_Programming/HFC/chapter3_datastreams
$ cat the_rest.csv
30.685163,-68.137207,Type=Yeti
29.132971,-71.136475,Type=Ship
27.868217,-68.005371,Type=Goatsucker
30.496017,-73.333740,Type=Disappearance
29.401320,-66.027832,Type=Ship
chan@CMA:~/C_Programming/HFC/chapter3_datastreams$ 

```



#### Pizza Pieces

```C
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <string.h>

int main(int argc, char *argv[]){
    char delivery = "";
    int think = 0;
    
    int count = 0;
    char ch;
    
    while((ch = getopt(argc, argv, "d:t")) != EOF){
        switch(ch){
            case 'd':
                delivery = optarg;
                break;
            case 't':
                thick = 1;
                break;
            default:
                fprintf(stderr, "Unknown option: '%s'\n", optarg);
                return 1;
        }
    }
    
    argc -= optind;
    argv += optind;
    
    if(thick){
        puts("Thick crust.");
    }
    if(delivery[0]){
        printf("To be delivered %s.\n", delivery);
    }
    puts("Ingredients: ");
    
    for(count = 0; count < argc; count++){
        puts(argv[count]);
    }
    return 0;
}
```



##### Pizza Pieces Code Breakdown line by line

- The main function accepts two parameters: `argc`, an integer representing the number of command-line arguments passed to the program, and `argv`, an array of strings containing those arguments.

- Declare variables for storing information about pizza delivery(`delivery`), crust thickness(`thick`), a counter(`count`), and a character(`ch`) for parsing command-line options. `delivery` is declared as a pointer to allow flexibility in storing and modifying string data.

- The while loop parses command-line options using `getopt`. It iterates over each option(`ch`) provided in the command-line arguments(`argv`). The `getopt` function returns the next option character(`d` for delivery and `t` for thick crust) or `EOF` when all options  have been processed. In this case, the loop will run until `EOF` end of files.

- The switch statement handles each option accordingly.

- After parsing options with `getopt`, `optind` contains the index of the first non-option argument in `argv`. These lines adjust `argc` and `argv` to exclude the parsed options. `argc -= optind` reduces `argc` by the number of parsed options, and `argv += optind` advances `argv` to the start of the remaining arguments.

  illustrations:

  ```bash
  ./pizza -d "home" -t cheese tomato mushroom
  ```

  - Initially, `argc` is 7 (including the program name).
  - After parsing options with `getopt`, `optind` points to the first non-option argument ("cheese").
  - `argc -= optind` reduces `argc` to 4 (3 non-option arguments).
  - `argv += optind` advances `argv` to point to the first non-option argument("cheese").

- If the `thick` variable is set indicating that the `t` option was given `-t`, a message is printed saying "Thick crust.".

- If the first character of the `delivery` string is not the null character indicating that the `d` option was given with a non-empty argument), a message is printed saying "To be delivered" followed by the delivery time.

- The remaining arguments which are not options are printed one per line. These are the pizza ingredients.



##### Test Drive

```bash
chan@CMA:~/C_Programming/HFC/chapter3_orderpizza
$ make order_pizza
cc     order_pizza.c   -o order_pizza

chan@CMA:~/C_Programming/HFC/chapter3_orderpizza
$ ./order_pizza
Ingredients: 

chan@CMA:~/C_Programming/HFC/chapter3_orderpizza
$ ./order_pizza Anchovies
Ingredients: 
Anchovies

chan@CMA:~/C_Programming/HFC/chapter3_orderpizza
$ ./order_pizza Anchovies Pineapple
Ingredients: 
Anchovies
Pineapple

chan@CMA:~/C_Programming/HFC/chapter3_orderpizza
$ ./order_pizza -d now -t Anchovies Pineapple
Thick crust.
To be delivered now.
Ingredients: 
Anchovies
Pineapple

chan@CMA:~/C_Programming/HFC/chapter3_orderpizza
$ ./order_pizza -d 9:40 -t Anchovies Pineapple
Thick crust.
To be delivered 9:40.
Ingredients: 
Anchovies
Pineapple

chan@CMA:~/C_Programming/HFC/chapter3_orderpizza
$ ./order_pizza -d
./order_pizza: option requires an argument -- 'd'
Unknown option: '(null)'

chan@CMA:~/C_Programming/HFC/chapter3_orderpizza
$ 

```



We first compile the program and try running with no options for the first couple of times we call it.

Then we try out the 'd' option and give it an argument of 'now'.

Then the 't' option. Remember the "t" option doesn't take any arguments.

Finally, we try skipping the argument for "d": it creates an error.



Q: Can I combine options like `-td` now instead of `-d` now `-t`?

A: Yes, you can. The `getopt()` function will handle all of that for you.



Q: What about changing the order of the options?

A: Because of the way we read the options, it won't matter if you type in `-d now -t` or `-t -d now` or `-td now`.



Q: But what if I want to pass negative numbers as command-line arguments like `set_temperature -c -4`?

Won't it think that the 4 is an option, not an argument?

A: In order to avoid ambiguity, you can split your main arguments from the options using `--`. So you would write `set_temperature -c -- -4`. `getopt()` will stop reading options when it sees the `--`, so the rest of the line will be read as simple arguments.



##### Bullet Points

- There are two versions of the `main()` function - one with command-line arguments, and one without.
- Command-line arguments are passed to `main()` as an argument count and an array of pointers to the argument strings.
- Command-line options are command-line arguments prefixed with "-".
- The `getopt()` function helps you deal with command-line options.
- You define valid options by passing a string to `getopt()` like `ae:`.
- A ":" colon following an option in the string means that the option takes an additional argument.
- `getopt()` will record the options argument using the `optarg` variable.
- After you've read all of the options, you should skip past them using the `optind` variable.
- You can create custom data streams with `fopen("filename", mode)`.

---



#### Chapter 4 Exercises

##### Exercise 1

```C
#include <stdio.h>

float total = 0.0;
short count = 0;
short tax_percent = 6;

float add_with_tax(float f);
// Function declaration

int main(){
    float val;
    printf("Price of the item: ");
    while(scanf("%f", &val) == 1){
        printf("Total so far: .2%f\n", total);
        printf("Price of the item: ");
        
    }
    printf("\nFinal Total: %.2f\n", total);
    printf("Number of items: %hi\n", count);
    return 0;
}

// Function to caculate tax based on tax percent and return the total.
float add_with_tax(float f){
    float tax_rate = 1 + tax_percent / 100.0;
    total = total + (f * tax_rate);
    count++;
    return total;
}
```



**Code Breakdown:**

1. We declared some global variables, `total` for the total of all prices entered, `count` for the number of items, and `tax_percent` is the tax rate.
2. The `add_with_tax` function takes a price as an argument. It calculates the tax_rate based on `tax_percent`, adds the tax to the price, adds the result to the total, increments the count of items and returns the new total.
3. The `main()` function starts by declaring a variable `val` to hold the price of the current item.
4. It then enters a loop where it prompts the user to enter a price, reads the price into `val`, adds `val` to the total using `add_with_tax`, and prints the new total. This loop continues until the user enters something that isn't a number.
5. Once the loop ends, the function prints the final total and the number of items.



```bash
chan@CMA:~/C_Programming/HFC$ ./test
Price of item: 25
Total so far: 26.50
Price of item: 24
Total so far: 51.94
Price of item: 34
Total so far: 87.98
Price of item: 45
Total so far: 135.68
Price of item: 


Final total: 135.68
Number of items: 4

```



**Code Execution:**

1. The user enters 25.

2. The `scanf` function reads the input into `val`. `val` is now 25.

3. The `add_with_tax` function is called with `val` as an argument. Inside `add_with_tax`, the tax rate is calculated as `1 + 6 / 100.0 = 1.06`.

4. The price including tax is calculated as `25 * 1.06 = 26.5`. This is added to `total` which was initially 0, so `total` is now 26.5. The count of items is incremented from 0 to 1.

5. The program then goes back to the start of the loop and prompts the user to enter another price. If the user enters a non-number, the loop ends and the final total and number of items are printed.

6. Now the total is `26.5`. The price `24` was added and `24* 1.06 = 25.44`. Then Total is calculated `total = 26.5 + 25.44 = 51.94`. The count is incremented from 1 to 2.

   

#### Compilers don't like surprises

What happened when we don't declared our function before the main function?. 

1. If the compiler see this line of code `printf("Total so far: %.2f\n", add_with_tax(val));`, the compiler figures that it will find out more about the function later in the source file.
2. The compiler needs to know what data type the function will return otherwise, it makes an assumption.
3. When it reaches the code for the actual function, it returns a "conflicting" types for 'add_with_tax' error. The compiler thinks it has two functions with the same name. One function is the real one in the file. The other is the one that the compiler assumed would return an `int`.



#### Split the declaration from the definition

We can avoid the compiler making assumptions by explicitly telling it what functions it should expect. When we tell the compiler about a function, it's called function declaration.

```C
float add_with_tax();
// A declaration has no body code. 
// It just ends with a ; (semicolon)
// The declaration tells the compiler what return value to expect
```



Even better than that, C allows you to take that whole set of declarations out of your code and put them in a header file.

#### Creating our first header file

1. Create a new file with a .h extension.

   `totaller.h`

   ```C
   float add_with_tax(float f);
   ```

2. Include the header file in our main program.

   ```C
   #include <stdio.h>
   #include "totaller.h"
   ```



- The reason why we should surround it with double quotes rather than angle brackets is that when the compiler sees an `include` line with angle brackets, it assumes it will find the header file somewhere off in the directories where the library code lives. 
- But our header file is in the same directory as our `.c` file. 
- By wrapping the header filename in quotes, we are telling the compiler to look for a local file.
- When the compiler reads the `#include` in the code, it will read the contents of the header file, just as if it had been typed into the code.
- `#include` is a preprocessor instruction.



Data types are different sizes on different platforms.

```C
#include <stdio.h>
#include <limits.h>
// This contains the values for the integer types like int and char.
#include <float.h>
// This contains the vlaues for floats and doubles.

int main(){
    printf("The value of INT_MAX is %i\n", INT_MAX);
    printf("The value of INT_MIN is %i\n", INT_MIN);
    printf("An int takes %zu bytes\n", sizeof(int));
    
    printf("The value of FLT_MAX is %f\n", FLT_MAX);
    printf("The value of FLT_MIN is %f\n", FLT_MIN);
    printf("A float takes %zu bytes\n", sizeof(float));
    return 0;
}
```



When you compile and run this code

```bash
The value of INT_MAX is 2147483647
The value of INT_MIN is -2147483648
An int takes 4 bytes
The value of FLT_MAX is 340282346638528859811704183484516925440.000000
The value of FLT_MIN is 0.000000
A float takes 4 bytes

```

`CHAR(chars)`, `DBL(double)`, `SHRT(shorts)`, or `LNG(longs)`



#### Message Hider

encrypt.c

```C
#include "encrypt.h"

void encrypt(char *message){
    char c;
    while(*message){
        *message = *message ^ 31;
        message++;
    }
}
```



encrypt.h

```C
void encrypt(char *message);
```



main.c

```C
#include <stdio.h>
#include "encrypt.h"

int main(){
    char msg[80];
    while(fgets(msg, 80, stdin)){
        encrypt(msg);
        printf("%s\n", msg);
    }
}
```



```C
chan@CMA:~/C_Programming/HFC/chapter_4/exercise_2$ 
gcc main.c encrypt.c -o main

// When we run the program, we can enter text and see the encrypted version.
chan@CMA:~/C_Programming/HFC/chapter_4/exercise_2$ 
./main
    
I am a secret message
V?~r?~?lz|mzk?rzll~xz
    
chan@CMA:~/C_Programming/HFC/chapter_4/exercise_2$ 
// We can even pass it the contents of the encrypt.h file to encrypt it.
./main < encrypt.h
ipv{?zq|mfok7|w~m?5rzll~xz6$
    
chan@CMA:~/C_Programming/HFC/chapter_4/exercise_2$ 

```

**Code Breakdown**

- In `main.c`, it includes the standard input/output library `stdio.h` and a custom header file `encrypt.h`. 
- Then inside the function, it initializes an array `msg` of 80 characters to store user input.
- It enters a loop where it continuously reads input form the user using `fgets()`, encrypts it using a function `encrypt()` defined in `encrypt.c` and prints the encrypted message using `printf()`.
- Moving on to `encrypt.c`, it defines the `encrypt()` function. This function takes a pointer to a character array `(char *message)` as its parameter. Inside this function, there's a loop that iterates over each character in the message. For each character, it performs a bitwise XOR(exclusive OR) operation with the value 31 `*message = *message ^ 31`. This is where the actual encryption happens.



Q: Why do we use `message` as a pointer variable?

A: Because we want to modify the original message passed to the `encrypt()` function. Using a pointer allows us to directly manipulate the memory where the message is stored.



Q: What is XOR(^)?

A: XOR(exclusive OR) is a bitwise operation in C. It returns true if the operands are different and false if they are the same. For example,

-  `0 ^ 0` results in `0`.
- `0 ^ 1` results in `1`.
- `1 ^ 0` results in `1`.
- `1 ^ 1` results in `0`.



Q: Why do we use 31 in the XOR operation?

A: The choice of 31 for the XOR operation is somewhat arbitrary. It's a simple value that's commonly used in basic encryption algorithms. You could use any other value, but 31 was chosen here probably because it provides a decent level of obfuscation for such a simple algorithm.



#### Make Magnets Solution

```bash
oggswing: oggswing.c oggswing.h
	gcc oggswing.c -o oggswing

swing.ogg: whitennerdy.ogg oggswing
	oggswing whitennerdy.ogg swing.ogg
```



Here's a breakdown:

1. **Line 1: `oggswing: oggswing.c oggswing.h`**
   - This line specifies that the target `oggswing` depends on `oggswing.c` and `oggswing.h`. In other words, if any of these source files are modified, `oggswing` needs to be rebuilt.
2. **Line 2: `[TAB] gcc oggswing.c -o oggswing`**
   - This line contains a command to compile the `oggswing.c` source file into an executable named `oggswing` using the GCC compiler. The `[TAB]` represents a tab character, which is often used as indentation in Makefiles.
3. **Line 4: `swing.ogg: whitennerdy.ogg oggswing`**
   - This line specifies that the target `swing.ogg` depends on `whitennerdy.ogg` and `oggswing`. In other words, before `swing.ogg` can be created, `whitennerdy.ogg` and `oggswing` must already exist.
4. **Line 5: `[TAB] oggswing whitennerdy.ogg swing.ogg`**
   - This line contains a command to execute `oggswing`, passing `whitennerdy.ogg` as input and producing `swing.ogg` as output. It seems like `oggswing` is some sort of program or script that processes audio files.

In summary, this script automates the compilation of a C program (`oggswing`) and its usage to process audio files (`whitennerdy.ogg` and `swing.ogg`). It ensures that the C program is built if its source files are modified and then uses it to process audio files.



```bash
chan@CMA:~/C_Programming/test
$ code hello.h main.c hello.c

```

hello.h

```C
void hello();
const char *get_message();
```

hello.c

```C
#include <stdio.h>
#include "hello.h"

void hello(){
    printf("%s", get_message());
}

const char *get_message(){
    return "Hello World\n";
}
```

main.c

```C
#include "hello.h"

int main(){
    hello();
    return 0;
}
```



```bash
chan@CMA:~/C_Programming/test$ gcc -c main.c

chan@CMA:~/C_Programming/test$ gcc -c hello.c

chan@CMA:~/C_Programming/test$ ls
hello.c  hello.h  hello.o  main.c  main.o

chan@CMA:~/C_Programming/test
$ gcc main.o hello.o -o final

chan@CMA:~/C_Programming/test
$ ls
final  hello.c  hello.h  hello.o  main.c  main.o

chan@CMA:~/C_Programming/test$ ./final
Hello World

chan@CMA:~/C_Programming/test$ code Makefile
```

#### Makefile

```makefile
all: final

final: main.o hello.o
	@echo "Linking and producing the final application"
	gcc main.o hello.o -o final

main.o: main.c
	@echo "Compiling the main file"
	gcc -c main.c

hello.o: hello.c
	@echo "Compiling the hello file"
	gcc -c hello.c

clean:
	@echo "Removing everything except the source files"
	@rm final main.o hello.o
```



```bash
chan@CMA:~/C_Programming/test
$ ls
final  hello.c  hello.h  hello.o  main.c  main.o  Makefile

chan@CMA:~/C_Programming/test
$ make clean
Removing everything except the source files

chan@CMA:~/C_Programming/test
$ ls
hello.c  hello.h  main.c  Makefile

chan@CMA:~/C_Programming/test
$ make all
Compiling the main file
gcc -c main.c 
Compiling the hello file
gcc -c hello.c 
Linking and producing the final application
gcc main.o hello.o -o final

chan@CMA:~/C_Programming/test
$ ls
final  hello.c  hello.h  hello.o  main.c  main.o  Makefile

chan@CMA:~/C_Programming/test
$ ./final
Hello World
chan@CMA:~/C_Programming/test$ 

```



If there's a change in one of the files, I don't have to rerun all the recipes and recompile all the code with `Makefile`.

Let's say if I change Hello World from Hello My Princess.

hello.c

```C
const char *get_message(){
    return "Hello My Princess!\n";
}

```

Then I only have to run this:

```bash
chan@CMA:~/C_Programming/test
$ make clean
Removing everything except the source files

chan@CMA:~/C_Programming/test
$ make all
Compiling the main file
gcc -c main.c 
Compiling the hello file
gcc -c hello.c 
Linking and producing the final application
gcc main.o hello.o -o final

chan@CMA:~/C_Programming/test
$ ./final
Hello My Princess!
chan@CMA:~/C_Programming/test$ 

```

---



#### Chapter 5 - Exercises

snappy.c

```C
#include <stdio.h>
#include "functions.h"

int main(){
  struct fish snappy = {"Snappy", "Piranha", 69, 4};
    
    catalog(snappy);
    label(snappy);
    
  return 0;
}
```



functions.c

```C
#include <stdio.h>
#include "functions.h"

void catalog(struct fish f){
    printf("%s is a %s with %i teeth. He is %i.\n", f.name, f.species, f.teeth, f.age);
}

void label(struct fish f){
    printf("Name: %s\nSpecies: %s\nAge: %i years old, %i teeth\n", f.name, f.species, f.teeth, f.age);
}
```



functions.h

```C
struct fish{
    const char *name;
    const char *species;
    int teeth;
    int age;
};

void catalog(struct fish f);

void label(struct fish f);
```



```make
all: final

final: snappy.o functions.o
	@echo "Compiling final executable file..."
	gcc snappy.o functions.o -o snappy

snappy.o: snappy.c
	@echo "Compiling the snappy file..."
	gcc -c snappy.c

functions.o: functions.c
	@echo "Compiling the functions file..."
	gcc -c functions.c

clean:
	@echo "Removing everything except the source files..."
	@rm snappy.o functions.o snappy
```



```bash
chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ make clean
Removing everything except the source files...

chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ ls
functions.c  functions.h  Makefile  snappy.c

chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ make all
Compiling the snappy file...
gcc -c snappy.c 
Compiling the functions file...
gcc -c functions.c 
Compiling final executable file...
gcc snappy.o functions.o -o snappy 

chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ ./snappy
Snappy is a Piranha with 69 teeth. He is 4.
Name: Snappy
Species: Piranha
Age: 69 years old, 4 teeth
```



##### Nest `Struct`

functions.h

```C
struct preferences{
    const char *food;
    float exercise_hours;
}

struct fish{
    const char *name;
    const char *species;
    int teeth;
    int age;
    struct preferences care;
};

void catalog(struct fish f);

void label(struct fish f);

void info(struct fish f);
```



snappy.c

```C
#include <stdio.h>
#include "functions.h"

int main(){
    
    struct fish snappy = {"Snappy", "Piranha", 69, 4, {"Meat", 7,5}};
    
    catalog(snappy);
    label(snappy);
    info(snappy);
    
    return 0;
}
```





functions.c

```C
#include <stdio.h>
#include "functions.h"

void catalog(struct fish f)
{
    printf("%s is a %s with %i teeth. He is %i.\n", f.name, f.species, f.teeth, f.age);
}

void label(struct fish f)
{
    printf("Name: %s\nSpecies: %s\nAge: %i years old, %i teeth\n", f.name, f.species, f.teeth, f.age);
}

void info(struct fish f){
    printf("Snappy likes to eat %s.\n", f.care.food);
    printf("Snappy likes to exercise for %.2f hours.\n", f.care.exercise_hours);
}
```



```bash
chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ make all
Compiling the snappy file...
gcc -c snappy.c 
Compiling final executable file...
gcc snappy.o functions.o -o snappy 

chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1
$ ./snappy
Snappy is a Piranha with 69 teeth. He is 4.
Name: Snappy
Species: Piranha
Age: 69 years old, 4 teeth
Snappy likes to eat Meat.
Snappy likes to exercise for 7.50 hours.

chan@CMA:~/C_Programming/HFC/chapter_5/exercise_1$ 

```

---



#### Piranha Puzzle - Exercise 1

piranha.h

```C
struct exercise{
    const char *description;
    float duration;
};

struct meal{
    const char *ingredients;
    float weight;
};

struct preferences{
    struct meal food;
    struct exercise exercise;
};

struct fish{
    const char *name;
    const char *species;
    int teeth;
    int age;
    struct preferences care;
};

void label(struct fish a);
```



functions.c

```C
#include <stdio.h>
#include "piranha.h"

void label(struct fish a){
    printf("Name:%s\nSpecies:%s\n%i years old, %i teeth", a.name, a.species, a.teeth, a.age);
    printf("Feed with %2.2f lbs of %s and allow to %s for %2.2f hours\n", a.care.food.weight, a.care.food.ingredients, a.care.exercise.description, a.care.exercise.duration);
}
```



piranha.c (main file)

```C
#include <stdio.h>
#include "piraha.h"

int main(void){
    struct fish snappy = {"Snappy", "Piranha", 69, 4, {"neat",0.2}, {"swim in the jacuzzi", 7.5}};
    label(snappy);
    return 0;
}
```



Makefile

```make
all: piranha

piranha: piranha.o functions.o
	@echo "Compiling the final piranha file..."
	gcc piranha.o functions.o -o piranha

piranha.o: piranha.c
	@echo "Compiling the piranha file..."
	gcc -c piranha.c

functions.o: functions.c
	@echo "Compiling the functions file..."
	gcc -c functions.c

clean: 
	@echo "Removing everything except the source files..."
	@rm piranha.o functions.o piranha
```



Execution

```bash
chan@CMA:~/C_Programming/HFC/chapter_5/piranha_puzzle
$ make clean
Removing everything except the source files...

chan@CMA:~/C_Programming/HFC/chapter_5/piranha_puzzle
$ ls
functions.c  Makefile  piranha.c  piranha.h

chan@CMA:~/C_Programming/HFC/chapter_5/piranha_puzzle
$ make all
Compiling the piranha file...
gcc -c piranha.c 
Compiling the functions file...
gcc -c functions.c 
Compiling the final piranha file...
gcc piranha.o functions.o -o piranha

chan@CMA:~/C_Programming/HFC/chapter_5/piranha_puzzle
$ ./piranha
Name: Snappy
Species: Piranha
69 years old, 4 teeth
Feed with 0.20 lbs of meat and allow to swim in the jacuzzi for 7.50 hours.

chan@CMA:~/C_Programming/HFC/chapter_5/piranha_puzzle$ 

```

---



#### Scuba Diver Exercise - Exercise 2

header.h

```C
typedef struct{
    float tank_capacity;
    int tank_psi;
    const char *suit_material;
} equipment;

typedef struct scuba{
    const char *name;
    equipment kit;
} diver;

//Here we'll just use "diver" although we give the struct name "scuba".

void badge(diver d);
```



functions.c

```C
#include <stdio.h>
#include "header.h"

void badge(diver d){
    printf("Name: %s Tank: %2.2f(%i) Suit: %s\n", d.name, d.kit.tank_capacity, d.kit.tank_psi, d.kit.suit_material);
}
```



main.c

```C
#include <stdio.h>
#include "header.h"

int main(){
    diver randy = {"Randy", {5.5, 3500, "Neoprene"}};
    badge(randy);
    return 0;
}
```



Makefile

```makefile
all: diver

diver: main.o functions.o
	@echo "Compiling the final exe diver file..."
	gcc main.o functions.o -o diver
	
main.o: main.c
	@echo "Compiling main file..."
	gcc -c main.c

functions.o: functions.c
	@echo "Compiling functions file..."
	gcc -c functions.c

clean:
	@echo "Removing everything except the source files..."
	@rm main.o functions.o diver
```



Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_5/scubadiver_puzzle
$ make all
Compiling main file...
gcc -c main.c 
Compiling the final exe diver file...
gcc main.o functions.o -o diver 

chan@CMA:~/C_Programming/HFC/chapter_5/scubadiver_puzzle
$ ./diver
Name: Randy Tank: 5.50(3500) Suit: Neoprene

```



---



#### Turtle Myrtle - Exercise 3



If we don't use the pointer to our `struct`

```C
#include <stdio.h>

typedef struct{
    const char *name;
    const char *species;
    int age;
} turtle;

void happy_birthday(turtle t){
    t.age = t.age + 1;
    printf("Happy Birthday %s! You are now %i years old!\n", t.name, t.age);
}

int main(){
    turtle myrtle = {"Myrtle", "Leatherback sea turtle", 99};
    happy_birthday(myrtle);
    printf("%s's age is now %i\n", myrtle.name, myrtle.age);
    return 0;
}
```



The `myrtle struct` will not be updated but its clone will.

```bash
chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ make all
Compiling the main file...
gcc -c main.c 
Compiling the final turtle exe file...
gcc main.o functions.o -o turtle

chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ ./turtle
Happy Birthday Myrtle! You are now 100 years old!
Myrtle's age is now 99
```



Now if we use pointers: 

Note: We'll also be implementing makefile for this.

main.c

```C
#include <stdio.h>
#include "header.h"

int main(void){
    turtle myrtle = {"Myrtle", "Leatherback sea turtle", 99};
    happy_birthday(&myrtle);
    printf("%s's age is now %i\n", myrtle.name, myrtle.age);
    return 0;
}
```

header.h

```C
typedef struct{
    const char *name;
    const char *species;
    int age;
} turtle;

void happy_birthday(turtle *t)
```

functions.c

```C
#include <stdio.h>
#include "header.h"

void happy_birthday(turtle *t){
    (*t).age = (*t).age + 1;
    printf("Happy Birthday %s! You are now %i years old!\n", (*t).name, (*t).age);
}
```



Makefile

```make
all: turtle

turtle: main.o functions.o
	@echo "Compiling the final turtle exe file..."
	gcc main.o functions.o -o turtle

main.o: main.c
	@echo "Compiling the main file..."
	gcc -c main.c

functions.o: functions.c
	@echo "Compiling the functions file..."
	gcc -c functions.c

clean:
	@echo "Removing everything except the source files..."
	@rm main.o functions.o turtle
```



```bash
chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ make all
Compiling the main file...
gcc -c main.c 
Compiling the functions file...
gcc -c functions.c 
Compiling the final turtle exe file...
gcc main.o functions.o -o turtle

chan@CMA:~/C_Programming/HFC/chapter_5/myrtle
$ ./turtle
Happy Birthday Myrtle! You are now 100 years old!
Myrtle's age is now 100

chan@CMA:~/C_Programming/HFC/chapter_5/myrtle$ 

```

Now the function is updated.

By passing a pointer to the `struct`, we allowed the function to update the original data rather than taking a local copy.



---



#### Safe Cracker - Exercise 4

```C
#include <stdio.h>

typedef struct{
    const char *description;
    float value;
} swag;

typedef struct{
    swag *swag;
    const char *sequence;
} combination;

typedef struct{
    combination numbers;
    const char *make;
} safe;

int main(void){
    swag gold = {"GOLD", 1000000.0};
    combination numbers = {&gold, "6502"};
    safe s = {numbers, "RAMACON250"};
    
    printf("%s\n", s.numbers.swag->description);
    return 0;
}
```



```bash
chan@CMA:~/C_Programming/HFC/chapter_5/safe_cracker
$ make main
cc     main.c   -o main

chan@CMA:~/C_Programming/HFC/chapter_5/safe_cracker
$ ./main
GOLD

```



#### String Length Exercise from CS50

```C
#include <stdio.h>
#include <string.h>

int name_length(char *name);

int main(void){
    char name[25];
    printf("Enter your name: \n");
    fgets(name, sizeof(name), stdin);
    name[strcspn(name, "\n")] = '\0';
    printf("Your name is %d letters long.\n", name_length(name));
    return 0;
}

int name_length(char *name){
    int i = 0;
    while(name[i] != '\0'){
        i++;
    }
    return i;
}
```

Code Execution:

```bash
chan@CMA:~/C_Programming/practice
$ make practice
cc     practice.c   -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
Enter your name: 
Chan Myae Aung
Your name is 14 letters long.
```



#### Code Magnets - Exercise 5

main.c

```C
#include <stdio.h>
#include "header.h"

int main(){
    fruit_order apples = {"apples", "England", .amount.count=144, COUNT};
    
    fruit_order strawberries = {"strawberries", "Spain", .amount.weight=17.6, POUNDS};
    
    fruit_order oj = {"orange juice", "U.S.A", .amount.volume=10.5, PINTS};
    
    display(apples);
    display(strawberries);
    display(oj);
    return 0;
}
```

header.h

```C
typedef enum{
    COUNT, POUNDS, PINTS
} unit_of_measure;

typedef union{
    short count;
    float weight;
    float volume;
}quantity;

typedef struct{
    const char *name;
    const char *country;
    quantity amount;
    unit_of_measure units;
} fruit_order;

void display(fruit_order order);
```

functions.c

```C
#include <stdio.h>
#include "header.h"

void display(fruit_order order){
    printf("This order contains ");
    if(order.units == PINTS){
        printf("%2.2f pints of %s\n", order.amount.volume, order.name);
    }else if(order.units == POUNDS){
        printf("%2.2f lbs of %s\n", order.amount.weight, order.name);
    }else{
        printf("%i %s\n", order.amount.count, order.name);
    }
}
```



Makefile

```makef
all: magnet

magnet: main.o functions.o
	@echo "Compiling the final exe file..."
	gcc main.o functions.o -o magnet

main.o: main.c 
	@echo "Compiling the main file..."
	gcc -c main.c 

functions.o: functions.c 
	@echo "Compiling the functions file..."
	gcc -c functions.c 

clean:
	@echo "Removing everything execept the source file..."
	@rm magnet main.o functions.o 
```



Code Execution

```bash
chan@CMA:~/C_Programming/HFC/chapter_5/codeMagnets
$ make all
Compiling the functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc main.o functions.o -o magnet

chan@CMA:~/C_Programming/HFC/chapter_5/codeMagnets
$ ./magnet
This order contains 144 apples
This order contains 17.60 lbs of strawberries
This order contains 10.50 pints of orange juice

```

---



## Chapter 6

#### Code Magnets - Exercise 1 Linked Lists

```C
#include <stdio.h>
#include <string.h>

typedef struct island{
    const char *name;
    const char *opens;
    const char *closes;
    struct island *next;
} island;

void display(island *start);

int main(void){
    island amity = {"Amity", "09:00", "17:00", NULL};
    island craggy = {"Craggy", "09:00", "17:00", NULL};
    island isla_nublar = {"Isla Nublar", "09:00", "17:00", NULL};
    island shutter = {"Shutter", "09:00", "17:00", NULL};
    
    amity.next = &craggy;
    craggy.next = &isla_nublar;
    isla_nublar.next = &shutter;
    
    island skull = {"Skull", "09:00", "17:00", NULL};
    isla_nublar.next = &skull;
    skull.next = &shutter;
    
    display(&amity);
    return 0;
}

void display(island *start){
    island *i = start;
    for(; i != NULL; i = i->next){
        printf("Name: %s open: %s-%s\n", i->name, i->opens, i->closes);
    }
}
```



```bash
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_1
$ make main
cc     main.c   -o main

chan@CMA:~/C_Programming/HFC/chapter_6/exercise_1
$ ./main
Name: Amity open: 09:00-17:00
Name: Craggy open: 09:00-17:00
Name: Isla Nublar open: 09:00-17:00
Name: Skull open: 09:00-17:00
Name: Shutter open: 09:00-17:00

```



Code Breakdown:

- The code starts with the definition of a `struct` called `island` with 4 fields: `name`, `opens`, `closes` and `next`. The `name`, `opens`, and `closes` are pointers to `char` which means they can hold the address of a string. The `next` field is a pointer to another `island` `struct`, which allows us to create a linked list of `island structs`.

- The display function takes a pointer to an `island` struct as an argument. 

  - The `island *i = start` is initializing a new pointer `i` to point to the same `island` struct that `start` is pointing to. The purpose of this line is to create a new pointer that can be used to traverse the linked list of islands without modifying the `start` pointer. This is important because the `start` pointer is needed to keep track of the beginning of the list. If you were to use `start` directly to traverse the list, you would lose the reference to the beginning of the list. 
  - In our code, the initialization part is empty, which is why we set a semicolon at the beginning of the for loop. This is because `i` is already initialized before the `for` loop `(island *i = start;)`.

  - It loops through the linked list of islands, starting from the given island and prints the `name`, `opens` and `closes` fields of each island. 
  - In the for loop that follows, `i` is updated to point to the next `island` in the list on each iteration `(i = i->next)`.
  - The loop continues until it reaches an island with `next` set to `NULL` which indicates the end of the list.

- In the main function, four `island` structs are created and initialized. The `next` field of each island is set to the address of the next island, creating a linked list.

- Then a new `island` struct `skull` is created and inserted into the list between `isla_nublar` and `shutter`.

- Finally the display function is called with `&amity` as the argument, which prints the details of all the islands in the linked list.



#### malloc() example

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

typedef struct island{
    const char *name;
    const char *opens;
    const char *closes;
    struct island *next;
} island;

void display(island *start);

island *create(char *name);

int main(){
    // predefined islands
    island amity = {"Amity", "09:00", "17:00", NULL};
    island craggy = {"Craggy", "09:00", "17:00", NULL};
    island isla_nublar = {"Isla Nublar", "09:00", "17:00", NULL};
    island shutter = {"Shutter", "09:00", "17:00", NULL};
    
    // Link islands
    amity.next = &craggy;
    craggy.next = &isla_nublar;
    isla_nublar.next = &shutter;
    
    // Adding a new island
    island skull = {"Skull", "09:00", "17:00", NULL};
    isla_nublar.next = &skull;
    skull.next = &shutter;
    
    // display the list of islands
    display(&amity);
    
    // Create new islands based on user input
    char name[80];
    // read name for the first island
    fgets(name, 80, stdin);
    // create the first island
    island *p_island0 = create(name);
    // read name for the second island
    fgets(name, 80, stdin);
    // create the second island
    island *p_island1 = create(name);
    // link the two newly created islands
    p_island0->next = p_island1;
    // display the newly created islands
    display(p_island0);
    
    return 0;
}

void display(island *start){
    island *i = start;
    
    for(;i != NULL; i = i->next){
        printf("Name: %s open: %s-%s\n", i->name, i->opens, i->closes);
    }
}

island *create(char *name){
    // allocate memory for the new island
    island *i = malloc(sizeof(island));
    // assign the name
    i->name = name;
    // default opening time
    i->opens = "09:00";
    // default closing time
    i->closes = "17:00";
    // initialize next to NULL
    i->next = NULL;
    
    // return the pointer to the new island
    return i;
}
```



We added a `create` function to create a new island `struct`, it uses dynamic memory allocation with malloc() to create the new island on the **heap** instead of the stack. 

- Memory on the heap persists until it's explicitly deallocated with free() so the island will continue to exist after create returns.
- `create` then returns a pointer to the new `island` on the heap.
- This is why the return type of `create` is `island*` (a pointer to an island) rather than island. The pointer allows us to access the new `island` on the heap after `create` returns.

But when we run the code, one issue remains:

Code Execution:

```bash
chan@CMA:~/C_Programming/practice
$ ./practice
Name: Amity open: 09:00-17:00
Name: Craggy open: 09:00-17:00
Name: Isla Nublar open: 09:00-17:00
Name: Skull open: 09:00-17:00
Name: Shutter open: 09:00-17:00
Atlantis
Titchmarsh Island
Name: Titchmarsh Island
 open: 09:00-17:00
Name: Titchmarsh Island
 open: 09:00-17:00

```

- The first island is now the same as the second one.
- What happened to the name of the first island?
- When the code records the name of the island, it doesn't take a copy of the whole name string; it just records the address where the name string lives in memory.
- The program asks the user for the name of each island, but both times it uses the name local `char` array to store the name.
- That means that the two islands share the same name string.
- As soon as the local variable gets updated with the name of the second island, the name of the first island changes as well.
- The `strdup()` function can reproduce a complete copy of the string somewhere on the heap:
- The `strdup()` function works out how long the string is, and then calls the malloc() function to allocate the correct number of characters on the heap.
- It then copies each of the characters to the new space on the heap.
- That means that `strdup()` always create space **on the heap**. It can't create space on the stack because that's for local variables, and local variables get cleared away too often.
- But because `strdup()` puts new strings on the heap, that means we must **always remember to release their storage with the free() function.**



#### Let's fix the code using the `strdup()` function

```C
island *create(char *name){
    island *i = malloc(sizeof(island));
    i->name = strdup(name);
    i->opens = "09:00";
    i->closes = "17:00";
    i->next = NULL;
    return i;
}
```

- We only need to put the `strdup()` function on the name field only because we are setting the opens and closes fields to string literals.
- String literals are stored in a **read-only** area of memory set aside for **constant values**.
- Because we always set the `opens` and `closes` fields to constant values, we don't need to take a defensive copy of them because they'll never change.
- But we had to take a defensive copy of the` name` array, because something might come and update it later.



```bash
chan@CMA:~/C_Programming/practice
$ make practice
cc     practice.c   -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
Name: Amity open: 09:00-17:00
Name: Craggy open: 09:00-17:00
Name: Isla Nublar open: 09:00-17:00
Name: Skull open: 09:00-17:00
Name: Shutter open: 09:00-17:00
Atlantis
Titchmarsh Island
Name: Atlantis
 open: 09:00-17:00
Name: Titchmarsh Island
 open: 09:00-17:00

```

Now that code works. Each time the user enters the name of an island, the create() function is storing it in a brand new string.



Q: If the island `struct` had a name array rather than a character pointer, would I need to use `strdup()` here?

A: No, each island `struct` would store its own copy, so you wouldn't need to make your own copy.



Q: So why would I want to use char pointers rather than char arrays in my data structures?

A: char pointers won't limit the amount of space you need to set aside for strings. If you use char arrays, you will need to decide in advance exactly how long your strings might need to be.



#### Pool Puzzle

```C
island *start = NULL;
island *i = NULL;
island *next = NULL;
char name[80];

// We'll keep looping until we don't get any more strings
for(; fgets(name, 80, stdin) != NULL; i = next){
    // This creates an island
    next = create(name);
    if(start == NULL){
        start = next;
    }
    if(i != NULL){
        i->next = next;
    } 
}
display(start);
```

Code Breakdown:

- `island *start = NULL`, This line initializes a pointer `start` to `NULL`. This pointer will eventually point to the first island in the list.
- `island *i = NULL`, This line initializes a pointer `i` to `NULL`. This pointer will be used to keep track of the current island in the list.
- `island *next = NULL`, This line initializes a pointer `next` to `NULL`. This pointer will be used to point to the next island to be added to the list.
- The for loop continues until we don't get any more strings from the Standard Input. At the end of each loop, set `i` to the next island we created which means that `i` always points to the last `island` that was added to the list.
- `next = create(name);` This line calls the `create` function with the name that was read from the standard input. The `create` function creates a new `island` with the given `name` and return a pointer to it. This pointer is then stored in `next`.
- `if(start == NULL) {start = next};` This `if` statement checks if `start` is `NULL`, which mean that the list is currently empty. If it is, `start` is set to `next`, which means that the first `island` in the list is the one that was just created. In other words, the first time through, `start` is set to `NULL`, so set it to the first island.
- `if (i != NULL) { i->next = next }` This if statement checks if `i` is not `NULL` which means that there is at least one island in the list. If there is, it sets the `next` field of the current `island` `i` to `next`, which adds the new `island` to the end of the list.



#### Free the memory when you're done

```C
void release(island *start){
    island *i = start;
    island *next = NULL;
    for(; i != NULL; i = next){
        next = i->next;
        free(i->name);
        free(i);
    }
}
```

Code Breakdown:

- `island *i = start;` - This line initializes a pointer `i` to `start`. This pointer will be used to traverse the list of islands.

- `island *next = NULL;` - This line initializes a pointer `next` to `NULL`. This pointer will be used to keep track of the next island in the list.

- `for (; i != NULL; i = next)` - This for loop continues as long as i is not NULL, which means that there are still islands left in the list. After each iteration, `i` is set to the current node and `next` will be set to the the next node before freeing the current node.

- `next = i->next;` - Inside the loop, this line sets `next` to `i->next`, which is the next island in the list. This is done before freeing `i` because freeing `i` will deallocate its memory, making `i->next` inaccessible.

  Before freeing the current node, save the pointer to the next node in `next`. This ensures that after freeing the current node, you still have a reference to the rest of the list.

- `free(i->name);` - This line frees the memory allocated for the name of the current island. The name was allocated with `strdup` in the create function, so it needs to be freed to avoid a memory leak.

- `free(i);` - This line frees the memory allocated for the current island. The island was allocated with `malloc` in the create function, so it needs to be freed to avoid a memory leak.

- After the for loop has finished, all memory allocated for the list of islands (both the islands themselves and their names) has been freed, and there are no memory leaks.



##### If we run the code

```bash
chan@CMA:~/C_Programming/practice
$ ./practice < trip1.txt
Name: Amity open: 09:00-17:00
Name: Craggy open: 09:00-17:00
Name: Isla Nublar open: 09:00-17:00
Name: Skull open: 09:00-17:00
Name: Shutter open: 09:00-17:00
Name: Delfino Isle
 open: 09:00-17:00
Name: Angel Island
 open: 09:00-17:00
Name: Wild Cat Island
 open: 09:00-17:00
Name: Neri's Island
 open: 09:00-17:00
Name: Great Todday
 open: 09:00-17:00
Name: Ramita de la Baya
 open: 09:00-17:00
Name: Island of the Blue Dolphins
 open: 09:00-17:00
Name: Fantasy Island
 open: 09:00-17:00
Name: Farne
 open: 09:00-17:00
Name: Isla de Muert
 open: 09:00-17:00
Name: Tabor Islandd
 open: 09:00-17:00
Name: Haunted Isle
 open: 09:00-17:00
Name: Sheena Island
 open: 09:00-17:00
chan@CMA:~/C_Programming/practice$ 
```

```bash
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_1$ 
make all
Compiling the main file...
gcc -c main.c 
Compiling the functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc main.o functions.o -o main 

chan@CMA:~/C_Programming/HFC/chapter_6/exercise_1$ 
./main < trip1.txt
Name: Amity open: 09:00-17:00
Name: Craggy open: 09:00-17:00
Name: Isla Nublar open: 09:00-17:00
Name: Skull open: 09:00-17:00
Name: Shutter open: 09:00-17:00
Name: Delfino Isle
 open: 09:00-17:00
Name: Angel Island
 open: 09:00-17:00
Name: Wild Cat Island
 open: 09:00-17:00
Name: Neri's Island
 open: 09:00-17:00
Name: Great Todday
 open: 09:00-17:00
Name: Ramita de la Baya
 open: 09:00-17:00
Name: Island of the Blue Dolphins
 open: 09:00-17:00
Name: Fantasy Island
 open: 09:00-17:00
Name: Farne
 open: 09:00-17:00
Name: Isla de Muert
 open: 09:00-17:00
Name: Tabor Islandd
 open: 09:00-17:00
Name: Haunted Isle
 open: 09:00-17:00
Name: Sheena Island
 open: 09:00-17:00
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_1$ 

```



It works. One thing worth noting in mind is that we had no way of knowing how long that file was going to be.

In this case, because we are just printing out the file, we could have simply printed it out without storing it all in memory. But because we do have it in memory, we're free to manipulate it. We could add in extra steps in the tour, or remove them. We could reorder to extend the tour.



#### Chapter 6 Final Exercise - SPIES Program

header.h

```C
typedef struct node {
    char *question;
    struct node *no;
    struct node *yes;
} node;

int yes_no(char *question);
node *create(char *question);
void release(node *n);
```

main.c

```C
#include <stdio.h>
#include <string.h>
#include "header.h"

int main(void) {
    char question[80];
    char suspect[20];

    // Create the initial decision tree
    node *start_node = create("Does suspect have a mustache");
    start_node->no = create("Loretta Barnsworth");
    start_node->yes = create("Vinny the Spoon");

    // Pointer to traverse the tree
    node *current;

    // do-while loop that continues running based on the user's response to "Run again".
    do {
        // Initialize current to point to the root node at the start of each iteration.
        current = start_node;
        while (1) {
            // Ask the current question.
            if (yes_no(current->question)) {
                // If the answer is yes and the 'yes' child exists, move to the 'yes' child.
                if (current->yes) {
                    current = current->yes;
                } else {
                    // If a suspect is identified (current->yes or current->no is NULL), print "SUSPECT IDENTIFIED".
                    printf("SUSPECT IDENTIFIED\n");
                    break;
                }
            } else if (current->no) {
                // If the answer is no and the 'no' child exists, move to the 'no' child.
                current = current->no;
            } else {
                // If the suspect is not identified, prompt the user for the new suspect's name and a distinguishing question.
                printf("Who's the suspect? ");
                fgets(suspect, 20, stdin);
                suspect[strcspn(suspect, "\n")] = '\0';  // Remove newline character

                // Make the yes-node the new suspect name.
                //This process effectively adds a new branch to the decision tree for the new suspect. The new question becomes the question in the current node, the 'yes' child becomes the new suspect, and the 'no' child becomes the old suspect/question.
                node *yes_node = create(suspect);
                current->yes = yes_node;

                // Make the no-node a copy of this question.
                node *no_node = create(current->question);
                current->no = no_node;

                // Then replace this question with the new question.
                printf("Give me a question that is TRUE for %s but not for %s? ", suspect, current->question);
                fgets(question, 80, stdin);
                question[strcspn(question, "\n")] = '\0';  // Remove newline character
                free(current->question);
                current->question = strdup(question);
                break;
            }
        }
    } while (yes_no("Run again"));

    // Release memory for the entire tree.
    release(start_node);

    return 0;
}

```

Code Breakdown:

1. **Create Initial Tree:**

   - `node *start_node = create("Does suspect have a mustache");`: Creates the root node with the initial question.

   - `start_node->no = create("Loretta Barnsworth");`: Creates a child node for the "no" answer.

   - `start_node->yes = create("Vinny the Spoon");`: Creates a child node for the "yes" answer.

2. **Traversal and User Interaction:**

   - The do-while loop ensures the program runs repeatedly based on user input.
   - `current = start_node;`: Sets current to the root node at the start of each iteration.
   - Inside the `while(1)` loop, the program:
   - Asks the current question using `yes_no(current->question)`.
   - Moves to the `yes` or `no` child node based on the user's response.
   - If a leaf node is reached (no child nodes), it prompts for a new suspect and question.
   - Updates the tree with the new suspect and question.

3. **Prompt for New Suspect and Question:**
   - `fgets(suspect, 20, stdin);`: Reads the new suspect's name from input.
   - `suspect[strcspn(suspect, "\n")] = '\0';`: Removes the newline character.
   - Creates `yes_node` for the new suspect and updates `current->yes`.
   - Creates `no_node` as a copy of the current question and updates `current->no`.
   - Prompts for a new question distinguishing the new suspect and updates `current->question`.

4. **Loop Continuation:**
   - `while (yes_no("Run again"));`: Asks if the user wants to run the program again.

5. **Release Memory:**
   - `release(start_node);`: Frees all allocated memory for the tree nodes.

functions.c

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "header.h"

int yes_no(char *question) {
    char answer[3];
    printf("%s? (y/n): ", question);
    // if the user choose to terminate the program, exit.
    if (fgets(answer, 3, stdin) == NULL) {
        printf("\n");
        exit(0);
    }
    // returns 1 (true) if the first character of the user's answer is 'y', and 0 (false) otherwise. This is how the function determines whether the user answered 'yes' or 'no' to the question.
    return answer[0] == 'y' || answer[0] == 'Y';
}

node *create(char *question) {
    // Allocate memory for a new node.
    node *n = malloc(sizeof(node));
    // Copy the question string into the node.
    n->question = strdup(question);
    // Initializes yes and no pointers to NULL.
    n->no = NULL;
    n->yes = NULL;
    //Return the newly created node
    return n;
}

void release(node *n) {
    // Recursively free memory for all nodes in the tree.
    if (n) {
        if (n->no) {
            release(n->no);
        }
        if (n->yes) {
            release(n->yes);
        }
        
        // Free the question string and the node itself.
        if (n->question) {
            free(n->question);
        }
        free(n);
    }
}

```



Makefile

```makefile
all: spies

spies: main.o functions.o 
	@echo "Compiling the final exe file..."
	gcc -g main.o functions.o -o spies 

main.o: main.c 
	@echo "Compiling the main file..."
	gcc -c main.c 

functions.o: functions.c 
	@echo "Compiling the functions file..."
	gcc -c functions.c 

clean: 
	@echo "Removing everything except the source files..."
	@rm spies main.o functions.o
```



Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_2
$ make all
Compiling the functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc -g main.o functions.o -o spies 

chan@CMA:~/C_Programming/HFC/chapter_6/exercise_2
$ valgrind --leak-check=full ./spies
==23531== Memcheck, a memory error detector
==23531== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==23531== Using Valgrind-3.18.1 and LibVEX; rerun with -h for copyright info
==23531== Command: ./spies
==23531== 
Does suspect have a mustache? (y/n): n
Loretta Barnsworth? (y/n): n
Who's the suspect? Hayden Fantucci
Give me a question that is TRUE for Hayden Fantucci
 but not for Loretta Barnsworth? Has a facial scar
Run again? (y/n): y
Does suspect have a mustache? (y/n): n
Has a facial scar
? (y/n): Y
Hayden Fantucci
? (y/n): y
SUSPECT IDENTIFIED
Run again? (y/n): Y
Does suspect have a mustache? (y/n): y
Vinny the Spoon? (y/n): y
SUSPECT IDENTIFIED
Run again? (y/n): N
==23531== 
==23531== HEAP SUMMARY:
==23531==     in use at exit: 19 bytes in 1 blocks
==23531==   total heap usage: 13 allocs, 12 frees, 2,287 bytes allocated
==23531== 
==23531== 19 bytes in 1 blocks are definitely lost in loss record 1 of 1
==23531==    at 0x4848899: malloc (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)
==23531==    by 0x491658E: strdup (strdup.c:42)
==23531==    by 0x10950A: create (in /home/chan/C_Programming/HFC/chapter_6/exercise_2/spies)
==23531==    by 0x10928B: main (in /home/chan/C_Programming/HFC/chapter_6/exercise_2/spies)
==23531== 
==23531== LEAK SUMMARY:
==23531==    definitely lost: 19 bytes in 1 blocks
==23531==    indirectly lost: 0 bytes in 0 blocks
==23531==      possibly lost: 0 bytes in 0 blocks
==23531==    still reachable: 0 bytes in 0 blocks
==23531==         suppressed: 0 bytes in 0 blocks
==23531== 
==23531== For lists of detected and suppressed errors, rerun with: -s
==23531== ERROR SUMMARY: 1 errors from 1 contexts (suppressed: 0 from 0)
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_2$ 

```



Visualization

```plaintext
"Does suspect have a mustache?"
|
├── yes: "Vinny the Spoon"
|
└── no: "Loretta Bransworth"
```



```plaintext
Does suspect have a mustache?
├── Yes: Vinny the Spoon
└── No: Has a facial scar?
    ├── Yes: Hayden Fantucci
    └── No: Loretta Barnsworth

```



But there's one problem. It was using almost **twice the amount of memory** it needed.

We have managed to detect it using `valgrind` tool which is used on the **Linux** OS. It can monitor the pieces of data that are allocated space on the heap. 

- It works by creating its own **fake version of malloc()**.
- When our program wants to allocate some heap memory, `valgrind` will intercept our calls to `malloc()` and `free()` and run its own versions of those functions.
- The `valgrind` version of `malloc()` will take note of which piece of code is calling it and which piece of memory it allocated.
- When the program ends, `valgrind` will report back on any data that was left on the heap and tell us where in our code the data was created.
- As we can see in our **makefile**, we used `gcc -g main.o functions.o -o spies` to tells the compiler to record the line numbers against the code it compiles.



If we look closely,

```bash
==23531==    definitely lost: 19 bytes in 1 blocks

==23531==   total heap usage: 13 allocs, 12 frees, 2,287 bytes allocated

==23531==    at 0x4848899: malloc (in /usr/libexec/valgrind/vgpreload_memcheck-amd64-linux.so)

==23531==    by 0x491658E: strdup (strdup.c:42)

==23531==    by 0x10950A: create (in /home/chan/C_Programming/HFC/chapter_6/exercise_2/spies)

==23531==    by 0x10928B: main (in /home/chan/C_Programming/HFC/chapter_6/exercise_2/spies)
```

- 19 bytes of memory were allocated but not freed.

- Looks like we allocated new pieces of memory 13 times, but freed only 12 of them.


Now that we've collected quite a few pieces of evidence.

1. Location

   We ran the code two times. The first time, there was no problem. The memory leak only happened when we entered a new suspect name. That means the leak can't be in the code that ran the first time. The problem lies in this section of code.

   ```C
   else if (current->no)
               {
                   current = current->no;
               }
               // If the suspect is not identified, prompt the user for the new suspect's name and a distinguishing question.
               // Create new nodes for the suspect and update the current node's 'yes' and 'no' pointers accordingly.
               else
               {
                   // Make the yes-node the new suspect name
                   printf("Who's the suspect? ");
                   fgets(suspect, 20, stdin);
   
                   node *yes_node = create(suspect);
                   current->yes = yes_node;
   
                   // Make the no-node a copy of this question
                   node *no_node = create(current->question);
                   current->no = no_node;
   
                   // Then replace this question with the new question
                   printf("Give me a question that is TRUE for %s but not for %s? ", suspect, current->question);
                   fgets(question, 80, stdin);
                   free(current->question);
                   current->question = strdup(question);
                   break;
               }
   ```

2.  Clues from valgrind

   When we ran the code through valgrind and added a single suspect, the program allocated 13 times, but only released 12 times. It told us that there were 19 bytes of data left on the heap when the program ended.

   - How many pieces of data were left on the heap?

     - There is one piece of data.

   - What was the piece of data left on the heap?

     - The string "Loretta Bransworth". It's 18 characters with a string terminator.

   - Which line or lines of code caused the leak?

     - The create() functions themselves don't cause leaks because they didn't on the first pass, so it must be this strdup() line:

     - ```C
       current->question = strdup(question);
       ```

   - How do we plug the leak?

     - If current->question is already pointing to something on the heap, free that before allocating a new question:
     - free(current->question);
     - current->question = strdup(question);



Now that we have added the fix to the code, it's time to run the code through valgrind again.

```bash
chan@CMA:~/C_Programming/HFC/chapter_6/exercise_2
$ valgrind --leak-check=full ./spies
==7591== Memcheck, a memory error detector
==7591== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==7591== Using Valgrind-3.18.1 and LibVEX; rerun with -h for copyright info
==7591== Command: ./spies
==7591== 
Does suspect have a mustache? (y/n): n
Loretta Barnsworth? (y/n): n
Who's the suspect? Hanna
Give me a question that is TRUE for Hanna
 but not for Loretta Barnsworth? Has a facial scar
Run again? (y/n): y
Does suspect have a mustache? (y/n): n
Has a facial scar
? (y/n): y
Hanna
? (y/n): y
SUSPECT IDENTIFIED
Run again? (y/n): n
==7591== 
==7591== HEAP SUMMARY:
==7591==     in use at exit: 0 bytes in 0 blocks
==7591==   total heap usage: 13 allocs, 13 frees, 2,277 bytes allocated
==7591== 
==7591== All heap blocks were freed -- no leaks are possible
==7591== 
==7591== For lists of detected and suppressed errors, rerun with: -s
==7591== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)

chan@CMA:~/C_Programming/HFC/chapter_6/exercise_2$ 

```

The leak is fixed.

- Spot when leaks happen.
- Identify the location where they happen.
- Check to make sure the leak is fixed.



Q: valgrind said the leaked memory was created on line 46, but the leak was fixed on a completely different line. How come?

A: The "Loretta..." data was put onto the heap on line 46, but the leak happened when the variable pointing to it (current->question) was reassigned without freeing it. Leaks don't happen when data is created; they happen when the program loses all references to the data.



Q: How does valgrind intercept calls to malloc() and free()?

A: The `malloc()` and `free()` functions are contained in the C Standard Library. But valgrind contains a library with its own versions of `malloc()` and `free()`. When you run a program with valgrind, your program will be using valgrind's functions, rather than the ones in the C Standard Library.



Q: Why doesn't the compiler always include debug information when it compiles code?

A: Because debug information will make your executable larger and it may also make your program slightly slower.

---



### Chapter 7 - Advanced Functions

### Exercises



#### Exercise - 1

Complete the find() function so it can track down all the sports fans in the list who don't also share a passion for Bieber.

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>

nt NUM_ADS = 7;
char *ADS[] = {
    "William: SBM GSOH likes sports, TV, dining",
    "Matt: SWM NS likes art, movies, theater",
    "Luis: SLM ND likes books, theater, art",
    "Mike: DWM DS likes trucks, sports and bieber",
    "Peter: SAM likes chess, working out and art",
    "Josh: SJM likes sports, movies and theater",
    "Jed: DBM likes theater, books and dining"};

void find();

int main(){
    find();
    return 0;
}

void find(){
    int i;
    puts("Search results:");
    puts("--------------------");
    
    for(i = 0; i < NUM_ADS; i++){
        if(strstr(ADS[i], "sports") && !strstr(ADS[i], "bieber")){
            printf("%s\n", ADS[i]);
        }
    }
    puts("--------------------");
}
```

Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_7/exercise_1
$ make main
cc     main.c   -o main

chan@CMA:~/C_Programming/HFC/chapter_7/exercise_1
$ ./main
Search results: 
---------------------------
William: SBM GSOH likes sports, TV, dining
Josh: SJM likes sports, movies and theater
---------------------------

chan@CMA:~/C_Programming/HFC/chapter_7/exercise_1$ 

```



#### Exercise 2

practice.h

```C
extern int num_ADS;
extern char *ADS[];

int sports_no_bieber(char *s);

int sports_or_workout(char *s);

int ns_theater(char *s);

int arts_theater_or_dining(char *s);

void find(int (*match)(char*));

```

 `void find(int (*match)(char *))` -`find` is a function that takes one argument: a pointer to a function. This function, in turn, accepts a `char *` as its parameter and returns an `int`.

functions.c

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"

int NUM_ADS = 7;
char *ADS[] = {
    "William: SBM GSOH likes sports, TV, dining",
    "Matt: SWM NS likes art, movies, theater",
    "Luis: SLM ND likes books, theater, art",
    "Mike: DWM DS likes trucks, sports and bieber",
    "Peter: SAM likes chess, working out and art",
    "Josh: SJM likes sports, movies and theater",
    "Jed: DBM likes theater, books and dining"
};

int sports_no_bieber(char *s){
    return strstr(s, "sports") && !strstr(s, "bieber");
}

int sports_or_workout(char *s){
    return strstr(s, "sports") || strstr(s, "working out");
}

int ns_theater(char *s){
    return strstr(s, "NS") && strstr(s, "theater");
}

int arts_theater_or_dining(char *s){
    return strstr(s, "art") || strstr(s, "theater") || strstr(s, "dining");
}

void find(int (*match)(char *)){
    int i;
    puts("Search result: ");
    puts("--------------------");
    for(i = 0; i < NUM_ADS; i++){
        if(match(ADS[i])){
            printf("%s\n", ADS[i]);
        }
    }
    puts("--------------------");
    printf("\n");
}
```

practice.c (main file)

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"
int main(){
    printf("Sports but no Bieber:\n");
    find(sports_no_bieber);
    
    printf("Sports or workout:\n");
    find(sports_or_workout);
    
    printf("non-smoking and theater:\n");
    find(ns_theater);
    
    printf("Arts, or theater or dining:\n");
    find(arts_theater_or_dining);
    
    return 0;
}
```

Code Execution

```bash
chan@CMA:~/C_Programming/practice
$ make all
Compiling functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc practice.o functions.o -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
Sports but no Bieber: 
Search result: 
--------------------
William: SBM GSOH likes sports, TV, dining
Josh: SJM likes sports, movies and theater
--------------------

Sports or workout:
Search result: 
--------------------
William: SBM GSOH likes sports, TV, dining
Mike: DWM DS likes trucks, sports and bieber
Peter: SAM likes chess, working out and art
Josh: SJM likes sports, movies and theater
--------------------

non-smoking and theater:
Search result: 
--------------------
Matt: SWM NS likes art, movies, theater
--------------------

Arts, or theater or dining:
Search result: 
--------------------
William: SBM GSOH likes sports, TV, dining
Matt: SWM NS likes art, movies, theater
Luis: SLM ND likes books, theater, art
Peter: SAM likes chess, working out and art
Josh: SJM likes sports, movies and theater
Jed: DBM likes theater, books and dining
--------------------


```



### Exercise - 3

practice.h

```C
int compare_scores(const void *score_a, const void *score_b);

int compare_scores_desc(const void *score_a, const void *score_b);

int compare_areas(const void *a, const void *b);

int compare_names(const void *a, const void *b);

int compare_areas_desc(const void *a, const void *b);

int compare_names_desc(const void *a, const void *b);
```

functions.c

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"

// Sort integer scores with the smalles number first
int compare_scores(const void *score_a, const void *score_b){
    int a = *(int *)score_a;
    int b = *(int *)score_b;
    return a - b;
}

// Sort integer scores with the largest number first
int compare_scores_desc(const void *score_a, const void *score_b){
    int a = *(int *)score_a;
    int b = *(int *)score_b;
    
    // If we subtract the numbers the other way around, we'll reverse the order of the final sort
    return b - a;
}

typedef struct{
    int width;
    int height;
} rectangle;

// sort the rectangles in area order, smallest first
int compare_areas(const void *a, const void *b){
    // First convert the pointers to the correct type.
    rectangle *ra = (rectangle *)a;
    rectangle *rb = (rectangle *)b;
    
    // Then calculate the areas.
    int area_a = (ra->width * ra->height);
    int area_b = (rb->width * rb->height);
    
    // Then use the subtraction trick
    return area_a - area_b;
}

// Sort the list of names in alphabetical order. case-sensitive
int compare_names(const void *a, const void *b){
    // A string is a pointer to a char, so the pointers you're given are pointers to pointers.
    char **sa = (char **)a;
    char **sb = (char **)b;
    
    // We need to use the * operator to find the actual strings.
    return strcmp(*sa,*sb);
}

// Sort the rectangles in area order, largest first.
int compare_areas_desc(const void *a, const void *b){
    return compare_areas(b, a);
    
    // Or you could have used -compare_areas(a,b);
}


// Sort a list of names in reverse alphabetical order. case-sensitive
int compare_names_desc(const void *a, const void *b){
    return compare_names(b, a);
    
    // Or you could have used -compare_names(a,b);
}
```

practice.c

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"

int main(void){
    int scores[] = {543, 323, 32, 554, 11, 3, 112};
    int i;
    qsort(scores, 7, sizeof(int), compares_scores_desc);
    puts("These are the scores in order: ");
    for(i = 0; i < 7; i++){
        printf("Score = %i\n", scores[i]);
    }
    
    char *names[] = {"Karen", "Mark", "Brett", "Molly"};
    qsort(names, 4, sizeof(char *), compare_names);
    puts("These are the names in order: ");
    for(i = 0; i < 4; i++){
        printf("%s\n", names[i]);
    }
    return 0;
}
```

To get the number of elements in the array, instead of manually typing it, we can divide **the total size of the array** by **the size of one element**.

```C
int num_elements = sizeof(scores) / sizeof(scores[0]);
```



Code Execution:

```bash
chan@CMA:~/C_Programming/practice
$ make all
Compiling practice file...
gcc -c practice.c 
Compiling functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc practice.o functions.o -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
These are the scores in order: 
Score = 554
Score = 543
Score = 323
Score = 112
Score = 32
Score = 11
Score = 3
These are the names  in order: 
Brett
Karen
Mark
Molly

```

**Note: Sorting explanation in `HFCTheories.md`**

Q: I don't understand the comparator function for the array of strings. What does `char **` mean?

Q: Each item in a string array is a `char` pointer `(char *)`. When `qsort()` calls the comparator function, it sends pointers to two elements in the array. That means the comparator receives two pointers-to-pointers-to-char. In C notation, each value is a `char **`.



Q: OK, but when I call the `strcmp()` function, why does the code say `strcmp(*a, *b)` ? Why not `strcmp(a,b)`?

A: `a` and `b` are of type `char **`. The `strcmp()` function needs values of type `char*`.



Q: Does `qsort()` create  a sorted version of an array?

A: It doesn't make a copy, it actually modifies the original array.



### Exercise - 4

#### Automating the Dear John letters

Imagine you're writing a mail-merge program to send out different types of messages to different people. One way of creating the data for each response is with a `struct` like this:

practice.h

```C 
enum response_type {
    DUMP,
    SECOND_CHANCE,
    MARRIAGE
};

typedef struct{
    char *name;
    enum response_type type;
} response;

void dump(response r);

void second_chance(response r);

void marriage(response r);
```

The `enum` gives you the names for each of the three types of response you'll be sending out, and that response type can be recorded against each response. Then you'll be able to use your new `response` data type by calling one of these three functions for each type of response.

functions.c

```C
void dump(response r){
    printf("Dear %s,\n", r.name);
    puts("Unfortunately your last data contacted us to");
    puts("say that they will not be seeing you again");
}

void second_chance(response r){
    printf("Dear %s,\n", r.name);
    puts("Good news: your last data has asked us to");
    puts("arrange another meeting. Please call ASAP.");
}

void marriage(response r){
    printf("Dear %s,\n", r.name);
    puts("Congratulations! Your last data has contacted");
    puts("us with a proposal of marriage.");
}
```



practice.c (main file)

```C
int main(void){
    response r[] = {
        {"Mike", DUMP}, {"Luis", SECOND_CHANCE}, {"Matt", SECOND_CHANCE}, {"William", MARRIAGE};
    };
    int i;
    for(i = 0; i < 4; i++){
        switch(r[i].type){ // Testing the type field each time
            case DUMP:
                dump(r[i]);
                break;
            case SECOND_CHANCE:
                second_chance(r[i]);
                break;
            default:
                marriage(r[i]);
        }
    }
    return 0;
}
```



Code Execution

```bash
chan@CMA:~/C_Programming/practice
$ ./practice
Dear Mike,
Unfortunately your last date contacted us to
say that they will not be seeing you again
Dear Luis,
Good news: your last date has asked us to
arrange another meeting. Please call ASAP.
Dear Matt,
Good news: your last date has asked us to
arrange another meeting. Please call ASAP.
Dear William,
Congratulations! Your last date has contacted
us with a proposal of marriage.

chan@CMA:~/C_Programming/practice$ 

```

Well, it's good that it worked but there is quite a lot of code in there just to call a function for each piece of `response` data.

And what will happen if you add a fourth response type? You'll have to change every section of your program that looks like this. Soon, you will have a lot of code to maintain and it might go wrong.



#### Create an array of function pointers

The trick is to create an array of function pointers that match the different response types.

```C
void (*replies[])(response) = {dump, second_chance, marriage};

// void - each function in the array will be a void function
// (*replies[]) - The variable will be called "replies" and it's not just a function pointer; it's a whole array of them.
// (response) - Just one parameter with type "response".

```

| Return type | (*Pointer variable) | (Param types) |
| ----------- | ------------------- | ------------- |
| void        | (*replies[])        | (response)    |

#### But how does an array help?

It contains a set of function names that are in exactly the same order as the types in the `enum`:

```C
enum response_type{
    DUMP,
    SECOND_CHANCE,
    MARRIAGE
};
```

This is really important because when C creates an `enum`, it gives each of the symbols a number starting at 0.

- So `DUMP == 0`, `SECOND_CHANCE == 1`, and `MARRIAGE == 2`. And that's really neat because it means you can get a pointer to one of your sets of functions using a `response_type`.

```C
replies[SECOND_CHANCE] = second_chance;

// replies - this is your "replies" array of functions
// SECOND_CHANCE - has the  value 1.
// second_chance - it's equal to the name of the second_chance function.

// void (*replies[])(response) = {dump, second_chance, marriage};
// replies[0] will run the funciton "dump"
// replies[1] will run the function "second_chance"
// replies[2] will run the function "marriage"

// replies[0] == DUMP
// replies[1] == SECOND_CHANCE
// replies[2] == MARRIAGE .Since enum gives each of the symbols a number starting at 0 as mentioned above.
```



#### Updated function for Exercise 4

practice.c (main file)

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include "practice.h"

void (*replies[])(response) = {dump, second_chance, marriage};

int main(void){
    response r[] = {
        {"Mike", DUMP}, {"Luis", SECOND_CHANCE}, {"Matt", SECOND_CHANCE}, {"William", MARRIAGE}
    };
    int i;
    for(i = 0; i < 4; i++){
        (replies[r[i].type])(r[i]);
        // adding a * after the opening parenthesise will also work the same way.
        // (*replies[r[i].type])(r[i])
    }
}
```



Code Execution:

```bash
chan@CMA:~/C_Programming/practice
$ make all
Compiling practice file...
gcc -c practice.c 
Compiling the final exe file...
gcc practice.o functions.o -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
Dear Mike,
Unfortunately your last date contacted us to
say that they will not be seeing you again
Dear Luis,
Good news: your last date has asked us to
arrange another meeting. Please call ASAP.
Dear Matt,
Good news: your last date has asked us to
arrange another meeting. Please call ASAP.
Dear William,
Congratulations! Your last date has contacted
us with a proposal of marriage.

chan@CMA:~/C_Programming/practice$
```



Now instead of an entire `switch` statement, you just have this: 

```C
(replies[r[i].type])(r[i]);
```

If we have to call the response functions at several places in the program, we won't have to copy a lot of code. 

And if we decide to add a new type and a new function, we can just add it to the array:

```C
enum response_type {
    DUMP,
    SECOND_CHANCE,
    MARRIAGE,
    LAW_SUIT
};

void (*replies[])(response) = {dump, second_chance, marriage, law_suit};
```

Arrays of function pointers can make our code much easier to manage. They are designed to make our code scalable by making it shorter and easier to extend.



#### Variadic Functions

**Theories and Code Breakdown are in `HFCTheories.md`.**

functions.c

```C
#include <stdarg.h>
#include "practice.h"

void print_ints(int args,...){
    va_list ap;
    va_start(ap, args);
    int i;
    for(i = 0; i < args; i++){
        printf("arguments: %i\n", va_arg(ap, int));
    }
    va_end(ap);
}
```

practice.h

```C
void print_ints(int args, ...);
```

practice.c (main)

```C
#include "practice.h"

int main(){
    print_ints(3, 79, 101, 32);
    return 0;
}
```

Code Execution:

```bash
chan@CMA:~/C_Programming/practice
$ make all
Compiling practice file...
gcc -c practice.c 
Compiling functions file...
gcc -c functions.c 
Compiling the final exe file...
gcc practice.o functions.o -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
arguments: 79
arguments: 101
arguments: 32

```



#### Exercise - 5

The guys in the Head First Lounge want to create a function that can return the total cost of a round of drinks:

functions.c

```C
#include <stdarg.h>
#include "practice.h"

double price(enum drink d){
    switch(d){
        case MUDSLIDE:
            return 6.79;
        case FUZZY_NAVEL:
            return 5.31;
        case MONKEY_GLAND:
            return 4.82;
        case ZOMBIE:
            return 5.89;
    }
    return 0;
}

double total(int args, ...){
    double total = 0;
    va_list ap;
    va_start(ap, args);
    int i;
    for(i = 0; i < args; i++){
        enum drink d = va_arg(ap, enum drink);
        total += price(d);
    }
    va_end(ap);
    return total;
}
```

practice.h

```C
enum drink{
    MUDSLIDE,
    FUZZY_NAVEL,
    MONKEY_GLAND,
    ZOMBIE
};

double price(enum drink d);

double total(int args, ...);
```

practice.c (main)

```C
#include "practice.h"

int main(){
    printf("Price is %.2f\n", total(2, MONKEY_GLAND, MUDSLIDE));
    printf("Price is %.2f\n", total(3, MONKEY_GLAND, MUDSLIDE, FUZZY_NAVEL));
    printf("Price is %2.f\n", total(1, ZOMBIE));
    return 0;
}
```

Code Execution

```bash
chan@CMA:~/C_Programming/practice
$ make all
Compiling practice file...
gcc -c practice.c 
Compiling the final exe file...
gcc practice.o functions.o -o practice

chan@CMA:~/C_Programming/practice
$ ./practice
Price is 11.61
Price is 16.92
Price is 5.89

chan@CMA:~/C_Programming/practice
$ 

```

---

### Chapter 7 - Exercise 1

Encrypt function encrypt the contents of a string. Checksum function check if the contents of a string have been modified.

**Detailed explanations are in `HFCTheories.md`.**

encrypt.h

```C
void encrypt(char *message);
```

encrypt.c

```C
#include "encrypt.h"

void encrypt(char *message){
    while(*message){
        *message = *message ^ 31;
        message++;
    }
}
```

checksum.h

```C
int checksum(char *message);
```

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

main.c

```C
#include <stdio.h>
#include "encrypt.h"
#include "checksum.h"

int main(){
    char s[] = "Speak friend and enter";
    char t[] = "abc";
    encrypt(s);
    printf("Encrypted to '%s'\n",s);
    printf("Checksum is %i\n", checksum(s));
    // If we call encrypt() again, it will decrypt it.
    encrypt(s);
    printf("Decrypted back to '%s'\n", s);
    printf("Checksum is %i\n", checksum(s));
    
    encrypt(t);
    printf("Encrypted to '%s'\n", t);
    printf("Checksum is %i\n", checksum(t));
    encrypt(t);
    printf("Decrypted back to '%s'\n", t);
    printf("Checksum is %i\n", checksum(t));
    return 0;
}
```

Makefile

```makefile
all: final

final: encrypt.o checksum.o main.o
	@echo "Compiling the final exe file..."
	gcc encrypt.o checksum.o main.o -o final

main.o: main.c
	@echo "Compiling the main file..."
	gcc -c main.c

encrypt.o: encrypt.c 
	@echo "Compiling the encrypt file..."
	gcc -c encrypt.c 

checksum.o: checksum.c 
	@echo "Compiling the checksum file..."
	gcc -c checksum.c 

clean: 
	@echo "Removing everything except the source files..."
	@rm encrypt.o checksum.o final

```



Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_8/exercise_1
$ make all
Compiling the main file...
gcc -c main.c
Compiling the final exe file...
gcc encrypt.o checksum.o main.o -o final

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_1
$ ./final
Encrypted to 'Loz~t?ymvzq{?~q{?zqkzm'
Checksum is 89561741
Decrypted back to 'Speak friend and enter'
Checksum is 89548156
Encrypted to '~}|'
Checksum is 382
Decrypted back to 'abc'
Checksum is 107

```

 #### Exercise - 2

Working with archive. 

encrypt.h

```C
void encrypt(char *message);
```

checksum.h

```C
int checksum(char *message);
```



bank_vault.c

```C
#include <stdio.h>
#include <string.h>
#include "encrypt.h"
#include "checksum.h"

int main(void)
{
    char t[] = "abc";

    encrypt(t);
    printf("Encrypted to '%s'\n", t);
    printf("Checksum is '%d'\n", checksum(t));
    encrypt(t);
    printf("Decrypted back to '%s'\n", t);
    printf("Checksum is '%d'\n", checksum(t));
    return 0;
}
```

checksum.c

```C
#include <stdio.h>
#include "checksum.h"

int checksum(char *message)
{
    int c = 0;
    while (*message)
    {
        c += c ^ (int)(*message);
        message++;
    }
    return c;
}
```

encrypt.c

```C
#include <stdio.h>
#include "encrypt.h"

void encrypt(char *message)
{
    while (*message)
    {
        *message = *message ^ 31;
        message++;
    }
}
```

Makefile

```makefile
all: bank_vault

bank_vault: bank_vault.o libhfsecurity.a 
	gcc bank_vault.c -I. -L. -lhfsecurity -o bank_vault
	
bank_vault.o: bank_vault.c
	gcc -c bank_vault.c -I. -o bank_vault.o

encrypt.o: encrypt.c 
	gcc -c encrypt.c -o encrypt.o

checksum.o: checksum.c 
	gcc -c checksum.c -o checksum.o


libhfsecurity.a: encrypt.o checksum.o
	ar -rcs libhfsecurity.a encrypt.o checksum.o
```

`-I.`, `-L.` - because the header files and the archive are in the current directory. Otherwise, we would have to specify the pathname like `-I../header_files/`.

`-lhfsecurity` - We need `-lhfsecurity` because the archive is called `libhfsecurity.a`.

Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_8/exercise_2
$ make all
gcc -c bank_vault.c -I. -I../header_files -o bank_vault.o
gcc bank_vault.c -I. -L. -lhfsecurity -o bank_vault

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_2
$ ./bank_vault
Encrypted to '~}|'
Checksum is '382'
Decrypted back to 'abc'
Checksum is '107'

chan@CMA:~/C_Programming/HFC/chapter_8/exercise_2$ 


```



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





---

### Chapter 9 - processes and system calls

#### Chapter 9 - Exercise 1

main.c

```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

char *now(){
    time_t t;
    time(&t); // Get the current calender time
    return asctime(localtime(&t));
    // Convert to local time and then to string
}

int main(){
    char comment[80];
    char cmd[120];
    fgets(comment, 80, stdin); // Read a comment from the user
    sprintf(cmd, "echo '%s %s' >> reports.log", comment, now()); // Format the command string
    
    system(cmd); // Execute the command to append to reports.log
    return 0;
}
```

Code Breakdown:

- `#include <time.h>`: Includes the time library, which provides functions for manipulating and formatting dates and times.
- `char *now()`: Declares a function named `now` that returns a pointer to a character (a string).
- `time_t t`: Declares a variable `t` of type `time_t`, which is typically used to store calendar time.
- `time(&t)`: Fills `t` with the current calendar time.
- **`localtime`**: Converts calendar time (`time_t`) to local time (`struct tm`).
- **`asctime`**: Converts local time (`struct tm`) to a human-readable string.
- `return asctime(localtime(&t))`: Converts the calendar time `t` to local time, formats it as a human-readable string and returns this string.
- `char comment[80]`: Declares an array `comment` to hold up to 79 characters of input plus a null terminator.

- `fgets(comment, 80, stdin)` - Using `fgets` for unstructured text. Reads up to 79 characters from the standard input (keyboard) into the `comment` array. It includes a newline character if there's space, and always null-terminates the string.
- `sprintf(cmd, "echo '%s %s' >> reports.log", comment, now())` - 
  - `sprintf` will print the characters to a string. it formats the string: `"echo 'comment current_time' >> reports.log"`, storing it in `cmd`.
  - `cmd` - The formatted string will be stored in the `cmd` array.
  - `"echo '%s %s' >> reports.log"` - This is the command template. The command will append the comment to a file.
  - `comment` - The comment will appear first.
  - `now()` - The timestamp appears second. `now()` is called which returns the current date and time as a string.
- `system(cmd)` - This runs the contents of the `cmd` string. Executes the command stored in `cmd` as if it were typed in the shell. This command appends the `comment` and current time to a file named `reports.log`.

Code Execution:

```bash
chan@CMA:~/C_Programming/HFC/chapter_9/exercise_1
$ ./main
Checked in Crom - a compound interest program.

chan@CMA:~/C_Programming/HFC/chapter_9/exercise_1
$ ./main
Blue Leader reports breach in jet walls.

chan@CMA:~/C_Programming/HFC/chapter_9/exercise_1$ 
```

reports.log

```markdown
Checked in Crom - a compound interest program.
 Fri Jun 14 22:54:34 2024

Blue Leader reports breach in jet walls.
 Fri Jun 14 22:55:34 2024
```



Q: Does the `system()` function get compiled into my program?

A: No. The `system()` function -- like all system calls -- doesn't live in our program. It lives in the main OS.



Q: So, when I make a system call, I'm making a call to some external piece of code, like a library?

A: Kind of. But the details depend on the OS. On some OS, the code for a system call lives inside the kernel of the OS. On other OS, it might simply be stored in some dynamic library.



#### Exercise 2 - Chapter 9 (`exec` functions)

`example.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){
    printf("PID of example.c = %d\n", getpid());
    
    // arguments for the new program
    char *args[] = {"./hello", "Hello", "C", "Programming", NULL};
    
    // Replace the current process image with a new one
    if(execv("./hello", args) == -1){
        // if execv fails, print this error.
        perror("execv failed");
    }
    printf("Back to example.c");
    return 0;
}

```

`hello.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[]){
    printf("We are in Hello.c\n");
    printf("PID of hello.c = %d\n", getpid());
    
    // printing the argument counts.
    printf("argc = %d\n", argc);
    
    // Print the arguments passed to this program.
    for(int i = 0; i < argc; i++){
        printf("argv[%d] = %s\n",i, argv[i]);
    }
    return 0;
}
```

Makefile

```make
CC = clang
CFLAGS = -Wall -Wextra -g

example: example.c
	$(CC) $(CFLAGS) example.c -o example
	
hello: hello.c
	$(CC) $(CFLAGS) hello.c -o hello
```

Code execution:

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/example
$ make example
make: 'example' is up to date.

chan@CMA:~/C_Programming/HFC/chapter_9/example
$ make hello
clang -Wall -Wextra -g hello.c -o hello

chan@CMA:~/C_Programming/HFC/chapter_9/example
$ ./example
PID of example.c = 31661
We are in Hello.c
PID of hello.c = 31661
argc = 4
argv[0] = ./hello
argv[1] = Hello
argv[2] = C
argv[3] = Programming

```

- argc - argument count
- argv - argument vector



#### Exercise 3 - Chapter 9 (diner info)

`diner_info.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(){
    printf("We are inside diner_info.c.\n");
    printf("PID of diner_info.c: %d\n", getpid());
    
    char *my_env[] = {"JUICE=peach and apple", NULL};
    
    char *args[] = {"./main", "Fears", "quiet", "magic", "4", NULL};
    
    if(execve("./main", args, my_env)){
        perror("execve failed!");
    }
    
    printf("Back in diner_info.c.\n");
    
    return 0;
}
```



`main.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[]){
    printf("We are now inside main.c.\n");
    printf("PID of main.c: %d\n", getpid());
    
    int i;
    for(i = 0; i < argc; i++){
        printf("argv[%d] = %s\n", i, argv[i]);
    }
    
    printf("Diners: %s\n", argv[4]);
    printf("Juice: %s\n", getenv("JUICE"));
    
    return 0;
}
```

`Makefile`

```makefile
CC = clang
CFLAGS = -Wall -Wextra -g

main: main.c 
	$(CC) $(CFLAGS) main.c -o main

diner_info: diner_info.c
	$(CC) $(CFLAGS) diner_info.c -o diner_info
	
clean:
	rm -f main diner_info
```



Code Execution:

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/diner_info
$ make main
clang -Wall -Wextra -g main.c -o main

chan@CMA:~/C_Programming/HFC/chapter_9/diner_info
$ make diner_info
make: 'diner_info' is up to date.

chan@CMA:~/C_Programming/HFC/chapter_9/diner_info
$ ./diner_info

We are now inside diner_info.c
PID of diner_info.c: 6543
We are now inside main.c.
PID of main.c: 6543
argv[0]: ./main
argv[1]: Fears
argv[2]: quiet
argv[3]: magic
argv[4]: 4
Diners: 4
Juice: peach and apple

chan@CMA:~/C_Programming/HFC/chapter_9/diner_info$ 


```



#### Exercise 4 - Chapter 9 

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <string.h>

int main(){
    if(execl("/sbin/ifconfig", "/sbin/ifconfig", NULL) == -1){
        if(execlp("ipconfig", "ipconfig", NULL) == -1){
            fprintf("stderr", "Cannot run ipconfig: %s\n", strerr(errno));
            return -1;
        }
    }
}
```

 Code Breakdown:

- `#include <unistd.h>` - We need this for the `exec()` functions.
- `#include <errno.h>` - We need this for the `errno` variable.
- `#include <string.h>` - This will let us display errors with `strerror()`.
- `execl("/sbin/ifconfig", "/sbin/ifconfig", NULL) == -1` - We use `execl()` because we have the path to the program file.
- If `execl()` returns -1, it failed, so we should probably look for `ipconfig`.
- `execlp()` will let us find the `ipconfig`  command on the path.
- The `strerror()` function will display any problems.

Code Execution:

```sh
chan@CMA:~/C_Programming/test
$ make all
make: Nothing to be done for 'all'.

chan@CMA:~/C_Programming/test
$ ./final

We are in hello.c
PID of hello.c: 8147
strcmp(str1, str2): -15
strcmp(str1,str3): -33
s1: Xbcdef
s2: abcdef
enp2s0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.105  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::19f0:82d1:7ed0:8796  prefixlen 64  scopeid 0x20<link>
        ether e4:a8:df:ba:4d:a0  txqueuelen 1000  (Ethernet)
        RX packets 31195  bytes 22229071 (22.2 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 33688  bytes 31468061 (31.4 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 97624  bytes 11603767 (11.6 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 97624  bytes 11603767 (11.6 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

wlo1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.1.124  netmask 255.255.255.0  broadcast 192.168.1.255
        inet6 fe80::fb0b:eac9:3edd:121a  prefixlen 64  scopeid 0x20<link>
        ether 38:d5:7a:65:22:fb  txqueuelen 1000  (Ethernet)
        RX packets 3580  bytes 1009135 (1.0 MB)
        RX errors 0  dropped 407  overruns 0  frame 0
        TX packets 1761  bytes 184536 (184.5 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0


```

---

#### Exercise 5 - Chapter 9 (Mixed Messages Solution)

`main.c`

```C
int main(){
    char *my_env[] = {"FOOD=coffee", NULL};
    
    if(execle("./coffee", "./coffee", "donuts", NULL, my_env) == -1){
        fprintf(stderr, "Can't run process 0: %s\n", strerror(errno));
        return 1;
    }
 	return 0;   
}
```

`coffee.c`

```C
int main(int argc, char *argv[]){
    char *w = getenv("EXTRA"); // Try to get the value of the environment variable "EXTRA".
    if(!w) // if "EXTRA" is not set,
        w = getenv("FOOD"); // then try to get the value of the environment variable "FOOD"
    if(!w) // if neither "EXTRA" nor "FOOD" is set,
        w = argv[argc - 1]; // use the last command-line argument as the value for w.
    
    char *c = getenv("EXTRA"); // Try to get the value of the environment variable "EXTRA".
    if(!c) // If "EXTRA" is not set,
        c = argv[argc - 1]; // use the last command-line argument as the value for c.
    printf("%s with %s\n", c, w);
    return 0; // Return 0 to indicate successful execution.
}
```

Code Execution for the main program above:

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/coffee
$ ./main
donuts with coffee

```

for this code in `main.c`: (2nd)

```C
char *my_env[] = {"FOOD=donuts", NULL};

    if (execle("./coffee", "coffee", NULL, my_env) == -1)
    {
        fprintf(stderr, "Can't run process 3: %s\n", strerror(errno));
        return 1;
    }
```

Code Execution will be: (2nd)

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/coffee
$ ./main
coffee with donuts
```

for this code in `main.c`: (3rd)

```C
	if (execl("./coffee", "coffee", NULL) == -1)
    {
        fprintf(stderr, "Can't run process 3: %s\n", strerror(errno));
        return 1;
    }
```

Code Execution will be: (3rd)

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/coffee
$ ./main
coffee with coffee
```

for this code in `main.c`: (4th)

```C
char *my_env[] = {"FOOD=donuts", NULL};	
if (execle("./coffee", "./coffee", "cream", NULL, my_env) == -1)
    {
        fprintf(stderr, "Can't run process 3: %s\n", strerror(errno));
        return 1;
    }
```

Code Execution will be: (4th)

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/coffee
$ ./main
cream with donuts

```

---

 ### Chapter 10 - Interprocess communication



#### Chapter 10 - Exercise 1

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <sys/type.h>
#include <sys/wait.h>

void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(1);
    // exit(1) will terminate our program with status 1 immediately.
}

int main(int argc, char *argv[]){
    // Check if the correct number of arguments is provided.
    if(argc < 2){
        error("Please provide a search phrase.");
    }
    
    // Get the search phrase from the command line arguments
    char *phrase = argv[1];
    
    // Set up an environment variable for the RSS feed URL
    char *vars[] = {"RSS_FEED=http://rss.cnn.com/rss/edition.rss", NULL};
    
    // Open a file called "stories.txt" for writing
    FILE *f = fopen("stories.txt", "w");
    
    // If opening the file fails, it calls the error function with a relavant message.
    if(!f){
        error("Can't open stories.txt");
    }
    
    // Create a new process by forking
    pid_t pid = fork();
    if(pid == -1)
        error("Can't fork process");
    
    // The child process code block
    if(!pid){
        
        // Print debug message indicating the child process has started
        printf("Child process starting.\n");
        
        // Redirect standard output to the file "stories.txt"
        if(dup2(fileno(f), 1) == -1){
            error("Can't redirect Standard Output");
        }
        
        // Print debug message with the search phrase
        printf("Executing script with phrase: %s\n", phrase);
        
        // Execute the Python script with the given phrase and environment variables
        if(execle("/usr/bin/python3", "/usr/bin/python3", "./dogriffiths-rssgossip-f0bb346/rssgossip.py", phrase, NULL, vars) == -1){
            error("Can't run script");
        }
    }
    
    // Wait for the child process to finish
    int pid_status;
    if(waitpid(pid, &pid_status, 0) == -1){
        error("Error waiting for child process");
    }
    
    // Print debug message indicating the child process has finished
    printf("Child process finished with status %d\n", pid_status);
    
    // Close the file.
    fclose(f);
    return 0;
    
}
```

- `if (!pid)` is equivalent to `if (pid == 0)`, meaning it executes only when `fork()` returns 0 (indicating the child process).



Code Execution:

```sh
chan@CMA:~/C_Programming/HFC/chapter_9/RSS_Feeds
$ make newshound
cc     newshound.c   -o newshound

chan@CMA:~/C_Programming/HFC/chapter_9/RSS_Feeds
$ ./newshound
Please provide a search phrase.: Success

chan@CMA:~/C_Programming/HFC/chapter_9/RSS_Feeds
$ ./newshound "shares"
Child process starting.
Child process finished with status 0

chan@CMA:~/C_Programming/HFC/chapter_9/RSS_Feeds
$ cat stories.txt
Executing script with phrase: shares
'Scary, cold, hungry and lonely': Volunteer soldier shares experience on front line
Emma Heming Willis shares family photos as daughter Mabel turns 11

```

- When the program opened the `stories.txt` file with `fopen()`, the OS registered the file `f` in the descriptor table.
- `fileno(f)` was the descriptor number it used.
- The `dup2()` function set the Standard Output descriptor(1) to point to the same file.

The program will change the descriptor table in the child script to look like this:

| #    | Data stream      |
| ---- | ---------------- |
| 0    | The keyboard     |
| 1    | File stories.txt |
| 2    | The screen       |
| 3    | File stories.txt |



### Chapter 10 - Opening a web page in a browser using system()

```C
void open_url(char *url){
    char launch[255];
    sprintf(launch, "x-www-browser '%s'", url);
    system(launch);
}

int main(){
    open_url("https://www.google.com");
    return 0;
}
```

The above code opens a web page in a browser on Linux.

For windows,

```C
sprintf(laung, "cmd /c start %s", url);
system(launch);
```

For Mac,

```C
sprintf(launch,"open '%s'", url);
system(launch);
```



Code Execution:

```sh
chan@CMA:~/C_Programming/test
$ ./final

[0702/213756.919081:ERROR:file_io_posix.cc(153)] open /home/chan/.config/google-chrome/Crash Reports/pending/20e95c88-a2fd-481d-9a23-075c5578e26a.lock: File exists (17)
Opening in existing browser session.

```

#### Chapter 10 - Exercise 2 (news_opener)



`hello.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/wait.h>
#include "hello.h"

void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerr(errno));
    exit(1);
}

void open_url(char *url){
    char launch[255];
    sprintf(launch, "x-www-browser '%s'", url);
    system(launch);
}
```

`main.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/wait.h>
#include "hello.h"

int main(int argc, char *argv[]){
    if(argc < 2){
        error("Please provide a search phrase.")
    }
    
    char *phrase = argv[1];
    char *var[] = {"RSS_FEED=http://rss.cnn.com/rss/edition.rss", NULL};
    
    char *python_path = "/usr/bin/python3";
    char *script_path = "../HFC/chapter_9/RSS_Feeds/dogriffiths-rssgossip-f0bb346/rssgossip.py";
    
    int fd[2];
    
    // This will create the pipe and store its descriptors in fd[0] and fd[1]
    if(pipe(fd) == -1){
        error("Can't create the pipe.");
    }
    
    pid_t pid = fork();
    if(pid == -1){
        error("Can't fork process");
    }
    
    if(pid == 0){ // Child process
        close(fd[0]); // the child won't read from the pipe, so close the read end.
        
        if(dup2(fd[1], 1) == -1){ // This will set the Standard Output to the write end of the pipe
            error("Can't redirect Standard Output.");
        }
        
        if(execle(python_path, python_path, script_path, phrase, NULL, vars) == -1){
            error("Can't run script.");
        }
    }else{ // Parent process
        close(fd[1]); // close writing end in the parent because the parent won't write to it
        
        // Redirecting the Standard Inpuyt to the read end of the pipe
        if(dup2(fd[0], 0) == -1){
            error("Can't redirect Standard Input");
        }
        
        char line[255];
        
        // We will read from the stdin because that's connected to the pipe.
        // We could also put fd[0].
        while(fgets(line, 255, stdin)){
            printf("Received line: %s", line);
            if(line[0] == '\t'){
                open_url(line + 1); // open url if it starts with a tab
                // line + 1 is the string starting after the tab character.
            }else{
                printf("%s", line); // Print other lines to stdout
            }
        }
        
        close(fd[0]); // close reading end in the parent
        
        int pid_status;
        if(waitpid(pid, &pid_status, 0) == -1){
            error("Error waiting for child process");
            
            printf("Child process finished with status %d\n", pid_status);
        }
    }
    return 0;
}
```



Code Execution:

```sh
chan@CMA:~/C_Programming/test
$ make all

Compiling the main file
clang -Wall -Wextra -g -c main.c 
Linking and producing the final application
clang -Wall -Wextra -g main.o hello.o -o final

chan@CMA:~/C_Programming/test
$ ./final 'shares'

Received line: 'Scary, cold, hungry and lonely': Volunteer soldier shares experience on front line
'Scary, cold, hungry and lonely': Volunteer soldier shares experience on front line
Received line: 	https://www.cnn.com/videos/world/2023/04/03/soldiers-russia-ukraine-war-david-mckenzie-pkg-ovn-intl-ldn-vpx.cnn
[0702/223653.208950:ERROR:file_io_posix.cc(153)] open /home/chan/.config/google-chrome/Crash Reports/pending/20e95c88-a2fd-481d-9a23-075c5578e26a.lock: File exists (17)
Opening in existing browser session.
Received line: Emma Heming Willis shares family photos as daughter Mabel turns 11
Emma Heming Willis shares family photos as daughter Mabel turns 11
Received line: 	https://www.cnn.com/2023/04/03/entertainment/emma-willis-mabel-birthday-photos-intl-scli/index.html
[0702/223653.277338:ERROR:file_io_posix.cc(153)] open /home/chan/.config/google-chrome/Crash Reports/pending/20e95c88-a2fd-481d-9a23-075c5578e26a.lock: File exists (17)
Opening in existing browser session.
Child process finished with status 0
```

- The program ran the `rssgossip.py` in a separate process and told it to display URLs for each story it found.
- All of the output of the screen was redirected through a pipe that was connected to the parent process.
- Pipes are a great way of connecting processes together.
- Now, we have the ability to not only **run** processes and **control** their environments, but we also have a way of **capturing their output**.



#### Signal Handler Exercise

`hello.c`

```C
#include <signal.h> // We need to include signal.h header.
// This is our new signal handler.
void diediedie(int sig){ // The OS passes signal to the handler.
	puts("Goodbye cruel world...\n");
    exit(1);
}

int catch_signal(int sig, void (*handler)(int)){
    
    struct sigaction action; // Declare a variable 'action' of type 'struct sigaction'
    
    action.sa_handler = handler; // Set the signal handler to the provided 'handler' function
    
    sigemptyset(&action.sa_mask); // Initialize the signal set to empty (no signals blocked during the handler execution)
    
    action.sa_flags = 0; // Set the flags to 0 (no special options)
    return sigaction (sig, &action, NULL);
    // Set the action for signal 'sig' using 'sigaction' system call
}
```

**`struct sigaction action;`**:

- This declares a variable `action` of type `struct sigaction`, which is used to specify how to handle a particular signal.

**`action.sa_handler = handler;`**:

- This sets the `sa_handler` member of the `struct sigaction` to the function pointer `handler`. This means that when the specified signal (`sig`) occurs, the `handler` function will be called.

**`sigemptyset(&action.sa_mask);`**:

- This initializes the signal set `sa_mask` to be empty. This means that no additional signals are blocked during the execution of the signal handler.

**`action.sa_flags = 0;`**:

- This sets the `sa_flags` member to 0, which means no special flags are set. It uses the default behavior for handling the signal.

**`return sigaction(sig, &action, NULL);`**:

- This calls the `sigaction` system call to change the action taken by the process on receipt of signal `sig`. It sets the action to the one specified by `action`. The third argument is `NULL`, which means we do not need to retrieve the old action.

`main.c`

```C
int main(){
    
    // SIGINT means we're capturing the interrupt signal
    if(catch_signal(SIGINT, diediedie) == -1){
        fprintf(stderr, "Can't map the handler");
        exit(2);
    }
    
    char name[30];
    printf("Enter your name: ");
    fgets(name, 30, stdin);
    printf("Hello %s\n", name);
    return 0;
}
```

- `SIGINT` - the interrupt signal typically triggered by pressing `Ctrl+C`. 
- When press `Ctrl+C`, the OS will automatically send the process an interrupt signal (`SIGINT`).
- The interrupt signal will be handled by the `sigaction` that was registered in the `catch_signal()` function. 
- The `sigaction` contains a pointer to the `diediedie()` function.
- This will then be called and the program will display a message and `exit()`.

Code Execution:

```sh
chan@CMA:~/C_Programming/test$ ./final
Enter your name: ^CGoodbye cruel world....

```

#### Chapter 10 - Long Exercise

#### `hello.c` 

```C
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <unistd.h>
#include <errno.h>
#include <sys/wait.h>
#include <sys/types.h>
#include <signal.h>
#include "hello.h"

void error(char *msg)
{
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(1);
}

void diediedie(int sig)
{
    puts("Goodbye cruel world....\n");
    exit(1);
}

void end_game(int sig)
{
    printf("\nFinal score: %d\n", score);
    exit(0);
}

int catch_signal(int sig, void (*handler)(int))
{
    struct sigaction action;
    action.sa_handler = handler;
    sigemptyset(&action.sa_mask);
    action.sa_flags = 0;
    return sigaction(sig, &action, NULL);
}

void times_up(int sig)
{
    puts("\nTIME'S UP!");
    raise(SIGINT);
}

```

`hello.h`

```C
extern int score;

void error(char *msg);

void diediedie(int sig);

void end_game(int sig);
int catch_signal(int sig, void (*handler)(int));

void times_up(int sig);
```

`main.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <errno.h>
#include <sys/types.h>
#include <sys/wait.h>
#include <time.h>
#include <signal.h>
#include "hello.h"

int main()
{
    catch_signal(SIGALRM, times_up);
    catch_signal(SIGINT, end_game);
    
    // This makes sure we get different random numbers each time
    srandom(time(0));
    while (1)
    {
        int a = random() % 11;
        int b = random() % 11;
        char txt[4];
        
        // Set the alarm to fire in 5 seconds.
        // As long as we go through the loop in less than 5 seconds, the timer will be reset and it will never fire.
        alarm(5);
        printf("\nWhat is %d times %d? ", a, b);
        fgets(txt, 4, stdin);
        
        // convert a string received from the user through fgets into an integer to compare this input to the product of a * b to determine if the user's answer is correct.
        int answer = atoi(txt);
        if (answer == a * b)
            score++;
        else
            printf("\nWrong! Score: %d\n", score);
    }
    return 0;
}

```

The `srandom(time(0))`function call in C is used to seed the random number generator with a starting point. Here's a breakdown:

- `srandom(unsigned int seed)`is a function that initializes the random number generator used by the `random()` function. The `seed` parameter is used to start the random number sequence. Different seeds produce different sequences of numbers generated by `random()`.
- `time(0)` returns the current time as the number of seconds elapsed since the Unix epoch (00:00:00 UTC on 1 January 1970). Passing `0` as an argument to `time()`means you're asking for the current time. This value changes every second, providing a simple way to get a "random" seed.

So, `srandom(time(0))` seeds the random number generator with the current time, ensuring that you get a different sequence of numbers from `random()` each time your program runs. This is useful for simulations, games, or any application requiring random number generation where you want to avoid the same sequence of numbers each time.

Code Execution:

```sh
chan@CMA:~/C_Programming/test
$ ./final

What is 6 times 0? 
TIME'S UP!

Final score: 0

chan@CMA:~/C_Programming/test$ ./final

What is 6 times 4? 24

What is 5 times 4? 20

What is 10 times 2? 20

What is 1 times 2? 2

What is 10 times 7? 70

What is 2 times 1? 3

Wrong! Score: 5

What is 5 times 0? ^C
Final score: 5
chan@CMA:~/C_Programming/test$ 
```

- The first testing, instead of hitting `CTRL-C`, we waited for at least five seconds on one of the answers and we can see that the alarm signal (`SIGALRM`) fires. The program was waiting for the user to enter an answer but because he took so long, the timer signal was sent; the process immediately displays "TIME'S UP!" message and then escalates the signal to a `SIGINT` that causes the program to display the final score.
- The second time, we hit `CTRL-C`, which sends the process an interrupt signal (`SIGINT`) that makes the program display the final score and then `exit()`.
- Signals are a little complex, but incredibly useful.
- They allow our programs to end gracefully, and the interval timer can help us deal with tasks that are taking too long.

---

### Chapter 11 - sockets and networking

#### Chapter 11 - Exercise 1

`main.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <sys/socket.h>
#include <unistd.h>
#include <arpa/inet.h>
#include "header.h"

int main(){
    
    // Array of advice strings to be sent to clients
    char *advice[] = {
        "Take smaller bites\r\n",
        "Go for the tight jeans. No they do NOT make you look fat.\r\n",
        "One word: inappropriate\r\n",
        "Just for today, be honest. Tell your boss what you *really* think\r\n",
        "You might want to rethink that haircut\r\n"};
    
    // Create a socket using the Internet address family (PF_INET) and TCP (SOCK_STREAM)
    int listener_d = socket(PF_INET, SOCK_STREAM, 0);
    if(listener_d == -1){
        error("Can't open socket!"); // Handle error if socket creation fails
    }
    
    // Set up the address structure
    struct sockaddr_in name;
    name.sin_family = PF_INET; // Use the internet address family
    
    // Set port number to 30000, converting to network byte order
    name.sin_port = (in_port_t)htons(30000);
    
    // Bind to any available network interface
    name.sin_addr.s_addr = htonl(INADDR_ANY);
    
    // bind the socket to the address and port number specified in the 'name' struct
    int c = bind(listener_d, (struct sockaddr *)&name, sizeof(name));
    if(c == -1){
        // Handle error if binding fails
        error("Can't bind to socket\n");
    }
    
    // Listen for incoming connections, with a backlog of 10 pending connections
    if(listen(listener_d, 10) == -1)){
     	error("Can't listen");   
    }
    puts("Waiting for connection"); // Print a message indicating the server is waiting for connections
    
    // Infinite loop to handle incoming connections
    while(1){
        struct sockaddr_storage client_addr;
    unsigned int address_size = sizeof(client_addr);
    int connect_d = accept(listener_d, (struct sockaddr *)&client_addr, &address_size);
    if(connect_d == -1){
        error("Can't open secondary socket!");
    }
    char *msg = advice[rand() % 5];
    
    if(send(connect_d, msg, strlen(msg), 0) == -1){
        error("send");
    }
    close(connect_d);
    }
    
    return 0;
}
```

- We should always check if socket, bind, listen, accept or send return -1.

`functions.c`

```C
void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(0);
}

```

`header.h`

```C
void error(char *msg);
```



Code Execution:



```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ make all
clang -Wall -Wextra -g -c main.c
clang -Wall -Wextra -g main.o functions.o -o test
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Waiting for connection
^C
```

On the second console

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Just for today, be honest. Tell your boss what you *really* think
Connection closed by foreign host.

chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Go for the tight jeans. No they do NOT make you look fat.
Connection closed by foreign host.

```



- We're using **127.0.0.1** as the IP address because the client is running on the same machine as the server.
- We could have connected to the server from anywhere on the network and we'd have gotten the same response.

If I start the server, then run the client one time, it works.

But then if I stop the server and restart it real quick, the client can't get a response anymore!

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Can't bind to socket: Address already in use

```

On the second console

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
telnet: Unable to connect to remote host: Connection refused

```

- When we bind a socket to a port, the OS will prevent anything else from rebinding to it for the next 30 seconds or so, and that includes the program that bound the port in the first place.
- To get around the problem, we need to set an option on the socket before we bind it:

The error message "Can't bind to socket: Address already in use" indicates that the port our server is trying to bind to is already occupied by another process. This is a common issue when a server process is terminated and immediately restarted. The underlying socket remains in a `TIME_WAIT` state for a period, preventing another process from binding to the same port.

To resolve this issue, you can set the `SO_REUSEADDR` socket option on our listener socket. This option tells the kernel that we're okay with reusing the local address (`port`), even if it's still in the `TIME_WAIT` state. Here's how we can modify our code to include this option:

1. Include the `setsockopt` function call right after creating our listener socket and before binding it to an address.
2. Use `SO_REUSEADDR` to allow the port to be reused.

Here's an example snippet showing how to set `SO_REUSEADDR`:

We have to insert it after creating the listener socket(`listener_d`) and before we bind it to an address.

```C
// We need an int variable to store the option. Setting it to 1 means "Yes, reuse  the port"
int reuse = 1;
if (setsockopt(listener_d, SOL_SOCKET, SO_REUSEADDR, (void *)&reuse, sizeof(int)) == -1) {
    error("Can't set the 'reuse address' option on the socket");
}
```

- If the server has responded to a client and then gets stopped and restarted, the call to the bind system call fails.
- If we didn't check for errors, the rest of the server code would run even though it couldn't use the server port.
- The above code makes the socket **reuse the port** when it's bound. That means now we can stop and restart the server and there will be no errors.

`SOL_SOCKET`is a constant used in socket programming to specify that the options being set or retrieved apply at the socket level, as opposed to a protocol level. 

- It's used with `setsockopt`and `getsockopt` functions to set or get options that are applicable to all types of sockets, regardless of the communication protocol chosen (e.g., TCP, UDP). 
- Essentially, `SOL_SOCKET` specifies that the options are to be interpreted by the socket layer itself, not by any specific protocol implementation.

`(void *)&reuse`

The reason for casting it to a different pointer type, such as `(char *)` or more appropriately `(void *)`, is primarily historical and due to the generic interface of `setsockopt`. The function is designed to accept a wide variety of option values, and thus it expects a pointer to `void` to accommodate this flexibility.

However, in modern C programming, casting to `(void *)` is the preferred approach when passing the address of `reuse` to `setsockopt`. This is because `(void *)` is a generic pointer type that can point to any type of data. Casting to `(char *)` works, but it's not semantically correct in this context since we're not working with character data. The correct and clear way to pass an integer option value to `setsockopt` is by using `(void *)&reuse`, as this accurately reflects that we're passing a pointer to an integer, treated generically.



#### Test Run

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Waiting for connection
^C

chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Waiting for connection
```

On the second console:

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Just for today, be honest. Tell your boss what you *really* think
Connection closed by foreign host.

chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Just for today, be honest. Tell your boss what you *really* think
Connection closed by foreign host.

```



#### Chapter 11 - Long Exercise

#includes are removed to save space.

`functions.c`

```C
void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(0);
}

int catch_signal(int sig, void (*handler)(int)){
    struct sigaction action;
    action.sa_handler = handler;
    sigemptyset(&action.sa_mask);
    action.sa_flags = 0;
    return sigaction(sig, &action, NULL);
}

int open_listener_socket(){
    int s = socket(PF_INET, SOCK_STREAM, 0);
    if(s == -1){
        error("Can't open socket");
    }
    return s;
}

void bind_to_port(int socket, int port){
    
    // Create a structure to hold the address
    struct sockaddr_in name;
    
    // Use the IPv4 protocol family
    name.sin_family = PF_INET;
    
    // Convert the port to network byte order
    name.sin_port = (in_port_t)htons(port);
    
    // Bind to any available network interface
    name.sin_addr.s_addr = htonl(INADDR_ANY);
    
    // Allow reuse of local addresses
    // Useful for avoiding "address already in use" error
    int reuse = 1;
    if(setsockopt(socket, SOL_SOCKET, SO_REUSEADDR, (void *)&reuse, sizeof(int)) == -1){
        error("Can't set the reuse option on the socket.");
    }
    
    // Bind the socket to the address and port
    int c = bind(socket, (struct sockaddr *)&name, sizeof(name));
    if(c == -1){
        error("Can't bind to socket");
    }
}

// Function to send a message to a socket
int say(int socket, char *s){
    // Send the message
    int result = send(socket, s, strlen(s), 0);
    
    // Don't call error() if there's a problem. We don't want to stop the server if there's just a problem with one client.
    if(result == -1){
        fprintf(stderr, "%s: %s\n", "Error talking to the client", strerror(errno));
    }
    return result;
}

// Function to read data from a socket
int read_in(int socket, char *buf, int len){
    // Pointer to the buffer
    char *s = buf;
    
    // Length of the buffer
    int slen = len;
    
    // Receive data from the socket
    int c = recv(socket, s, slen, 0);
    
    // Continue receiving until a newline character is found
    while((c > 0) && (s[c-1] != '\n')){
        s += c; // Move the buffer pointer
        slen -= c; // Decrease the remaining length
        
        // Receive more data
        c = recv(socket, s, slen, 0);
    }
    
    // Check if receiving failed
    if(c < 0){
        return c; // Return the error code
    }else if(c == 0){ // Check if the connection is closed
        buf[0] = '\0'; // Null-terminate the buffer
    }else{
        s[c-1] = '\0'; // Replace the newline character with null terminator.
    }
    
    // Return the number of bytes read
    return len - slen;
}

// Global variable for the listener socket descriptor
int listener_d;

// If someone hits CTRL+C when the server is running, this function will close the socket before the program ends.
void handle_shutdown(){
    
    // Closes the listener socket if it is open
    if(listener_d){
        close(listener_d);
    }
    
    // Print a message before exiting the program.
    fprintf(stderr, "Bye!\n");
    exit(0);
}
```

`main.c`

```C
// Main function, entry point of the program
int main(int argc, char *argv[]){
    
    // Set up signal handler for SIGINT (Ctrl+C)
    if(catch_signal(SIGINT, handle_shutdown) == -1){
        error("Can't set the interrupt handler.");
    }
    
    // Open a listening socket
    listener_d = open_listener_socket();
    
    // Bind the socket to port 30000
    bind_to_port(listener_d, 30000);
    
    // Start listening for incoming connections
    if(listen(listener_d, 10) == -1){
        error("Can't listen");
    }
    
    // Storage for client address
    struct sockaddr_storage client_addr;
    // Size of the address
    unsigned int address_size = sizeof(client_addr);
    puts("Waiting for connection");
    
    char buf[255]; // Buffer to store receive data
    
    // Infinite loop to accept and handle incoming connections
    while(1){
        // Accept a new connection
        int connect_d = accept(listener_d, (struct sockaddr*)&client_addr, &address_size);
        if(connect_d == -1){
            error("Can't open secondary socket");
        }
        
        // Send a welcome message to the client
        if(say(connect_d, "Internet Knock-Knock Protocol Server\r\nVersion 1.0\r\nKnock!Knock!\r\n>") != -1)
        {
            // Read client's response
            read_in(connect_d, buf, sizeof(buf));
            // Check if the client responded with "Who's there?"
            // Checking if the return value is not zero which means the strings aren't equal.
            // non-zero = true so it's like saying if(true) if strings aren't equal
            // zero = false which means strings are equal. So the condition becomes false and we proceed to else block.
            if(strncasecmp("Who's there?", buf, 12)){
                // If not, prompt the correct response
                say(connect_d, "You should say 'Who's there?!");
            }else{
                // If correct, proceed with the joke
                if(say(connect_d, "Oscar\r\n") != -1){
                    // Read client's next response
                    read_in(connect_d, buf, sizeof(buf));
                    // Check if the client responded with "Oscar who?"
                    if(strncasecmp("Oscar who?", buf, 10)){
                        // If not, prompt the correct response
                        say(connect_d, "You should say 'Oscar who?'!\r\n");
                    }else{
                        // Complete the joke
                        say(connect_d, "Oscar silly question, you get a silly answer\r\n");
                    }
                }
            }
            // Close the connection to the client
            close(connect_d);
        }
    }
    return 0;
}
```

Code Execution:

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Waiting for connection
^CBye!
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
Waiting for connection


```

On the second console

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Internet Knock-Knock Protocol Server
Version1.0
Knock!Knock!
>Who's there?
Oscar
> Oscar who?
Oscar silly question, you get a silly answer
Connection closed by foreign host.

chan@CMA:~/C_Programming/HFC/chapter_10/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Internet Knock-Knock Protocol Server
Version1.0
Knock!Knock!
>Come in
You should say 'Who's there?'!Connection closed by foreign host.

```

- If we break the protocol and send back in invalid response like the second one "Come in", the server is able to validate the data we send and close the connection immediately.

Code Explanation (`main.c`)

**Signal Handling**:

```C
if (catch_signal(SIGINT, handle_shutdown) == -1)
{
    error("Can't set the interrupt handler");
}
```

- Sets up a signal handler for `SIGINT` (interrupt signal, typically sent when the user presses Ctrl+C). If it fails, it calls the `error` function.

**Open Listening Socket**:

```C
listener_d = open_listener_socket();
```

- Opens a socket for listening to incoming connections.

**Bind to Port**:

```C
bind_to_port(listener_d, 30000);
```

- Binds the opened socket to port 30000.

**Start Listening**:

```C
if (listen(listener_d, 10) == -1)
{
    error("Can't listen");
}
```

- Starts listening on the bound socket with a backlog of 10 connections.

**Client Address Storage**:

```C
struct sockaddr_storage client_addr;
unsigned int address_size = sizeof(client_addr);
puts("Waiting for connection");
char buf[255];
```

- Prepares a structure to store client address information, gets the size of this structure, prints a waiting message, and allocates a buffer for reading data.

**Infinite Loop for Accepting Connections**:

```C
while (1)
{
```

- Starts an infinite loop to continuously accept and handle incoming connections.

**Accept Connection**:

```C
int connect_d = accept(listener_d, (struct sockaddr *)&client_addr, &address_size);
if (connect_d == -1)
{
    error("Can't open secondary socket");
}
```

- Accepts a new connection. If it fails, calls the `error` function.

**Send Welcome Message**:

```C
if (say(connect_d, "Internet Knock-Knock Protocol Server\r\nVersion1.0\r\nKnock!Knock!\r\n>") != -1)
{
```

- Sends a welcome message to the connected client. If it succeeds, proceeds to the next steps.

**Read Client's Response**:

```C
read_in(connect_d, buf, sizeof(buf));
if (strncasecmp("Who's there?", buf, 12))
{
    say(connect_d, "You should say 'Who's there?'!");
}
```

- Reads the client's response into `buf`. If the response is not "Who's there?" (case-insensitive comparison), sends a prompt for the correct response.

**Proceed with the Joke**:

```C
else
{
    if (say(connect_d, "Oscar\r\n> ") != -1)
    {
        read_in(connect_d, buf, sizeof(buf));
        if (strncasecmp("Oscar who?", buf, 10))
        {
            say(connect_d, "You should say 'Oscar who?'!\r\n");
        }
        else
        {
            say(connect_d, "Oscar silly question, you get a silly answer\r\n");
        }
    }
}
```

- If the client responds correctly with "Who's there?", proceeds to the next part of the joke. If the client responds correctly with "Oscar who?", completes the joke.

**Close Connection**:

```C
close(connect_d);
```

- Closes the connection to the client after handling the interaction.

Right now, our servers can only talk to one person at a time.

first console:

```sh
chan@CMA:~/C_Programming/test$ ./final
Waiting for connection


```

second console is accessed by one user.

```sh
chan@CMA:~/C_Programming/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Internet Knock-Knock Protocol Server
Version 1.0
Knock!Knock!
>

```

If someone else tries to get through to the server, he can't; it's busy with the first user:

```sh
chan@CMA:~/C_Programming/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.

```

- The server is still busy talking to the first guy.
- The main server socket will keep the client waiting until the server calls the `accept()` system call again.

But

#### We can fork() a process for each client

- When the clients connect to the server, they start to have conversation on a separate, newly created socket.
- That means the main server socket is free to go and find another client.
- When a client connects, we can `fork()` a separate child process to deal with the conversation between the server and the client.
- While the client is talking to the child process, the server's parent process can go connect to the next client.



#### The parent and child use different sockets

- One thing to bear in mind is that the parent server process will only need to use the main listener socket.
- That's because the main listener socket is the one that's used to `accept()` new connections.
- On the other hand, the child process will only ever need to deal with the secondary socket that gets created by the `accept()` call. 
- That means once the parent has forked the child, the parent can close the secondary socket and the child can close the main listener socket.

```C
// After forking the child, the parent can close this socket
close(connect_d);

// Once the child gets created, it can close this socket.
close(listener_d);
```

Q: If I create a new process for each client, what happens if hundreds of clients connect? Will my machine create hundreds of processes?

A: Yes. If we think our server will get a lot of clients, we need to control how many processes we create. The child can signal us when it's finished with a client and we can use that to maintain a count of current child processes.



#### Modified Code for our Exercise (`main.c`)

```C
while(1){
    int connect_d = accpet(listener_d, (struct sockaddr*)&client_addr, &address_size);
    if(connect_d == -1){
        error("Can't open secondary socket");
    }
    
    // This creates the child process and we know that if the fork() call returns 0, we must be in the child.
    if(!fork()){
        
        // In the child, we need to close the main listener socket.
        // The child will use only the connect_d socket to talk to the client
        close(listener_d);
        if(say(connect_d, "Internet Knock-Knock Protocol Server\r\nVersion 1.0\r\nKnock! Knock!\r\n>") != -1){
            read_in(connect_d, buf, sizeof(buf));
            if(strncasecmp("Who's there?", buf, 12)){
                say(connect_d, "You should say 'Who's there?!");
            }else{
                if(say(connect_d, "Oscar\r\n> ") != -1){
                    read_in(connect_d, buf, sizeof(buf));
                    if(strncasecmp("Oscar who?", buf, 10)){
                        say(connect_d, "You should say 'Oscar who?'!\r\n");
                    }else{
                        say(connect_d, "Oscar silly question, you get a silly answer\r\n");
                    }
                }
            }
        }
        // Once the conversation is over, the child can close the socket to the client.
        close(connect_d);
        
        // Once the child has finished talking, it should exit. That will prevent it from falling into the main server loop.
        exit(0);
    }
    close(connect_d);
}
return 0;
```



Code Execution:

```sh
chan@CMA:~/C_Programming/test$ ps -ef
...
chan       10928    3699  0 22:40 pts/0    00:00:00 ./final
chan       10931    9619  0 22:40 pts/1    00:00:00 telnet 127.0.0.1 30000
chan       10932   10928  0 22:40 pts/0    00:00:00 ./final

```

```sh
  10928 pts/0    00:00:00 final
  10931 pts/1    00:00:00 telnet
  10932 pts/0    00:00:00 final
  11640 ?        00:00:00 chrome
  11659 ?        00:00:00 kworker/10:0H-kblockd
  11660 ?        00:00:00 kworker/u32:0-events_power_efficient
  11746 pts/2    00:00:00 telnet
  11747 pts/0    00:00:00 final

```



- There are now two processes for the server: one for the parent and one for the child.
- That means we can connect, even while the first client is still talking to the server.

Now running our program

first console:

```sh
chan@CMA:~/C_Programming/test
$ make all
Compiling the main file
clang -Wall -Wextra -g -c main.c 
Linking and producing the final application
clang -Wall -Wextra -g main.o hello.o -o final

chan@CMA:~/C_Programming/test
$ ./final
Waiting for connection

```



second console:

```sh
chan@CMA:~/C_Programming/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Internet Knock-Knock Protocol Server
Version 1.0
Knock!Knock!
>Who's there?
Oscar

```



third console:

```sh
chan@CMA:~/C_Programming/test$ telnet 127.0.0.1 30000
Trying 127.0.0.1...
Connected to 127.0.0.1.
Escape character is '^]'.
Internet Knock-Knock Protocol Server
Version 1.0
Knock!Knock!
>Who's there?

```



#### Chapter 11 - Last Exercise

`functions.c`

```C
#include <string.h>
#include <sys/types.h>
#include <sys/socket.h>
#include <netdb.h>
#include <unistd.h>

// Error handling function to print an error message and exit
void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(0);
}

// Function tto set up a signal handler for a given signal
int catch_signal(int sig, void (*handler)(int)){
    // Declare a sigaction struct to specify the signal handling action
    struct sigaction action;
    // Set the handler function for the signal
    action.sa_handler = handler;
    // Initialize the signal mask to empty
    sigemptyset(&action.sa_mask);
    // No special flags
    action.sa_flags = 0;
    // Set the action for the specified signal
    return sigaction(sig, &action, NULL);
}

// Function to open a listener socket
int open_listener_socket(){
    // Create a socket using the IPv4 protocol family and stream socket type
    int s = socket(PF_INET, SOCK_STREAM, 0);
    
    // Check if socket creation failed.
    if(s == -1){
        error("Can't open socket");
    }
    // return the socket descriptor
    return s;
}

// Function to open a socket and connect to a specified host and port
int open_socket(char *host, char *port){
    struct addrinfo *res;
    struct addrinfo hints;
    
    // zero out the hints struct
    memset(&hints, 0, sizeof(hints));
    
    // Specify the address family as unspecified (both IPv4 and IPv6)
    hints.ai_family = PF_UNSPEC;
    
    // Specify the socket type as a stream socket (TCP)
    hints.ai_socktype = SOCK_STREAM;
    
    // Get address information for the specified host and port
    if(getaddrinfo(host, port, &hints, &res) == -1){
        error("Can't resolve the address.");
    }
    
    // Create a socket with the obtained address information
    int d_sock = socket(res->ai_family, res->ai_socktype, res->ai_protocol);
    
    // Check if socket creation was successful
    if(d_sock == -1){
        error("Can't open socket");
    }
    
    // Connect the socket to the address obtained
    int c = connect(d_sock, res->ai_addr, res->ai_addrlen);
    
    // Free the address information allocated by getaddrinfo()
    freeaddrinfo(res);
    if(c == -1){
        error("Can't connect to socket");
    }
    
    // Return the connected socket descriptor
    return d_sock;
}

// Function to bind a socket to a specified port
void bind_to_port(int socket, int port){
    // Create a structure to hold the address
    struct sockaddr_in name;
    // Use the IPv4 protocol family
    name.sin_family = PF_INET;
    // Convert the port to network byte order
    name.sin_port = (in_port_t)htons(port);
    // Bind to any available network interface
    name.sin_addr.s_addr = htonl(INADDR_ANY);
    
    // Allow reuse of local addresses
    // Useful for avoiding "Address already in use" error
    int reuse = 1;
    if(setsockopt(socket, SOL_SOCKET, SO_REUSEADDR, (void *)&reuse, sizeof(int)) == -1){
        error("Can't set the reuse option on the socket.");
    }
    
    // Bind the socket to the address and port
    int c = bind(socket, (struct sockaddr *)&name, sizeof(name));
    if(c == -1){
        error("Can't bind to socket");
    }
}

// Function to send a message through a socket
int say(int socket, char *s){
    // Send the message
    int result = send(socket, s, strlen(s), 0);
    if(result == -1){
        fprintf(stderr, "%s: %s\n", "Error talking to the client", strerror(errno));
    }
    // Return the result of send
    return result;
}

// Function to read a message from a socket
int read_in(int socket, char *buf, int len){
    // Initialize pointers and length for receiving data
    char *s = buf;
    int slen = len;
    // Receive data from the socket
    int c = recv(socket, s, slen, 0);
    // Loop until we receive a newline character or encounter an error
    while((c > 0) && (s[c-1] != '\n')){
        s += c;
        slen -= c;
        c = recv(socket, s, slen, 0);
    }
    
    // Check if there was an error receiving data
    if(c < 0){
        return c;
    }else if(c == 0){
        buf[0] = '\0';
    }else{
        s[c-1] = '\0';
    }
    
    // Return the number of bytes read
    return len - slen;
}

// Global variable to hold the listener socket descriptor
int listener_d;

// Function to handle shutdown signals
void handle_shutdown(){
    // Check if the listener socket is open
    if(listener_d){
        // if so, close the listener socket
        close(listener_d);
    }
    // print a shutdown message
    fprintf(stderr, "Bye!\n");
    // exit the program with status 0.
    exit(0);
}
```

`main.c`

```C
int main(int argc, char *argv[]){
    (void argc); // Explicitly mark argc as unused to avoid compiler warnings
    
    // Declare a variable to hold the socket descriptor
    int d_sock;
    
    // Open a socket and connect to the specified host and port
    d_sock = open_socket("en.wikipedia.org", "80");
    
    // Format the GET request string and store it in buf
    sprintf(buf, "GET /wiki/O'Reilly_Media HTTP/1.1\r\n");
    
    // Send the GET request to the server
    say(d_sock, buf);
    
    // Send the Host header to the server
    say(d_sock, "Host: en.wikipedia.org\r\n");
    
    // Buffer to hold the received data
    char rec[256];
    
    // Receive data from the server
    int bytesRcvd = recv(d_sock, rec, 255, 0);
    
    // Loop to receive all data from the server
    while(bytesRcvd){
        // Check if receiving data failed
        if(bytesRcvd == -1){
            error("Can't read from the server");
        }
        
        // Null-terminate the received data
        rec[bytesRcvd] = '\0';
        
        // Print the received data
        printf("%s", rec);
        
        // Receive more data
        bytesRcvd = recv(d_sock, rec, 255, 0);
    }
    // Close the socket
    close(d_sock);
    return 0;
}
```



Code Execution:

```sh
chan@CMA:~/C_Programming/HFC/chapter_10/test$ ./test
HTTP/1.1 301 Moved Permanently
content-length: 0
location: https://en.wikipedia.org/wiki/O'Reilly_Media
server: HAProxy
x-cache: cp5022 int
x-cache-status: int-tls
connection: close


```

Since, the page has been permanently moved to another location and http has been changed and upgraded to https, our fetch was successful but we need to use another approach for fetching `https` sites.



### Fetching HTTPS Pages 

To fetch HTTPs pages,

- We need to use a library that supports SSL/TL because HTTPS is HTTP over TLS(Transport Layer Security).
- One commonly used library for this in C is OpenSSL.
- Here is how we can modify our code to use OpenSSL for fetching HTTPS pages.



First we need to install the OpenSSL library if it is not already installed.

```sh
sudo apt-get install libssl-dev
```



#### Exercises (Fetching HTTPS Pages)

`main.c`

```C
...
#include <openssl/ssl.h>
#include <openssl/err.h>
int main(int argc, char *argv[])
{
    (void)argc;

    // Initialize SSL context
    SSL_CTX *ctx = init_ssl_context();

    // Open socket and connect to server
    int d_sock = open_socket("en.wikipedia.org", "443");

    // Connect SSL over the socket
    SSL *ssl = connect_ssl(d_sock, ctx);

    // Send request
    char buf[255];
    snprintf(buf, sizeof(buf), "GET /wiki/O'Reilly_Media HTTP/1.1\r\n");
    ssl_say(ssl, buf);
    ssl_say(ssl, "Host: en.wikipedia.org\r\n");
    ssl_say(ssl, "Connection: close\r\n\r\n");

    // Receive response
    char rec[256];
    //Read the response
    int bytesRcvd = ssl_read_in(ssl, rec, sizeof(rec) - 1);
    
    // Keep reading if there's a response
    while (bytesRcvd > 0)
    {
        rec[bytesRcvd] = '\0'; // Null-terminate the received data
        printf("%s", rec); // Print the received data
        bytesRcvd = ssl_read_in(ssl, rec, sizeof(rec) - 1);
    }
    // Clean up and close connections
    close_ssl(ssl, ctx, d_sock);
    return 0;
}
```

### Explanation of `main.c`

1. **Initialize SSL Context:**

   ```C
   SSL_CTX *ctx = init_ssl_context();
   ```

   - This initializes the SSL context, which is required for creating SSL connections.

2. **Open Socket and Connect to Server:**

   ```C
   int d_sock = open_socket("en.wikipedia.org", "443");
   ```

   - This creates a socket and connects it to the specified server (`en.wikipedia.org`) on port 443 (HTTPS).

   - Port 443 is the standard port for HTTPS (HTTP Secure) communications. 

   - HTTPS is the secure version of HTTP, which is the protocol used for transmitting web pages. 

   - It uses SSL/TLS to encrypt the data exchanged between the client and server, ensuring that the data cannot be intercepted or tampered with by third parties.

   - When connecting to "en.wikipedia.org" on port 443, you are establishing a secure connection using SSL/TLS. This is necessary for securely fetching content from websites that support HTTPS, ensuring that the data received and sent is encrypted for privacy and security.

     

3. **Connect SSL over the Socket:**

   ```C
   SSL *ssl = connect_ssl(d_sock, ctx);
   ```

   - This wraps the socket with SSL, establishing an encrypted connection.

4. **Send HTTP Request:**

   ```C
   snprintf(buf, sizeof(buf), "GET /wiki/O'Reilly_Media HTTP/1.1\r\n");
   ssl_say(ssl, buf); // Send the request line
   ssl_say(ssl, "Host: en.wikipedia.org\r\n"); // Send the Host header
   ssl_say(ssl, "Connection: close\r\n\r\n"); // Send the Connection header and end of headers
   ```

   - These lines build and send the HTTP request to the server.

   - HTTP Request Format: we need to ensure that our HTTP request is properly formatted. 

   - HTTP/1.1 requests should end with an additional `CRLF (\r\n)` after the headers to indicate the end of the request headers as in `ssl_say(ssl, "Connection: close\r\n\r\n");`

     

5. **Receive and Print Response:**

   ```C
   char rec[256];
   int bytesRcvd;
   while ((bytesRcvd = ssl_read_in(ssl, rec, sizeof(rec) - 1)) > 0) // Read the response
   {
       rec[bytesRcvd] = '\0'; // Null-terminate the received data
       printf("%s", rec); // Print the received data
   }
   ```

   - This loop reads data from the server and prints it until there's no more data.

6. **Error Handling:**

   ```C
   if (bytesRcvd < 0) // Check for errors
   {
       ERR_print_errors_fp(stderr); // Print any SSL errors
   }
   ```

   - If there was an error reading from the server, print the error details.

7. **Clean Up and Close Connections:**

   ```C
   close_ssl(ssl, ctx, d_sock);
   ```

   - This function cleans up and closes the SSL connection, socket, and SSL context.



`functions.c`

```C
#define _XOPEN_SOURCE 700
#include <openssl/ssl.h>
#include <openssl/err.h>

int open_socket(char *host, char *port)
{
    struct addrinfo *res;
    struct addrinfo hints;
    memset(&hints, 0, sizeof(hints));
    hints.ai_family = PF_UNSPEC;
    hints.ai_socktype = SOCK_STREAM;
    if (getaddrinfo(host, port, &hints, &res) == -1)
    {
        error("Can't resolve the address");
    }

    int sock_d = socket(res->ai_family, res->ai_socktype, res->ai_protocol);

    if (sock_d == -1)
    {
        error("Can't open socket");
    }

    if (connect(sock_d, res->ai_addr, res->ai_addrlen) == -1)
    {
        error("Can't connect to socket");
    }

    freeaddrinfo(res);
    return sock_d;
}

SSL_CTX *init_ssl_context()
{
    // Load human-readable error strings for libssl
    SSL_load_error_strings();
    
    // Initialize the OpenSSL library
    SSL_library_init();
    
    // Load all available encryption algorithms
    OpenSSL_add_all_algorithms();
	
    // Create a new SSL context using TLS
    SSL_CTX *ctx = SSL_CTX_new(TLS_client_method());
    if (ctx == NULL)
    {
        // Print any errors if context creation fails
        ERR_print_errors_fp(stderr);
        // Exit the program with failure
        exit(EXIT_FAILURE);
    }
    
    // Return the created SSL context
    return ctx;
}

SSL *connect_ssl(int socket, SSL_CTX *ctx)
{
    // Create a new SSL structure for a connection 
    SSL *ssl = SSL_new(ctx);
    // Bind the SSL object to the socket file descriptor
    SSL_set_fd(ssl, socket);
	
    // Perform the SSL/TLS handshake with the server
    if (SSL_connect(ssl) == -1)
    {
        // Print any errors if the handshake fails
        ERR_print_errors_fp(stderr);
        exit(EXIT_FAILURE);
    }
    
    // Return the SSL structure
    return ssl;
}

void close_ssl(SSL *ssl, SSL_CTX *ctx, int socket)
{
    // Shut down the SSL connection
    SSL_shutdown(ssl);
    // Free the SSL structure
    SSL_free(ssl);
    // Close the socket file descriptor
    close(socket);
    // Free the SSL context
    SSL_CTX_free(ctx);
}

int ssl_say(SSL *ssl, char *s)
{
    // Write data to the SSL connection
    int result = SSL_write(ssl, s, strlen(s));
    // Check if the write operation failed
    if (result == -1)
    {
        // Print any SSL errors
        ERR_print_errors_fp(stderr);
    }
    // Return the result of the write operation
    return result;
}

int ssl_read_in(SSL *ssl, char *buf, int len)
{
    // Read data from the SSL connection
    int received = SSL_read(ssl, buf, len - 1);
    
    // Check if data was received
    if (received > 0)
    {
        // Null-terminate the received data
        buf[received] = '\0';
    }
    // Return the number of bytes received
    return received;
}

```

### Detailed Explanations

#### What does `SSL_CTX` stand for?

- `SSL_CTX` is a structure used in OpenSSL to hold all SSL/TLS settings. 
- The [`SSL_CTX`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) structure is part of the OpenSSL library, which is widely used for secure communication over networks. 
- It stands for "SSL Context," where SSL refers to Secure Sockets Layer (and by extension, its successor, Transport Layer Security or TLS). 
- This context is used to manage settings and certificates across multiple SSL/TLS connections. 
- It includes options for configuring the SSL/TLS protocols, choosing which certificates and keys to use, and setting up other security-related options.

#### Notes to remember

In C, the presence of an asterisk (`*`) in a function declaration indicates that the function returns a pointer, while its absence means the function returns a value directly. Here's a breakdown of the difference:

- `SSL *connect_ssl()`: This function declaration indicates that `connect_ssl` returns a pointer to an `SSL` object. The asterisk (`*`) signifies that the return type is a pointer. This is common when the function allocates an object in memory and returns a reference to it, allowing the function to return complex data structures or instances of structures/classes.
- `SSL connect_ssl()`: This declaration indicates that `connect_ssl` returns an `SSL` object directly, not a pointer. This means the function returns a copy of the object itself. Returning complex objects like this can be less efficient than returning pointers due to the overhead of copying the object's data. However, it can be used for simple data types or when returning a static or global object that does not need to be dynamically allocated.

In summary, the key difference is in how the function returns its result: either as a direct copy (`SSL connect_ssl()`) or as a reference/pointer to the object (`SSL *connect_ssl()`). The choice between these two approaches depends on the specific requirements of the function, including efficiency considerations and whether the object needs to be modified by the caller.

##### Why SSL Context is necessary?

An SSL context `SSL_CTX *` is necessary for several reasons related to setting up and managing SSL/TLS connections securely. Here's why it's needed:

1. **Initialization and Configuration**: The SSL context acts as a factory for creating SSL/TLS connections. It holds all the configuration options, methods, and certificates required for the SSL/TLS handshake. When we create an SSL object with `SSL_new(ctx)`, it inherits the settings from the context. This allows us to configure multiple SSL connections similarly without setting options individually for each connection.
2. **Certificate Management**: The SSL context is used to load and manage certificates and private keys. These are essential for the SSL/TLS handshake process, where the server (and optionally the client) must present a certificate to prove its identity. The context provides a centralized way to manage these credentials.
3. **Protocol Options**: SSL/TLS has evolved over time, with newer versions offering better security. The context allows us to specify which versions of the SSL/TLS protocols our application supports or should use, enabling us to enforce stronger security policies.
4. **Performance Optimization**: Creating an SSL context can be resource-intensive, as it involves loading cryptographic algorithms, processing certificates, and more. By using a single context for multiple connections, we can amortize the cost of these operations across all connections, improving performance.
5. **Error Handling**: The context also plays a role in error handling. For example, when an SSL/TLS error occurs, functions like [`ERR_print_errors_fp`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html) can use the context to obtain detailed error messages, helping in diagnosing and resolving issues.

In summary, the SSL context ([`SSL_CTX *`](vscode-file://vscode-app/usr/share/code/resources/app/out/vs/code/electron-sandbox/workbench/workbench.html)) in OpenSSL is a critical structure that encapsulates global configuration for SSL/TLS operations. It simplifies managing multiple connections, enhances security by centralizing certificate and protocol management, and improves performance by reusing expensive cryptographic operations.

#### `init_ssl_context`

1. Load Error Strings:
   - `SSL_load_error_strings()` loads human-readable error messages for SSL functions.
2. Initialize SSL Library:
   - `SSL_library_init()` initializes the SSL library.
3. Add All Algorithms:
   - `OpenSSL_add_all_algorithms()` loads all available encryption algorithms.
4. Create SSL Context:
   - `SSL_CTX_new(TLS_client_method())` creates a new SSL context using the TLS client method.
5. Error Handling:
   - If the context creation fails, `ERR_print_errors_fp(stderr)` prints the errors and exits the program.

#### `connect_ssl`

1. Create SSL Structure:
   - `SSL_new(ctx)` creates a new SSL structure for the connection.
2. Set File Descriptor:
   - `SSL_set_fd(ssl, socket)` binds the SSL structure to the socket file descriptor.
3. Perform Handshake:
   - `SSL_connect(ssl)` performs the SSL/TLS handshake with the server.
4. Error Handling:
   - If the handshake fails, `ERR_print_errors_fp(stderr)` prints the errors and exits the program.

#### `close_ssl`

1. Shutdown SSL Connection:
   - `SSL_shutdown(ssl)` shuts down the SSL connection.
2. Free SSL Structure:
   - `SSL_free(ssl)` frees the SSL structure.
3. Close Socket:
   - `close(socket)` closes the socket file descriptor.
4. Free SSL Context:
   - `SSL_CTX_free(ctx)` frees the SSL context.

#### `ssl_say`

1. Write Data:
   - `SSL_write(ssl, s, strlen(s))` writes data to the SSL connection.
2. Error Handling:
   - If the write operation fails, `ERR_print_errors_fp(stderr)` prints the SSL errors.

#### `ssl_read_in`

1. Read Data:
   - `SSL_read(ssl, buf, len - 1)` reads data from the SSL connection. The -1 ensures there is space for a null terminator to be added, making the buffer a valid C string.
2. Null-Terminate Data:
   - If data is received, it is null-terminated to ensure it is a valid C string.

`MakeFile`

```makefile
CC = clang
CFLAGS = -Wall -Wextra -g -D_XOPEN_SOURCE=700
LIBS = -lssl -lcrypto

all: final

final: main.o hello.o
	@echo "Linking and producing the final application"
	$(CC) $(CFLAGS) main.o hello.o -o final $(LIBS)

main.o: main.c 
	@echo "Compiling the main file"
	$(CC) $(CFLAGS) -c main.c 

hello.o: hello.c 
	@echo "Compiling the hello file"
	$(CC) $(CFLAGS)  -c hello.c 

clean:
	@echo "Removing everything except the source files"
	@rm main.o hello.o final
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/test$ ./final "O'Reilly_Media"
HTTP/1.1 200 OK
date: Thu, 18 Jul 2024 14:28:52 GMT
server: mw-web.codfw.main-f564bd77d-7dwv6
x-content-type-options: nosniff
content-language: en
origin-trial: AonOP4SwCrqpb0nhZbg554z9iJimP3DxUDB8V4yu9fyyepauGKD0NXqTknWi4gnuDfMG6hNb7TDUDTsl0mDw9gIAAABmeyJvcmlnaW4iOiJodHRwczovL3dpa2lwZWRpYS5vcmc6NDQzIiwiZmVhdHVyZSI6IlRvcExldmVsVHBjZCIsImV4cGlyeSI6MTczNTM0Mzk5OSwiaXNTdWJkb21haW4iOnRydWV9
accept-ch: 
vary: Accept-Encoding,Cookie,Authorization
last-modified: Thu, 04 Jul 2024 14:28:52 GMT
content-type: text/html; charset=UTF-8
age: 0
x-cache: cp5017 miss, cp5017 miss
x-cache-status: miss
server-timing: cache;desc="miss", host;desc="cp5017"
strict-transport-security: max-age=106384710; includeSubDomains; preload
report-to: { "group": "wm_nel", "max_age": 604800, "endpoints": [{ "url": "https://intake-logging.wikimedia.org/v1/events?stream=w3c.reportingapi.network_error&schema_uri=/w3c/reportingapi/network_error/1.0.0" }] }
...
```

---



## Chapter 12 - threads

### Exercise 1 - Chapter 12



`functions.c`

```C
#define _XOPEN_SOURCE 700
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <strings.h>
#include <unistd.h>
#include <errno.h>
#include <pthread.h> // Include the pthread library for threading support.
#include "headers.h"

void error(char *msg){
    fprintf(stderr, "%s: %s\n", msg, strerror(errno));
    exit(1);
}

// Define a function that will be executed by a thread
// This function prints "Does not!" five times, with a 1-second pause between each print
void *does_not(void *a){
    // Initialize a loop counter
    int i = 0;
    // Loop 5 times
    for(i = 0; i < 5; i++){
        // Pause executaion for 1 second
        sleep(1);
        // Print "Does not!" to the standard output.
        puts("Does not!");
    }
    // Return NULL since this function does not need to return any value
    return NULL;
}

// Define another function that will be executed by a different thread
// This function prints "Does too!" five times, with a 1-second pause between each print
void *does_too(*void a){
    // Initialize a loop counter
    int i = 0;
    // Loop 5 times
    for(i = 0; i < 5; i++){
        sleep(1);
        puts("Does too");
    }
    return NULL;
}
```



`main.c`

```C
#include <stdio.h>
#include <stdlib.h>
#include <pthread.h> // Include the pthread library for threading support.

int main(){
    // Declare thread identifiers.
    pthread_t t0;
    pthread_t t1;
    
    // Create the first thread, running the does_not function.
    if(pthread_create(&t0, NULL, does_not, NULL) == -1){
        // If thread creation fails, print an error message and exit.
        error("Can't create thread t0.");
    }
    
    // Create the second thread, running the does_too function.
    if(pthread_create(&t1, NULL, does_too, NULL) == -1){
        error("Can't create thread t1.")
    }
    
    
    // Declare a pointer to store the return value of the threads 
    void *result;
    
    // Wait for the first thread to finish and check for errors
    // If thread join fails, print error message and exit
    if(pthread_join(t0, &result) == -1){
        error("Can't join thread t0");
    }
    
    // Wait for the second thread to finish and check for errors
    if(pthread_join(t1, &result) == -1){
        error("Can't join thread t1.");
        
    }
    return 0;
}
```

`Makefile`

```makefile
CC = clang
CFLAGS = -Wall -Wextra -g
LIBS = -lpthread
OBJDIR = ./obj
LIBDIR = ./libs

all: main

main: $(OBJDIR)/main.o $(LIBDIR)/libfunctions.a
	$(CC) $(OBJDIR)/main.o -L$(LIBDIR) -lfunctions -o main $(LIBS)

$(OBJDIR)/main.o: main.c headers.h
	$(CC) $(CFLAGS) -c main.c -o $(OBJDIR)/main.o

$(OBJDIR)/functions.o: functions.c headers.h 
	$(CC) $(CFLAGS) -c functions.c -o $(OBJDIR)/functions.o

$(LIBDIR)/libfunctions.a: $(OBJDIR)/functions.o 
	ar -rcs $(LIBDIR)/libfunctions.a $(OBJDIR)/functions.o

clean: 
	@echo "Removing everything except the source files..."
	rm -f *.o $(OBJDIR)/*.o $(LIBDIR)/libfunctions.a main
```

- In C, the `sleep` function suspends the execution of the current thread for a given number of seconds. 
- Specifically, `sleep(1)` means the current thread is paused for 1 second. This function is useful for introducing delays or reducing CPU usage in programs where timing is not critical.
- Running code on two threads means that the program can execute two sets of instructions concurrently, utilizing the CPU's capability to run multiple threads in parallel. 
- This doesn't necessarily mean running two separate programs, but rather running different parts of the same program simultaneously. 
- Modern CPUs have multiple cores, each capable of running one or more threads, allowing for true parallel execution. 
- In the context of our main.c code, it means the `does_not` and `does_too` functions can run at the same time, each on its own thread, potentially on separate CPU cores, improving the program's overall execution efficiency and responsiveness.
- When we look carefully at our `Makefile`, we need to include `-lpthread` which will link the `pthread` library since we are using the `pthread` library.

`Code Execution`

```sh
chan@CMA:~/C_Programming/HFC/chapter_12/test$ ./main
Does not!
Does too!
Does not!
Does too!
Does too!
Does not!
Does too!
Does not!
Does too!
Does not!
chan@CMA:~/C_Programming/HFC/chapter_12/test$ 
```

- When we run the code, we can see both functions running at the same time.



Q: If both functions are running at the same time, why don't the letters in the messages get mixed up? Each message is on its own line.

A: That's because of the way the Standard Output works. The text from `puts()` will all get output at once.



Q: I removed the `sleep()` function, and the output showed all the output from one function and then all the output from the other function. Why is that?

A: Most machines will run the code so quickly that without the `sleep()` call, the first function will finish before the second thread starts running.



### Beers Magnet - Exercise 2 Chapter 12

`functions.c`

```C
void error(char *msg) {
    fprintf(stderr, "%s: %s\n", msg, strerror(errno)); // Print error message and description
    exit(1); // Exit the program with a status of 1 indicating an error
}

// Global variable representing the number of beers
int beers = 2000000;

// Function to be run by each thread
void *drink_lots(void *a){
    (void)a; // Cast argument to avoid unused parameter warning
    int i;
    // Each thread will decrement the beer count 100,000 times
    for(i = 0; i < 100000; i++){
        beers = beers - 1;
    }
    // Return NULl as the thread function has to return a void pointer
    return NULL;
}
```



`headers.h`

```C
void error(char *msg);

extern int beers;

void *drink_lots(void *a);
```



`main.c`

```C
int main(){
    // Array to hold thread identifiers for 20 threads
    pthread_t threads[20];
    int t;
    
    // Print initial number of beers
    printf("%i bottles of beer on the wall\n%i bottles of beer\n", beers, beers);
    
    // Create 20 threads
    for(t = 0; t < 20; t++){
        // Create a thread that runs the drink_lots function
        if(pthread_create(&threads[t], NULL, drink_lots, NULL) == -1){
            error("Can't create thread");
        }
    }
    
    // Pointer to store the return value of the threads
    void *result;
    //Wait for all 20 threads to finish
    for(t = 0; t < 20; t++){
        // Join each thread, ensuring it has completed before moving on
        if(pthread_join(threads[t], &result) == -1){
            error("Can't join threads");
        }
    }
    
    // Print the remaining number of beers
    printf("There are now %i bottles of beer on the wall\n", beers);
    return 0;
}
```

1. **Global Variable:**

   ```C
   int beers = 2000000;
   ```

   - `beers` is a global variable representing the total number of beers.

2. **Thread Function:**

   ```C
   void *drink_lots(void *a) {
       (void)a;
       int i;
       for (i = 0; i < 100000; i++) {
           beers = beers - 1;
       }
       return NULL;
   }
   ```

   - `drink_lots` is the function executed by each thread.
   - It takes a `void*` argument (not used in this case).
   - Each thread decrements the `beers` variable 100,000 times.
   - Returns `NULL` as the thread function must return a `void*`.

3. **Main Function:**

   ```C
   int main() {
       pthread_t threads[20];
       int t;
       printf("%i bottles of beer on the wall\n%i bottles of beer\n", beers, beers);
       for (t = 0; t < 20; t++) {
           if (pthread_create(&threads[t], NULL, drink_lots, NULL) == -1) {
               error("Can't create thread");
           }
       }
   
       void *result;
       for (t = 0; t < 20; t++) {
           if (pthread_join(threads[t], &result) == -1) {
               error("Can't join threads");
           }
       }
       printf("There are now %i bottles of beer on the wall\n", beers);
       return 0;
   }
   ```

   - **Thread Array:**

     ```C
     pthread_t threads[20];
     ```

     - Declare an array to hold the identifiers for 20 threads.

   - **Print Initial Beers:**

     ```C
     printf("%i bottles of beer on the wall\n%i bottles of beer\n", beers, beers);
     ```

     - Print the initial number of beers.

   - **Create Threads:**

     ```C
     for (t = 0; t < 20; t++) {
         if (pthread_create(&threads[t], NULL, drink_lots, NULL) == -1) {
             error("Can't create thread");
         }
     }
     ```

     - Create 20 threads using a loop.
     - Each thread runs the `drink_lots` function.
     - Handle errors if thread creation fails.

   - **Join Threads:**

     ```C
     void *result;
     for (t = 0; t < 20; t++) {
         if (pthread_join(threads[t], &result) == -1) {
             error("Can't join threads");
         }
     }
     ```

     - Use `pthread_join` to wait for each thread to finish.
     - Handle errors if joining a thread fails.

### Logic

- The main logic is to create 20 threads that each decrement a shared global variable (`beers`) 100,000 times.
- By using threads, the program simulates concurrent operations on the shared resource.
- After all threads complete, the program prints the final value of `beers`.



`Code Execution`

```sh
chan@CMA:~/C_Programming/HFC/chapter_12/test$ ./main
2000000 bottles of beer on the wall
2000000 bottles of beer
There are now 1758496 bottles of beer on the wall
chan@CMA:~/C_Programming/HFC/chapter_12/test$ ./main
2000000 bottles of beer on the wall
2000000 bottles of beer
There are now 1710145 bottles of beer on the wall
chan@CMA:~/C_Programming/HFC/chapter_12/test$ ./main
2000000 bottles of beer on the wall
2000000 bottles of beer
There are now 1736959 bottles of beer on the wall
```

- We can see the the result is quite unpredictable.
- Why doesn't the `beers` variable get set to zero when  all the threads have run?



### The code is not thread-safe

- The great thing about threads is that lots of different tasks can run at the same time and have access to the same data.
- The downside is that all those different threads have access to the same data.
- The threads in the second program are all reading and changing a shared piece of memory: the `beers` variable.
- To understand what's going on, let's see what happens if two threads try to reduce the value of `beers` using this line of code:

```C
beers = beers - 1;
```

![Screenshot from 2024-07-22 22-14-54](/home/chan/Pictures/Screenshots/Screenshot from 2024-07-22 22-14-54.png)

- Even though both of the threads were trying to reduce the value of `beers` by 1, they didn't succeed.
- Instead of reducing the value by 2, they only decreased it by 1.
- That's why the `beers` variable didn't get reduced to zero
- The threads kept getting in the way of each other.
- The reason why the result was so unpredictable is because the threads didn't always run the line of code at exactly the same time.
- Sometimes the threads didn't crash into each other, and sometimes they did.
- Be careful to look out for code that isn't thread-safe. How will we know? 
- If two threads read and write to the same variable, it's not.



### We need to add traffic signals

- Multithreaded programs can be powerful, but they can also behave in unpredictable ways, unless we put some controls in place.
- Imagine two cars want to pass down the same narrow stretch of road. 
- To prevent an accident, we can add traffic signals.
- Those traffic signals prevent the cars from getting access to a shared resource(the road) at the same time.
- It's the same thing when we want two or more threads to access a shared data resource.
- We need to add traffic signals so that no two threads can read the data and write it back at the same time.

![Screenshot from 2024-07-22 22-23-00](/home/chan/Pictures/Screenshots/Screenshot from 2024-07-22 22-23-00.png)

- The traffic signals that prevent threads from crashing into each other are called `mutexes`, and they are one of the simplest ways of making our code thread-safe.
- `Mutexes` are sometimes just called locks.
- MUT-EX = Mutually Exclusive.



### Use a `mutex` as a traffic signal

- To protect a section of code, we will need to create a mutex lock like this:

```C
pthread_mutex_t a_lock = PTHREAD_MUTEX_INITIALIZER;
```

- The `mutex` needs to be visible to all of the threads that might crash into each other, so that means we'll probably want to create it as a global variable.
- `PTHREAD_MUTEX_INITIALIZER` is actually a macro.
- When the compiler sees that, it will insert all of the code our program needs to create the `mutex` lock properly.



1. `Red means stop`.

   - At the beginning of our sensitive code section, we need to place our first traffic signal.
   - The `pthread_mutex_lock()` will let only **one thread** get past.
   - All the other threads will have to wait when they get to it.

   ```C
   // Only one thread at a time will get past this.
   pthread_mutex_lock(&a_lock);
   /* Sensitive code starts here...*/
   ```

2. `Green means go.`

   - When the thread gets to the end of the sensitive code, it makes a call to `pthread_mutex_unlock()`.
   - That sets the traffic signal back to green and another thread is allowed onto the sensitive code.

   ```C
   /* End of sensitive Code */
   pthread_mutex_unlock(&a_lock);
   ```

   - Now that we know how to create locks in our code, we have a lot of control over exactly how our threads will work.



### Passing Long Values to Thread Functions 

- Thread functions can accept a single void pointer parameter and return a single void pointer value.
- Quite often, we'll want to pass and return integer values to a thread.
- One trick is to use `long` values.
- `long`s can be stored in void pointers because they are the same size.

`functions.c`

```C
// A thread function can accept a single void pointer parameter
void *do_stuff(void *param){
    // Convert parameter to long to get thread number
    long thread_no = (long)param;
    // Print the thread number
    printf("Thread number %ld\n", thread_no);
    // Cast it back to a void pointer when it is returned.
    // Return thread number + 1 as the thread's return value
    return (void*)(thread_no + 1);
}
```

`main.c`

```C
int main(){
    // array to hold thread identifiers
    pthread_t threads[20];
    // Loop variable and thread argument
    long t;
    
    // Create 3 threads
    for(t = 0; t < 3; t++){
        // Create a thread and check for errors
        // (void*)t - Convert the long t value to a void pointer
        if(pthread_create(&threads[t], NULL, do_stuff, (void*)t) == -1){
            error("Cannot create threads");
        }
    }
    
    // Pointer to hold the return value of the threads
    void *result;
    // Join 3 threads
    
    for(t = 0; t < 3; t++){
        if(pthreads_join(threads[t], &result) == -1){
            error("Cannot join threads");
        }
        // Print the return value of the thread
        // Convert the return value to a long before using it.
        printf("Thread %ld returned %ld\n", t, (long)result);
    }
    return 0;
}
```

`Code Execution`

```sh
chan@CMA:~/C_Programming/test$ ./final
Thread number 0
Thread number 1
Thread number 2
Thread 0 returned 1
Thread 1 returned 2
Thread 2 returned 3

```

- Each thread receives its thread number.
- Each thread returns its thread number + 1.



### Long Exercise - Chapter 12

- There's no simple way to decide where to put the locks in our code.
- Where we put them will change the way the code performs.
- Here are two versions of `drink_lots()` function that lock the code in different ways.

`Version A`

```C
pthread_mutex_t beers_lock = PTHREAD_MUTEX_INITIALIZER;

void *drink_lots(void *a){
    int i;
    pthread_mutex_lock(&beers_lock);
    for(i = 0; i < 100000; i++){
        beers = beers - 1;
    }
    pthread_mutex_unlock(&beers_lock);
    printf("beers = %i\n", beers);
    return NULL;
}
```

- **Granularity**: Fine-grained locking.
- **Concurrency**: Higher concurrency, as the lock is held for a very short duration—just enough to decrement the `beers` variable. This allows other threads to acquire the lock between iterations of the loop.
- **Performance**: Potentially lower performance due to the overhead of acquiring and releasing the lock 100,000 times per thread. This can lead to significant contention and overhead if many threads are trying to lock and unlock in rapid succession.
- **Safety**: Safer in terms of data consistency. Each decrement operation is protected, ensuring that no two threads can modify `beers` simultaneously, which prevents race conditions.

`Version B`

```C
pthread_mutex_t beers_lock = PTHREAD_MUTEX_INITIALIZER;

void *drink_lots(void *a){
    int i;
    for(i = 0; i < 100000; i++){
        pthread_mutex_lock(&beers_lock);
        beers = beers - 1;
        pthread_mutex_unlock(&beers_lock);
    }
    printf("beers = %i\n", beers);
    return NULL;
}
```

- **Granularity**: Coarse-grained locking.
- **Concurrency**: Lower concurrency, as the lock is held for the duration of the entire loop, preventing other threads from accessing `beers` until the loop completes.
- **Performance**: Potentially higher performance due to reduced lock contention. The lock is acquired and released only once per thread, reducing the overhead associated with locking mechanisms.
- **Safety**: Still safe in terms of data consistency for the entire operation of decrementing `beers` 100,000 times, but it could lead to longer wait times for other threads, as they must wait for the current thread to finish the entire loop before getting a chance to execute.

Granularity, in the context of locking mechanisms like `mutexes` in multithreaded programming, refers to the scope and duration a lock covers during its acquisition. It's a measure of the size of the locked resource or the amount of code protected by the lock. Granularity can be broadly categorized into two types:

1. **Fine-Grained Locking**:
   - Locks are used to protect a small amount of data or a very specific part of the code.
   - This approach allows for higher concurrency, as it minimizes the time a lock is held and reduces the chances of contention among threads.
   - However, it can lead to higher overhead due to the frequent locking and unlocking operations.
2. **Coarse-Grained Locking**:
   - Locks are used to protect a large set of data or larger sections of code.
   - This approach simplifies the design and reduces the overhead of acquiring and releasing locks.
   - However, it can significantly reduce concurrency, as threads may be blocked for longer periods while waiting for access to the locked resource.

Choosing the right level of granularity is crucial for balancing performance and concurrency in multithreaded applications. Fine-grained locking is preferred for high-concurrency applications where performance is critical, while coarse-grained locking might be suitable for applications where simplicity and lower overhead are more important than maximizing concurrency.

`main.c`

```C
int main(){
    pthread_t threads[20];
    int t;
    printf("%d bottles of beers on the wall\n%d bottles of beers\n", beers, beers);
    for(t = 0; t < 20; t++){
        if(pthread_create(&threads[t], NULL, drink_lots, NULL) == -1){
            error("Can't create threads");
        }
    }
    
    void *result;
    for(t = 0; t < 20; t++){
        if(pthread_join(threads[t], &result) == -1){
            error("Can't join threads");
        }
    }
    
    printf("%d bottles of beers on the wall\n%d bottles of beers\n", beers, beers);
    return 0;
}
```



`Code Execution (Version A)`

```sh
chan@CMA:~/C_Programming/test$ ./final
2000000 bottles of beer on the wall
2000000 bottles of beer
beers: 1900000
beers: 1800000
beers: 1700000
beers: 1600000
beers: 1500000
beers: 1400000
beers: 1300000
beers: 1200000
beers: 1100000
beers: 1000000
beers: 900000
beers: 800000
beers: 700000
beers: 600000
beers: 500000
beers: 400000
beers: 300000
beers: 200000
beers: 100000
beers: 0
0 bottles of beer on the wall
0 bottles of beer

```

`Code Execution (Version B)`

```sh
chan@CMA:~/C_Programming/test$ ./final
2000000 bottles of beer on the wall
2000000 bottles of beer
beers: 93835
beers: 93728
beers: 88356
beers: 83394
beers: 75964
beers: 67523
beers: 55832
beers: 39614
beers: 27938
beers: 27074
beers: 9064
beers: 8584
beers: 8140
beers: 7523
beers: 6289
beers: 3813
beers: 1965
beers: 1301
beers: 357
beers: 0
0 bottles of beer on the wall
0 bottles of beer

```

- As we can see both pieces of code use a `mutex` to protect the `beers` variable.
- Each displays the value of `beers` before they exit.
- Because they are locking the code in different places, they generate different output on the screen.
- In summary, locking inside the loop offers better safety for each individual operation at the cost of performance due to frequent locking and unlocking. 
- Locking outside the loop improves performance by reducing lock contention but decreases concurrency, as threads have to wait longer for their turn to execute the loop.



### Locking Outside the Loop vs Inside The loop

The discrepancy in the starting value of `beers` when locking inside the loop, specifically starting from `93835` instead of decrementing in a predictable manner from `2000000`, can be attributed to the behavior of concurrent threads and the overhead of locking and unlocking in each iteration. Here's a breakdown to explain this phenomenon:

### Locking Outside the Loop

When the lock is placed outside the loop, the entire decrement operation (from `2000000` to `0`) is protected by a single lock for each thread. This means that once a thread acquires the lock, it will decrement `beers` by `100000` (assuming each thread is supposed to decrement `beers` by `1` a total of `100000` times) without any interruption from other threads. This results in a predictable, linear decrement of `beers` because threads execute their loops in complete isolation from each other, leading to the expected output where `beers` is decremented in large, consistent chunks.

### Locking Inside the Loop

When the lock is placed inside the loop, each decrement operation is individually protected. This means that after a thread decrements `beers` by `1`, it releases the lock, allowing another thread to acquire the lock and perform its decrement operation. This leads to a few key consequences:

1. **Concurrency Overhead**: The frequent locking and unlocking in each iteration introduce significant overhead, slowing down the decrement operations.
2. **Thread Scheduling and Preemption**: The operating system's scheduler determines which thread runs at any given time. Threads can be preempted (temporarily paused) to allow other threads to run. This scheduling is not deterministic from the perspective of your application, leading to non-linear decrements in `beers`.
3. **Contention**: With many threads contending for the same lock, some threads may acquire the lock more frequently than others, especially if they are preempted less often or scheduled more favorably. This can lead to an uneven rate of decrement across threads.

The starting value of `93835` suggests that by the time the program printed the first value of `beers`, multiple threads had already decremented it from `2000000` in a highly non-linear and unpredictable manner due to the reasons mentioned above. The specific starting value you see is a result of the complex interplay between thread scheduling, the overhead of locking and unlocking, and the specific behavior of your system's scheduler and CPU.

This unpredictability and the overhead associated with fine-grained locking inside the loop highlight the trade-offs between concurrency, performance, and the granularity of locking in multithreaded applications.
