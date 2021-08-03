# Linux Fundamentals

These are my notes from Sander Van Vugt's video course Linux Fundamentals.

## Module 1: Essential Commands

### Lesson 1: Installing Linux

### Lesson 2: Using Essential Tools

It is possible to log into Linux through a graphical interface, a console, or remotely through SSH.

Essential CLI tools:
* `whoami` - show current user
* `hostname` - show current name of host
* `date` - show date and time
* `uname` - print system information
* `passwd` - manage passwords
* `touch` - create a new password
* `last` - last logged in user
*  `| less` to show output across pages
*  `sudo -i` to open root shell on Ubuntu `su -` on CentOS.

`man` can be used to show command information and documentation. Man pages have sections with page 1 for end-user commands, 8 for root commands, and 5 for configuration files. Examples can be found near the end. `/sometext` can be used to search 'sometext'. Use `q` to get out of the man page. `man -k` can be used to find keywords within man pages. `man -k user | grep 8 | grep create` will return man pages with the word user and '8' in the entry and the word 'create'. `man db` can be used to update the man database.

`pinfo coreutils [command] invocation` provides documentation on some commands in Linux. It was a historical alternative to command documentation and includes more features than man pages but has less documentation. `/usr/share/log` can also be used to find command information.


### Lesson 3: Essential File Management Tools

Every Linux file is located within `/`. Within the root directory by default are `bin`, `home`, `var`, `usr`, etc. A mount is a storage device connected to a separate directory. Putting a directory on a separate disk can ensure that dedicated storage space is available. This separate disk is mounted onto the file directory.

Default root directory subdirectories:
* `bin` is a symbolic pointer to `/usr/bin`. `bin` contains regular binaries, `sbin` contains system binaries.
* `boot` is for booting and is usually on a separate partition to be available at all times.
* `dev` stands for devices and containers drivers connecting Linux to hardware. `sda` is the hard disk and partitions are labeled `sda1`, `sda2`, etc.
* `etc` is for configuration files.
* `proc` is an interface to the Linux kernel and can provide extensive information about the Linux system.
* `root `is the home directory for the root user.
* `run` is the new temp directory for processes.
* `sys` is for hardware information.
* `tmp` is the old temp directory.
* `media`, `mnt`, and `opt`, and `srv` are rarely used.
* `usr` is like Program Files in Windows.
* `var` contains a wide variety of files created by the OS. `var/log` is one of the most commonly used subdirectories for log files.
* 

#### ls ####

Important options for `ls`:
* `-l` gives a long list of files and directories with creation date/time, link counter, and permission information.
* `-a` shows all files including hidden files prefixed with `*`.
* `-lrt` sorts files by last modification date.
* `-ld [directory]` shows directory properties rather than contents

#### Globbing ####

* `*` globs for everything. 
* `?` globs for a single character.
* `[a-c]` specifies a range of a, b, and c.
* `[a-c]*` will return all files starting with a, b, or c.
* `?[z-s]*` shows all files where the second character is a z or s.

#### cp ####

Copy can copy both files and directories from a source to a destination.

`cp -R` recursively copies all files in a directory instead of just a single file.

#### cd, mkdir ####

`cd` is used to navigate between directories. A `/` is used to distinguish between absolute and relative directories.
* `cd -` returns to previous directories.
* `mkdir` creates a directory.
* `rmdir` removes directories only if the directory is empty. `rm -rf` is more useful.





### Lesson 4: Working with Text Files

### Lesson 5: Connecting to a Server

### Lesson 6: Working with the Bash Shell

## Module 2: User and Group Management and Permissions

### Lesson 7: User and Group Management

### Lesson 8: Permissions Management

### Lesson 9: Storage Management Essentials

## Module 3: Operating Running Systems

### Lesson 10: Managing Networking

### Lesson 11: Managing Time

### Lesson 12: Working with Systemd

### Lesson 13: Process Management

### Lesson 14: Managing Software

### Lesson 15: Scheduling Tasks

### Lesson 16: Reading Log Files
