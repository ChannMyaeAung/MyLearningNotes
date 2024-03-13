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


$ echo $(((5**2) * 3))
75


```

