# Quick Guide to Changing Permissions in Linux
This guide explains how to change file and folder permissions in Linux using the chmod command, including recursive operations. This guide also includes a permission cheat sheet.

---
## Changing Permissions for Files and Folders
1. Basic Syntax for chmod:

```bash
chmod [permissions] filename
```
Replace `[permissions]` with the desired permission values and `filename` with the actual file or folder name.

2. Using chmod Recursively: To change permissions for a folder and all its subdirectories and files, use the `-R` option:

```bash
chmod -R [permissions] foldername
```
3. Running `chmod` as Root: If a file or folder is owned by `root` or another user, you may need sudo to change its permissions:

```bash
sudo chmod [permissions] filename
```
4. Numeric vs. Symbolic Permissions:

- Numeric: Permissions are specified with a three-digit number.
- Symbolic: Permissions are set with symbols like `u`, `g`, `o`, `a`, combined with `+`, `-`, or `=` to add, remove, or set permissions.

--- 
## Cheat Sheet: Permission Codes

| Permission | Numeric | Symbolic | Meaning              |
|------------|---------|----------|----------------------|
| `r`        | 4       | `r`      | Read                |
| `w`        | 2       | `w`      | Write               |
| `x`        | 1       | `x`      | Execute             |
| `-`        | 0       | `-`      | No permission       |

### Common Permission Values

| Numeric Code | Symbol      | Permissions for `user`, `group`, `others`                |
|--------------|-------------|----------------------------------------------------------|
| `777`        | `rwxrwxrwx` | Read, write, and execute for all                         |
| `755`        | `rwxr-xr-x` | Full for user, read/execute for group and others         |
| `644`        | `rw-r--r--` | Read/write for user, read for group and others           |
| `700`        | `rwx------` | Full for user, none for group and others                 |

---
## Examples
- Change File Permissions to `644`:

```bash
chmod 644 myfile.txt
```
This gives the owner read and write permissions, while others have read-only access.

- Change Folder Permissions Recursively to `755`:

```bash
chmod -R 755 myfolder/
```
This sets full permissions for the owner, and read/execute for everyone else on all files and subfolders.

- Set Permissions Symbolically:

```bash
chmod u+rwx,go+rx myfile.txt
```
This grants the user full permissions, and read/execute permissions to group and others.
