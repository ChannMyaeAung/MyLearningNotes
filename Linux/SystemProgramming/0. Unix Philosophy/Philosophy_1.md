# Everything is a process - if it is not a process, it's a file

- A process is an instance of a program in execution.
- A file is an object on the filesystem (regular file with plain text or binary content, directory, a symbolic link, a device-special file, a named pipe, or a (Unix-domain) socket).
- The Unix design philosophy abstracts peripheral devices (keyboard, monitor, mouse, a sensor, and touchscreen) as **files - what it calls device files.**
  - By doing this, Unix allows the application programmer to conveniently ignore the details and just treat(peripheral) devices as though they are ordinary disk files.
- The kernel provides a layer to handle this very abstraction - also called the **Virtual Filesystem Switch(VFS)**.
- So, with this in place, the application developer can open a device file and perform I/O (reads and writes) upon it, all using the usual API interfaces provided.
- A **filter** is a program that reads from its standard input, possibly modifies the input, and writes the filtered result to its standard output. E.g `cat`, `wc`, `sort`, `grep`, `perl`, `head1 and `tail`.

```sh
sort fruit.txt
```

- In the above example, if `sort` is a filter, it should read from its stdin (keyboard) and writes to stdout. But when we provide a parameter, the sort program treats it as **standard input**.
- So we could say `sort fruit.txt` is identical to `sort < fruit.txt`.