# Red Hat Certified System Administrator Certification Notes

##  Module 1: Performing Basic System Management Tasks

### Lesson 1: Installing Red Hat Enterprise Linux Server

### Lesson 2: Using Essential Tools

### Lesson 3: Essential File Management Tools

### Lesson 4: Working with Text Files

#### 4.1 Common Text Tools ####

`more` was the original file pager. `less` was developed to offer more advanced features. Even though `more` was developed to compensate for this (it includes a percentage to show how far through the file the user is), `less` is the better file pager because it allows the user to scroll upward.

Use `head` to show the first 10 lines of a text file. Use `tail` for the last 10 lines. Use `-n` to show a custom number of lines. For example, `head -n 5` shows the first 5 lines.

`cat` dumps text file contents on the screen. Important flags include `-A` that shows all non-printable characters. `-b` numbers the lines. `-s` suppresses repeated empty lines. `tac` is the same as `cat` but displays it in reverse order.

* `cut` filters output.
* `sort` sorts output.
* `tr` translates output.

Examples: 
* `cut -f 3 -d : /etc/passwd | sort -n | less`. This filters the 3rd field with a specified delimiter and shows it in `less` sorted numerically.
* `cut -f 1 -d : /etc/passwd | sort | tr [:lower:] [:upper:]` takes the first field (usernames) and sorts them then turns it all from lowercase to uppercase. 


#### 4.2 Using `grep` ####

`grep` is excellent for finding text in files or output. `ps aux | grep ssh` will find a list of files with lines containing 'ssh'.

`grep -i ssh *` looks in the current directory for the text 'ssh'. The `-i` makes it case-insensitive. `grep -A5` and `grep -B5` prints the fives lines before and after the matching text respectively. `grep -R root /etc` searches recursively.


#### 4.3 Understanding Regular Expressions ####

Regular expressions are text patterns used by `grep`, `vim`, `awk`, and `sed`. It is not globbing.

`grep 'a*' a*` - single quotes are helpful to prevent process confusion. The `-e` flag (or `egrep`) is needed for extended regex.

See `man 7 regex` for documentation on regex.

Regular expressions are built around atoms--what text is going to be matched. The second element is the repetition operator, specifying how often a character occurs. Third is indicating where to find the next character.

Examples: 
* `grep 'b.*t' regtext` can find "bit", "boot", "boat".
* `egrep 'b.?t' regtext` can find 'bt', 'bit', and 'bite'.`

Common regex symbols:
* `^` - beginning of line
* `$` - end of line
* `\<` - beginning of word
* `\>` - end of word
* `*` - zero or more occurrences
* `+` - one or more occurrences
* `?` - zero or one time
* `{n}` - exactly n times


#### 4.4 Using `awk` ####

`awk` is a powerful text processing utility that specializes in data extraction and reporting. It can perform actions based on selectors.

Examples: 
* `awk -F : '/ssh/ { print $4 }' /etc/passwd` - look for the text 'ssh' and print 4 on matching lines.
* `awk -F : ' {print $NF }' /etc/passwd` - prints the last field in the line.
* `ls -l /etc | awk '/pass/ { print }' | less` - pipes the outline of ls -l to less.


#### 4.5 Using `sed` ####

`sed` is the *s*tream *ed*itor that provides text modifications. It is the older version of `vi`. It is most useful for substitution and deleting lines without opening the file.

Examples:
* `sed -n 4p file` prints the fourth line of the file.
* `sed -i s/four/FOUR/g file` searches and replaces 'four' with 'FOUR' in the file.
* `sed -i -e '2d' file` edits and deletes the second line.

### Lesson 5: Connecting to a RHEL Server

#### 5.1 Understanding the Root User ####

The kernel space is connected to the hardware through drivers. The user space is connected to users and processes. Permissions and `syscall` separate the user space from the kernel space. The root user on Linux has no restrictions (unlike the admin user on Windows) and can make changes to the drivers.


#### 5.2 Logging into the GUI ####

The GUI displays ordinary user accounts and allow the user to click, enter their password, and login.


#### 5.3 Logging into the Console ####

Type user and password to log into the CLI.


#### 5.4 Understanding Virtual Terminals ####

Thirty years ago, terminals were connected to big servers that did all the computing centrally. Many different termianls can be connected. The server was called `dev/tty` with terminals being `dev/tty1` etc. with a default number of 6 terminals.

This idea persists in current Linux systems as virtual terminals.


#### 5.5 Switching between Virtual Terminals ####

`tty1-tty6` are available for login. If installed and active, the GUI is on `tty1`. Use `chvt` to switch between virtual terminals or `Ctrl+Alt+Fn` on a GUI. Only the root user can do this in a graphical environment.

#### 5.6 Using `su` to Work as Another User ####

`su` is used to open shell as another user, most useful for opening a root shell. The password of the target user is required. Use `su -` to open a login shell that will give complete access to the environment of the target user. `su` only gives partial access. This is not the most secure option for a team.

#### 5.7 Using `sudo` to Perform Administrator Tasks ####

`sudo` is used to run tasks as another user, prompting for the password of the *current* user, authorizing if that user has been give the right access. Authorization through `/etc/sudoers` and `/etc/sudoers.d/`. Do not edit directly but use `visudo`.

Example lines in `sudo` configuration file:
* `%wheel ALL=(ALL)    ALL` allows people in all machines to run all commands.
* `%users localhost=/sbin/shutdown -h now` allows users to shut down the machine.
* `linda   ALL=/usr/sbin/useradd, /usr/bin/passwd` allows the user `linda` to add users and change passwords.

If a user is regrouped, they must be logged out and logged back in because groups are read upon logging in.


#### 5.8 Using `ssh` to Login Remotely ####

SSH is secure shell to establish a secured remote connection. Identity of target server is verified through host keys. After the initial conneciton, the host key is stored in `~/.ssh/known_hosts`. Sensitive data will be sent through an encrypted connection.

Use `ssh -X` or `ssh -Y` to display graphical screens from the target server locally, if supported. Use `exit` to close the SSH session.

Examples:
* `ssh localhost` connects to the localhost.
* `ssh -Y linda@localhost` allows user to login as Linda on the localhost with a GUI.


### Lesson 6: Managing Users and Groups

#### 6.1 Understanding the Need for User Accounts ####

A user is a security principle. User accounts are used to provide individuals or processes with access to system resources. Processes use system accounts (low user ID). People use regular user accounts (high user ID).

#### 6.2 Understanding User Properties ####

Syntax for user properties:
`student:x:1000:1000:student:/home/student:bin/bash`
name:password:UID (user):GID (group):GECOS (additional info):home directory:default shell

#### 6.3 Creating and Managing Users ####

`useradd` is used to create user accounts. Option `-c` adds a comment.
`usermod` modifies user accounts. `-G` creates a new list of supplementary groups. `-aG` appends to a list of groups.
`userdel` deletes user accounts. `-f` forces the deletion. `-r` removes home directory and mail spool as well. Not advised to use `-r` unless absolutely necessary.
`passwd` sets passwords. `-l` locks the password. `-u` unlocks the password. `-e` expires the password.

#### 6.4 Managing User Default Settings ####

`useradd -D` specifies default settings from the command line. `-D` prints some of the defaults. Files in `/etc/default/useradd` apply to `useradd` only.

* `GROUP=100` is overwritten and is no longer relevant. 
* Can alternatively write default settings to `/etc/login.defs`.
* Files in /etc/skel are templates automatically placed in a new user's home directory when the user is created.

#### 6.5 Understanding `/etc/passwd` and `/etc/shadow` ####

`/etc/passwd` stores user properties.`/etc/shadow` stores password properties. It is bad practice to edit `/etc/shadow` through a file editor. `/etc/group` stores group properties.

#### 6.6 Understanding Group Membership ####

Each user must be a member of at least one group. Primary group membership is managed through `/etc/passwd`. The user primary group becomes group-owner if a user creates a file. Secondary groups can be defined as well, and these are managed through `/etc/groups`. Use `id` to see which groups a user is a member of.


#### 6.7 Creating and Managing Groups ####

`groupadd` adds a group. `groupdel` and `groupmod` can be used to delete and modify groups respectively. Their options are not particularly useful in most cases.

`lid -g [groupname]` lists all users that are a member of a specific group.


#### 6.8 Managing Password Properties ####

Basic password requirements are set in `/etc/login.defs`. Pluggable Authentication Modules (PAM) can be used for advanced password properties (cf. pam_tally2 module). To change password settings for current users use, `chage` or `passwd` as root user.


[Lab]

### Lesson 7: Managing Permissions

#### 7.1 Understanding Ownership ####

Every file has a user owner, a group owner, and the 'others' entity that is also granted permissions `ugo`. Linux permissions are not additive, they are applied by sequentially checking levels of ownership.

`ls -l` displays current ownership and associated permissions.

`drwx------` - `d` indicates directory. `rwx` indicates read-write-execute permission for the user-owner. The second set of dashes is for group permission. The third is for others. The second and third groups here have no read-write-execute permissions.


#### 7.2 Changing File Ownership ####

Use `chown user[:group] file` to set user-ownership. Group name is not required. Use `chgrp group file` to set group ownership.


#### 7.3 Understanding Basic Permissions ####

Three kinds of permissions: Read (4), Write (2), Execute (1). Read can read files and list the directory. Write can modify files and delete or create directories. Execute can run files and `cd` into the directory. Read and execute are needed together.


#### 7.4 Managing Basic Permissions ####

Use `chmod` to manage permissions. It can be used in absolute or relative mode.

Example of absolute mode: `chmod 750 myfile`
7 = Read (4) + Write (2) + Execute (1) for user.
5 = Read (4) + Execute (1) for group.
0 = Nothing for others.

Examples of relative mode: 
`chmod +x myscript` makes the the script executable for all users (implied `ugo`).
`chmod u+x myscript` will grant the owner execution permissons.


#### 7.5 Understanding `umask` ####

The `umask` is a shell setting that subtracts the `umask` from the default permissions. Default permissions for a file are 666. Default permissions for a directory are 777. `umask` is applied to a shell, not a file. A `umask` setting of 022 will make all new files 644 and directories 755. It removes write permissons from the group and others.


#### 7.6 Understanding Special Permissions ####

`suid` (4) - set user id - means one can run a file as owner. `sgid` (2) - set group id - means one can run a file as group owner. If set on a directory, any files in the directory will inherit the group owner. Sticky bit (1) means a directory can be deleted only if user is the owner.


#### 7.7 Managing Special Permissions ####

`suid` can be written as `chmod 4770 myfile` or `chmod u+s myfile`. It is dangerous and should not be used in most circumstances because it creates a  subshell that makes the user root and can jeopardize the entire system. Files like `/usr/bin/passwd` use `suid` because it needs permissions to access `/etc/shadow`.

`sgid` can be written as `chmod 2770 mydir` or `chmod g+s mydir`.

Sticky bit is applied through `chmod 1770 mydir` or `chmod +t mydir`.

#### 7.8 Understanding ACLs ####

Access Control Lists are used to grant permissions to additional users and groups. The normal ACL applies to existing files only.Use a default ACL on a directory if you want it to apply to new files also. `getfacl` shows current settings.

Example: `setfacl -R -m g:somegroup:rx /data/groups`. This sets an ACL recurisvely on some group where `-m` modifies and `rx` are the permissions. To ensure new files are categorized in this ACL, run `setfacl -m d:g:somegroup:rx /data/groups`. A `+` at the end of a file's permissions means an ACL is active. The default ACL set on a directory is inherited by its files.


#### 7.9 Managing ACLs ####

Video demonstration of 7.8's principles. 


#### 7.10 Troubleshooting Permissions ####

Demonstration of identifying permissions.
