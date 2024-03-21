# Building a Playground



#### Creating Directories

```bash
$ cd
$ mkdir playground
$ cd playground
$ mkdir dir1 dir2
```



#### Copying Files

```bash
$ cp /etc/passwd .

$ ls -l
total 12
drwxrwxr-x 2 me me 4096 2018-01-10 16:40 dir1
drwxrwxr-x 2 me me 4096 2018-01-10 16:40 dir2
-rw-r--r-- 1 me me 1650 2018-01-10 16:07 passwd

$ cp -v /etc/passwd .
`/etc/passwd' -> `./passwd'

$ cp -i /etc/passwd .
cp: overwrite `./passwd'?
```



#### Moving and Renaming Files

```bash
$ mv passwd fun // Rename it to fun

$ mv fun dir1 // move to dir1

$ mv dir1/fun dir2 // move to dir2

$ mv dir2/fun . // brings back to current 						working directory

$ mv fun dir1
$ mv dir1 dir2
$ ls -l dir2
total 4
drwxrwxr-x 2 me me 4096 2018-01-11 06:06 dir1

$ ls -l dir2/dir1
total 4
-rw-r--r-- 1 me me 1650 2018-01-10 16:33 fun
```



#### Putting Everything Back

```bash
$ mv dir2/dir1 .
$ mv dir1/fun .
```



#### Creating Hard Links

```bash
$ ln fun fun-hard
$ ln fun dir1/fun-hard
$ ln fun dir2/fun-hard
```



Now we have four instances of the fiel fun



```bash
$ ls -l
total 16
drwxrwxr-x 2 me me 4096 2018-01-14 16:17 dir1
drwxrwxr-x 2 me me 4096 2018-01-14 16:17 dir2
-rw-r--r-- 4 me me 1650 2018-01-10 16:33 fun
-rw-r--r-- 4 me me 1650 2018-01-10 16:33 fun-hard

$ ls -li
total 16
12353539 drwxrwxr-x 2 me me 4096 2018-01-14 16:17 dir1
12353540 drwxrwxr-x 2 me me 4096 2018-01-14 16:17 dir2
12353538 -rw-r--r-- 4 me me 1650 2018-01-10 16:33 fun
12353538 -rw-r--r-- 4 me me 1650 2018-01-10 16:33 fun-hard

** Both fun and fun-hard share the same inode number
```



#### Creating Symbolic Links

```bash
$ ln -s fun fun-sym
$ ln -s ../fun dir1/fun-sym
$ ln -s ../fun dir2/fun-sym


$ ls -l dir1
total 4
-rw-r--r-- 4 me me 1650 2018-01-10 16:33 fun-hard
lrwxrwxrwx 1 me me 6 2018-01-15 15:17 fun-sym -> ../fun


$ ln -s dir1 dir1-sym
$ ls -l
total 16
drwxrwxr-x 2 me me 4096 2018-01-15 15:17 dir1
lrwxrwxrwx 1 me me 4 2018-01-16 14:45 dir1-sym -> dir1
drwxrwxr-x 2 me me 4096 2018-01-15 15:17 dir2
-rw-r--r-- 4 me me 1650 2018-01-10 16:33 fun
-rw-r--r-- 4 me me 1650 2018-01-10 16:33 fun-hard
lrwxrwxrwx 1 me me 3 2018-01-15 15:15 fun-sym -> fun
```



#### Removing  Files and Directories

```bash
$ rm fun-hard
$ ls -l
total 12
drwxrwxr-x 2 me me 4096 2018-01-15 15:17 dir1
lrwxrwxrwx 1 me me 4 2018-01-16 14:45 dir1-sym -> dir1
drwxrwxr-x 2 me me 4096 2018-01-15 15:17 dir2
-rw-r--r-- 3 me me 1650 2018-01-10 16:33 fun
lrwxrwxrwx 1 me me 3 2018-01-15 15:15 fun-sym -> fun

$ rm -i fun
rm: remove regular file `fun'?

$ ls -l
total 8
drwxrwxr-x 2 me me 4096 2018-01-15 15:17 dir1
lrwxrwxrwx 1 me me 4 2018-01-16 14:45 dir1-sym -> dir1
drwxrwxr-x 2 me me 4096 2018-01-15 15:17 dir2
lrwxrwxrwx 1 me me 3 2018-01-15 15:15 fun-sym -> fun

$ less fun-sym
fun-sym: No such file or directory

$ rm fun-sym dir1-sym
$ ls -l
total 8
drwxrwxr-x 2 me me 4096 2018-01-15 15:17 dir1
drwxrwxr-x 2 me me 4096 2018-01-15 15:17 dir2

$ cd
$ rm -r playground
```



#### Exercising Our Privileges

(Note to myself): Refer to page 129 on google drive.
