# Perceiving Permissions
We can see the permissions using 'ls -l' command :
```bash
hacker@dojo:~$ mkdir pwn_directory
hacker@dojo:~$ touch college_file
hacker@dojo:~$ ls -l
total 4
-rw-r--r-- 1 hacker hacker    0 May 22 13:42 college_file
drwxr-xr-x 2 hacker hacker 4096 May 22 13:42 pwn_directory
hacker@dojo:~$
```
**File Type** : determined by the first character . 'd' = directory and '-' = normal file . <br>
<br>
(Group: Files also have a group associated with them, and all users in that group have certain permissions (defined by the file's permission settings).)<br>
**The Permissions** : he next nine characters are the actual access permissions of the file or directory,<br>
split into 3 characters denoting the permissions that the user who owns the file (termed the "owner") has to the file, <br>
3 characters denoting the permissions that the group that owns the file (termed the "group") has to the file, <br>
and 3 characters denoting the permissions that all other access (e.g., by other users and other groups) has to the file. <br>
<br>
Like in the example, for college_file first 3 characters 'rw-' denotes the owner can read and write , next 3 characters 'r--' tell that the group<br>
can only read and then 'r--' says that others can only read as well.<br><br>
**Ownership Info** : There are two columns showing the user that owns the file (in this case, user hacker) and then the group that owns the file <br>
(in this case, also group hacker).

## Thought Process
**Changing File Ownership** : Every file in linux is owned by a user on the system. In shared systems, like a computer lab,<br>
each user account will have its own files. On your personal system, service accounts (like root) manage essential tasks.
Two key accounts: <br>
Your user account (e.g., the hacker user in pwn.college).<br>
root: The system administrator, who has ultimate control over everything. This is often a target in security contexts, <br>
because controlling root means controlling the system.
```bash
-r-------- 1 root root 53 Jul 4 04:47 /flag

```
Here the /flag file is owner by the root and is in the root group as well hence only the root user can read the file.<br>
Even though you have access to the system as the hacker user, you're blocked from reading /flag because you don't have the necessary permissions.<br><br>

chown: Changes the owner of a file.
```bash
chown [username] [file]

```
As you've seen, if a file like /flag is owned by root, you (as a non-root user) can't read it.<br>
But if you could become root or use chown to make hacker the owner, you'd gain access.<br><br>
**Typically, chown can only be invoked by the root user.**
### Solving 
Typically u have to be a root user ie: owner to change the ownership but in this challenge its possible to use chown command as hacker user <br>
and hence get acess to reading the flag file.
#### Challenge 1
```bash
hacker@permissions~changing-file-ownership:~$ chown hacker /flag
hacker@permissions~changing-file-ownership:~$ cat /flag
pwn.college{8uUcB0XynKyM6JaseOXy75lAGvz.dFTM2QDLykTN0czW}
```
## Thought Process
**Groups and Files** : A group is a collection of users. Users within the same group can share access to files or system resources.<br>
Each user can belong to one or more groups, and a file can be assigned both an owning user and an owning group.<br>
To check what groups we belong to we can use the 'id' command.<br>
A typical Linux user will be part of multiple groups. Each group gives them access to various system functionalities (e.g., audio, video, networking). <br>
Just like you can change the owner of a file with chown, you can change the group ownership using the chgrp command.<br>

### Solving 
Here since the group permissions were needed from the root user we changed the group user to hacker using chgrp command and read the flag file.
#### Challenge 2
```bash
hacker@permissions~groups-and-files:~$ chgrp hacker /flag
hacker@permissions~groups-and-files:~$ cat /flag
pwn.college{kNSYgzE5NshXm5O-c42WlZ40nG4.dFzNyUDLykTN0czW}
```
## Thought Process
**Fun with Group Names** : So we've seen that normally by convention for the hacker user grp name will be hacker and we can also change the grp name <br>
but many times all group names are under on name for all shared users so we can even create group names.
### Solving 
Here the group name was randomised so i first used id command to figure out all the groups im in as a user and saw what group i need to change to using chgrp <br>
to get the permission to read the flag file using cat /flag command. So the gid=() should give the primary group name.
#### Challenge 3
```bash
hacker@permissions~fun-with-groups-names:~$ id
uid=1000(hacker) gid=1000(grp18795) groups=1000(grp18795)
hacker@permissions~fun-with-groups-names:~$ chgrp grp18795 /flag
hacker@permissions~fun-with-groups-names:~$ cat /flag
pwn.college{wF5N5WS5yTgFspWvS2cjoMu-gxW.dJzNyUDLykTN0czW}
```
## Thought Process
**Changing Permissions** : As we read in the introduction about file types and permissions, this is a further insight : <br>
<br>
r - user/group/other can read the file (or list the directory)<br>
w - user/group/other can modify the files (or create/delete files in the directory)<br>
x - user/group/other can execute the file as a program (or can enter the directory, e.g., using `cd`)<br>
- - nothing <br>
<br>
Like ownership, file permissions can also be changed. This is done with the chmod (change mode) command. The basic usage for chmod is: <br>
chmod [OPTIONS] MODE FILE <br>
  we can specify the MODE in two ways: as a modification of the existing permissions mode, or as a completely new mode to overwrite the old one.<br>
  modifying an existing mode :<br>
  
### Solving 

#### Challenge 4
```bash

```
## Thought Process
**Executable Files** : 
### Solving 

#### Challenge 5
```bash

```
## Thought Process
**Permission Tweaking Practice** : 
### Solving 

#### Challenge 6
```bash

```
## Thought Process
**Permission Setting Practice** : 
### Solving 

#### Challenge 7
```bash

```
## Thought Process
**The SUID bit** : 
### Solving 

#### Challenge 8
```bash

```
