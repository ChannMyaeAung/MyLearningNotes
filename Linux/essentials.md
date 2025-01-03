# Essentials of Linux

Updating the system or apps on Ubuntu Linux:

```bash
sudo apt update
sudo apt upgrade code
```



The commands `sudo apt update` and `sudo apt upgrade` are both used in managing packages on Debian-based systems, such as Ubuntu, but they serve different purposes. Here's a detailed explanation of each:

### `sudo apt update`

- **Purpose**: The `sudo apt update` command is used to update the package lists for the repositories configured on your system. This command does not upgrade any packages; it merely refreshes the list of available packages and their versions.
- **Function**: It fetches the latest version information for all packages from the configured repositories (e.g., from `/etc/apt/sources.list` and `/etc/apt/sources.list.d/`).
- **Use Case**: Run this command before installing a new package to ensure you get the latest version available. It is typically the first command you run when performing system maintenance or before `sudo apt upgrade`.

#### Example:

```sh
sudo apt update
```

### `sudo apt upgrade`

- **Purpose**: The `sudo apt upgrade` command is used to install the newest versions of all currently installed packages on your system that have updates available. This command will upgrade your installed packages to the latest versions as per the updated package lists.
- **Function**: It will go through the list of available package updates obtained from `sudo apt update` and upgrade the installed packages to these new versions. It will install new package versions if needed and remove obsolete packages.
- **Use Case**: Run this command after `sudo apt update` to actually apply the updates and upgrade your installed packages.

#### Example:

```sh
sudo apt upgrade
```

### Combined Usage

- **Routine Update**: It's common to run `sudo apt update` followed by `sudo apt upgrade` to ensure your system packages are up-to-date.

#### Combined Example:

```sh
sudo apt update
sudo apt upgrade
```

### Differences in Detail

1. **Operation Scope**:
   - `sudo apt update`: Updates the package lists.
   - `sudo apt upgrade`: Upgrades installed packages based on the updated package lists.
2. **Execution Order**:
   - `sudo apt update` should always be run before `sudo apt upgrade` to ensure you are upgrading to the latest package versions available.
3. **Package Changes**:
   - `sudo apt update`: No packages are upgraded, installed, or removed.
   - `sudo apt upgrade`: Packages are upgraded; however, no packages are removed or newly installed (except for new versions of existing packages).

### Advanced Usage

- `sudo apt full-upgrade`:
  - In some cases, you might use `sudo apt full-upgrade` (or `sudo apt dist-upgrade` in older versions) instead of `sudo apt upgrade`. This command can handle changing dependencies and might remove some packages to allow the upgrade of others.

#### Example:

```sh
sudo apt update
sudo apt full-upgrade
```

- `sudo apt autoremove`:
  - After upgrading packages, you might want to clean up unnecessary packages that were automatically installed to satisfy dependencies for other packages and are no longer needed.

#### Example:

```sh
sudo apt autoremove
```

By understanding these commands and their differences, you can effectively manage your system's packages and keep your software up-to-date.

If you didn't install Visual Studio Code through the repository but used a `.deb` file instead, you’ll need to download the latest `.deb` file from the Visual Studio Code website and install it.

1. **Install the `.deb` file**:

   ```bash
   sudo dpkg -i ~/Downloads/code_*.deb
   sudo apt -f install
   ```

This will update Visual Studio Code to the latest version on our system.

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



In Linux, a distribution (often abbreviated as "distro") refers to a version or variant of the Linux operating system that includes the Linux kernel, system utilities, libraries, and additional software packaged together. Linux distributions typically include a package management system to simplify the process of installing, updating, and removing software.

There are many different Linux distributions available, each with its own goals, target audience, and characteristics. Some of the most popular Linux distributions include Ubuntu, Debian, Fedora, CentOS, Arch Linux, and openSUSE, among others.

These distributions can vary significantly in terms of their default desktop environment, package management system (such as apt, yum, pacman), release cycle, level of user-friendliness, and the software they include out of the box. Users can choose a distribution that best fits their needs and preferences, whether it's for general-purpose desktop use, server deployment, specialized applications, or embedded systems.



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



Pathname Expansion of Hidden Files

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



-rw-r--r-- 

A regular file that is readable and writable by the file’s owner.
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



#### Changing Identities

- When we want to gain superuser privileges to carry out some administrative task.
- Three ways to take on an alternate identity
  - Log out and log back in as the alternate user.
  - Use the  `su` command.
  - Use the `sudo` command.



#### su: Run a shell with substitute User and Group IDs

The `su` command allows you to assume the identity of another user and either start a new shell session with that user's ID or issue a single command as that user.

The `su` command is used to start a shell as another user.

```bash
su [-[l]] [user]


** if the -l option is included, the resulting shell session is a login shell for the specified user. This means the user's environment is loaded and the working directory is changed to the user's home directory.
** if the user is not specified, the superuser is assumed.
```

```bash
$ su -
// -l is abbreviated as -
Password:
[root@~]# 

```

Indicating that the shell has superuser priviledges (# instead of $) and the current working directory is now the home directory for the superuser (normally /root)

Enter **exit** to return to the previous shell.

````bash
# exit
$ 
````



**Note:** The root account is disabled by default in Ubuntu. so there is no root password, that's why `su` failed with an authentication error. The initial user account is granted full access to superuser privileges via `sudo` and may grant similar powers to subsequent user accounts.

Use `sudo` to become root:

````bash
sudo -i
[sudo] password for chan: 
root@CMA:~# exit
logout

````



#### sudo: Execute a Commmand As Another User

- The administrator can configure `sudo` to allow an ordinary user to execute commands as a different user (usually the superuser) in a controlled way.
- In particular, a user may be restricted to one or more specific commands and no others.
- The use of `sudo` does not require access to the superuser's password. Authenticating using `sudo` requires the user's password.



Let's say the `sudo` has been configured to allow us to run a fictitious backup program called backup_script which requires superuser privileges. 

```bash
$ sudo backup_script
Password:
System Backup Starting...
```



One important difference between `su` and `sudo` is that `sudo` does not start a new shell nor does it load another user's environment.



- To start an interactive superuser session, specify the -i option.

  ```bash
  $ sudo -l
  User me may run the following commands on this host:
  (ALL) ALL
  ```

  



#### chown: Change File Owner and Group

- Used to change the owner and group owner of a file or directory.

- Superuser privileges are required to use this command.

- The syntax of chown looks like this:

  ```bash
  chown [owner][:[group]] file...
  ```

  ```bash
  chown [options] newowner:newgroup file(s)
  ```

  

  chown can change the file owner and/or the file group owner depending on the first argument of the command.

- You can specify both the new owner and group using the `newowner:newgroup` syntax.

  ```bash
  chown user:group filename
  ```

- **Changing Owner Only:**

  If you want to change the owner and keep the group the same, you can omit the group part.

  ```bash
  chown user filename
  ```

- **Changing Group Only:**

  Similarly, if you only want to change the group and keep the owner the same, you can specify the group without specifying  the owner.

  ```bash
  chown :group filename
  ```

  

- chown Argument Examples



| Argument   | Results                                                      |
| ---------- | ------------------------------------------------------------ |
| bob        | Changes the ownership of the file from its current owner to user bob. |
| bob::users | Changes the ownership of the file from its current owner to user bob and changes the file group owner to group users. |
| :admins    | Changes the group owner to the group admins. The file owner is unchanged. |
| bob:       | Changes the file owner from the current owner to user bob and changes the group owner to the login group of user bob. |

##### Examples

Let's say we have two users: janet, who has access to superuser privileges, and tony, who does not. User janet wants to copy a file from her home directory to the home directory of tony. Because janet wants tony to be able to edit the file, janet changes the ownership of the copied file from janet to tony.

```bash
[janet@linuxbox ~]$ sudo cp myfile.txt ~tony
Password:

[janet@linuxbox ~]$ sudo ls -l ~tony/myfile.txt
-rw-r--r-- 1 root root root 2018-03-20 14:30 /home/tony/myfile.txt

[janet@linuxbox ~]$ sudo chown tony: ~tony/myfile.txt
[janet@linuxbox ~]$ sudo ls -l ~tony/myfile.txt
-rw-r--r-- 1 tony tony tony 2018-03-20 14:30 /home/tony/myfile.txt
```

- Janet copy the file from her directory to the home directory of user tony.
- Janet changes the ownership of the file from root(a result of using sudo) to tony.
- Using the trailing colon in the first argument, janet also changed the group ownership of the file to the login group of tony which happens to be group tony.

**Note:** after the first use of `sudo`, janet was not prompted for her password. This is because `sudo` in most configurations "trusts" you for several minutes until its timer runs out.



Let's say you have a file named "example.txt" and you want to change its permissions to give read and write permissions to the owner and only read permissions to others. and also change the owner to "user" and the group to "group"

```bash
// Change permissions
chmod 640 example.txt

//Change owner and group
chown user:group example.txt
```



#### Package Management

Package Management is a method of installing and maintaining software on the system.



##### Packing Systems

Most distributions fall into one of two camps of packaging technologies: the Debian .deb camp and Red Hat .rpm camp.

| Packaging System     | Distributions (partial listing)                    |
| -------------------- | -------------------------------------------------- |
| Debain-style (.deb)  | Debain, Ubuntu, Linux Mint, Raspbian               |
| Red-Hat-style (.rpm) | Fedora, CentOs, Red Hat Enterprise Linux, OpenSUSE |



##### How a Package System Works

The method of software distribution found in the proprietary software industry usually entails buying a piece of installation media such as "install disk" or visiting each vendor's website and downloading the product and then running an "installation wizard" to install a new application on the system.

Linux doesn't work that way. Virtually all software for a Linux system will be found on the Internet. Most of it will be provided by the distribution vendor in the form of package files and the rest will be available in source code form that can be installed manually.



##### High- and Low-Level Package Tools

Package management systems usually consist of two types of tools.

- Low-level tools that handle tasks such as installing and removing package files.
- High-level tools that perform metadata searching and dependency resolution.

| Distributions                            | Low-level tools | High-level tools       |
| ---------------------------------------- | --------------- | ---------------------- |
| Debian-style                             | dpkg            | apt-get, apt, aptitude |
| Fedora, Red hat Enterprise Linux, CentOS | rpm             | yum, dnf               |



##### Common Package Management Tasks

Package_name - the actual name of a package.

Package_term - the name of the file that contains the package.



###### Finding a Package in a Repo

Using the high-level tools to search repository metadata,

| Style   | Command(s)                                                   |
| ------- | ------------------------------------------------------------ |
| Debian  | apt-get update,                                          apt-cache search search_string |
| Red Hat | yum search search_string                                     |

E.g yum search emacs



###### Installing a Package from a Repo

High-level tools permit a package to be downloaded from a repository and installed with full dependency resolution

| Style   | Command(s)                                   |
| ------- | -------------------------------------------- |
| Debian  | apt-get update, apt-get install package_name |
| Red Hat | yum install package_name                     |

E.g apt-get update; apt-get install emacs



###### Installing a Package from a Package File

If a package file has been downloaded from a source rather than a repository, it can be installed directly using a low-level tool

| Style   | Command(s)           |
| ------- | -------------------- |
| Debian  | dpkg -i package_file |
| Red Hat | rpm -i package_file  |

E.g rpm -i emacs-22.1-7.fc7-i386.rpm

**Note:** Because this technique uses the low-level rpm program to perform the installation, no dependency resolution is performed. If rpm discovers a missing dependency, rpm will exit with an error.



###### Removing a Package

Package can be uninstalled using either the high-level or low-level tools.

| Style   | Command(s)                  |
| ------- | --------------------------- |
| Debian  | apt-get remove package_name |
| Red Hat | yum erase package_name      |

E.g apt-get remove emacs



###### Updating Packages from a Repository

The most common package management task is keeping the system up-to-date with the latest versions of packages. The high-level tools can perform this vital task in a single step.

| Style   | Command(s)                      |
| ------- | ------------------------------- |
| Debian  | apt-get update; apt-get upgrade |
| Red Hat | yum update                      |

E.g apt-get update; apt-get upgrade



###### Upgrading a Packing from a Package File

If an updated version of a package has been downloaded from a non-repository source, it can be installed, replacing the previous version.

| Style   | Command(s)           |
| ------- | -------------------- |
| Debian  | dpkg -i package_file |
| Red Hat | rpm -U package_file  |

E.g rpm -U emacs-22.1-7.fc7-i386.rpm

**Note:** dpkg does not have a specific option for upgrading a package vs installing one as rpm does.



###### Listing installed Packages

| Style   | Command(s) |
| ------- | ---------- |
| Debian  | dpkg -l    |
| Red Hat | rpm -qa    |



###### Determining Whether a Package is Installed

below are low-level tools we can use to display whether a specified package is installed.

| Style   | Command(s)           |
| ------- | -------------------- |
| Debian  | dpkg -s package_name |
| Red Hat | rpm -q package_name  |

E.g dpkg -s emacs



###### Displaying information about an installed Package

| Style   | Command(s)                  |
| ------- | --------------------------- |
| Debian  | apt-cache show package_name |
| Red Hat | yum info package_name       |

E.g apt-cache show emacs



###### Finding Which Package Installed a File

To determine what package is responsible for the installation of a particular file,

| Style   | Command(s)        |
| ------- | ----------------- |
| Debian  | dpkg -S file_name |
| Red Hat | rpm -qf file_name |

E.g rpm -qf /usr/bin/vim



To install the "Space Mono" font on Ubuntu and use it in Visual Studio Code (VS Code), you can follow these steps:

### Step 1: Download and Extract the Font

1. **Download the font**: You've already downloaded the "Space Mono" font as a zip file.

2. Extract the zip file:

   - Open the terminal and navigate to the directory where the zip file is located.

   - Use the `unzip`command to extract the zip file. For example:

     ```sh
     unzip space-mono.zip -d space-mono
     ```

### Step 2: Install the Font

1. Create a directory for the font: 

   ​	Fonts can be installed either system-wide or for a single user. For simplicity, we'll install it for a single user.

   ```sh
   mkdir -p ~/.local/share/fonts
   ```

2. Copy the font files: Move the extracted font files (usually  `.ttf`or  `.otf` files) to the fonts directory.

   ```sh
   cp space-mono/*.ttf ~/.local/share/fonts/
   ```

3. Update the font cache:

   ```sh
   fc-cache -fv
   ```

### Step 3: Configure VS Code to Use the Font

1. **Open VS Code**: Launch Visual Studio Code.

2. Open Settings:

   - You can open the settings by clicking on the gear icon in the lower left corner and then selecting "Settings".
   - Alternatively, you can press `Ctrl` + `,` to open the settings directly.

3. **Search for Font Family**: In the search bar at the top of the settings window, type `Font Family`.

4. Set the Font Family: In the "Editor: Font Family" field, add `"Space Mono"`at the beginning of the list of fonts. Make sure to enclose the font name in double quotes. For example:

   ```tex
   "Space Mono", Consolas, "Courier New", monospace
   ```

### Verifying the Installation

1. **Restart VS Code**: Sometimes, VS Code needs to be restarted to apply the new font settings.
2. **Check the Font**: Open any file in VS Code and check if the font has changed to "Space Mono".

### Example Commands

Here is a summary of the commands you might use in the terminal:

```shell
cd ~/Downloads
unzip space-mono.zip -d space-mono
mkdir -p ~/.local/share/fonts
cp space-mono/*.ttf ~/.local/share/fonts/
fc-cache -fv
```

### Troubleshooting

- **Font not appearing**: If the font does not appear in VS Code, ensure that the font files are correctly copied to the `~/.local/share/fonts` directory and that the font cache is updated.
- **Correct font name**: Double-check that the font name in VS Code settings matches the actual font name. Sometimes the name in the settings might need to match exactly, including capitalization.

By following these steps, you should be able to install the "Space Mono" font on Ubuntu and configure VS Code to use it.
