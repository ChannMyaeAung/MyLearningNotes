# Commands

### What exactly are commands?

A command can be one of four different things

- **An executable program:** like all those files in */usr/bin*. Within this category, programs can be *compiled binaries* (programs written in C and C++),

  (programs written in scripting languages such as the shell, Perl, Python, Ruby and so on.)

- **A command built into the shell itself:** *bash* supports a number of commands internally called shell builtin. The cd command, for example, is a shell builtin.

- **A shell function: **Shell functions are miniature shell scripts incorporated into the *environment*.  

- **An alias:** Aliases are commands that we can define ourselves, built from other commands.



#### General commands

```bash
$ pwd       // print name of current working directory

**get lists of files from multiple directories**
$ ls ~ /usr   

**Determine a file type with file**
$ file picture.jpg
picture.jpg: JPEG image data, JFIF standard 1.01

**Viewing file contents with less
$ less filename

```



#### Get Help for Shell Builtins - help

```bash
** built-in help facility
$ help cd

** displays a description of the command's supported syntax and options
** some programs might not support
$ mkdir --help
```



#### Creating directories - mkdir

````bash
**Creating directories
$ mkdir dir1 
$ mkdir dir1 dir2 dir3  // create 3 directories named dir1, dir2 and dir3
````



#### Copying files and directories - cp

```bash
**Copy files and directories
$ cp item1 item2  
//copies the single file or directory item1 to the file or directory item2

$ cp item... directory
// copies multiple items (either files or directories) into a directory

$ cp file1 file2
// Copy file1 to file2. If file2 exists, it is overwritten with the contents of file1
// if file2 does not exist, it is created.

$ cp i file1 file2
// Same as previous command, except that if file2 exists, the user is prompted before it is overwritten.

$ cp file1 file2 dir1
// Copy file1 and file2 into directory dir1. The directory dir1 must already exist.

$ cp dir1/* dir2
// Copy all the files in dir1 into dir2 using a wildcard.
// The directory dir2 must already exist.

$ cp -r dir1 dir2
// Copy the content of directory dir1 to directory dir2.
// If directory dir2 does not exist,it is created.
// after the copy, will contain the same contents as directory dir1. 
// If directory dir2 does exist, then directory dir1 and its contents
// will be copied into dir2.
```



#### Moving or Renaming files - mv

```bash

** Move and rename files
$ mv  // The mv command perform both file moving and file renaming, depending on how 		// it is used.

$ mv item1 item2 
// move or rename the file or directory item1 to item2.
$ mv item1... directory
// move one or more items from one directory to another.

$ mv file1 file2
// Move file1 to file2. If file2 exists, it is overwritten with the contents of file1.
// If file2 does not exist, it is created. 
// In either case, file1 ceases to exist.

$ mv file1 file2 dir1
// Move file1 and file2 into directory dir1. The directory dir1 must already exist.

$ mv dir1 dir2
// If directory dir2 does not exist, create directory dir2 and move the contents of
// directory dir1 to dir2 and delete directory dir1.
// If directory dir2 does exist, move directory dir1 and its contents into directory dir2.

```



#### Removing files - rm

```bash
**Remove files and directories
$ rm item... // item is one or more files or directories

```



#### Creating links - ln

```bash
**Create links
$ ln  // Creates a hard link
$ ln -s item link   // Creates a symbolic links (symlinks or soft links)
// where item is either a file or a directory
```



#### Display a command's type - type

```bash
** Displays the kind of command the shell wil execute

$ type type
type is a shell builtin
$ type ls
ls is aliased to `ls --color==auto`
$ type cp
cp is /bin/cp
$ type foo
bash: type: foo: not found
```



#### Display an Executable's Location - which

```bash
** Determines the exact location of a given executable
** only for executable programs.
** not builtins or aliases

$ which ls
/bin/ls
```



#### Display a Program's Manual Page - man

```bash
$ man ls

** refering a specific section of the manual 
$ man section search_term
$ man 5 passwd
```



#### Display One-line Manual Page Descriptions - whatis

```bash
** displays the name and a one-line description of a man page matching a specified keyword.
$ whatis ls
ls
```



#### Creating Our Own Commands with alias

```bash
**check whether the alias we are going to use is already being used.
$ type test
test is a shell builtin

**if it is, try another alias.
$ type foo
bash: type: foo: not found

$ alias foo='cd /usr; ls; cd -'

$ foo
bin games include lib local sbin share src
/home/me

$ type foo
foo is aliased to `cd /usr; ls; cd -'

**to remove an alias, unalias is used.
$ unalias foo
$ type foo
bash: type: foo: not found
```



#### Redirection

```bash
** Redirecting Standard Output
$ ls -l /usr/bin > ls-output.txt

// Created a list of /user/bin directory and sent the results to the file ls-output.txt.

$ ls -l ls-output.txt
-rw-rw-r-- 1 me me 167878 2024-03-11 15:07 ls-output.txt

$ less ls-output.txt

$ ls -l /bin/usr > ls-output.txt
ls: cannot access /bin/usr: No such file or directory 
//print to the screen because we redirected only standard output and not standard error.

$ ls -l ls-output.txt
-rw-rw-r-- 1 me me 0 2024-03-11 15:08 ls-output.txt
//The file has now zero length because ">" rewrites the destination file and stopped because of the error resulting in its truncation.

$ > ls-output.txt
// ">" with no command preceding it will truncate an existing file or create a new, empty file.

$ ls -l /usr/bin >> ls-output.txt
// Using ">>" will result in the output being appended to the file.
// If the file does not already exist, it is created just as though the > operator had been  used.

$ ls -l /usr/bin >> ls-output.txt
$ ls -l /usr/bin >> ls-output.txt
$ ls -l /usr/bin >> ls-output.txt
$ ls -l ls-output.txt
-rw-rw-r-- 1 me me  503634 2024-03-11 15:45 ls-output.txt

// The file is 3 times large as we repeated the command three times.
```



#### Redirecting Standard Error

```bash
$ ls -l /bin/usr 2> ls-error.txt

// The shell references the standard input, output and error interanally as file descriptors 0, 1 and 2 respectively.
// Becase standard error is the same as file descriptor number 2, standard error can be redirected with the preceding notation.
```



#### Redirecting Standard Output and Standard Error to One File

```bash
**Traditional way (works with older versions as well).

$ ls -l /bin/usr > ls-output.txt 2>&1

*/ First we redirect the standard output to the file ls-output.txt and then redirect the file descriptor 2 (standard error) to file descriptor 1 (standard output) using the notation 2>&1.  /*

** the redirection of standard error must always occur after redirecting standard output.

**The follwing command will not work
$ 2>&1 > ls-output.txt


**Recent Streamlined method
$ ls -l /bin/usr &> ls-output.txt

*/ We use the single notation &> to redirect both standard output and standard error to the file ls-output.txt. /*

*Same goes for appending method as well
$ ls -l /bin/usr &>> ls-output.txt
```



#### Disposing of Unwanted Output

```bash
**Redirecting output to /dev/null
*/dev/null accepts input and does nothing with it.
* Also called bit bucket.

$ ls -l /bin/usr 2> /dev/null

// When someone says they are sending your comments to /dev/null, now you know what it means.
```



#### Concatenate Files - cat

Read one or more files and copies them to standard output.

Because it accepts more than one files as an argument,

it can also be used to join files together.

```bash
$ cat ls-output.txt
```



Suppose we have downloaded a large file that has been split into multiple parts and we want to join them back together

if the files were named as follows:

```bash
movie.mpeg.001 movie.mpeg.002 ... movie.mpeg.099
```

We could join them back together with this command:

```bash
$ cat movie.mpeg.0* > movie.mpeg
```



##### If there is no arguments, 

```bash
$ cat
```

it just sits there, reads from standard input and since standard input is by default attached to the keyboard, it is waiting for us to type something!

CTRL - D to tell cat that it has reached end of file (EOF) on standard input.



We can use this behavior to create short text files.

```bash
$ cat > lazy_dog.txt
The quick brown fox jumped over the lazy dog.
```

To see our result, we can use cat to copy the file to stdout  again.

```bash
$ cat lazy_dog.txt
The quick brown fox jumped over the lazy dog.
```



Let's try redirecting standard input

```bash
$ cat < lazy_dog.txt
The quick brown fox jumped over the lazy dog.
```

Using the < redirection operator, we change the source of standard input from the keyboard to the file *lazy_dog.txt* .



#### Pipelines

In Linux, a pipeline is a way to connect commands together. Imagine it like a kitchen assembly line. The output (like chopped vegetables) from one tool (like a knife) goes directly into the next tool (like a pan) for further processing.

In Linux, the pipe symbol "|" connects the output of one command (the knife) to the input of another (the pan). This lets you automate complex tasks without needing temporary files. It's a powerful way to streamline your work!

```bash
$ ls -l /usr/bin | less
```



#### Filters

Pipelines are often used to perform complex operations on data. It is possible to put several commands together into a pipeline referred to as filters.

Filters take input, change it somehow and then output it.



```bash
$ ls /bin /usr/bin | sort | less

** including sort in our pipeline, we changed the data to produce a single, sorted list.
```



#### Report or Omit Repeated Lines - uniq

- Often used in conjunction with sort
- accepts a sorted list of data from standard input or a single filename argument
- **Main Functionality:** removes any duplicates from the list

```bash
$ ls /bin /usr/bin | sort | uniq | less
// Removes any duplicates from the output of the sort command.
```



To see the list of dupicates,

```bash
$ ls /bin /usr/bin | sort | uniq -d | less
```



#### Print Line, Word, and Byte Counts - wc

The wc (word count) command is used to display the number of lines, words and bytes contained in files.

```bash
$ wc ls-output.txt
2  18 112 ls-output.txt
// (lines, words and bytes)
```



Adding it to a pipeline is a handy way to count things.

```bash
$ ls /bin /usr/bin | sort | uniq | wc -l
1420
```



#### Print Lines Matching a Pattern - grep

grep is a powerful program used to find text patterns within files.

```bash
$ grep pattern filename
```



- It prints out the lines containing a pattern.
- advanced patterns are called regular expression.

```bash
$ ls /bin /usr/bin | sort | uniq | grep zip
bunzip2
bzip2
bzip2recover
funzip
gpg-zip
gunzip
gzip
preunzip
prezip
prezip-bin
streamzip
unzip
unzipsfx
zip
zipcloak
zipdetails
zipgrep
zipinfo
zipnote
zipsplit

```



Handy options:

- -i, causes grep to ignore case when performing the search (normally searches are case sensitive)
- -v. which tells grep to print only those lines that do not match the pattern.



#### Print First/Last Part of Files - head/tail

- Used to print the first or last 10 lines of a file by default.

- Can be adjusted with -n option.

  ```bash
  $ head -n 5 ls-output.txt
  total 192828
  -rwxr-xr-x 1 root root       51648 Jan  8 22:56 [
  -rwxr-xr-x 1 root root       35344 Jun  6  2023 aa-enabled
  -rwxr-xr-x 1 root root       35344 Jun  6  2023 aa-exec
  -rwxr-xr-x 1 root root       31248 Jun  6  2023 aa-features-abi
  
  $ tail -n 5 ls-output.txt
  -rwxr-xr-x 1 root root      875096 Mar 25  2022 zstd
  lrwxrwxrwx 1 root root           4 Mar  8 21:37 zstdcat -> zstd
  -rwxr-xr-x 1 root root        3869 Mar 25  2022 zstdgrep
  -rwxr-xr-x 1 root root          30 Mar 25  2022 zstdless
  lrwxrwxrwx 1 root root           4 Mar  8 21:37 zstdmt -> zstd
  
  ```

  These can be used in pipelines as well:

  ```bash
  $ ls /usr/bin | tail -n 5
  zstd
  zstdcat
  zstdgrep
  zstdless
  zstdmt
  
  ```

  

Tail has an option that allows you to view files in real time.

```bash
$ tail -f /var/log/messages
```



Using the -f option, tail continues to monitor the file and when new lines are appended, they immediately appear on the display. This continues until you type CTRL-C.

Here are some synonyms for "strange" depending on the specific nuance you want to convey.



#### Read from Stdin and Output to Stdout and Files - tee

- Reads standard input and copies it to both standard output (allowing the data to continue down the pipeline) and to one or more files.
- Useful for capturing a pipeline's contents at an intermediate state of processing.

```bash
**tee to capture the entire directory listing to the file ls.txt before grep filters the pipeline's contents:

$ ls /usr/bin | tee ls.txt | grep zip
bunzip2
bzip2
bzip2recover
funzip
gpg-zip
gunzip
gzip
preunzip
prezip
prezip-bin
streamzip
unzip
unzipsfx
zip
zipcloak
zipdetails
zipgrep
zipinfo
zipnote
zipsplit

```



#### Expansion

Each time we type a command and press the ENTER key, bash performs several substitutions upon the text before it carries out our command. The process that makes this happen is called expansion.

With expansion, we enter something, and it is expanded into something else before the shell acts upon it.



#### Display a line of text - echo

Echo is a shell builtin that performs a very simple task. It prints its text arguments on standard output.



```bash
$ echo this is a test
this is a test

$ echo *
Desktop directory... Documents Downloads github.com ls-error.txt ls-output.txt ls.txt Music Pictures Public snap Templates Videos

```

**Note:** Why didn't echo print *? Because * means match any characters in a file name. The shell expands the * into something else (in this instance, the names of the files in the current working directory) before the echo command is executed. When the ENTER key is pressed, the shell automatically expands any qualifying characters on the command line before the command is carried out, so the echo command never saw the *, only its expanded result.



#### Pathname Expansion

The mechanism by which wildcards work is called pathname expansion.

```bash
$ ls
Desktop  Documents  github.com  ls-output.txt  Music  Public Templates
directory...  Downloads  ls-error.txt  ls.txt    Pictures  snap    Videos

$ echo D*
Desktop Documents Downloads

$ echo *s
Documents Downloads Pictures Templates Videos

$ echo [[:upper:]]*
Desktop Documents Downloads Music Pictures Public Templates Videos

$ echo /usr/*/share
/usr/local/share
```

**Note:** Check essentials.md file for "Pathname expansion of Hidden Files"



#### Tilde Expansion - ~

- Replaces the tilde with the actual directory path it represents
- ~ stands for your home directory

```bash
$ echo ~
/home/chan

// If user "foo" has an account, then it expands into this:
$ echo ~foo
/home/foo
```



#### Arithmetic Expansion

$((expression))

```bash
$ echo $((2 + 2))
4

**Spaces are not significant in arithmetic expressions,
** expressions may be nested.
$ echo $(($((5**2)) * 3))
75

$ echo $(((5**2) * 3))
75

**Example using the division and remainder operators.

$ echo Five divided by two equals $((5/2))
Five divided by two equals 2
$ echo with $((5%2)) left over.
with 1 left over.

```





#### Brace Expansion

- Create multiple text strings from a pattern containing  braces.

  ```bash
  $ echo Front-{A,B,C}-Back
  Front-A-Back Front-B-Back Front-C-Back
  ```

  

- May contain either a comma-separated list of strings or a range of integers or single characters.

  ```bash
  $ echo Number_{1..5}
  Number_1 Number_2 Number_3 Number_4 Number_5
  ```

- In bash version 4.0 and newer, integers may also be zero-padded like so:

  ```bash
  $ echo {01..15}
  01 02 03 04 05 06 07 08 09 10 11 12 13 14 15
  
  $ echo {001..15}
  001 002 003 004 005 006 007 008 009 010 011 012 013 014 015
  
  $ echo {Z..A}
  Z Y X W V U T S R Q P O N M L K J I H G F E D C B A
  
  $ echo a{A{1,2},B{3,4}}b
  aA1b aA2b aB3b aB4b
  
  ```

- Optimal Usage:

  - To make lists of files or directories to be created.

  - Imagine being a photographer and wanted to organize a large collection of images into years and months and sort them in chronological order.

    ```bash
    $ mkdir Photos
    $ cd Photos
    
    **Year-Month format
    $ mkdir {2007..2009}-{01..12}
    
    $ ls
    2007-01 2007-07 2008-01 2008-07 2009-01 2009-07
    2007-02 2007-08 2008-02 2008-08 2009-02 2009-08
    2007-03 2007-09 2008-03 2008-09 2009-03 2009-09
    2007-04 2007-10 2008-04 2008-10 2009-04 2009-10
    2007-05 2007-11 2008-05 2008-11 2009-05 2009-11
    2007-06 2007-12 2008-06 2008-12 2009-06 2009-12
    ```

    

#### Pathname Expansion

- More useful in shell scripts than directly on the command line.

- Many of its capabilities have to do with the system's ability to store small chunks of data called variables and to give each chunk a name.

  ```bash
  $ echo $USER
  chan
  ```

  

​	To see a list of available variables	

```bash
$ printenv | less
```



- Unlike any other types of expansion, if there is a mistype, the expansion will still take place but will result in an empty string. (In other expansions, the expansion will not take place and the echo command will simply display the mistyped pattern.)

```bash
$ echo $SUSER
$ 
```





#### Quoting

```bash
$ echo this is a      test
this is a test
// word splitting by the shell removed extra whitespace from the echo command's list of arguments.

$ echo The total is $100.00
The total is 00.00
// parameter expansion substituted an empty string for the value of $1 because it was an undefined variable.

// The shell provides a mechanism called quoting to selectively suppress unwanted expansions.
```



#### Double Quotes

- By placing text inside double quote, all the special characters used by the shell lose their special meaning and are treated as ordinary characters.
- The exceptions are $ , \, and `. (word splitting, pathname expansion, tilde expansion, and brace expansion are suppressed.) (parameter expansions, arithmetic expansion and command substitution are still carried out.)



Say we are the unfortunate victim of a file called *two words.txt*. If we tried to use this on the command line, word splitting would cause this to be treated as two separate arguments rather than the desired single argument.



```bash
$ ls -l two words.txt
ls: cannot access two: No such file or directory
ls: cannot access words.txt: No such file or directory
```



Double quotes stop the word splitting,

```bash
$ ls -l "two words.txt"
-rw-rw-r-- 1 me me 18 2018-02-20 13:03 two words.txt
```



Parameter expansion, arithmetic expansion and command substitution still take place within double quotes.

```bash
$ echo "$USER $((2+2)) $(cal)"
chan 4      March 2024       
Su Mo Tu We Th Fr Sa  
                1  2  
 3  4  5  6  7  8  9  
10 11 12 13 14 15 16  
17 18 19 20 21 22 23  
24 25 26 27 28 29 30  
31            
```



```bash
$ echo "this is a     test"
this is a     test
// word splitting is suppressed and the embedded spaces are not treated as delimiters.
```



- Newlines are considered delimiters by the word-splitting mechanism causes an interesting, albeit subtle, effect on command substitution.

  ```bash
  $ echo $(cal)
  March 2024 Su Mo Tu We Th Fr Sa 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31
  
  $ echo "$(cal)"
  March 2024       
  Su Mo Tu We Th Fr Sa  
                  1  2  
   3  4  5  6  7  8  9  
  10 11 12 13 14 15 16  
  17 18 19 20 21 22 23  
  24 25 26 27 28 29 30  
  31        
  ```

  In the first instance, the unquoted command substitution resulted in a command line containing 38 arguments.

  In the second, it resulted in a command line with one argument that includes the embedded spaces and newlines.



#### Single Quotes

- Single quotes are used when the need to suppress all expansions.



##### Comparison of unquoted, double quotes and single quotes

```bash
$ echo text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER
text /home/chan/lazy_dog.txt /home/chan/ls-error.txt /home/chan/ls-output.txt /home/chan/ls.txt a b foo 4 chan

$ echo "text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER"
text ~/*.txt {a,b} foo 4 chan

$ echo 'text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER'
text ~/*.txt {a,b} $(echo foo) $((2+2)) $USER

```



#### Escaping Characters

- The escaping character "\\" is used when we want to quote only a single character, inside double quotes to selectively prevent an expansion.

```bash
$ echo "The balance for user $USER is: \$5.00"
The balance for user chan is: $5.00

```

- It is possible to use characters in filenames that normally have special meaning to the shell with "\\"

​	($, !, &, spaces)



```bash
$ mv bad\&filenme good_filename
```



**Note:** Within single quotes, the backslash loses its special meaning and is treated as an ordinary character.



#### Backslash Escape Sequences

​	In addition to its role as the escape character, backslash is also used as part of a notation to represent certain special characters called control codes.



Backslash Escape Sequences

| Escape sequence | Meaning                                                  |
| --------------- | -------------------------------------------------------- |
| \a              | Bell (an alert that causes the computer to beep)         |
| \b              | Backspace                                                |
| \n              | Newline; on Unix-like systems, this produces a line feed |
| \r              | Carriage return                                          |
| \f              | Tab                                                      |



- Adding -e to echo will enable interpretation of escape sequences. You can also place them inside $'  '. 
- Below is using the sleep command that just waits for the specified number of seconds and then exits, we can create a primitive countdown timer;



```bash
$ sleep 10; echo -e "Time's up \a"
Time's up //After 10 seconds
```

We could also do this:

```bash
$ sleep 10; echo "Time's up" $'\a'
Time's up //After 10 seconds
```



### Permissions

- **id:** Display user identity
- **chmod:** Change a file's mode
- **umask:** Set the default file permissions
- **su:** Run a shell as another user
- **sudo:** Execute a command as another user
- **chown:** Change a file's owner
- **chgrp:** Change a file's group ownership
- **passwd:** Change a user's password



**Note:** Also check in essentials.md



##### Set Default Permissions - umask

- controls the default permissions given to a file when it is 

- It uses octal notation to express a mask of bits to be removed from a file's mode attributes.

```bash
$ rm -f foo.txt
$ umask
0002 (0022 is another common default value)
$ > foo.txt
$ ls -l foo.txt
-rw-rw-r-- 1 chan chan 0 Mar 19 14:56 foo.txt

**We can see that both the owner and group get read and write permission while world gets only read permission. The reason that the world does not have write permission is because of the value of the mask

```



Let's repeat our example, this time setting the mask ourselves

```bash
$ rm foo.txt
$ umask 0000
$ > foo.txt
$ > ls -l foo.txt
-rw-rw-rw- 1 chan chan 0 Mar 19 15:22 foo.txt

```

When we set the mask to 0000 (effectively turning it off), the file is now world writable. To understand how this works, we have to look at octal numbers again. If we take the mask, expand it into binary, and then compare it to the attributes, we can see what happens.

| Original file mode | --- rw- rw- rw- |
| ------------------ | --------------- |
| Mask               | 000 000 000 010 |
| Result             | --- rw- rw- r-- |

where the 1 appears in our mask, an attribute was removed- in this case, the world write permission. That's what the mask does. Everywhere a 1 appears in the binary value of the mask, an attribute is unset.



If we look at a mask value of 0022, we can see what it does.

| Original file mode | --- rw- rw- rw- |
| ------------------ | --------------- |
| Mask               | 000 000 010 010 |
| Result             | --- rw- r-- r-- |



Again where a 1 appears in the binary value, the corresponding attribute is unset.

Clean up:

```bash
$ rm foo.txt; umask 0002
```

 Most of the time we won't have to change the mask; the default provided by your distribution will be fine. In some high-security situations, however, we will want to control it.



#### chmod- Change the permissions

- used to change the permissions(read, write, execute) of files and directories in Linux.

- Refers to the essentials.md file as well.

  ```bash
  $ chmod [options] mode file(s)
  ```

​	r = 4, w = 2, x = 1

​	-rw- rw-r--

​	6-6-4 

1. Changing Permissions Numerically:

   You can use numeric representations to set permissions. The digits represent permissions for the owner, group and others respectively with each digit ranging from 0 to 7.

   - 0: No permissions
   - 1: Execute permission
   - 2: Write permission
   - 3: Write and execute permissions
   - 4: Read permission
   - 5: Read and execute permissions
   - 6: Read and write permissions
   - 7: Read, write and execute permissions

For example, to give read, write and execute permissions to the owner and only read permission to others:

```bash
$ chmod 754 filename
```



2. Changing Permissions Symbolically:

   You can also use symbolic representations to set permissions. The symbols include 'u' for user(user), 'g' for group, 'o' for others and 'a' for all(user, group and others).

   - +: Adds permissions
   - -: Removes permissions
   - =: Sets permissions explicitly

For example, to give read and write permissions to the owner, and only execute permissions to others:

```bash
$ chmod u=rw,go=x filename
```



#### Some special Permissions

Though we usually see an octal permission mask expressed as a three-digit number, it is more technically correct to express it in four digits because in addition to read, write and execute permissions, there are some other, less used, permissions settings.



1. **setuid bit (octal 4000) ** - if applied to an executable file it changes the *effective user ID* from that of the real user (the user actually running the program) to that of the program's owner.
   - given to a few programs owned by the superuser
   - When an ordinary user runs a program that is *setuid root*, the program runs with the effective privileges of the superuser.
   - This allows the program to access files and directories that an ordinary user would normally be prohibited from accessing.
   - For example, the `passwd` command has the SUID permission, allowing regular users to change their passwords even though password file is typically only writable by the root user.

2. **setgid bit (octal 2000): ** changes the effective groupd ID from the real group ID of the real user to that of the file owner.
   - If the setgit bit is set on a directory, newly created files in the directory will be given the group ownership of the directory rather the group ownership of the file.
   - This is useful in a shared directory when members of a common group need access to all the files in the directory, regardless of the file owner's primary group.

3. **sticky bit (octal 1000):**  On files, Linux ignores the sticky bit, but if applied to a directory, it prevents users from deleting or renaming files unless the user is either the owner of the directory, the owner of the file or the superuser.

   - This is often used to control access to a shared directory such as */temp*.

   

```bash
** assigning setuid to a program
$ chmod u+s program


** assigning setgid to a directory
$ chmod g+s dir

** assigning the sticky bit to a directory
$ chmod +t dir
```



When viewing the output from ls, we can determine the special permissions.

```bash
** an example of a program that is setuid
-rwsr-xr-x

** an example of a directory that has the setgid attribute
drwxrwsr-x

** an example of a directory with the sticky bit set
drwxrwxrwt
```



#### Changing Your Password

- passwd: setting passwords for yourself and for other users if you have access to superuser privileges.

  ```bash
  $ passwd
  Changing password for chan.
  Current password: 
  New password: 
  BAD PASSWORD: The password is the same as the old one
  New password: 
  BAD PASSWORD: The password contains the user name in some form
  New password: 
  BAD PASSWORD: The password is shorter than 8 characters
  passwd: Have exhausted maximum number of retries for service
  passwd: password unchanged
  ```

- The passwd command will try to enforce use of "strong" passwords which aren't too short, or are similar to previous passwords or dictionary words or are too easily guessed.



#### Processes

Modern operating systems are usually multitasking, meaning they create the illusion of doing more than one thing at once by rapidly switching from one executing program to another. The karnel manages this through the use of processes. Processes are how Linux organizes the different programs waiting for their turn at the CPU.

```bash
$ ps // Report a snapshot of current processes
$ top // Display tasks
$ jobs // List active jobs
$ bg // Place a job in the background
$ fg // Place a job in the foreground
$ kill // Send a signal to a process
$ killall // Kill processes by name
$ shutdown // Shut down or reboot the system
```

#### How a Process Works

- kernel initiates a few of its own processes and launches `init`
- `init` runs a series of shell scripts located in `/etc` called `init scripts` which start all the system services implemented as `daemon programs` programs that just sit in the background and do their thing without having any user interface.
- Like files, processes also have owners and user IDs, effective user IDs and so on.



#### Viewing Processes

```bash
$ ps
PID TTY          TIME CMD
4035 pts/0    00:00:00 bash
5183 pts/0    00:00:00 ps
```

- `ps` show just the process associated with the current terminal session. To see more, we need to add some options.

- TIME field is the amount of CPU time consumed by the process.

- TTY is short for "teletype" and refers to the `controlling terminal` for the process.

- If we add an option, we can get a bigger picture of what the system is doing.

  ```bash
  $ ps x
  --LIST OF PROCESSES WILL APPEAR--
  ```

  Add the `x` option tells `ps` to show all of our processes regardless of what terminal they are controlled by. The presence of a `?` in the TTY column indicates no controlling terminal.

​	A new column titled STAT has been added to the output. STAT is short for `state` and reveals the current status of the process as shown in the table below.

| State | Meaning                                                      |
| ----- | ------------------------------------------------------------ |
| R     | Running. This means the process is running or ready to run.  |
| S     | Sleeping. The process is not running; rather, it is waiting for an event, such as a keystroke or network packet. |
| D     | Uninterruptible sleep. The process is waiting for I/O such as a disk drive. |
| T     | Stopped. The process has been instructed to stop.            |
| Z     | A defunct or "zombie" process. This is a child process that has terminated but has not been cleaned up by its parent. |
| <     | A high-priority process. It's possible to grant more importance to a process, giving it more time on the CPU. This property of a process is called *niceness*. A process with high priority is said to be less nice because it's taking more of the CPU's time, which leaves less for everybody else. |
| N     | A low-priority process. A process with low priority (a nice process) will get processor time only after other processes with higher priority have been serviced. |

- Another popular set of options is `aux` which gives us even more information.

  ```bash
  $ ps aux
  USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
  root           1  0.0  0.0 166904 11968 ?        Ss   18:59   0:01 /sbin/init spla
  root           2  0.0  0.0      0     0 ?        S    18:59   0:00 [kthreadd]
  root           3  0.0  0.0      0     0 ?        I<   18:59   0:00 [rcu_gp]
  root           4  0.0  0.0      0     0 ?        I<   18:59   0:00 [rcu_par_gp]
  ......
  ```

USER - User ID. This is the owner of the process.

VSZ - Virtual memory size.

RSS - Resident set size. This is the amount of physical memory (RAM) the process is using in kilobytes.

START- Time when the process started. For values over 24 hours, a date is used.

###### Viewing Processes Dynamically with top

- `top` command provide a more dynamic view of the machine's activity.
- `ps` provides only a snapshot of the machine's state at the moment the `ps` command is executed.

```bash
$ top
**Displays a continuously updating display of the system processes.
```

- The name `top` comes from the fact that the `top` program is used to see the "top" processes on the system.
- The `top` display consists of two parts:
  - a system summary at the top of the display
  - followed by a table of processes sorted by CPU activity.

Table - `top` information Fields

| Row  | Field         | Meaning                                                      |
| ---- | ------------- | ------------------------------------------------------------ |
| 1    | top           | The is the name of the program                               |
|      | 14:59:20      | This is the current time of day.                             |
|      | up 6:30       | This is called uptime. It is the amount of time since the machine was last booted. In this example, the system has been up for six and a half hours. |
|      | 2 users       | There are two users logged in.                               |
|      | load average: | load average refers to the number of processes that are waiting to run; that is, the number of processes that are in a runnable state and are sharing the CPU. Three values are shown, each for a different period of time. The first is the average for the last 60 seconds, the next the previous 5 minutes, and finally the previous 15 minutes. Value less than 1.0 indicate that the machine is not busy. |
| 2    | Tasks:        | This summarizes the number of processes and their various process states. |
| 3    | Cpu(s):       | This row describes the character of the activities that the CPU is performing. |
|      | 0.7%us        | 0.7 percent of the CPU is being used for `user processes`. This means processes outside the kernel. |
|      | 1.0%sy        | 1.0 percent of the CPU is being used for system (kernel) processes. |
|      | 0.0% ni       | 0.0 percent of the CPU is being used by "nice" (low-priority) processes. |
|      | 98.3%id       | 98,3 percent of the CPU is idle/.                            |
|      | 0.0%wa        | 0.0 percent of the CPU is waiting for I/O.                   |
| 4.   | Mem:          | This shows how physical RAM is being used.                   |
| 5    | Swap:         | This shows how swap space (virtual memory) is being used.    |
|      |               |                                                              |

- `top` is better than the graphical versions because it is faster and it consumes far fewer system resources.
- `top` is similar to `Task Manager` in Windows.



#### Controlling Processes

```bash
$ xlogo
```



- **Interrupting a Process:**

  ```bash
  $ xlogo
  // return to the terminal window and press CTRL-C
  $  
  ```

  

  In a terminal, pressing CTRL-C interrupts a program. Many but not all command line programs can be interrupted by using this technique.

- **Putting a Process in the Background:**

  - an ampersand (&) character is used

  ```bash
  $ xlogo &
  [1] 6049
  $
  ```

​	

​	After entering the command, the xlogo window appeared  and the shell prompt returned. but some funny numbers were printed too. This message is part of a shell feature called `job control`. With this message, the shell is telling us that we have started job number ([1]) and that it has PID 6049. If we run `ps`, we can see our process

```bash
$ ps
PID TTY          TIME CMD
   3835 pts/0    00:00:00 bash
   6049 pts/0    00:00:00 xlogo
   6050 pts/0    00:00:00 ps

```



The shell's job control facility also gives us a way to list the jobs that have been launched from our terminal. Using the `jobs` command, we can see this list:

```bash
$ jobs
[1]+  Running                 xlogo &

```



- **Returning a Process to the Foreground:**

  - A process in the background is immune from terminal keyboard input, including any attempt to interrupt it with CTRL-C. To return the foreground, use the `fg` command in this way

    ```bash
    $ fg %1
    xlogo
    ```

  - The `fg` command followed by a percent sign and the job number (called a jobspec) does the trick. If we have only one background job, the jobspec is optional. To terminate xlogo, press CTRL-C.

- **Stopping (Pausing) a Process:**

  - To stop a foreground process and place it in the background, press CTRL-Z.

  - type xlogo, press ENTER and then press CTRL-Z

    ```bash
    $ xlogo
    ^Z
    [1]+  Stopped                 xlogo
    $
    ```

  - We can either continue the program's execution in the foreground, using the `fg` command, or resume the program's execution in the background with the `bg` command.

    ```bash
    $ bg %1
    [1]+ xlogo &
    $
    ```

    As with the `fg` command, the jobspec is optional if there is only one job.

    Moving a process from the foreground to the background is handy if we launch a graphical program from the command line but forget to place it in the background by appending the trailing &.

  **Note:** Why would we want to launch a graphical program from the command line? There are two reasons.

  - The program you want to run might not be listed on the window manager's menus (such as xlogo).
  - By launching a program from the command line, you might be able to see error messages that would otherwise be invisible if the program were launched graphically. Sometimes, a program will fail to start up when launched from the graphical menu. By launching it from the command line instead, we may see an error message that will reveal the problem. Also, some programs have interesting and useful command line options.

#### Signals

The `kill` command is used to 'kill' processes. This allows  us to terminate programs that need killing (that is, some kind of pausing or termination).

```bash
$ xlogo &
[1] 7624
$ kill 7624
$ ps
PID TTY          TIME CMD
   3835 pts/0    00:00:00 bash
   7625 pts/0    00:00:00 ps
[1]+  Terminated              xlogo

```



**Note:** The `kill` command doesn't exactly "kill" processes; rather, it sends them signals. Signals are one of several ways that the operating system communicates with programs. When the terminal receives one of these keystrokes, it sends a signal to the program in the foreground. In the case of CTRL-C, a signal called INT (interrupt) is sent; with CTRL-Z, a signal called TSTP (terminal stop) is sent. Programs, in turn, "listen" for signals and may act upon them as they are received. The fact that a program can listen and act upon signals allows a program to do things such as save work in progress when it is sent a termination signal.



- **Sending Signals to Processes with `kill`:**

  The `kill` command is used to send signals to programs. Its most common syntax looks like this:

  ```bash
  kill -signal PID...
  ```

  If no signal is specified on the command line, the the TERM (terminate) signal is sent by default. The kill command is most often used to send signals listed in the table below.

  | Number | Name | Meaning                                                      |
  | ------ | ---- | ------------------------------------------------------------ |
  | 1      | HUP  | Hang up. This is a vestige of the good old days when terminals were attached to remote computers with phone lines and modems. The signal is used  to indicate programs that the controlling terminal has "hung up". The effect of the signal can be demonstrated by closing a terminal session. The foreground program running on the terminal will be sent the signal and will terminate. This signal is also used by many daemon programs to cause a reinitialization. This means that when a daemon is sent this signal, it will restart and reread its configuration file. The Apache web server is an example of a daemon that uses the HUP signal in this way. |
  | 2      | INT  | Interrupt. This performs the same function as CTRL-C sent from the terminal. It will usually terminate a program. |
  | 9      | KILL | Kill. This signal is special. Whereas programs may choose to handle signals sent to them in different ways, including ignoring them all together, the KILL signal is never actually sent to the target program. |
  | 15     | TERM | Terminate. This is the default signal sent by the kill command. If a program is still "alive" enough to receive signals, it will terminate. |
  | 18     | CONT | Continue. This will restore a process after a STOP or TSTP signal. This signal is sent by the `bg` and `fg` command. |
  | 19     | STOP | Stop. This signal causes a process to pause without terminating. Like the KILL signal, it is not sent to the target process, and thus it cannot be ignored. |
  | 20     | TSTP | Terminal stop. This is the signal sent by the terminal when CTRL-Z is pressed. Unlike the STOP signal, the TTP signal is received by the program, but the program may choose to ignore it. |



​	Let's try the kill command.

```bash
$ xlogo &
[1] 12169
$ kill -1 12169
$ ps
PID TTY          TIME CMD
   3835 pts/0    00:00:00 bash
  12171 pts/0    00:00:00 ps
[1]+  Hangup                  xlogo

**We can press ENTER again to display the message
[1]+  Hangup                  xlogo

```



Signals may be specified either by number or by name including the name prefixed with the letters SIG.

```bash
chan@CMA:~$ xlogo &
[1] 12190
chan@CMA:~$ kill -INT 12190
chan@CMA:~$ 
[1]+  Interrupt               xlogo
chan@CMA:~$ xlogo &
[1] 12191
chan@CMA:~$ kill -SIGINT 12191
chan@CMA:~$ 
[1]+  Interrupt               xlogo
chan@CMA:~$ 

```



We can also use jobspecs in place of PIDs.

Processes, like files, have owners and you must be the owner of a process or the superuser to  send it signals with `kill`.

Other signals frequently used by the system.

| Number | Name  | Meaning                                                      |
| ------ | ----- | ------------------------------------------------------------ |
| 3      | QUIT  | Quit.                                                        |
| 11     | SEGV  | Segmentation violation. This signal is sent if a program makes illegal use of memory; that is if it tried to write somewhere it was not allowed to write. |
| 28     | WINCH | Window change. This is the signal sent by the system when a window changes size. Some programs, such as top and less will respond to this signal by redrawing themselves to fit the new window dimensions. |

To display a complete list of signals,

```bash
$ kill -l
```



- **Sending Signals to Multiple Processes with `killall`:**

  It's is also possible to send signals to multiple processes matching a specified program or username by using the `killall` command.

  Here is the syntax:

  ```bash
  $ killall [-u user] [-signal] name...
  ```

  ```bash
  chan@CMA:~$ xlogo &
  [1] 12425
  chan@CMA:~$ xlogo &
  [2] 12426
  chan@CMA:~$ killall xlogo
  chan@CMA:~$ 
  [1]-  Terminated              xlogo
  [2]+  Terminated              xlogo
  chan@CMA:~$ 
  ```

  

#### Shutting Down the System

The process of shutting down the system involves the orderly termination of all the processes on the system as well as performing some vital housekeeping chores (such as syncing all of the mounted file stystems) before the system powers off.

Four commands can perform this function:

- halt

- poweroff

- reboot

- shutdown

  ```bash
  $ sudo reboot
  
  ```



The `shutdown` command is a bit more interesting. With it, we can specify which of the actions to perform (halt, power down, or reboot) and provide a time delay to the shutdown event. 

```bash
$ sudo shutdown -h now

** To reboot the system
$ sudo shutdown -r now
```



Once the shutdown command is executed, a message is "broadcast" to all logged-in users warning them of the impending event.



#### More Process-Related Commands

| Command | Description                                                  |
| ------- | ------------------------------------------------------------ |
| pstree  | Outputs a process list arranged in a tree-like pattern showing the parent-child relationships between processes. |
| vmstat  | Outputs a snapshot of system resource usage including memory, swap, and disk I/O. To see a continuous display, follow the command with a time delay (in seconds) for updates. Here's an example: vmstat 5. Terminate the output with CTRL-C |
| xload   | A graphical program that draws a graph showing system load over time. |
| tload   | Similar to the xload program but draws the graph in the terminal. Terminate the output with CTRL-C. |

