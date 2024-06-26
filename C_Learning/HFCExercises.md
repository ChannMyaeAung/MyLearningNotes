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

