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

