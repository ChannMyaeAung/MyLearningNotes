#### Advanced Keyboard Tricks

- clear - Clear the terminal screen
- history - Display or manipulate the history list



| Key    | Action                                                       |
| ------ | ------------------------------------------------------------ |
| CTRL-A | Move cursor to the beginning of the line.                    |
| CTRL-E | Move cursor to the end of the line.                          |
| CTRL-F | Move cursor forward one character; same as the right arrow key. |
| CTRL-B | Move cursor backward one character; same as the left arrow key. |
| ALT-F  | Move cursor backward one word                                |
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

