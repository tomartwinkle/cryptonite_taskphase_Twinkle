# Processes and Jobs
Computers execute software to get stuff done. In modern computing, this software is split into two categories:<br>
operating system kernels (about which we will learn much later) and processes.<br>
When Linux starts up, it launches an init (initializer) process that, in turn, launches a bunch of other processes which launch more processes<br>
until, eventually, you are looking at your command line shell, which is also a process! The shell, of course, launches processes in response to the commands you enter.
## Thought Process 
**Listing Processes** : "ps" command helps list processes (process snapshot or process status) , ps will bydefault list processes running in the terminal.<br>
It will show the name of the process , the terminal on which the commands are running as well as a unique no. that identifies every process running in linux (PID),<br> and the total amount of cpu time that the process has used up so far.<br>
To make the ps command more useful we need to pass it through some arguments and there are 2 ways to do so : <br>
1. Standard syntax : " -e " to list every process and " -f " for full format output,including arguments. We can combine these as  " -ef ".
2. BSD syntax : use "a" to list processes for all users, "x" to list processes that aren't running in the terminal, "u" for user-readable output and we can <br>
combine them as "aux".
(There are many commonalities between ps -ef and ps aux: both display the user (USER column), the PID, the TTY, the start time of the process (STIME/START), the <br> total utilized CPU time (TIME), and the command (CMD/COMMAND). ps -ef additionally outputs the Parent Process ID (PPID), which is the PID of the process<br>
that launched the one in question, while ps aux outputs the percentage of total system CPU and Memory that the process is utilizing.)
### Solving 
So in this challenge the file where flag is, is renamed and we can't use ls command but it is launched ie: process is running so we can simply list the running <br>
processes and see where the flag is using the ps command with either of the arguments -ef or aux.
#### Challenge  1
```bash
hacker@processes~listing-processes:~$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 03:48 ?        00:00:00 /sbin/docker-init -- /nix/var/nix/profiles/default/bin/dojo-init /ru
root           7       1  0 03:48 ?        00:00:00 /run/dojo/bin/sleep 6h
root          68       1  0 03:48 ?        00:00:00 /challenge/9433-run-18609
root          72      68  0 03:48 ?        00:00:00 sleep 6h
hacker        73       0  0 03:48 pts/0    00:00:00 /run/dojo/bin/ssh-entrypoint
hacker        91      73  0 03:49 pts/0    00:00:00 ps -ef
hacker@processes~listing-processes:~$ /challenge/9433-run-18609
Yahaha, you found me! Here is your flag:
pwn.college{o5w2pjVbeXHO3cg2ByIbYYCR7WT.dhzM4QDLykTN0czW}
Now I will sleep for a while (so that you could find me with 'ps').
```
## Thought Process 
**Killing Processes** : To terminate running processes we can use the 'kill' command. By default kill will terminate a process in a way that gives it a chance to <br> get its affairs in order before ceasing to exist.For example : sleep is a process that exists for only the amount of time we specify ,<br>
sleep 1337 (the process will last for 1337 seconds)<br>
now if i want to kill this process i'll simply use the kill command and the PID of the sleep process as the argument that we can find using ps -e<br>
ps -e | grep sleep (listing processes and grepping for sleep in it that will give only the sleep process)<br>
the code will look like this : <br>
<br>
```bash
hacker@dojo:~$ sleep 1337
hacker@dojo:~$ ps -e | grep sleep
 342 pts/0    00:00:00 sleep
hacker@dojo:~$ kill 342
hacker@dojo:~$ ps -e | grep sleep
hacker@dojo:~$
```
### Solving 
In this challenge the file where our flag is , wont run until we terminate the process of /challenge/dont_run running. 
#### Challenge  2
```bash
hacker@processes~killing-processes:~$ ps -ef
UID          PID    PPID  C STIME TTY          TIME CMD
root           1       0  0 03:57 ?        00:00:00 /sbin/docker-init -- /nix/var/nix/profiles/default/bin/dojo-init /ru
root           7       1  0 03:57 ?        00:00:00 /run/dojo/bin/sleep 6h
root          71       1  0 03:57 ?        00:00:00 su -c /challenge/.launcher hacker
hacker        73      71  0 03:57 ?        00:00:00 /challenge/dont_run
hacker        74      73  0 03:57 ?        00:00:00 sleep 6h
hacker        75       0  0 03:57 pts/0    00:00:00 /run/dojo/bin/ssh-entrypoint
hacker        95      75  0 03:58 pts/0    00:00:00 ps -ef
hacker@processes~killing-processes:~$ kill 73
hacker@processes~killing-processes:~$ /challenge/run
Great job! Here is your payment:
pwn.college{cRZXZ-ljHsvMp7GjCWRjF8Ro0S1.dJDN4QDLykTN0czW}
```
## Thought Process 
**Interrupting Processes** : 
### Solving 

#### Challenge  3
```bash

```
## Thought Process 
** ** : 
### Solving 

#### Challenge  4
```bash

```
## Thought Process 
** ** : 
### Solving 

#### Challenge  5
```bash

```
## Thought Process 
** ** : 
### Solving 

#### Challenge  6
```bash

```
## Thought Process 
** ** : 
### Solving 

#### Challenge  7
```bash

```
## Thought Process 
** ** : 
### Solving 

#### Challenge  8
```bash

```
## Thought Process 
** ** : 
### Solving 

#### Challenge  9
```bash

```

