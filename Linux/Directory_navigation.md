# Directory Navigation: `pwd`, `cd`, and `mkdir`

This guide will walk you through the essential commands for navigating and managing directories in Linux. These commands are foundational for working with the file system in the terminal.

---

## 1. `pwd` - Print Working Directory

### Description:
The `pwd` (print working directory) command shows the current directory you're in.

### Syntax:
```bash
pwd
```
### Usage:
- When you run the pwd command, it prints the absolute path of your current directory.

```bash
$ pwd
/home/user/Documents
```

## 2. cd - Change Directory
### Description:
The cd (change directory) command allows you to navigate between directories in the terminal.

### Syntax:
```bash
cd [directory]
```
### Usage:
- To move into a directory:

```bash
cd directory_name
```

```bash
cd Documents
```
- To go up one level (to the parent directory):

```bash
cd ..
```
- To move to the home directory:

```bash
cd ~
```
- To go directly to the root directory:

```bash
cd /
```
- To move back to the previous directory:

```bash
cd -
```
- To move to a specific path:

```bash
cd /home/user/Downloads
```

## 3. `mkdir` - Make Directory
### Description:
The `mkdir` (make directory) command creates a new directory.

### Syntax:
```bash
mkdir [directory_name]
```
### Usage:
To create a directory:

```bash
mkdir new_directory
```
- To create multiple directories at once:

```bash
mkdir dir1 dir2 dir3
```
- To create parent directories if they do not exist (use the -p option):

```bash
mkdir -p parent/child/grandchild
```
- To create a directory with specific permissions (using -m):

```bash
mkdir -m 755 new_directory
```

### **Directory Navigation Cheatsheet**

| Command            | Description                                           | Example                          |
|--------------------|-------------------------------------------------------|----------------------------------|
| `pwd`              | Print the absolute path of the current directory      | `pwd`                           |
| `cd directory`     | Change to a specific directory                        | `cd Documents`                  |
| `cd ..`            | Move up one directory level                           | `cd ..`                         |
| `cd ~`             | Move to the home directory                            | `cd ~`                          |
| `cd /`             | Move to the root directory                            | `cd /`                          |
| `cd -`             | Return to the previous directory                      | `cd -`                          |
| `mkdir directory`  | Create a new directory                                | `mkdir new_folder`              |
| `mkdir dir1 dir2`  | Create multiple directories                           | `mkdir folder1 folder2`         |
| `mkdir -p path`    | Create nested directories (parents if they don't exist)| `mkdir -p parent/child/grandchild` |
| `mkdir -m 755 dir` | Create a directory with specific permissions          | `mkdir -m 755 secure_folder`    |
