# Combine tools seamlessly

- A **pipe (" | ")** essentially is redirection performed twice.

**Example-1**: sort the list of mounted filesystems by space available(in reverse order).

```sh
chan@CMA:~$ df --local | sed '1d' > tmp
chan@CMA:~$ sed --in-place '1d' tmp
chan@CMA:~$ sort -k4nr tmp
/dev/sdb1      488348672 199694336 288654336  41% /media/chan/My Passport
/dev/sda4      159892500  38651276 113046360  26% /
tmpfs            7852276     57988   7794288   1% /dev/shm
tmpfs            1570452       152   1570300   1% /run/user/1000
/dev/nvme0n1p1    262144     36984    225160  15% /boot/efi
tmpfs               5120        12      5108   1% /run/lock
efivarfs             148        68        76  48% /sys/firmware/efi/efivars

```

Using pipes - clean and elegant:

```sh
chan@CMA:~$ df --local | sed '1d' | sort -k4nr
/dev/sdb1      488348672 199694336 288654336  41% /media/chan/My Passport
/dev/sda4      159892500  38651832 113045804  26% /
tmpfs            7852276     57988   7794288   1% /dev/shm
tmpfs            1570452       152   1570300   1% /run/user/1000
tmpfs            1570456      2408   1568048   1% /run
/dev/nvme0n1p1    262144     36984    225160  15% /boot/efi
tmpfs               5120        12      5108   1% /run/lock
efivarfs             148        68        76  48% /sys/firmware/efi/efivars

```

- Not only is this elegant, it is also far superior performance-wise, as writing to memory (the pipe is a memory object) is much faster than writing to disk.



**Example-2**: Display the three processes taking the most (physical) memory. Only display their PID, virtual size (VSZ), resident set size(RSS) (RSS is a fairly accurate measure of physical memory usage).

```sh
chan@CMA:~$ ps au | sed '1d' | awk '{printf("%6d %10d %10d %-32s\n", $2, $5, $6,$11)}' | sort -k3n | tail -n3
  3118      11260       5376 bash                            
  1896     235668       6016 /usr/libexec/gdm-wayland-session
  1900     298204      16256 /usr/libexec/gnome-session-binary
```

