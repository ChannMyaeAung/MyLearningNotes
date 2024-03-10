# Commands

```bash
$ pwd       // print name of current working directory

**get lists of files from multiple directories**
$ ls ~ /usr   

**Determine a file type with file**
$ file picture.jpg
picture.jpg: JPEG image data, JFIF standard 1.01

**Viewing file contents with less
$ less filename

**Creating directories
$ mkdir dir1 
$ mkdir dir1 dir2 dir3  // create 3 directories named dir1, dir2 and dir3

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


**Remove files and directories
$ rm item... // item is one or more files or directories

**Create links
$ ln  // Creates a hard link
$ ln -s item link   // Creates a symbolic links (symlinks or soft links)
// where item is either a file or a directory
```



