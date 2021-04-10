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
