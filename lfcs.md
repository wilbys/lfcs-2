# Linux Basics
### Lesson 3: Linux File System Hierarchy
/ is the root of the file system. Within it, you may have directories like /bin, /var, /usr, /home 
You can choose to mount one or more of these directories on separate disks using the mount command.  
`/usr`: All binaries are stored here under /bin (user commands) and /sbin (system commands).  
/boot: For booting. Often, based on your linux configuratoin, the /boot directory is on a separate disk.  
/dev: For devices. This directory provides access to hardware.  
/etc: For configuration files. Almost all of these files are text files and human readable.  
/home: For user home directories.  
/media and /mnt are used for mounts.  
/opt: For applications that has more than a couple of files. Example: A database  
/proc: Is an interface to the linux kernel. cpuinfo and meminfo, for example provide interfaces to the cpu and memory respectively.  
/run: A new temporary directory  
/srv: Is usually empty. It is used to store documents for services like web services or ftp services  
/sys: Hardware information  
/tmp: The old /run  
/var: For files dynamically created by the OS. Example /var/log for log files.  

#### Wildcards
* for everything
? for one single character
[a-c] implies a, b or c.
[a-c]* is all files that start with a, b or c.
[s-z] is the same as [z-s]
?[z-s]* All files where the second character is anything from z to s.


### Lesson 3: Networking
Applying Persistent Networking in Ubuntu
Starting 17.19, Ubuntu uses netplan 
Configuration is in a YAML file in /etc/netplan/50-cloud-init.yaml. 
After applying Configuration, use sudo netplan apply 

Show routing table with
ip route show


### Lesson 6:
/etc/environment contains a list of variables and is the first file that is processed while starting bash (empty by default on Red Hat). It is universal and works for everything -- variables and users.
/etc/profile is executed while users login (for users only).
/etc/profile.d is used as a snapin directory that contains additional configuration.
~/.bash_profile can be used as a user-specific version.
~/.bash_logout is processed when a user logs out
/etc/bashrc is processed every time a subshell is started
A user specific ~/.bashrc file may be used
Aliases by default are available in subshells. Variables are not.

## Module 2
### Lesson 7: User and Group Management
getent passwd ubuntu

#### Defaults for new users
useradd -D can be used from the command line. This is similar to /etc/default/useradd
/etc/login.defs is used as the default configuration file

/etc/skel contents are copied to user home directory upon user creation
Linux does not offer any easy solution to apply new defaults to previously created users.

#### Passwords
/etc/shadow has settings of passwords
You can lock passwords. This is used to deactivate a user. Use this instead of deleting a user. Deleting a user will delete files as well.
passwd -S login 
to report password status on the named account
echo password | passwd --stdin linda
To set password for many users
chage linda
To change the age on the password

sudo vipw
To edit /etc/passwd

sudo vipw -s 
To edit /etc/shadow

#### User Session Management
who and w show who is logged in
loginctl allows for current session management 

#### Commands
useradd, usermod, userdel, groupadd, groupmod, groupdel, id, getent, passwd, chage, w, who, vipw, vigr, loginctl

#### Very important:
File to set default values associated with logins
sudo less /etc/login.defs
File to set user's defaults like home directory
sudo less /etc/default/useradd

### Lesson 8: Permission Management


Lesson 9: Storage Management Essentials
Storage can be offered externally through SAN with protocols like iSCSI and Fiberchannel 
The storage device has a name like /dev/sda (where sd stands for scsi disk and 'a' is the first disk)
Partition is a slice of the disk. It is created to separate specific areas of the computer. Say /home is installed on one partition and / (root) is installed on another.
LVM is like a partition within a partition.
There are two solutions to partitioning: MBR and GPT
MBR is older and has a few limitions. CentOS still uses MBR, while Ubuntu uses GPT.
Create Partitions using either fdisk or gdisk. fdisk is prefered because it is not destructive.

/proc/partitions lists the list of partitions 

Use partprobe to write the partions after creating them in memory. 

Creating File System
mkfs, mkfs.ext4, mkfs.xfs
XFS is the default file system on RHEL and Suse.
EXT4 is the default file system on Ubuntu
VFAT is the only file system compatible across Windows, Linux and MacOS.
BTRFS (ButterFS) is a copy-on-write (COW) file system
NTFS is a Windows file system (not Linux) and is not supported by RHEL

mkfs.ext4 /dev/sdb

Mounting is the process of connecting a file system to directory.
mount /dev/sdb1 /mnt 

Create a Partition, Create a File System on the Partition, Mount the Partition on a directory

Mounts seem to be automated through /etc/fstab
systemd-fstab-generator processes /etc/fstab and converts mounts found there to systemd managed mounts
Administrators can manually create systemd mounts through mount units
See /usr/lib/systemd/system/tmp.mount for an example. 

Linux offers a template file for systemd mounts. Search for tmp.mount under /

blkid, lsblk

Lesson 16: Logging
journalctl shows the complete journalctl
journalctl -u <unit> shows the information about specific unit (use tab completion)
To show the kernel messages, run
journalctl --dmsg 
combined filters can be used. Example:
journalctl -u crond --since yesterday --until 9:00 -p info

Rsyslog
The rsyslog d service works with facility, priority and desitnation
The facility is what rsyslogd should be logging for
The priority indicates the severity of a log event
The destination defines where the message should be written to

/etc/rsyslog.*
/var/log/messages*

To write directly to /var/log/messages*
logger hello

Commands
journalctl
tail 
less 
logger

Lesson 21: Email 
MUA is the user program that uses IMAP or POP to fetch Email
MTA is the mail transfer agent, which uses SMTP to send mail to the recipient 
MDA is the Mail Delivery Agent which stores the message so that users can access it locally or using protocols such as POP3 and IMAP 

Lesson 22: Managing Proxies