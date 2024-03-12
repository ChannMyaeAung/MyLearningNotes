# Essentials of Linux

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
- pen_spark