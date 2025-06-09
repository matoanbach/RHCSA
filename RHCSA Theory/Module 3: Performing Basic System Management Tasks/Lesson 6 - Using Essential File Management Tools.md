## 6.1 Exploring the Filesystem Hierarchy

- RUN `man hier`to the definition of the filesystem hierarchy 
- `/afs` - Used for AFS (Andrew File System), often empty unless AFS is installed
- `/bin` - Essential user commands and binaries used in single-user or rescue mode.
- `boot` - Contains the Linux kernel and files needed to boot the system
- `/dev` - Holds device files (e.g., /dev/sda for disks, /dev/null) 
- `/home` - Default directory for user personal files (e.g., /home/toan)
- `/lib` - Libraries needed by binaries in /bin and /sbin
- `/lib64` - 64-bit versions of system libraries (used on 64-bit system)
- `/media` - Temporarty mount point for removable media (USB, CD-ROM)
- `/mnt` - Standard mount point for manually mounted filesystems
- `/opt` - Optional software packages (often third-party apps)
- `/proc` - Virtual filesystem with real-time info about system and processes
- `/root` - Home directory for the root (administrator) user.
- `/run` - Runtime data used by system services during boot and operation
- `/sbin` - Essential system administration binaries (used mostly by root)
- `/srv` - Data served by the system (e.g., web or FTP servers)
- `/sys` - Interface to the kernel, representing devices and drivers
- `/usr` - Secondary hierarchy for user software and data (not user files)
- `/var` - Variable files like logs, mail, spool files, and databases

## 6.2 Using Essential File Manangement Commands

- `ls` - list files
- `mkdir` - make a directory
- `cp` - copy files
- `mv`- move files
- `rmdir` - remove an empty directory
- `rm` - remove files

## 6.3 Finding Files
- `ls` to list files
- `which` looks for binaries in $PATH
- `locate` uses a database, built by `updatedb`to find files in a database
- `find` is the most flexible tool that allows you to find files based on many criteria.
    - `find / -type f -size +100M`
    - Find and exec: `mkdir -p find/contents; find /etc -size +1k -exec grep -l student {} \; -exec cp {} find/contents/ \; 2>/dev/null`
    - `find /etc/ -name '*' -type f | xargs grep "127.0.0.1"`

## 6.4 Mounting Filesystems
- To access a device, it must be connected to a directory
- This is known as mounting the device
- The Linux filesystem typically uses multiple mounts
- Different types of data typically are on different devices for multiple reasons
    - Security
    - Manageability
    - Specific mount options
- To mount a device, use `mount /dev/devicename /directory`
    - e.g: `mount /dev/sdb1 /mnt`
- The findmnt command shows all currently mounted devices and their place in the filesystem
- `findmnt` -> what is mounted and where
- `lsblk` -> what storage devices exist (even if not mounted)

## 6.5 Using Links
- Links are pointers to files in a different location
- Compare to shortcuts on other operating systems
- Links can be useful to make the same file available on multiple locations
- Create hard links with `ln` and symbolic links with `ln -s`

## 6.6 Archiving Files
- `tar` is the Tape Archiver and was created a long time ago
- By default, it doesn't compress data
- Basic use is to compress, extract, or list
    - `tar -cvf my_archive.tar /home /etc`
    - `tar -tvf` will show contents of an archive
    - `tar -xvf my_archive` extracts to the current directory
        - Use `-c` to switch the output path
- To add compression, use `-z`, `-j`, `-J` 
    - `-z` -> `.tgz`
    - `-j` -> `.tar.bz2`
    - `-J` -> `.xz`

## 6.7 Working with Compressed Files
- A wide range of compression solution is available for Linux
    - `gzip (-z)` is still the most common compression utility
    - `bzip2 (-j)` is an alternative utility
    - `zip` is also available and has Windows-compatible syntax
    - `xz (-J)` is showing up more often as well