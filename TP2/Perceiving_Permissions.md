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
**The Permissions** : The next nine characters are the actual access permissions of the file or directory,<br>
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
'-' - nothing <br>
<br>
Like ownership, file permissions can also be changed. This is done with the chmod (change mode) command. The basic usage for chmod is: <br>
chmod [OPTIONS] MODE FILE <br>
  we can specify the MODE in two ways: as a modification of the existing permissions mode, or as a completely new mode to overwrite the old one.<br>
  modifying an existing mode :<br>
  chmod allows you to tweak permissions with the mode format of WHO+/-WHAT, where WHO is user/group/other and WHAT is read/write/execute.
For example, to add read access for the owning user, we add u+r as argument. More examples:<br><br>
u+r, as above, adds read access to the user's permissions<br>
g+wx adds write and execute access to the group's permissions<br>
o-w removes write access for other users<br>
a-rwx removes all permissions for the user, group, and world<br><br>
where u-user , g-group , o-others , r - read , w-write , x-execute , a- all permissions and if we use '*' it will remove those permissions like so :<br>
<br>
```bash
root@dojo:~# mkdir pwn_directory
root@dojo:~# touch college_file
root@dojo:~# ls -l
total 4
-rw-r--r-- 1 root root    0 May 22 13:42 college_file
drwxr-xr-x 2 root root 4096 May 22 13:42 pwn_directory
root@dojo:~# chmod go-rwx *
root@dojo:~# ls -l
total 4
-rw------- 1 hacker root    0 May 22 13:42 college_file
drwx------ 2 root   root 4096 May 22 13:42 pwn_directory
```
<br>
Typically, you need to have write access to the file in order to change its permissions

### Solving 
At first i tried doing u+r but the user is root not hacker so that was wrong, instead its correct to give permission to all users using a+r and then specifying the /flag file in chmod command.

#### Challenge 4
```bash
hacker@permissions~changing-permissions:~$ chmod u+r /flag
hacker@permissions~changing-permissions:~$ cat /flag
cat: /flag: Permission denied
hacker@permissions~changing-permissions:~$ chmod a+r /flag
hacker@permissions~changing-permissions:~$ cat /flag
pwn.college{IgcZTU0J96jJAAlgj_63JZq6a0a.dNzNyUDLykTN0czW}
```
## Thought Process
**Executable Files** : Just like read permission allows us to read a file, we need execute permissions to be able to execute the program.
### Solving 
So, here there are 2 ways to solve i can either use a+x or just do +x for the /challenge/run program to make it executable using chmod command. <br>
chmod +x: Adds execute permission only to the owner.<br>
chmod a+x: Adds execute permission to all users (owner, group, and others).
#### Challenge 5
```bash
hacker@permissions~executable-files:~$ chmod a+x /challenge/run
hacker@permissions~executable-files:~$ /challenge/run
Successful execution! Here is your flag:
pwn.college{sdADL6v72PVfATjrOCrRWY-4qhB.dJTM2QDLykTN0czW}
```
or 
```bash
hacker@permissions~executable-files:~$ chmod +x /challenge/run
hacker@permissions~executable-files:~$ /challenge/run
Successful execution! Here is your flag:
pwn.college{sdADL6v72PVfATjrOCrRWY-4qhB.dJTM2QDLykTN0czW}
```
## Thought Process
**Permission Tweaking Practice** : 
### Solving 
At first while solving i did the 3rd round incorrectly as i used * sign when it wasnt needed and didnt know to use commas. <br>
When you specify * with o-w, it implies you want to remove write permissions from all files in the current directory and also apply it to /challenge/pwn.<br>
 The command does not set the specific permissions you want; it only attempts to remove write and execute permissions from the owner and write permissions from others.<br>
 Hence i started removing permissions specifically using this kind of format 'chmod u-rw,g-r /challenge/pwn' as an example 
#### Challenge 6
```bash
Connected!
hacker@permissions~permission-tweaking-practice:~$ /challenge/run
Round 0 (of 8)!

Current permissions of "/challenge/pwn": rw-r--r--
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rwxr--r--
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod u+x /challenge/pwn
You set the correct permissions!
Round 1 (of 8)!

Current permissions of "/challenge/pwn": rwxr--r--
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rwxrw-rw-
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod a+w /challenge/pwn
You set the correct permissions!
Round 2 (of 8)!

Current permissions of "/challenge/pwn": rwxrw-rw-
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": r--rw-r--
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod u-wx * o-w * /challenge/pwn
/usr/bin/chmod: changing permissions of 'c': Operation not permitted
/usr/bin/chmod: changing permissions of 'my_file': Operation not permitted
/usr/bin/chmod: changing permissions of 'my_flag': Operation not permitted
/usr/bin/chmod: cannot operate on dangling symlink 'new'
/usr/bin/chmod: cannot operate on dangling symlink 'new_backup'
/usr/bin/chmod: cannot operate on dangling symlink 'new_file'
/usr/bin/chmod: changing permissions of 'not-the-flag': Operation not permitted
/usr/bin/chmod: changing permissions of 'pwn': Operation not permitted
/usr/bin/chmod: cannot access 'o-w': No such file or directory
/usr/bin/chmod: changing permissions of 'c': Operation not permitted
/usr/bin/chmod: changing permissions of 'my_file': Operation not permitted
/usr/bin/chmod: changing permissions of 'my_flag': Operation not permitted
/usr/bin/chmod: cannot operate on dangling symlink 'new'
/usr/bin/chmod: cannot operate on dangling symlink 'new_backup'
/usr/bin/chmod: cannot operate on dangling symlink 'new_file'
/usr/bin/chmod: changing permissions of 'not-the-flag': Operation not permitted
/usr/bin/chmod: changing permissions of 'pwn': Operation not permitted
NEEDED, BUT UNMET permissions of "/challenge/pwn": r--rw-r--
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

CURRENT, INCORRECT permissions of "/challenge/pwn": r--rw-rw-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

You set the permissions incorrectly! Restarting the game!
hacker@permissions~permission-tweaking-practice:~$ /challenge/run
Round 0 (of 8)!

Current permissions of "/challenge/pwn": rw-r--r--
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rwxr--r--
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod u+x /challenge/pwn
You set the correct permissions!
Round 1 (of 8)!

Current permissions of "/challenge/pwn": rwxr--r--
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rwxrw-rw-
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod a+w /challenge/pwn
You set the correct permissions!
Round 2 (of 8)!

Current permissions of "/challenge/pwn": rwxrw-rw-
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": r--rw-r--
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod u-wx,o-w /challenge/pwn
You set the correct permissions!
Round 3 (of 8)!

Current permissions of "/challenge/pwn": r--rw-r--
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": r-xrw-r-x
* the user does have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
* the world does have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod u+x,o+x /challenge/pwn
You set the correct permissions!
Round 4 (of 8)!

Current permissions of "/challenge/pwn": r-xrw-r-x
* the user does have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
* the group does have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
* the world does have execute permissions

Needed permissions of "/challenge/pwn": r-x-w----
* the user does have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod g-r,o-r,o-x /challenge/pwn
You set the correct permissions!
Round 5 (of 8)!

Current permissions of "/challenge/pwn": r-x-w----
* the user does have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": r-x-wx--x
* the user does have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
* the world does have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod a+x /challenge/pwn
You set the correct permissions!
Round 6 (of 8)!

Current permissions of "/challenge/pwn": r-x-wx--x
* the user does have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
* the world does have execute permissions

Needed permissions of "/challenge/pwn": --x--x--x
- the user doesn't have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
* the group does have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
* the world does have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod u-r,g-w /challenge/pwn
You set the correct permissions!
Round 7 (of 8)!

Current permissions of "/challenge/pwn": --x--x--x
- the user doesn't have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
* the group does have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
* the world does have execute permissions

Needed permissions of "/challenge/pwn": rwx--x--x
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
* the group does have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
* the world does have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod u+rw /challenge/pwn
You set the correct permissions!
You've solved all 8 rounds! I have changed the ownership
of the /flag file so that you can 'chmod' it. You won't be able to read
it until you make it readable with chmod!

Current permissions of "/flag": ---------
- the user doesn't have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permission-tweaking-practice:~$ chmod a+r /flag
hacker@permissions~permission-tweaking-practice:~$ /flag
ssh-entrypoint: /flag: Permission denied
hacker@permissions~permission-tweaking-practice:~$ cat /flag
pwn.college{oGxXsytICiT1DTdFYqvZynyK72m.dBTM2QDLykTN0czW}
```
## Thought Process
**Permission Setting Practice** : Instead of just adding or removing permissions we can also set them using = instead of +/- and OVERWRITES the existing permissions .<br>
<br>
u=rw sets read and write permissions for the user, and wipes the execute permission<br>
o=x sets only executable permissions for the world, wiping read and write<br>
a=rwx sets read, write, and executable permissions for all<br>
<br>
to set different permissions for user , different ones for group etc. we use commas : <br>
chmod u=rw,g=r /challenge/pwn  (will set the user permissions to read and write, and the group permissions to read-only) <br>
and we can also put '-' to zero out the permissions. <br>
When - appears after = (e.g., u=r-x), it denotes the removal of all permissions from the specified user (owner, group, or others) before setting the new permissions.<br>
For example, u=r means "set the user permissions to read only," effectively removing write and execute permissions.<br>
When - appears directly after u, g, or o (e.g., u-w, g+x), it indicates the removal of specific permissions.<br>
For example, u-w means "remove write permission from the user," and g+x means "add execute permission to the group."<br>

### Solving 
I found setting permissions easier way to approach rather than tweaking permission for this challenge so i used that for every level and eventually got <br>
the flag, i did have to restart it once as i made a silly mistake and read one permission wrong otheriwse, faced no issues solving here.
#### Challenge 7
```bash
hacker@permissions~permissions-setting-practice:~$ /challenge/run
Round 0 (of 8)!

Current permissions of "/challenge/pwn": rw-r--r--
* the user does have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": r-x------
* the user does have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=rx,g=-,o=- /challenge/pwn
You set the correct permissions!
Round 1 (of 8)!

Current permissions of "/challenge/pwn": r-x------
* the user does have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": r--r---w-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
* the world does have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=r,g=r,o=w /challenge/pwn
You set the correct permissions!
Round 2 (of 8)!

Current permissions of "/challenge/pwn": r--r---w-
* the user does have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
* the world does have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": rwx-----x
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
* the world does have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=rwx,g=-,o=x /challenge/pwn
You set the correct permissions!
Round 3 (of 8)!

Current permissions of "/challenge/pwn": rwx-----x
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
* the world does have execute permissions

Needed permissions of "/challenge/pwn": r-xrwxrwx
* the user does have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
* the group does have read permissions
* the group does have write permissions
* the group does have execute permissions
* the world does have read permissions
* the world does have write permissions
* the world does have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=rx,g=rwx,o=rwx /challenge/pwn
You set the correct permissions!
Round 4 (of 8)!

Current permissions of "/challenge/pwn": r-xrwxrwx
* the user does have read permissions
- the user doesn't have write permissions
* the user does have execute permissions
* the group does have read permissions
* the group does have write permissions
* the group does have execute permissions
* the world does have read permissions
* the world does have write permissions
* the world does have execute permissions

Needed permissions of "/challenge/pwn": -wxr----x
- the user doesn't have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
* the world does have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=wx,g=r,o=x /challenge/pwn
You set the correct permissions!
Round 5 (of 8)!

Current permissions of "/challenge/pwn": -wxr----x
- the user doesn't have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
* the world does have execute permissions

Needed permissions of "/challenge/pwn": -w--wxr--
- the user doesn't have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=w,g=wx,o=r /challenge/pwn
You set the correct permissions!
Round 6 (of 8)!

Current permissions of "/challenge/pwn": -w--wxr--
- the user doesn't have read permissions
* the user does have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
* the group does have write permissions
* the group does have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions

Needed permissions of "/challenge/pwn": -----xr-x
- the user doesn't have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
* the group does have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
* the world does have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=-,g=x,o=rx /challenge/pwn
You set the correct permissions!
Round 7 (of 8)!

Current permissions of "/challenge/pwn": -----xr-x
- the user doesn't have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
* the group does have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
* the world does have execute permissions

Needed permissions of "/challenge/pwn": rwxr-xr--
* the user does have read permissions
* the user does have write permissions
* the user does have execute permissions
* the group does have read permissions
- the group doesn't have write permissions
* the group does have execute permissions
* the world does have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod u=rwx,g=rx,o=r /challenge/pwn
You set the correct permissions!
You've solved all 8 rounds! I have changed the ownership
of the /flag file so that you can 'chmod' it. You won't be able to read
it until you make it readable with chmod!

Current permissions of "/flag": ---------
- the user doesn't have read permissions
- the user doesn't have write permissions
- the user doesn't have execute permissions
- the group doesn't have read permissions
- the group doesn't have write permissions
- the group doesn't have execute permissions
- the world doesn't have read permissions
- the world doesn't have write permissions
- the world doesn't have execute permissions
hacker@permissions~permissions-setting-practice:~$ chmod a+r /flag
hacker@permissions~permissions-setting-practice:~$ cat /flag
pwn.college{gHZuYjoQDwEHmjrXhrmnxCRn1-z.dNTM5QDLykTN0czW}
```
## Thought Process
**The SUID bit** : The SUID bit is the ' set user ID ' bit ,It allows non-root users to execute specific programs with the privileges of the file’s owner,
often root, which is crucial for various system tasks that require elevated access. The SUID bit (s) in file permissions allows a program to run as the file <br>owner, even if a non-privileged user executes it. This is how programs like sudo can give temporary root access.
Setting the SUID bit on a program owned by root might lead to security vulnerabilities. If an attacker finds a way to misuse that program, they might be able to escalate their privileges and gain full control of the system. This is why system administrators need to be very cautious when assigning the SUID bit to any executable. when we add it to file permissiosn we can see an additional 's' which shows that it has the SUID bit.
### Solving 
i used chmod u+s /challenge/getroot to put the suid bit in the permissions so that i can run the program with the privilages of the owner even if im 
another user. Then at first attempt i had tried straightaway reading the flag file using cat /flag but it didnt run as i didnt get the file permissions yet 
i had to first set the suid bit program running so it could run as root ie: running the /challenge/getroot file after adding SUID in the permissions so it runs<br>
as root and then i can read the flag file using cat command as now it will give me owner's privilages.
#### Challenge 8
```bash
root@permissions~the-suid-bit:~# chmod u+s /challenge/getroot
root@permissions~the-suid-bit:~# /challenge/getroot
SUCCESS! You have set the suid bit on this program, and it is running as root!
Here is your shell...
root@permissions~the-suid-bit:~# cat /flag
pwn.college{cyMKX5lNDVet7cfdCkMo_pFdEs3.dNTM2QDLykTN0czW}
```
