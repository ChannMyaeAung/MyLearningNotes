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