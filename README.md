# Linux

Table of contents
=================

<!--ts-->
   * [Stop Prompting for PASSWD all the time](#stop-prompting-for-passwd-all-the-time)
   * [Increase Performance of Linux Server](#increase-performance-of-linux-server)
      * [relatime](#relatime)
      * [barriers](#barriers)
<!--te-->

Stop Prompting for PASSWD all the time
======================================

- edit the sudovers.d file
```
sudo visudo
```
- add the following line to the end of doc
```
<user_name> ALL=(ALL) NOPASSWD: ALL
```
- save and exit
- run following command to remove cashed passwd
```
sudo -k
```
- check weather it works!
```
sudo ls
```
Increase Performance of Linux Server 
====================================
- Tuning (25-30)%

- Two ways 
```
1. ralatime
2. barrier
```

relatime
----------------------------------------------
- relatime is a mount option in /etc/fstab
- POSIX standard OS required to maintain filesystem metadata that record each time when a file was last accessed, last modified, last changed.

### file system metadata

> stat file.txt

![](https://github.com/GimhanDissanayake/Linux/blob/main/stat.PNG)
```
- atime (access) 
- mtime (modification)
- ctime (create)
```
- prior to RHEL6 for each time which file was read --> atime has to record --> waste --> unneccesary load on the operating system
- Using "relatime" , atime is updated only when the file or directory data is been modified. No modification of filesystem metadata for read or accessed operation.
- filesystem atime is updated when, modified OR accessed after morethan sertain period(RHEL6, its 1 day)


> cat /etc/fstab
- you can see mount option for mounted file systems (ex- default)
![](https://github.com/GimhanDissanayake/Linux/blob/main/fstab.PNG)


> cat /proc/mounts
- you can see options for default
![](https://github.com/GimhanDissanayake/Linux/blob/main/proc.PNG)

- To set up ralatime for RHEL4,5

> cat /etc/fstab
```
/dev/sda1	           /mount-point      ext4      defaults,relatime   0  0
```
- can suppress it by using "norelatime" insted



barriers 
========
- mount option in /etc/fstab

> cat /proc/mounts

"barriers=1"

- barrier control JBD (Journalling Block Device) . Barrier have 2 values (1 for enable or 0 for disable)
- filesystem enforce a propper journal commit. It is used to reduce data corruption chances, IN CASE OF POWER FAILURE
- when enabled, constantly flushing write cache --> reduce performance
- but.... nowadays all servers are battery backuped
- can safely improve performance by disabling barrier on barrery backedup laptop or server

> vim /etc/fstab
```
/dev/root       ext4     defaults,barriers=0    1   1
# save and exit

mount -o remount   /

cat /proc/mounts
```
