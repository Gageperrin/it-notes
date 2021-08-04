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
* `rmdir` removes directories only if the directory is empty. `rm -rf` is more useful to force remove directories.

### link ####

A hard link is different from a symbolic (soft) link. Every file has one `inode` that provides all administration of a file. Users access an `inode`'s blocks through names. This is a hard link. A symbolic link like `sym1` points to a name rather than directly to a node.

A hard link cannot be used across devices and cannot have directories. Symbolic links must be used instead in these situations.

The command `ln` creates a link in the same way as `cp` takes a source and destination.

Symbolic links should always use absolute pathnames for when they are moved.

Links are useful for creating pointers while maintaining only one copy of a file, improving the navigability of a file system.

#### find ####

`find / -name "hosts"` will find any file in the root directory that includes "hosts". `{}` can be used in a second command after "find" to use the results of the find query.

`-type` and `-size` can be used to filter the find command's searches. `-xargs` is a command in a pipe that will work on the results of a command line by line. It can be used to search for a particular string inside the resulting files from a search.

#### tar ####

`tar` is the tape archiver that is used to compress, extract, or list files to a tape.

Commands:
* `tar -cvf [name] [target]` creates a tar archive. 
* `tar -xvf [name] -C [destination]` extracts to a particular directory.
* `tar -tvf` will show the contents of an archive

Add compression using `-z` or `-j` for gzip and bzip respectively. 

#### File Compression ####

* Gzip is most common and very fast
* bzip2 has a better compression ratio.
* zip can also be used and has a Windows-compatible syntax
* There are other less common utilities outside the scope of this course

Unlike other file compression utilities, zip creates a copy of the file rather than compressing the file itself.



### Lesson 4: Working with Text Files

`Vim` is `vi` + more. `Vim` automatically starts in command mode rather than input mode. To enter input mode, one can use `i`, `o`, `O`, `a`, or the "Insert" key. To return to command mode, use the Esc key.

Most common commands:
* `:wq` writes and quits
* `u` is undo
* `dd` deletes a line
* `:q!` to just exit
* `v` makes a block with arrow keys
* `y` copies
* `p` pastes
* `/` searches with a string match
* `gg` brings the screen to top of document
* `%s/[FROM]/[TO]/` is a search and replace to make it global add `g` to the end

From the terminal run `vi [filename]` to create a file (or modify it if it exists).

The command `vimtutor` opens a Vim tutor with various instructions.

`nano` is another text editor but it is not as widespread. Nano is more intuitive but not installed by default on most Linux systems.

`more` was the original file pager to view files. `less` was developed to offer more advanced features.

`less` commands:
* `/` searches text
* The "N" key goes to the next occurrence
* Lower case "g" goes to the bottom of the file, upper case "G" to the top.
* The "Q" key is used to quit

#### cat and tac ####

`cat` prints all text in a file (`tac` does so in the opposite order).

Options:
* `-A` all non-printable characters
* `-b` numbers lines
* `-n` numbers lines but not empty lines
* `-s` suppresses repeated empty lines


#### grep ####

`grep` can be used in a pipe to filter the previous commands.

Options:
* `-i` case insensitive
* `-R` searches all subdirectories as well
* `-l` just shows the matching file

#### regex ####

Regular expressions are text patterns within strings. Regular expressions are built around atoms. Atoms specify what text is to be matched. It can be a single character, a range of characters, a dot if it is unknown or a class. The second element is the repetition operator which identifies how many times a character should occur. The third element indicates where to find the next character.

Common regex:
* `^` beginning of the line
* `$` end of the line
* `\<` beginning of a word
* `\>` end of a word
* `\A` start of a file
* `\Z` end of a file
* `{n}` exact n times
* `{n,}` minimum of n times
* `{,n}` n times max
* `{n,o}` between n and o times
* `*` zero or more times
* `+` one or more times
* `?` zero or one time

#### Text Processing Utilities ####

* `cut` filters output from a text file
* `sort` sorts files, often used in pipes
* `tr` translates uppercase to lowercase
* `awk` searches for specific patterns
* `sed` powerful stream editor to batch-modify text files

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
