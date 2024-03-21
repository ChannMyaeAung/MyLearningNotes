#### Advanced Keyboard Tricks

- clear - Clear the terminal screen
- history - Display or manipulate the history list



| Key    | Action                                                       |
| ------ | ------------------------------------------------------------ |
| CTRL-A | Move cursor to the beginning of the line.                    |
| CTRL-E | Move cursor to the end of the line.                          |
| CTRL-F | Move cursor forward one character; same as the right arrow key. |
| CTRL-B | Move cursor backward one character; same as the left arrow key. |
| ALT-F  | Move cursor forward one word                                 |
| ALT-B  | Move cursor backward one word                                |
| CTRL-L | Clear the screen and move the cursor to the top-left corner. The clear command does the same thing. |



#### Modifying Text

| Key    | Action                                                       |
| ------ | ------------------------------------------------------------ |
| CTRL-D | Delete the character at the cursor location.                 |
| CTRL-T | Transpose(exchange) the character at the cursor location with the one preceding it. |
| ALT-T  | Transpose the word at the cursor location with the one preceding it. |
| ALT-L  | Convert the characters from the cursor location to the end of the word to lowercase. |
| ALT-U  | Convert the characters from the cursor location to the end of the word to uppercase. |



#### Cutting and Pasting (Killing and Yanking) Text

- Killing and Yanking refers to cutting and pasting.
- Items that are cut are stored in a buffer (a temporary storage area in memory) called the kill-ring.



| Key           | Action                                                       |
| ------------- | ------------------------------------------------------------ |
| CTRL-K        | Kill text from the cursor location to the end of line.       |
| CTRL-U        | Kill text from the cursor location to the beginning of the line. |
| ALT-D         | Kill text from the cursor location to the end of the current word. |
| ALT-BACKSPACE | Kill text from the cursor location to the beginning of the current word. If the cursor is at the beginning of a word, kill the previous word. |
| CTRL-Y        | Yank text from the kill-ring and insert it at the cursor location |



#### Completion

- Another way that the shell can help you is through a mechanism called completion.
- It occurs when you press the TAB key while typing a command.

```bash
$ ls l
```

Now press TAB

```bash
$ ls l //Press TAB
lazy_dog.txt   ls-error.txt   ls-output.txt ls.txt  

$ ls D //Press TAB
Desktop/   Documents/ Downloads/ 
```



#### Using History

##### Searching History

```bash
$ history | less
```

- By default, bash stores the last 500 commands we have entered.

Let's say we want to find the commands we used to list /usr/bin.

```bash
$ history | grep /usr/bin
```



```bash
774  ls -l /usr/bin > ls-output.txt

The 774 is the line number of the command in the history list.

To use this immediately, history expansion is used.

$ !774
ls -l /usr/bin > ls-output.txt

```



To start incremental search,

- Press CTRL-R followed by the text you are looking for.
- Either press ENTER to execute the command or press CTRL-J to copy the line from the history
- To quit searching, press either CTRL-G or CTRL-C.

```bash
Press CTRL-R
(reverse-i-search)`': 

// it is reverse because we are searching from "now" to sometime in the past.

**Try searching /usr/bin
(reverse-i-search)`u': ls -l /usr/bin > ls-output.txt


//Press Enter to execute or CTRL-J to copy the command
//Let's Press CTRL-J
ls -l /usr/bin > ls-output.txt

$ ls -l /usr/bin > ls-output.txt

```

