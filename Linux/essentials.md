# Essentials of Linux

### Linux Is About Imagination

```
When I am asked to explain the difference between Windows and Linux, I
often use a toy analogy.
Windows is like a Game Boy. You go to the store and buy one all shiny
new in the box. You take it home, turn it on, and play with it. Pretty graphics,
cute sounds. After a while, though, you get tired of the game that came with
it, so you go back to the store and buy another one. This cycle repeats over
and over. Finally, you go back to the store and say to the person behind the
counter, “I want a game that does this!” only to be told that no such game
exists because there is no “market demand” for it. Then you say, “But I only
need to change this one thing!” The person behind the counter says you can’t
change it. The games are all sealed up in their cartridges. You discover that
your toy is limited to the games that others have decided you need.
Linux, on the other hand, is like the world’s largest Erector Set. You open
it, and it’s just a huge collection of parts. There’s a lot of steel struts, screws,
nuts, gears, pulleys, motors, and a few suggestions on what to build. So, you
start to play with it. You build one of the suggestions and then another. After a
while you discover that you have your own ideas of what to make. You don’t
ever have to go back to the store, as you already have everything you need.
The Erector Set takes on the shape of your imagination. It does what you want.
Your choice of toys is, of course, a personal thing, so which toy would you
find more satisfying?
```



## Soft Links (Symbolic links or Symlinks)

- **Concept :**  A soft link is a separate file that contains the path to the original file. It's like a shortcut that points to another location.
- **Behaviour:**
  - Deleting the original file breaks the soft link, as it points to a non-existent location.
  - Changes made to the original file are reflected as long as the original file remains accessible through the path stored in the soft link.
  - Hard links offer more flexibility:
    - They can link files across different systems.
    - They can link directories (not possible with hard links).

- **Things To Remember:**  Most file operations are carried out on the link's target, not the link itself. *rm* is an exception. When you delete a link, it is the link that is deleted, not the target.



## Hard Links

- **Concept :**  A hard link acts like an additional name for the same data block on the storage device. Hard links share the same inode number, which is a unique identifier for a file's data location.

- **Behaviour:**
  - Deleting the original file won't affect the hard link, as it still points to the valid data.
  - Changes made to the original file are reflected in the hard link, and vice versa, since they're essentially two doorways to the same data.
  - Hard links have limitations:
    - They can only be created within the same file system which means they cannot span physical devices.
    - They cannot link directories, only files.



## Analogy

- Imagine a library book. A hard link is like having another library card for the same book. If the original book is lost, you can still access it with the other card. A soft link is like a note saying where to find the book on the shelf. If the book is moved, the note becomes useless unless it is updated with the new location.



## Optimal Usage

- **File location:** if the files reside on different file systems, use soft links.
- **Data integrity:** If both references must always point to the same data, use hard links (assuming they're on the same file system).
- **Link updates:** If the original file might be moved, soft links might require manual updates to reflect the new location.



## Redirection

I/O redirection allows us to change where output goes and where input comes from.



A standard output of many programs consists of two types:

- The program's results: that is the data the program is designed to produce
- Status and error messages that tell us how the program is getting along



E.g. *ls* displays its results and its error messages on the screen.

###### ls Sends

- their results to standard output (stdout).

- status messages to standard error (stderr).

**By default, both standard output and standard error are linked to the screen and not saved into a disk file.**

Many programs take input from standard input (stdin) which is by default attached to the keyboard.



## Difference Between > And |

The redirection operator connects a command with a file, while the pipeline operator connects the output of one command with the input of a second command.

**Redirection:**

- **Destination:** Files. Redirection sends the output (stdout or stderr) of a command to a file.
- **Functionality:** Saves or captures the output for later use or analysis.
  - Examples:
    - `ls > filelist.txt` - Saves the directory listing to a file named "filelist.txt".
    - `cat errors.log 2> error_messages.txt` - Captures error messages (stderr) to a separate file "error_messages.txt".

**Pipelines:**

- **Destination:** Another command. Pipelines use the `|` symbol to connect the output of one command as the input for the next.
- **Functionality:** Chains commands together for processing in a single line.
  - Example:
    - `ls | grep .txt` - Shows only files with the ".txt" extension by piping the directory listing (ls) to grep for filtering.

Here's a table summarizing the key differences:

| Feature       | Redirection              | Pipelines                |
| :------------ | :----------------------- | :----------------------- |
| Destination   | Files                    | Another command          |
| Functionality | Saves/captures output    | Chains commands together |
| Symbol        | `>`, `>>`, `<` (various) | `                        |

**In simpler terms:**

- Think of redirection as taking output and putting it in a box (file) for later.
- Think of pipelines as taking output and feeding it directly to another tool (command) for further work.



Pathname Expa nsion of Hidden Files

```
As we know, filenames that begin with a period character are hidden. Pathname
expansion also respects this behavior. An expansion such as the following does
not reveal hidden files:
echo * 

It might appear at first glance that we could include hidden files in an
expansion by starting the pattern with a leading period, like this:
echo .*


It almost works. However, if we examine the results closely, we will see
that the names . and .. will also appear in the results. Because these names
refer to the current working directory and its parent directory, using this pattern
will likely produce an incorrect result. We can see this if we try the following
command:
ls -d .* | less

To better perform pathname expansion in this situation, we have to
employ a more specific pattern.
echo .[!.]*

This pattern expands into every filename that begins with only one period
followed by any other characters. This will work correctly with most hidden files

(though it still won’t include filenames with multiple leading periods). The ls com-
mand with the -A option (“almost all”) will provide a correct listing of hidden files.

ls -A
```





#### Permissions

-rwx------ 

A regular file that is readable, writable, and executable by the file’s owner. No one else has any access.



-rw------- 

A regular file that is readable and writable by the file’s owner. No one else has any access.



-rw-r--r-- A regular file that is readable and writable by the file’s owner.
Members of the file’s owner group may read the file. The file is world-readable.



-rwxr-xr-x 

A regular file that is readable, writable, and executable by the file’s
owner. The file may be read and executed by everybody else.



-rw-rw---- 

A regular file that is readable and writable by the file’s owner and members of the file’s group owner only.



lrwxrwxrwx 

A symbolic link. All symbolic links have “dummy” permissions. The real permissions are kept with the actual file pointed to by the symbolic link.



drwxrwx--- 

A directory. The owner and the members of the owner group may
enter the directory and create, rename, and remove files within the
directory.

drwxr-x--- 

A directory. The owner may enter the directory and create, rename,
and delete files within the directory. Members of the owner group
may enter the directory but cannot create, delete, or rename files.



```
What the Heck Is Octal?

Octal (base 8) and its cousin, hexadecimal (base 16), are number systems
often used to express numbers on computers. We humans, owing to the fact
that we (or at least most of us) were born with 10 fingers, count using a base
10 number system. Computers, on the other hand, were born with only one
finger and thus do all their counting in binary (base 2). Their number system
has only two numerals, 0 and 1. So, in binary, counting looks like this:
0, 1, 10, 11, 100, 101, 110, 111, 1000, 1001, 1010, 1011. . .
In octal, counting is done with the numerals zero through seven, like so:
0, 1, 2, 3, 4, 5, 6, 7, 10, 11, 12, 13, 14, 15, 16, 17, 20, 21. . .
Hexadecimal counting uses the numerals zero through nine plus the letters
A through F.
0, 1, 2, 3, 4, 5, 6, 7, 8, 9, A, B, C, D, E, F, 10, 11, 12, 13. . .
While we can see the sense in binary (since computers have only one
finger), what are octal and hexadecimal good for? The answer has to do with
human convenience. Many times, small portions of data are represented on
computers as bit patterns. Take, for example, an RGB color. On most computer
displays, each pixel is composed of three color components: eight bits of red,
eight bits of green, and eight bits of blue. A lovely medium blue would be a
24-digit number.

010000110110111111001101

How would you like to read and write those kinds of numbers all day? I
didn’t think so. Here’s where another number system would help. Each digit in

a hexadecimal number represents four digits in binary. In octal, each digit rep-
resents three binary digits. So, our 24-digit medium blue could be condensed

to a six-digit hexadecimal number, 436FCD.
Because the digits in the hexadecimal number “line up” with the bits in
the binary number, we can see that the red component of our color is 43, the
green 6F, and the blue CD.
These days, hexadecimal notation (often spoken as hex) is more common
than octal, but as we will soon see, octal’s capability to express three bits of
binary will be very useful . . .
```



File Modes in Binary and Octal

| Octal | Binary | File mode |
| ----- | ------ | --------- |
| 0     | 000    | ---       |
| 1     | 001    | --x       |
| 2     | 010    | -w-       |
| 3     | 011    | -wx       |
| 4     | 100    | r--       |
| 5     | 101    | r--x      |
| 6     | 110    | rw-       |
| 7     | 111    | rwx       |



By using three octal digits, we can set the file mode for the owner, group owner, and world

```bash
$ > foo.txt
$ ls -l foo.txt
-rw-rw-r-- 1 chan chan 0 Mar 18 22:15 foo.txt
$ chmod 600 foo.txt
$ ls -l foo.txt
-rw------- 1 chan chan 0 Mar 18 22:15 foo.txt

```

​	By passing the argument 600, we were able to set the permissions of the owner to read and write while removing all permissions from the group owner and world.

A few common ones: 700(rwx), 6(rw-), 5(r-x), 4(r--) and 0(---)

chmod also supports a symbolic notation for specifying file modes. Symbolic notation is divided into three parts.

- Who the change will affect

- Which operation will be performed

- What permission will be set

  To specify who is affected, a combination of the characters u, g, o and a is used.

| Symbol | Meaning                                                 |
| ------ | ------------------------------------------------------- |
| u      | Short for "user" but means the file or directory owner. |
| g      | Group owner.                                            |
| o      | Short for "others" but means world                      |
| a      | Short for "all." This is a combination of u, g and o.   |



chmod Symbolic Notation Examples

| Notation   | Meaning                                                      |
| ---------- | ------------------------------------------------------------ |
| u+x        | Add execute permission for the owner.                        |
| u-x        | Remove execute permission from the owner.                    |
| o-rw       | Remove the read and write permissions from anyone besides the owner and group owner. |
| go-rw      | Set the group owner and anyone besides the owner to have read and write permissions. If either the group owner or the world previously had execute permission, it is removed. |
| u+x, go-rx | Add execute permission for the owner and set the permissions for the group and others to read and execute. Multiple specifications may be separated by commas. |
|            |                                                              |
