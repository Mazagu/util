# 🗃️ Linux File System: Server-Focused Overview

This guide explores the **Linux filesystem hierarchy** with a focus on what’s **relevant for server environments** — particularly in distributions like **Debian**, **Ubuntu**, and **Alpine Linux**.

Whether you’re managing containers, configuring services, or building CI/CD pipelines, this knowledge is essential.

---

## 🗂️ Filesystem Hierarchy Overview (FHS)

Linux organizes files into a **tree-like structure** rooted at `/`. Most server-relevant data lives in well-defined subdirectories.

### 📂 Top-Level Directories

| Path         | Purpose                                       |
|--------------|-----------------------------------------------|
| `/`          | Root of the entire filesystem                 |
| `/bin`       | Essential binaries for all users              |
| `/sbin`      | Essential system binaries for root/admins     |
| `/etc`       | System configuration files                    |
| `/var`       | Variable data (logs, caches, spool)           |
| `/usr`       | Userland apps and libraries                   |
| `/home`      | User home directories (less relevant for servers) |
| `/root`      | Home directory for `root` user                |
| `/tmp`       | Temporary files, auto-cleared on reboot       |
| `/opt`       | Optional software packages                    |
| `/lib`, `/lib64` | Shared libraries used by binaries        |
| `/dev`       | Device files (disks, terminals, etc.)         |
| `/proc`, `/sys` | Virtual files for system/kernel info       |
| `/run`       | Volatile runtime files (e.g., PID files)      |
| `/mnt`, `/media` | Mount points (manual or removable devices) |

---

## 🧰 Server-Critical Directories In Depth

### 🔧 `/etc` — Configuration

- Contains system-wide config files (mostly plain text)
- Common contents:
  - `/etc/hosts`: Static hostname resolution
  - `/etc/passwd`, `/etc/group`: User accounts
  - `/etc/fstab`: Mount configuration
  - `/etc/network/interfaces` or `/etc/netplan/`: Network setup (Debian/Ubuntu)
  - `/etc/hostname`, `/etc/hosts`: Host identity
  - `/etc/systemd/`: Service definitions
  - `/etc/nginx/`, `/etc/apache2/`: Web server configs

```
# View SSH configuration
cat /etc/ssh/sshd_config
```

---

### ⚙️ `/var` — Variable Runtime Data

- Stores logs, spools, and caches
- Server-relevant subdirs:
  - `/var/log/`: System logs (syslog, auth.log, nginx, etc.)
  - `/var/lib/`: Persistent state (e.g. MySQL, apt, Docker)
  - `/var/run/` → symlink to `/run`: PID and runtime state
  - `/var/spool/`: Queued jobs (mail, print)

```
# Tail logs
tail -f /var/log/syslog
```

---

### 📦 `/usr` — Installed Software and Shared Resources

- Subdirs:
  - `/usr/bin/`, `/usr/sbin/`: Most installed binaries
  - `/usr/lib/`: Libraries
  - `/usr/share/`: Architecture-independent data (man pages, locale)
  - `/usr/local/`: Custom-installed software

> ⚠️ On Debian systems, `apt install` places binaries in `/usr/bin` not `/bin`.

---

### 🪪 `/run` — Runtime Volatile Files

- Replaces older `/var/run`
- Lives in memory (`tmpfs`)
- Contains:
  - PID files for services
  - Socket files (e.g. `/run/docker.sock`)
  - Network state

---

### 🧪 `/tmp` — Temporary Files

- Writable by all users
- Auto-cleared on reboot (unless configured otherwise)

Use it for:
- Transient cache
- Intermediate build files

```
mktemp /tmp/example.XXXXXX
```

---

### 🔒 `/root` — Root User Home

- The home directory of the `root` user
- Not to be confused with `/`, the root of the filesystem

---

### 📁 `/opt` — Optional Software

- Third-party software that doesn’t fit system layout
- Useful for deploying tools manually (e.g. custom binaries, dev tools)

---

## 🐧 Alpine-Specific Notes

Alpine follows FHS but is stripped-down for minimalism:

| Debian Path        | Alpine Path       | Notes                              |
|--------------------|-------------------|------------------------------------|
| `/etc/init.d/`     | `/etc/init.d/`    | OpenRC init system used in Alpine  |
| `/lib`, `/lib64`   | Merged into `/lib`| Smaller footprint                  |
| `/var/lib/apk/`    | Alpine's package metadata |

Use Alpine for:
- Containers
- Lightweight distros

---

## 🧹 Tips for Server Environments

- **Don't write to `/usr` or `/bin`** directly — use package managers
- **Use `/etc` for config**, version it via git (`/etc/git`)
- **Log to `/var/log`**, rotate with `logrotate`
- **Store state in `/var/lib`**, ephemeral data in `/tmp` or `/run`

---

## 🛠 Useful Commands

```
# Inspect disk usage by directory
du -sh /*

# See mounts
mount | column -t

# List services
systemctl list-units --type=service

# Find config file for a service
dpkg -L nginx | grep /etc
```

---

## 📚 Further Reading

- [Filesystem Hierarchy Standard (FHS)](https://refspecs.linuxfoundation.org/FHS_3.0/fhs/index.html)
- [Debian Filesystem Overview](https://wiki.debian.org/FilesystemHierarchyStandard)
- [Alpine Linux Wiki](https://wiki.alpinelinux.org/)
- [Linux File System Layout — Red Hat Docs](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html-single/managing_file_systems/index#file-system-hierarchy)

---
