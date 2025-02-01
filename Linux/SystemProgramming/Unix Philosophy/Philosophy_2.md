# One tool to do one task

- On Unix, there is no one utility to list mounted filesystems and sort them by available space simultaneously.

```sh
chan@CMA:~$ df --local > tmp
chan@CMA:~$ sed --in-place '1d' tmp
chan@CMA:~$ sort -k4nr tmp
/dev/sdb1      488348672 199694336 288654336  41% /media/chan/My Passport
/dev/sda4      159892500  38648812 113048824  26% /
tmpfs            7852276     41604   7810672   1% /dev/shm
tmpfs            1570452       148   1570304   1% /run/user/1000
tmpfs            1570456      2400   1568056   1% /run
/dev/nvme0n1p1    262144     36984    225160  15% /boot/efi
tmpfs               5120        12      5108   1% /run/lock
efivarfs             148        68        76  48% /sys/firmware/efi/efivars
```

- `sed` eliminates the first line, the header from the output of `df`.
- Here, we used three utilities, `df`, to list the mounted filesystems, `sed` to eliminate the header line and `sort` to sort whatever input it is given.
- Combine them all, and we get more than the sum of its parts! Unix tools typically do one task and they do it to its logical conclusion; no one does it better!