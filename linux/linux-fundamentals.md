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

#### Root User ####

The kernel environment accesses the hardware through drivers. Users can only access the kernel through system calls or permissions. The root user transcends system call and permissions restrictions.

Commands:
* `su` can be used to switch users. This user will be maintained during the lifecycle of the terminal.
* `sudo` is "superuser do" and is used to run commands as the superuser.
* `sudo -i` opens a root shell after the user password is entered.
* `usermod -aG [name] user` adds a user to a group.

`sudo` can be edited through `visudo`, it is usually stored in `/etc/sudoers.d`.

#### Terminals ####

`chvt` changes between virtual terminals (tty's). By default, Red Hat tty1 is graphical while tty2 is not graphical. Most environments do not have a graphical environment but the main advantage of graphical is being able to run multiple text-based terminals at once.

#### SSH ####

SSH is secure shell and replaces options like `telnet` which passes a string password over a network. SSH reaches out to the server to fetch a key that establishes identity and enables encryption. Remote connection identity is established through a ECDSA fingerprint.

`ssh-keygen` generates a private key and public key, the latter is copied over the server through `ssh-copy-id`.

`scp` is inside SSH and is a secure copy protocol.

Commands:
* `ssh [user]@[ip]` is the default syntax to connect to a remote server through SSH.
* `ssh-keygen` to generate a public/private RSA key pair.
* `ssh-copy-id [target-IP]` to copy a public key to a target IP.
* `scp [ip]:[target-path]` to securely copy a file to a destination.


### Lesson 6: Working with the Bash Shell



## Module 2: User and Group Management and Permissions

### Lesson 7: User and Group Management


(7.5 Managing Users and Groups)

`/etc/shadow` is the main file for user management.

Commands:
* `w` shows active users and current activity.
* `who` is an abbreviated version of `w`
* `getent [passwd] [user]` gets information about the user from `passwd`
* `userdel` and `groupdel` can delete users or groups 


*  `usermod` to modify user properties
*  `-L` to lock an account
*  `-G` sets a new list of supplementary groups, overriding the current one
* `-aG` will change a list of supplementary groups without overriding the current one

Defaults for new users:
* `useradd -D` to add a user, specifying default settings
* `/etc/login.defs` is the default configuration file
* `/etc/skell` contents is copied to user home directory upon user creation

Linux does not offer an easy solution to apply new defaults to pre-existing users.

`passwd` has several useful features for a system administrator.
* `-l` locks the account which is safest for removing a user without deleting files
* `echo password | passwd --stdin [user]` securely passes in a password
* `chage` sets time restrictions for password use
* `vipw` and `vigr` can be used to securely edit user or group settings.
* `loginctl session-status [session #]` show activity for the specified session.


### Lesson 8: Permissions Management

Permissions are assigned to `ugo`: user owner, group owner, or others. `chown` and `chgrp` change the ownership of a file.

Permission mode consists of read, write, and execute. For files this is `open`, `modify`, and `run`. For directories this is `list`, `create/delete`, and `cd`. Read is `4`, write is `2`, and execute `1`.

This is configured in ugo format. For example, `chmod 471 [file]` sets 4 for user owner, 7 for group owner, and 1 for others.

Special Permissions:
Suid (4): Runs a file as owner
Sgid (2): Runs file as group owner or inherits directory group owner
Sticky Bit (1): Can only delete a directory if owner

This is prepended to chmod permissions, changing it to a four digit sequence.

The `umask` is a shell setting that defines a mask that subtracts from default permissions. Default permissions for directories are 777 and for files are 666. a `umask 022` will set a file to 644. A `umask 027` will set a directory to 750.



### Lesson 9: Storage Management Essentials

Storage must be presented to Linux through some medium whether a disk in the machine or a Storage Area Network (SAN) through Iscsi or Fibre. Storage is often located in `/dev/sda`. `sda` stands for the scicsi disk.

A disk can partitioned into several partitions to provide greater isolation and security. LVM is the logical volume manager is a partition in a partition.

There are two partioning schemes: MBR and GPT. MBR is legacy and faces severe limitations. CentOS uses MBR and Ubuntu uses GPT.

`lsblk` lists block devices. `fdisk` is the command for configuring a partition table.

`gdisk` is used for GPT partitions on Ubuntu. `fdisk` and `gdisk` essentially have the same options.

`mkfs` creates a filesystem. `vfat` is the best protocol for Windows compatibility. `.xfs` and `.btrfs` are alternative options. NTFS is not supported in Linux.

`mount` mounts a device to a target directory. `findmnt` finds mounted devices, and `umount` unmounts a device. `lsof` locates active files on a mounted device.

## Module 3: Operating Running Systems

### Lesson 10: Managing Networking

IPv4 basics (skipped)

#### IPv6 basics ####
* Addresses are 128 bits and written in hexidecimal
* Leading zeroes can be omitted, as well as long strings of zeros
* When referring to address and port, the address should be between square brackets
* Default subnet mask is /64
* Node bit of the address may contain the device MAC address

Reserved addresses:
* `::1/128` is localhost
* `::` is unspecified address
* `::/0` is the default route
* `2000::/3` is the global unicast address
* `fd00::/8` is a unique local address
* `fe80::/64` link-local address
* `ff00::/8` multicast

`fe80::` address is used by default. IPv6 uses DHCPv6, multicast is sent out to `[ff02::1:2]:547 UDP` This is the all-DHCP multicast group. The DHCP server sends a packet back to client 546/UDP. It also uses Statelesss Address Auto Configuration (SLAAC). Router solicitation is sent to `ff02::2`, the all-routers multicast group. The router sends back an IP address, which allows the host to learn the network prefix. Install the `radvd` package to use this.


Run `ip a` to get current network configuration. `ip route show` will show the routing configuration. DNS configuration is included in `/etc/resolv.conf`. `ip addr` will show the current IP addresses. `dh` can be used to access the DHCP client. IP addresses can be manually added with `ip addr add`. Do not use `ifconfig` as it is legacy and does not include useful information.

Network devices initially consisted of Ethernet (`eth0`, `eth1`, etc.). Biosdevnames uses device names to reveal information about physical location and `systemd-udevd` generates the network device names. `em123` is the Ethernet Motherboard Portnumber, `p<port>p<slot>` (PCI, PCI port), `eno123` is EtherNet Onboard, and if driver does not reveal sufficient information then `etho0` is used.

`hostname` command returns the FQDN. `hostname -I` shows all IPs currently assigned to this host. The hostname is stored in `/etc/hostname` for RH distributions. `hostnamectl` allows hostname configuration.

`etc/hosts` allows DNS resolution if there is no Internet access. A server can be made inaccessible from the Internet by creating a fake host.

`ping` can be used to ping a server while `ping -f` can be used to test flood a server. `netstat -tulpen` gives a list of everything listening on the machine. `ss -tuna` is the more modern version of this command. `nmap` is a rich network scanner but can be abused. `dig` includes other helpful information as well for DNS troubleshooting.

### Lesson 11: Managing Time

System time is set from hardware time at boot, but it is often inaccurate so NTP (Network Time Protocol) synchronizes time with the Internet.

Commands like `date` and `timedatectl` manage system time. `hwtime` manages hardware time and can synchronize hardware and system time. `hwclock` can be used as well but has been replaced by `timedatectl`.

Every server involved with NTP are assigned a stratum. Servers at stratum 1 are directly connected to atomic time. Servers synchronizing with stratum 1 are ranked stratum 2, and so on. Stratum 10 is a local NTP server and is therefore not taking part in Internet synchronization. Stratum 16 is an error and should not be used.

An "insane clock" is too far away from current time and NTP will not use it for synchronization. These don't apply often anymore because iburst protocol is used to ignore an insane clock.

### Lesson 12: Working with Systemd

`Systemd` manages everything. It is the first thing started after booting the Linux kernel. It starts processes and can do that in parallel. It manages mounts, timers, paths, and much more. It is event driven. Items managed by `systemd` are called units. Default units are in `/usr/lib/systemd/system`.

Commands:
* `systemctl -t help` shows unit types
* `systemctl list-unit-files` lists all isntalled units
* `systemctl list-units` lists active units
* `systemctl enable name.service` enables but does not start a service
* `systemctl start name.service` starts a service
* `systemctl disable name.service` disables but does not stop a service
* `systemctl stop name.service`  stops a service
* `systemctl status name.service` give information about a service
* `systemctl cat name.service` reads the current unit configuration
* `systemctl show` shows all available configuration parameters
* `systemctl edit name.service` allows you to edit service configuration
* `systemctl daemon-reload` after modifying service configuration to reload the config
* `systemctl restart name.service` to restart the service with applied changes.

A target is a group of services. Some targets are isolatable to use for a state that the system is in `emergency.target`, `rescue.target`, `multi-user.target`, and `graphical.target`. 

Commands for managing targets:
* `systemctl list-depedendencies name.target` can be used to show what is included in a target.
* `systemctl isolate name.target`
* `systemctl start name.target`
* `systemctl get-default`
* `systemctl set-default name.target`

### Lesson 13: Process Management

### Lesson 14: Managing Software

### Lesson 15: Scheduling Tasks

### Lesson 16: Reading Log Files
