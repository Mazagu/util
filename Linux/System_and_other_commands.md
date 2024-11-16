### System Information & Management

#### man
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `man <command>` | Display the manual page for a command.    | `man ls`                           |

#### ps
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `-e`    | Show all processes.                      | `ps -e`                            |
| `-f`    | Show full-format listing.                | `ps -ef`                           |
| `-u <user>` | Show processes for a specific user.     | `ps -u username`                   |

#### top
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `-d <seconds>` | Delay between updates in seconds.     | `top -d 5`                         |
| `-n <count>` | Number of iterations to display.       | `top -n 1`                         |

#### df
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `-h`    | Human-readable output (e.g., MB, GB).    | `df -h`                            |
| `-T`    | Show file system type.                   | `df -T`                            |

#### mount
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `<device> <mount_point>` | Mount a device to a directory.       | `mount /dev/sda1 /mnt`             |
| `-t <type>` | Specify file system type.               | `mount -t ext4 /dev/sda1 /mnt`     |

#### ifconfig
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `-a`    | Show all interfaces.                     | `ifconfig -a`                      |
| `eth0`  | Display specific interface details.      | `ifconfig eth0`                    |

#### traceroute
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `-n`    | Show IP addresses instead of domain names. | `traceroute -n google.com`         |
| `-m <max>` | Limit the number of hops.               | `traceroute -m 10 google.com`      |

#### service
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `start` | Start a service.                         | `service apache2 start`            |
| `stop`  | Stop a service.                          | `service apache2 stop`             |
| `status`| Check the status of a service.          | `service apache2 status`           |

#### ssh
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `<user>@<host>` | SSH into a remote machine.             | `ssh username@host`                |
| `-p <port>` | Specify a port number.                 | `ssh -p 2222 username@host`        |

#### ufw
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `enable` | Enable the firewall.                    | `ufw enable`                       |
| `disable`| Disable the firewall.                   | `ufw disable`                      |
| `allow <port>` | Allow traffic on a specific port.   | `ufw allow 80`                     |

---

### File Management & Text Processing

#### sort
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `-r`    | Sort in reverse order.                   | `sort -r file.txt`                 |
| `-n`    | Sort numerically.                        | `sort -n file.txt`                 |

#### export
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `VAR=value` | Set an environment variable.          | `export PATH=/usr/local/bin:$PATH` |
| `-p`    | Print all exported variables.            | `export -p`                        |

#### alias
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `<name>`=`<command>` | Create an alias for a command. | `alias ll='ls -l'`                 |
| `-p`    | Display all defined aliases.             | `alias -p`                         |

#### whereis
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `<command>` | Locate binary, source, and man pages.   | `whereis ls`                       |

#### whatis
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `<command>` | Show a brief description of a command.  | `whatis ls`                        |

---

### Process & System Control

#### kill
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `<PID>`| Kill a process by its PID.               | `kill 1234`                        |
| `-9`    | Force kill a process (SIGKILL).          | `kill -9 1234`                     |

#### killall
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `<name>`| Kill all processes by name.              | `killall firefox`                  |
| `-9`    | Force kill all processes (SIGKILL).      | `killall -9 firefox`               |

---

### Network & Downloading

#### wget
| Option  | Description                              | Example                             |
|---------|------------------------------------------|-------------------------------------|
| `-O`    | Specify output file name.                | `wget -O file.html http://example.com` |
| `-r`    | Download recursively.                    | `wget -r http://example.com`        |
| `-c`    | Resume a previous download.              | `wget -c http://example.com/file`   |

---
