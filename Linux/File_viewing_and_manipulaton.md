# File Viewing and Manipulation: cat, echo, less, head, tail, and clear

This guide provides an overview of common Linux commands to display, manipulate, and navigate file contents, as well as manage terminal output.

---

## **1. cat - Concatenate and Display File Contents**

### Description:
The `cat` command is used to display the contents of files, combine multiple files, or create new ones.

### Syntax:
```bash
cat [options] [file...]
```
### Common Options:
| Option    | Description                                      | Example                     |
|-----------|--------------------------------------------------|-----------------------------|
| `-n`      | Number all output lines                         | `cat -n file.txt`           |
| `-b`      | Number non-blank output lines                   | `cat -b file.txt`           |
| `-s`      | Suppress repeated empty lines                   | `cat -s file.txt`           |
| `-A`      | Show all characters, including non-printing ones| `cat -A file.txt`           |
| `-T`      | Display tabs as `^I`                            | `cat -T file.txt`           |
| `-E`      | Display a `$` at the end of each line           | `cat -E file.txt`           |
| `file1 file2 > combined` | Combine multiple files into one         | `cat file1.txt file2.txt > combined.txt` |

### Examples:
- Display the content of a file:
```bash
cat file.txt
```
- Combine multiple files into one:
```bash
cat file1.txt file2.txt > combined.txt
```
- Number lines in a file:
```bash
cat -n file.txt
```
## 2. echo - Print Text or Variables
### Description:
The `echo` command outputs text or variables to the terminal or files.

### Syntax:
```bash
echo [options] [string...]
```
### Common Options:
| Option    | Description                                         | Example                              |
|-----------|-----------------------------------------------------|--------------------------------------|
| `-e`      | Enable interpretation of backslash escapes          | `echo -e "Line1\nLine2"`             |
| `-n`      | Do not output the trailing newline                 | `echo -n "No newline at the end"`    |
| `--help`  | Display help and usage information                  | `echo --help`                        |

### Common Backslash Escapes
| Escape Sequence | Description                                  | Example                             |
|------------------|----------------------------------------------|-------------------------------------|
| `\n`            | New line                                     | `echo -e "Line1\nLine2"`            |
| `\t`            | Horizontal tab                               | `echo -e "Column1\tColumn2"`        |
| `\\`            | Backslash                                    | `echo -e "This is a backslash: \\"` |
| `\"`            | Double quote                                 | `echo -e "She said, \"Hello!\""`    |
| `\a`            | Alert (bell sound)                           | `echo -e "\a"`                      |
| `\b`            | Backspace                                    | `echo -e "Text\b"`                  |

### Examples:
- Print a string:
```bash
echo "Hello, World!"
```
- Print a variable:
```bash
echo $HOME
```
- Write output to a file:
```bash
echo "This is a line" > file.txt
```
## 3. less - View File Contents Interactively
### Description:
The `less` command allows you to scroll through file contents interactively.

### Syntax:
```bash
less [options] [file]
```
### Command-Line Options
| Option         | Description                                       | Example                |
|----------------|---------------------------------------------------|------------------------|
| `-N`           | Show line numbers                                | `less -N file.txt`     |
| `-S`           | Do not wrap long lines (horizontal scrolling)    | `less -S file.txt`     |
| `-X`           | Prevent clearing the screen upon exit            | `less -X file.txt`     |
| `-G`           | Disable search result highlighting               | `less -G file.txt`     |
| `-i`           | Ignore case in search unless uppercase is used   | `less -i file.txt`     |
| `-p pattern`   | Start at the first occurrence of a pattern       | `less -p "search_term" file.txt` |
| `-?` or `--help` | Display help and options                        | `less --help`          |

---

### Keybindings (Interactive Navigation)
| Key            | Action                                           |
|----------------|--------------------------------------------------|
| `Up` / `Down`  | Scroll up/down one line                         |
| `PageUp` / `PageDown` | Scroll up/down one page                     |
| `Space`        | Scroll forward one page                         |
| `b`            | Scroll back one page                            |
| `G`            | Jump to the end of the file                     |
| `g`            | Jump to the beginning of the file               |
| `/pattern`     | Search forward for a pattern                    |
| `?pattern`     | Search backward for a pattern                   |
| `n`            | Repeat the last search (forward)                |
| `N`            | Repeat the last search (backward)               |
| `h`            | Display help screen                             |
| `q`            | Quit `less`                                     |

### Example:
- View a file:
```bash
less file.txt
```
## 4. head - Display the Beginning of a File
### Description:
The `head` command shows the first few lines of a file (default is 10).

### Syntax:
```bash
head [options] [file]
```
### Common Options:
| Option    | Description                                   | Example                     |
|-----------|-----------------------------------------------|-----------------------------|
| `-n N`    | Display the first N lines of the file         | `head -n 5 file.txt`        |
| `-c N`    | Display the first N bytes of the file         | `head -c 20 file.txt`       |
| `-q`      | Suppress file name headers when multiple files are used | `head -q file1.txt file2.txt` |
| `-v`      | Always show file name headers                | `head -v file.txt`          |
| `file`    | Specify the file to read from                | `head file.txt`             |

### Examples:
- Display the first 10 lines:
```bash
head file.txt
```
- Display the first 5 lines:
```bash
head -n 5 file.txt
```
## 5. tail - Display the End of a File
### Description:
The `tail` command shows the last few lines of a file (default is 10). It can also be used to monitor file changes in real time.

### Syntax:
```bash
tail [options] [file]
```
### Common Options:
| Option    | Description                                           | Example                      |
|-----------|-------------------------------------------------------|------------------------------|
| `-n N`    | Display the last N lines of the file                  | `tail -n 10 file.txt`        |
| `-c N`    | Display the last N bytes of the file                  | `tail -c 50 file.txt`        |
| `-f`      | Follow the file as it grows (useful for log files)    | `tail -f logfile.log`        |
| `-q`      | Suppress file name headers when multiple files are used | `tail -q file1.txt file2.txt` |
| `-v`      | Always show file name headers                         | `tail -v file.txt`           |
| `-F`      | Similar to `-f`, but it will attempt to reopen the file if it is rotated | `tail -F logfile.log`         |

### Examples:
- Display the last 10 lines:
```bash
tail file.txt
```
- Display the last 20 lines:
```bash
tail -n 20 file.txt
```
- Monitor a log file in real time:
```bash
tail -f logfile.log
```
## 6. clear - Clear Terminal Screen
### Description:
The `clear` command clears all output from the terminal screen.

### Syntax:
```bash
clear
```
### Example:
- Clear the screen:
```bash
clear
```
---
# Cheatsheet: File Viewing and Manipulation
| Command          | Description                                      | Example                          |
|------------------|--------------------------------------------------|----------------------------------|
| `cat file`       | Display the contents of a file                   | `cat file.txt`                  |
| `cat file1 file2 > combined` | Combine files into one                 | `cat file1.txt file2.txt > out.txt` |
| `echo "text"`    | Print text or variables                          | `echo "Hello, World!"`          |
| `less file`      | View file contents interactively                 | `less file.txt`                 |
| `head file`      | Display the first 10 lines of a file             | `head file.txt`                 |
| `head -n 5 file` | Display the first 5 lines of a file              | `head -n 5 file.txt`            |
| `tail file`      | Display the last 10 lines of a file              | `tail file.txt`                 |
| `tail -n 20 file`| Display the last 20 lines of a file              | `tail -n 20 file.txt`           |
| `tail -f file`   | Monitor a file for changes in real time          | `tail -f logfile.log`           |
| `clear`          | Clear the terminal screen                       | `clear`                         |
