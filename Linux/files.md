---

---

# Files on Linux



1. **`/`** (Root Directory): This is the starting point of the file system hierarchy, and all other files and directories branch out from here.
2. **`/bin`**: This directory contains essential user binary files or executable programs needed for system boot and user commands like `ls`, `cp`, `mv`, etc.
3. **`/sbin`**: This directory holds binary executable files for system administration, usable only by the root user or superuser.
4. **`/etc`**: This is the central directory for system configuration files, such as `/etc/passwd` (user account information), `/etc/hosts` (hostname to IP address mapping), and `/etc/fstab` (filesystem table).
5. **`/dev`**: This directory contains device files representing physical or virtual devices, such as `/dev/sda` (hard disk), `/dev/tty` (terminal), and `/dev/null` (null device).
6. **`/proc`**: This is a virtual filesystem that provides information about running processes and system resources.
7. **`/var`**: This directory contains variable data files, such as log files (`/var/log`), spool files (`/var/spool`), and temporary files (`/var/tmp`).
8. **`/tmp`**: This is a directory for temporary files that can be accessed by all users and is usually cleared on system reboot.
9. **`/usr`**: This directory holds user utilities, applications, libraries, and multi-user documentation. Subdirectories include `/usr/bin` (user binary files), `/usr/sbin` (system admin binaries), and `/usr/lib` (libraries).
10. **`/home`**: This is the parent directory for individual users' home directories, where their personal files, settings, and configurations are stored.
11. **`/root`**: This is the home directory for the root (superuser) account.
12. **`/boot`**: This directory contains the bootloader files (e.g., GRUB) and the Linux kernel.
13. **`/lib`**: This directory contains shared libraries required by the system and applications.
14. **`/opt`**: This directory is used for optional or third-party software packages.
15. **`/mnt`**: This is a temporary mount point for mounting external filesystems (e.g., USB drives, network shares).
16. **`/media`**: This is another common mount point for removable media devices (e.g., USB drives, CD-ROMs).



#### /usr

> /user directory tree is likely the largest one on a Linux system. It contains all the programs and support files  used by regular users.




#### /usr/bin (user binary files)

> /usr/bin contains the binaries or  executable programs installed by your Linux distribution.

E.g ls, cp, mv



#### /usr/sbin (system admin binaries)

> /usr/sbin contains system binaries that should only be executed by the root user. (E.g. mount, deleteuser)



#### /etc - ET CETERA or Editable Text Config

>  This is the central directory for system configuration files, such as `/etc/passwd` (user account information), `/etc/hosts` (hostname to IP address mapping), and `/etc/fstab` (filesystem table).
>
> Here's a breakdown of the `/etc` directory's role:
>
> - **Configuration Files:** It stores essential configuration files that govern various system aspects like network settings (e.g., `/etc/network/interfaces`), user accounts (e.g., `/etc/passwd`), printers (e.g., `/etc/cups/printers.conf`), and more. Modifying these files allows you to customize system behavior.
> - **Historical Context:** Initially, `/etc` served as a generic repository for various files that didn't fit neatly into other standard directories. Over time, its purpose evolved into specifically storing configuration files.
> - **Importance:** The configuration files within `/etc` are vital for the system's proper operation. Editing them requires caution, as mistakes can lead to malfunctions. It's generally recommended for experienced users to modify these files.
