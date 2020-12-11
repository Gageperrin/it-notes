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


### Lesson 8: Configuring Networking

#### 8.1 Understanding IPv4 Networking ####

In IPv4, each node needs its own IP address which is written in dotted decimal notation. Each IP address must be indicated with a subnet mask behind it. The default router or gateway specifies which server to forward packets to that have an external destination. The DNS nameserver is the IP address of a server that helps resolve names to IP addresses and vice versa (the most common is 8.8.8.8).

IPv4 is still the most common even though IPv6 is gaining popularity. RHCSA does not use IPv6. IPv6 addresses are written in hexadecimal notation. IPv4 and IPv6 can co-exist on the same network interface.

The computer must have an IP address and subnet range, a gateway address and a specified DNS in order to network properly.


#### 8.2 Understanding NIC Naming ####

NIC (network interface card). IP address configuration needs to be connected to a specific network device. Use `ip link show` to see current devices and `ip addr show` to check their configuration. Every system has an `lo` device for internal networking. The name of the real network device will also be presented as a BIOS name.

Classic naming uses device names like `eth0` and `eth1` but this shows no information about the device. BIOS naming is based on hardware properties to give more specific information in the device name. `em[1-N]` for embedded NICs, `eno[nn] for embedded NICs`, and `p<slot>p<port>` for NICs on the PCI bus. If the driver does not reveal network device properties, classic naming is used.

#### 8.3 Managing Runtime Configuration with IP ####

The `ip` tool can be used to manage all aspects of IP networking and replaces legacy `ifconfig` tool. Use `ip addr` to manage address properties. Use `ip link` to show link properties. Use `ip route` to manage route properties.

To add an IP address, run `ip addr add dev ens33 [ip-address]`. Adding a secondary IP address is useful for container and cloud hosts.

#### 8.4 Understanding RHEL 8 Networking ####

The file `/etc/sysconfig/networkscripts/ifcfg-ens33` provides configuration to connect `ens33` to the NetworkManager. The `nmcli` and `nmtui`can be used to talk to NetworkManager. The configuration file can also be edited directly but would need to be restarted.


#### 8.5 Managing Persistent Networking with nmcli ####

`nmcli` is NetworkManager command line interface. A connection is a configuration added to a network device and is stored in configuration files. The NetworkManager service must be running to manage these files. Ensure that the bash-completion RPM package is installed when working with `nmcli`.

Run `rpm -qa | grep bash-completion` to check RPM for composing long commands.
Run `nmcli con add con-name my-con-em1 ifname em1 type ethernet \ ip4 192.168.100.100/24 gw4 192.168.100.1 ip4 1.2.3.4 ip6 abbe::cafe` to add a connection.
Run `nmcli con mod my-con-em1 ipv4.dns "8.8.8. 8.8.4.4"` to modify a connection.

#### 8.6 Managing Persistent Networking with `nmtui` ####

`nmtui` is NetworkManager text user interface and is better for simple and fast adjustments. It provides a graphical interface for NetworkManager configuration. Changes will not automatically be activated; they must be activated in the interface.


#### 8.7 Verifying Network Configuration Files ####

The network configuration files are in `/etc/sysconfig/network-scripts/`.


#### 8.8 Testing Network Connections ####

`ping` is used to test connectivity. `ping -c 1` to send a single ping rather than a constant stream. `ip addr show` shows current configuration. `ip route show` shows current routing table. `dig` can test to see if the DNS is working.

If a ping is not working and says "Name or service not known" then it is a DNS error. So ping the DNS `8.8.8.8`. If that is unreachable then it is a connectivity problem. Either the local IP address is wrong or there is a networking problem. Run `ip route show` to check the route table. Can check the contents of the configuration file as well.



## Module 2: Operating Running Systems

### Lesson 9: Managing Processes

#### 9.1 Understanding Jobs and Processes ####

All tasks are started as processes. Processes have a PID. Common process management tasks include scheduling priority and sending signals. Some processes are multi-threaded which means individual threads cannot be managed.

Tasks that are managed from a shell can be managed as jobs. Jobs can be started in the foreground or background.


#### 9.2 Managing Shell Jobs ####

Append `&` to a command to start a job in the background. To move a job to the background, stop it using `ctrl+z` then type `bg` to move it to the background. Use `jobs` for a complete overview of running jobs. Use `fg [n]` to move the last job back to the foreground.


#### 9.3 Getting Process Information with `ps` ####

The `ps` command has two dialects: BSD and System5. System5 commands can take a `-` for options while BSD commands cannot. 

* `ps` shows an overview of current processes while
* `ps aux` offers an overview of all processes.
* `ps -fax` shows hierarchical relations between processes.
* `ps -fU lidna` shows all processes owned by Linda. 
* `ps -f --forest -C sshd` shows a process tree for a specific process.
* `ps L` shows format specifiers.
* `ps -eo pid,ppid,user,cmd` uses some of these specifiers to show a list of processes

#### 9.4 Understanding Memory Usage ####

Linux places as many files as possible in cache to guarantee fast access to the files, and consequently Linux memory often shows as saturated. Swap is used as an overflow buffer of emulated RAM on disk. The Linux kernel moves inactive memory to swap first. Use `free -m` to get details about current memory usage.


#### 9.5 Understanding CPU Load ####

Tasks are sent to a `runqueue` where they are loaded into a `scheduler` which sends the processes to corresponding CPUs. A CPU can only do one task at a time. `uptime` shows load average time. `watch uptime` repeats the command every two seconds. `lscpu` shows CPU information in the system.


#### 9.6 Monitoring System Activity with `top` ####

`top` is a dashboard that allows you to monitor current system activity. Press `f` to show and select from available display fields, `M` to filter on memory usage, and `W` to save new display settings. 


#### 9.7 Sending Signals to Processes ####

A signal allows the operating system to interrupt a process. Interrupts are comparable to signals but generated from hardware. A limited amount of signals can be used, it is documented in `man 7 signals`. Not all signals work in all cases. The `kill` command is used to send signals to PID's. Can also use `k` from top. Different kill-like commands like `pkill` or `killall` can be used.

Signal 15 is a polite kill request. Signal 9 is an immediate kill command. These are the most used signals, but signal 9 should only be used in emergencies.


#### 9.8 Managing Priorities and Niceness ####

By default, Linux processes are started with the same priority. In kernel-land real-time processes can be started which will always have highest priority. To change priorities of non-realtime processes, the `nice` and `renice` commands can be used. Nice values range from `-20` up to `19`. Negative nice value indicates an increased priority, a positive nice value indicates decreased priority. Users can set their processes to a lower priority, to increase priorities you need root access.

To change `dd` processes, use `r` from `top` to reassign nice values. From the CLI, `renice` can alter the priority of processes.


#### 9.9 Using Tuned Profiles

`tuned` is a service that allows for performance optimization in an easy way. Different profiles are provided to match specific server workloads. To use `tuned`, first check that the `tuned` service is enabled and started. `tuned-adm list` will show a list of profiles. `tuned-adm profile <name>` will set a profile. `tunead-adm active` will show the current profile.


### Lesson 10: Managing Software

#### 10.1 Understanding RPM Packages ####

RPM (Redhat Package Manager) is the foundation of software management. The package contains an archive of files compressed with `cpio` as well as metadata and a list of package dependencies. RPM packages may contain scripts as well. Repositories are used to install packages. Individual packages may be installed, but this is not recommended.


#### 10.2 Setting up Repository Access ####

Create a local repository to install packages from the RHEL 8 installation disk ISO image.

1. Create an ISO image with `dd if=/dev/sr0 of=/rhel8.iso bs=1M`. The last option makes the command work with 1 MB of blocks for more efficiency.
2. Create a directory /repo with `mkdir /repo`. 
3. Edit `/etc/fstab` and add this line to the end ` /rhel8.iso   /repo   iso9660   defaults   0 0'.
4. Use `mount -a` to mount the ISO.

To access the local repository:
1. Create the file `/etc/yum.repos.d/appstream.repo` with the following:
```
[appstream]
name=appstream
baseurl=file:///repo/AppStream
gpgcheck=0
```

#### 10.3 Understanding Modules and Application Streams ####

RHEL 8 introduces application streams and modules. Application streams separate user space packages from core kernel opeations. Application streams make it easier to work with different versions of packages. Base packages are provided through the BaseOS repository. AppSTream is provided as a separate repository. Application streams are delivered either as traditional RPMs or new modules.

Modules can contain streams to make multiple versions of applications available. Enabling a module stream gives access to RPM packages in that stream. Modules can have profiles. A profile is a list of packages that bleong to a specific use-case. The package list of a module can contain packages outside the module stream. Use the `yum` module to manage modules.


#### 10.4 Managing Packages with `yum` ####

* Use `yum search` to search for software.
* Use `yum install` to install software.
* Use `yum remove` to remove software. This does not work on protected packages.
* Use `yum update` to update installed packages. Can specify particular packages as well.
* Use `yum provides` a deeper search functionality that goes into files to find matches.
* Use `yum info` to get information about a package.
* Use `yum list` to get information about various packages in a list. Append `installed` to find installed packages.


#### 10.5 Managing Modules and Application Streams ####

* Use `yum module list` to lsit modules.
* Use `yum module provides httpd` to search the module and provide a specific package.
* Use `yum module info` to get information about a package.
* Use `yum module info --profile` to show profiles.
* Use `yum module list` to show which streams are available.
* Use `yum module install @[name]:[version}` to install a specific version.
* Use `yum module install [name]:[version]/devel` to install a specific profile.
* Use `yum module enable [name]:[version]` to enable a module but not install anything.

To upgrade or downgrade packages from a previous module stream that are not listed in profiles that are installed with the module update, use `yum distro-sync`.


#### 10.6 Using `yum` Groups ####

`yum` groups are provided to give access to specific categories of software. 

* `yum groups list` gives a list of the most common yum groups.
* `yum groups list hidden` shows all yum groups.
* `yum groups info <groupname>` shows which packages are in a group.
* `yum groups install <groupname>` will install a specific yum group.


#### 10.7 Managing `yum` updates and `yum` history ####

* `yum history` gives a list of recently used commands.
* `yum history undo` allows an undo of a specific command.
* `yum update` updates all packages on the system.


#### 10.8 Using RPM Queries ####

`rpm` is the legacy command to manage RPM packages. Do not use `rpm` because it does not account for dependencies, but it is useful for package queries against the installation database.

Examples:
* `rpm -qf /any/file` - package name of the file name
* `rpm -ql mypackage` - the files in a package
* `rpm -qc mypackage` - configuration files
* `rpm -qp --scripts mypackage-file.rpm` - to query packages on file that are not yet installed and see which scripts will be installed. Since scripts are installed with root privileges, it is good to review scripts included in the installation.


#### 10.9 Using Red Hat Subscription Manager ####

To work with RHEL repositories, a subscription is needed. A free developer subscription can be used to evaluate repositories. Use `subscription-manager register` to register the subscription. Use `subscription-manager attach --auto` to connect your current subscription.



### Lesson 11: Working with `systemd`

#### 11.1 Understanding `systemd` units ####

`systemd` is the master manager after the Linux kernel starts. Managed items are called units and can be services, mounts, timers, etc. `systemctl` is the management interface to work with `systemd`.


#### 11.2 Managing `systemd` Services ####

System administrators must be able to manage module states. Disabled/enabled determines if a module should be automatically started while booting. Start/stop manages the runtime state of a service.

Use `systemctl status` to examine the state of a module. Services should be enabled on the exam otherwise they will not start on reboot.


#### 11.3 Modifying `systemd` Service Configuration ####

Default `systemd` unit files are in `/usr/lib/systemd/system`, but only customize unit files in `/etc/systemd/system`. Run-time automatically generated unit files are placed in `/run/systemd`. It is best to use `systemctl edit unit.service` to edit unit files. Use `systemctl show` to show available parameters. Use `systemctl daemon-reload` to ensure changes are implemented. Can use `killall` to terminate any processes with the old configuration.


### Lesson 12: Modifying Tasks

#### 12.1 Understanding `cron` and `at` ####

`cron` is a daemon that triggers jobs on a regular basis. It works with different configuration files that specify when a job should be started. It should be used for regular re-occurring jobs.

`at` is used for tasks that need to be started once.

Systemd timers provide a new alternative to `cron`.

#### 12.2 Understanding `cron` scheduling options ####

User-specific `cron` jobs, created using `crontab -e`. Generic time-specific `cron` jobs stored in `/etc/cron.d`. Scripts can be executed on an hourly, daily, weekly, or monthly basis. Generic time-specific cron jobs in `/etc/crontab` (deprecated).

#### 12.3 Understanding `anacron` ####

`anacron` is a service that executes jobs on a regular basis, not a specific time. It manages jobs in `/etc/cron.{hourly}`. Configured in `/etc/anacrontab`.

#### 12.4 Scheduling with `cron` ####

It is recommended to schedule with `crontab -e` as a specific user and create a cron file in `/etc/cron.d`.

Use the time specification: `*/10 4 11 12 1-5`.`*/10` is the minute. 4 is the hour. 11 is the day of the month. 12 is the month. 1-5 are the days of the week. 

`cron` does not have `stdout`. No `echo` allowed. 

`man 5 crontab` is useful for a quick reference of time specifications.

#### 12.5 Scheduling Tasks with Systemd Timers ####

Systemd timers allow for scheduling jobs on a regular basis. `cron` is still the standard.

Useful `man` pages are `man 7 systemd-timer` and `man 7 systemd-time`.

`fstrim.timer` must work with `fstrim.service` with a path, a socket and a timer.

Run `systemctl enable fstrim.timer` to enable the timer. And then start it with `system ctl start fstrim.timer`. It then waits until the service file specifies it should be run.

#### 12.6 Using `at` ####

The `atd` service must be running to run a once-only job **at** a specific time. Use `at <time>` to schedule jobs, `atq` for a list of scheduled jobs, and `atrm` to remove specific jobs from the list.

#### 12.7 Managing Temporary Files ####

The `/usr/lib/tmpfiles.d` directory manages settings for temporary files. The `systemd-tmpfiles-clean.timer` unit can be configured to automatically clean up temporary files.

The `systemd-tmpfiles-clean.timer` automatically cleans files. The `usr/lib/tmpfiles.d/tmp.conf` file contains settings for the automatica temporary file cleanup. When making modifications, copy the file to `/etc/tmpfiles.d`

In `tmp.confg`, lines specify which directory to monitor, which permissions are set on that directory, which owners, and after how many days of not being used the temporary files will be removed.

Consult `man tmpfiles.d` as a reference for navigating these files.


### Lesson 13: Managing Logs ###

#### 13.1 Understanding RHEL 8 Logging Options ####

`rsyslogd` has been the primary solution for a very long. `systemd` and the associated `systemd-journald` is now heart of all logging since RHEL 7. `/dev/log` connects `systemd` to `rsyslogd` and ensures that everything is written to `/var/log` as well by default.

To make the `systemd-journald` persistent, create a `/var/log/journal`. `journalctl` allows for advanced querying.


#### 13.2 Configuring `rsyslog` Logging ####

`rsyslog` needs the `rsyslog` service to run. The main configuration file is `/etc/rsyslog.conf`. Snap-in files can be placed in `/etc/rsyslog.d/`. Each logger line contains three items: facility (the facility the log is created for), severity (the serverity from when it should be logged), and destination (where the log is written to).

Use the `logger` command to write messages to `rsyslog` manually.

`rsyslogd` is backwards compatible with the old `syslog` service. `syslog` had a fixed number of defined facilities. To work with services that do not have own local facility, {0..7} can be used. Because of the shortage of facilities, some services take care of their own logging.


#### 13.3 Working with `systemd-journald`####

`systemd-journald` is the log service inside systemd. It integrates with `systemctl status <unit>` output. Alternatively there is `journalctl` that can read log entries in the journal. Messages are also logged to `rsyslogd` using the `rsyslogd imjournal` module. To make the journal persistent, use `mkdir /var/log/journal`.


#### 13.4 Preserving the Systemd Journal####

The journal is written to `/run/log/journal` which is automatically cleared after a system reboot. Edit `/etc/systemd/journald.conf` to make the journal persistent. Set the storage parameter in this file:
* `persistent` will store the journal in `/var/log/journal`.
* `volatile` stores the journal only in `/run/log/journal`.
* `auto` will store the journal in `/var/log/journal` if that directory exists and in `/run/log/journal` if the former does not exist. This is the default.

There is built in log rotation on a monthly basis. The journal cannot grow beyond 10% fo the size of the fily system. It will also make sure at least 15% of the file system remains available. These settings can be adjusted through `/etc/systemd/journald.conf`.

#### 13.5 Configuring `logrotate` ####

`logrotate` is started through `cron.daily` to ensure log files to not grow too big. Main configuration is in `/etc/logrotate.conf`.


### Lesson 14: Managing Storage

#### 14.1 Understanding Disk Layout ####

Partitioning a disk isolates data. A common disk name is `dev/sda` for the first SCSI disk on a system. In the exam, the system disk will be `/dev/vda`.

A system can either be a BIOS or UEFI. BIOS is kind of outdated. BIOS comes with Master Boot Records. UEFI come with GUID Based Partition Tables. Master Boot Record has only 64 bytes to store partition information. GPT can go up to 128 partitions. MBR invented logical partitions to extend partitions to go beyond the 4 partition cap.

There are multiple partition utilities such as `parted`.

#### 14.2 Understanding Linux Storage Options ####

Partitions are the classic solution for Linux storage. It is used to allocate dedicated storage to specific types of data. LVM Logical volumes are used at default installation of RHEL. It adds flexibility to storage (resize, snapshots and more). A third option is Stratis which is a next generation Volume Managing Filesystem that uses thin provisioning by default. It is implemented in user space which makes API access possible. A fourth option is Virtual Data Optimizer.

#### 14.3 Understanding GPT and MBR Partitions

Master Boot Record is part of the 1981 PC specification. It uses 512 bytes to store boot information, 64 bytes to store partitions, and has space for 4 partitions only with a maximum size of 2 TiB each. Extended and logical partitions must be used to create more partitions.

GUID Partition Table is a newer partition table invented in 2010 with more space to store limitations and overcome MBR limitations. It has a maximum of 128 partitions.

#### 14.4 Creating partitions with `parted` ####

While creating a partition, you do not automatically create a file system. The `parted` file system attribute only writes some unimportant file system metadata. In RHEL 8, `parted` is the default utility. Alternatively, use `fdisk` to work with MBR and `gidsk` to use GUID partitions.

Start with `parted /dev/sdb`. `print` will show if there is a current partition table. `mklabel msdos|gpt` creates a partition table. `mkpart [part-type] [name] [fs-type] [start] [end]` to create a partition. For example `mkpart primary 1024MiB 2048MiB`. Use `print` to verify the created partition and `quit` to exit the parted shell. `udevadm settle` ensures that hte new partition device is created. `cat /proc/partitions` or `lsblk` can also be used to verify the partition.

#### 14.5 Creating MBR Partitions with `fdisk` ####

While `parted` is the default, `fdisk` is still a very useful utility in RHEL 8. 

Inside `fdisk`, run `m` to get a list of commands. Use `n` to create a new partition, `p` to print, `w` to write changes.

#### 14.6 Understanding File System Differences ####

XFS is the default file system in RHEL 8. It is fast, scalable, uses CoW to guarantee data integrity, and size can be increased (but not decreased).

Ext4 was default in RHEL 6 and is still used. Backwards compatible back to Ext2 and uses Journal to guarantee data integrity. Size can be both increased and decreased.

Btrfs is an experimental RHEL 7 solution that has not yet taken off.

#### 14.7 Making and Mounting File Systems ####

`mkfs.xfs` creates an XFS file system. Use `mkfs.[Tab][Tab]` to show a list of available file systems. Do not use `mkfs` by itself unless you intend to create an Ext2 file system. After making the file system, it can be mounted in runtime using the `mount` command. Use `umount` before disconnecting a device.

To get a cleaner output of `mount`, use `grep` to pipe the argument with `'^/'.`

#### 14.8 Mounting Partitions through `/etc/fstab` ####

`/etc/fstab` is the main configuration file to persistently mount partitions. `/etc/fstab` content is used to generate systemd mounts by the `systemd-fstab-generator` utility. To update systemd, make sure to use `systemctl daemon-reload` after editing `/etc/fstab`.

Use `mount -a` to mount file systems.

#### 14.9 Managing Persistent Naming Attributes ####

In datacenter environments, block device names may change. Different solutions exist for persistent naming. UUID is automatically generated for each device that contains a file system or anything similar. While creating the file system, `-L` can be used to set an arbitrary name (label) that can be used for mounting the file system. Unique device names are created in `/dev/disk`. `tune2fs` can be used to set a label too if it is not an xfs file system. For the RHCSA exam, UUID and label are most important to know.

#### 14.10 Managing Systemd Mounts ####

`/etc/fstab` mounts already are systemd mounts. Mounts can be created using systemd `.mount` files. Using `.mount` files allows you to be more specific in defining dependencies. Use `systemctl cat tmp.mount`. to see an example.

Systemd mounts are easier to use than `/etc/fstab` because systemd has greater leeway for managing dependency relations.

#### 14.11 Managing XFS File Systems ####

The `xfsdump` utility can be used for creating backups of XFS formatted devices and considers specific XFS attributes. It only works on a complete XFS device and can make full backups (-l 0) or different levels of incremental backups. `xfsdump -l 0 -f /backupfiles/data.xfsdump /data` creates a full backup of the contents of the /data directory.

The `xfsrestore` command is used to restore a backup made with `xfsdump`. `xfsrepair` can be manually started to repair broken XFS file systems.

#### 14.12 Creating a Swap Partition ####

Swap is RAM that is emulated on disk. It is a good supplement to available RAM. The amount of swap depends on server use. Swap can be created on any block device including swap files. While creating swap with `parted`, set the file system to `linux-swap`. After creating the swap partition, use `mkswap` to create the swap FS. Activate it using `swapon`.

Swap partition mounts are not persistent unless there is a line in `etc/fstab`. (There is an example line in this file showing how to do it.)

### Lesson 15: Managing Advanced Storage

#### 15.1 Understanding LVM, Stratis, and VDO ####

Logical volumes (LVM) add flexibility to storage. It is the default storage solution used during the RHEL installation. Stratis uses thin provisioning by default and is implemented in user space, making API access possible. Stratis is excellent for working with containers. Virtual Data Optimizer (VDO) focuses on efficiently storing files and manages deduplicated and compressed storage pools.

#### 15.2 Understanding LVM Setup ####

The volume group is the abstraction of all storage available on a system. Storage devices can be disks, partitions, etc. A volume group can be extended by adding more physical volumes. LV take disk space out of the volume group and do not have a direct relationship with physical volumes (unless configured otherwise).
