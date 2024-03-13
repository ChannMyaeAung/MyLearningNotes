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

