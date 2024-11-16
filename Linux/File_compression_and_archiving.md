# File Compression and Archiving Commands
Linux provides several commands for creating, extracting, and managing compressed and archived files. These commands help you to save space and organize your files efficiently.

## `tar` - Archive Files
`tar` is a utility for creating and extracting archive files. It supports many formats, including .tar, .tar.gz, .tar.bz2, and more.

### Basic Usage:
```bash
tar [options] [archive_file] [file(s)_to_archive]
```
### Common Options:
| Option  | Description                                                                 | Example                                  |
|---------|-----------------------------------------------------------------------------|------------------------------------------|
| -c      | Create a new archive.                                                        | `tar -cvf archive.tar /path/to/folder`   |
| -x      | Extract files from an archive.                                              | `tar -xvf archive.tar`                   |
| -v      | Verbose mode, shows files being processed.                                  | `tar -cvf archive.tar -v /path/to/folder`|
| -f      | Specify the archive file name.                                              | `tar -cf archive.tar /path/to/folder`    |
| -z      | Compress the archive with gzip (for .tar.gz).                               | `tar -czf archive.tar.gz /path/to/folder`|
| -j      | Compress the archive with bzip2 (for .tar.bz2).                             | `tar -cjf archive.tar.bz2 /path/to/folder`|
| -J      | Compress the archive with xz (for .tar.xz).                                 | `tar -cJf archive.tar.xz /path/to/folder`|
| -t      | List the contents of an archive without extracting.                         | `tar -tf archive.tar`                    |
| -r      | Append files to an existing archive.                                        | `tar -rvf archive.tar newfile.txt`       |
| -u      | Update an archive with newer files.                                         | `tar -uvf archive.tar updatedfile.txt`   |
| -C      | Extract or create an archive in a specific directory.                       | `tar -xvf archive.tar -C /target/dir`    |
| -A      | Concatenate multiple archives.                                              | `tar -Af archive1.tar archive2.tar`      |
| --exclude | Exclude files matching the pattern.                                         | `tar -czf archive.tar.gz --exclude="*.log" /path/to/folder`|
---
## `zip` - Create Zip Archives
`zip` is used to compress files and directories into .zip archives.

### Basic Usage:
```bash
zip [options] archive.zip file1.txt file2.txt
```
### Common Options:
| Option  | Description                                                                 | Example                                  |
|---------|-----------------------------------------------------------------------------|------------------------------------------|
| -r      | Recursively zip a directory.                                                | `zip -r archive.zip /path/to/folder`     |
| -e      | Encrypt the archive with a password.                                        | `zip -e archive.zip /path/to/folder`     |
| -9      | Set maximum compression level.                                              | `zip -9 archive.zip /path/to/folder`     |
| -q      | Quiet mode, suppress output.                                                | `zip -q archive.zip /path/to/folder`     |
| -v      | Verbose mode.                                                               | `zip -v archive.zip /path/to/folder`     |
| -d      | Delete files from the archive.                                              | `zip -d archive.zip file.txt`            |
| -x      | Exclude files from the archive.                                             | `zip -r archive.zip /path/to/folder -x "*.log"`|
| -u      | Update files in the archive.                                                | `zip -u archive.zip updatedfile.txt`     |
| -z      | Compress files that are already compressed.                                 | `zip -z archive.zip /path/to/folder`     |
---
## `unzip` - Extract Zip Archives
`unzip` is used to extract files from .zip archives.

### Basic Usage:
```bash
unzip archive.zip
```
### Common Options:
| Option  | Description                                                                 | Example                                  |
|---------|-----------------------------------------------------------------------------|------------------------------------------|
| -l      | List the contents of the archive without extracting.                        | `unzip -l archive.zip`                   |
| -d      | Extract files to a specific directory.                                      | `unzip archive.zip -d /path/to/dir`      |
| -o      | Overwrite existing files without prompting.                                  | `unzip -o archive.zip`                   |
| -n      | Never overwrite existing files.                                             | `unzip -n archive.zip`                   |
| -t      | Test the integrity of the archive.                                          | `unzip -t archive.zip`                   |
| -p      | Extract to standard output (stdout).                                        | `unzip -p archive.zip`                   |
---
## `gzip` - Compress Files Using Gzip
`gzip` is used to compress individual files using the .gz format.

### Basic Usage:
```bash
gzip file.txt
```
### Common Options:
| Option  | Description                                                                 | Example                                  |
|---------|-----------------------------------------------------------------------------|------------------------------------------|
| -d      | Decompress a .gz file.                                                      | `gzip -d file.gz`                        |
| -c      | Output to standard output without modifying the original file.              | `gzip -c file.txt > file.txt.gz`         |
| -r      | Recursively compress files in directories.                                  | `gzip -r /path/to/folder`                |
| -k      | Keep the original file after compression.                                   | `gzip -k file.txt`                       |
| -v      | Verbose mode, show compression ratio.                                       | `gzip -v file.txt`                       |
| -1 to -9 | Set compression level (1 = fastest, 9 = maximum compression).              | `gzip -9 file.txt`                       |
---
## `gunzip` - Decompress Gzip Files
`gunzip` is used to decompress .gz files.

### Basic Usage:
```bash
gunzip file.txt.gz
```
### Common options
| Option  | Description                                                                 | Example                                  |
|---------|-----------------------------------------------------------------------------|------------------------------------------|
| -d      | Decompress a .gz file.                                                      | `gunzip file.gz`                         |
| -c      | Output to standard output without modifying the original file.              | `gunzip -c file.gz > file.txt`           |
| -k      | Keep the original file after decompression.                                 | `gunzip -k file.gz`                      |
| -v      | Verbose mode, show decompression details.                                   | `gunzip -v file.gz`                      |
---
## `bzip2` - Compress Files Using Bzip2
`bzip2` is used to compress files using the .bz2 format. It typically offers better compression than gzip.

### Basic Usage:
```bash
bzip2 file.txt
```
### Common Options:
| Option  | Description                                                                 | Example                                  |
|---------|-----------------------------------------------------------------------------|------------------------------------------|
| -d      | Decompress a .bz2 file.                                                     | `bzip2 -d file.bz2`                      |
| -k      | Keep the original file after compression.                                   | `bzip2 -k file.txt`                      |
| -z      | Compress the file (default action).                                         | `bzip2 file.txt`                         |
| -v      | Verbose mode, show compression ratio.                                       | `bzip2 -v file.txt`                      |
| -1 to -9 | Set compression level (1 = fastest, 9 = maximum compression).              | `bzip2 -9 file.txt`                      |
---
## `bunzip2` - Decompress Bzip2 Files
`bunzip2` is used to decompress .bz2 files.

### Basic Usage:
```bash
bunzip2 file.txt.bz2
```
### Common Options
| Option  | Description                                                                 | Example                                  |
|---------|-----------------------------------------------------------------------------|------------------------------------------|
| -d      | Decompress a .bz2 file.                                                     | `bunzip2 file.bz2`                       |
| -k      | Keep the original file after decompression.                                 | `bunzip2 -k file.bz2`                    |
| -v      | Verbose mode, show decompression details.                                   | `bunzip2 -v file.bz2`                    |
---
## `xz` - Compress Files Using Xz
`xz` is a command-line tool used for compressing files using the .xz format, which provides high compression ratios.

### Basic Usage:
```bash
xz file.txt
```
### Common Options:
| Option  | Description                                                                 | Example                                  |
|---------|-----------------------------------------------------------------------------|------------------------------------------|
| -d      | Decompress a .xz file.                                                      | `xz -d file.xz`                          |
| -k      | Keep the original file after compression.                                   | `xz -k file.txt`                         |
| -z      | Compress the file (default action).                                         | `xz file.txt`                            |
| -v      | Verbose mode, show compression ratio.                                       | `xz -v file.txt`                         |
| -1 to -9 | Set compression level (1 = fastest, 9 = maximum compression).              | `xz -9 file.txt`                         |
---
## `7z` - Compress Files Using 7zip
`7z` is a powerful compression tool used for creating .7z archives. It supports a wide range of compression formats.

### Basic Usage:
```bash
7z a archive.7z file1.txt file2.txt
```
### Common Options:
| Option  | Description                                                                 | Example                                  |
|---------|-----------------------------------------------------------------------------|------------------------------------------|
| a       | Add files to the archive.                                                   | `7z a archive.7z /path/to/folder`        |
| x       | Extract files from an archive.                                              | `7z x archive.7z`                        |
| e       | Extract files to the current directory.                                     | `7z e archive.7z`                        |
| l       | List the contents of an archive.                                            | `7z l archive.7z`                        |
| -p      | Set the password for encrypted archives.                                    | `7z a -pPASSWORD archive.7z file.txt`    |
| -y      | Assume "yes" to all queries (no confirmation).                              | `7z a -y archive.7z /path/to/folder`     |
| -r      | Recursively add files in directories.                                       | `7z a -r archive.7z /path/to/folder`     |
---