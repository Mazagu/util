# User, Groups, and Permissions Commands
Linux provides various commands to manage users, groups, and file permissions. These commands are essential for managing access to files and resources on the system.

## unamep - Print System Information
`uname` prints system information, including details about the operating system and kernel.

### Basic Usage:
```bash
uname
```
### Common Options:
## uname Options Table
| Option           | Description                                                        | Example                                  |
|------------------|--------------------------------------------------------------------|------------------------------------------|
| `-a`             | Show all information (kernel name, hostname, kernel version, etc.) | `uname -a`                               |
| `-s`             | Show the kernel name                                               | `uname -s`                               |
| `-n`             | Show the hostname of the machine                                   | `uname -n`                               |
| `-r`             | Show the kernel release                                            | `uname -r`                               |
| `-v`             | Show the kernel version                                            | `uname -v`                               |
| `-m`             | Show the machine hardware name                                      | `uname -m`                               |
| `-p`             | Show the processor type                                            | `uname -p`                               |
| `-i`             | Show the hardware platform                                          | `uname -i`                               |
| `-o`             | Show the operating system                                          | `uname -o`                               |

---

## whoami - Print Current User
The `whoami` command prints the username of the currently logged-in user.

### Basic Usage:
```bash
whoami
```
## chmod - Change File Permissions
`chmod` is used to change the permissions of a file or directory. Permissions can be set for the owner (user), group, and others.

### Basic Usage:
```bash
chmod permissions file
```
### Common Permissions:
```
r: Read permission
w: Write permission
x: Execute permission
```

| Option           | Description                                                        | Example                                  |
|------------------|--------------------------------------------------------------------|------------------------------------------|
| `+x`             | Add execute permission to a file                                    | `chmod +x script.sh`                     |
| `-x`             | Remove execute permission from a file                              | `chmod -x script.sh`                     |
| `+r`             | Add read permission to a file                                      | `chmod +r file.txt`                      |
| `-r`             | Remove read permission from a file                                  | `chmod -r file.txt`                      |
| `+w`             | Add write permission to a file                                     | `chmod +w file.txt`                      |
| `-w`             | Remove write permission from a file                                | `chmod -w file.txt`                      |
| `u+x`            | Add execute permission for the user                                | `chmod u+x script.sh`                    |
| `g+w`            | Add write permission for the group                                  | `chmod g+w file.txt`                     |
| `o-r`            | Remove read permission for others                                   | `chmod o-r file.txt`                     |
| `a+r`            | Add read permission for all users (user, group, others)             | `chmod a+r file.txt`                     |
| `777`            | Set full permissions (read, write, execute) for user, group, and others | `chmod 777 file.txt`                |
| `644`            | Set read/write for owner and read-only for group and others         | `chmod 644 file.txt`                     |
| `-v`             | Show verbose output (display changes)                               | `chmod -v +x script.sh`                  |
| `--reference`    | Set the file permissions to match the reference file                | `chmod --reference=ref_file file.txt`    |

---
## chown - Change File Owner and Group
`chown` is used to change the owner and/or group of a file or directory.

### Basic Usage:
```bash
chown owner:group file
```

| Option           | Description                                                        | Example                                  |
|------------------|--------------------------------------------------------------------|------------------------------------------|
| `user:group`     | Change the owner and/or group of a file                            | `chown user:group file.txt`              |
| `user`           | Change the owner of a file                                          | `chown user file.txt`                    |
| `:group`         | Change the group of a file                                          | `chown :group file.txt`                  |
| `-R`             | Change ownership recursively for directories and files             | `chown -R user:group dir/`               |
| `-v`             | Show verbose output (display changes)                               | `chown -v user file.txt`                 |

---

## chgrp - Change Group Ownership
`chgrp` is used to change the group ownership of a file or directory.

### Basic Usage:
```bash
chgrp group file
```

| Option           | Description                                                        | Example                                  |
|------------------|--------------------------------------------------------------------|------------------------------------------|
| `group`          | Change the group of a file                                          | `chgrp group file.txt`                   |
| `-R`             | Change group ownership recursively                                 | `chgrp -R group dir/`                    |
| `-v`             | Show verbose output (display changes)                               | `chgrp -v group file.txt`                |

---
## id - Show User and Group Information
The `id` command displays user and group information for the current user or a specified user.

### Basic Usage:
```bash
id
```

| Option           | Description                                                        | Example                                  |
|------------------|--------------------------------------------------------------------|------------------------------------------|
| (none)           | Show user and group information about the current user             | `id`                                    |
| `-u`             | Show the user ID (UID)                                             | `id -u`                                  |
| `-g`             | Show the group ID (GID)                                            | `id -g`                                  |
| `-G`             | Show all the group IDs the user is a member of                     | `id -G`                                  |
| `-n`             | Show the names of user/group IDs instead of numbers                | `id -n`                                  |

---
## groups - Show Group Membership
The `groups` command shows which groups the current user or a specified user belongs to.

### Basic Usage:
```bash
groups
```

| Option           | Description                                                        | Example                                  |
|------------------|--------------------------------------------------------------------|------------------------------------------|
| (none)           | Show the groups the current user is a member of                    | `groups`                                 |
| `username`       | Show the groups a specific user is a member of                     | `groups username`                        |

---
## umask - Set Default File Creation Permissions
The `umask` command sets the default permissions for files created by the user. It defines the permissions that are not set on new files.

### Basic Usage:
```bash
umask
```

| Option           | Description                                                        | Example                                  |
|------------------|--------------------------------------------------------------------|------------------------------------------|
| (none)           | Show the current umask (default file creation permissions)         | `umask`                                  |
| `mode`           | Set a new umask value (affects default file creation permissions)  | `umask 022`                              |
