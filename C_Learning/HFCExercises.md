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

