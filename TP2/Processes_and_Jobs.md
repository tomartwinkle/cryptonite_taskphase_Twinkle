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
**Interrupting Processes** : Sometimes instead of killing a process, we just want to the get rid of the process thats clogging the terminal <br>
what this means is sometimes a process might get stuck or we'd want to stop it manually so we can use the shortcut ctrl+c to cleanly exit.<br>
It's a quick and easy way to stop programs that are waiting for input or are unresponsive, without having to use more complex commands.<br>
The difference between kill and interrupting is the way they are used to stop processes and the signals they send to the processes <br>
In short, interrupting is a softer politer way and killing is a hard and stronger way to terminate a process.
### Solving 
Here, the process wont start by running the /challenge/run file till we interrupt it.
#### Challenge  3
```bash
hacker@processes~interrupting-processes:~$ /challenge/run
I could give you the flag... but I won't, until this process exits. Remember,
you can force me to exit with Ctrl-C. Try it now!
^C
Good job! You have used Ctrl-C to interrupt this process! Here is your flag:
pwn.college{ggnTL4rzKGIa9xiT9R237GvOGB_.dNDN4QDLykTN0czW}
```
## Thought Process 
**Suspending Processes** : Another softer method is to just suspend the processes to the background using ctrl+z .
### Solving 
Here the run wants to see another copy of itself ie: we have to start the process again but for one single process to run twice simultaneously <br>
we can run it once and suspend it in the background and run it again , that way there will be 2 processes .
#### Challenge  4
```bash
hacker@processes~suspending-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running in
this terminal... Let's check!

UID          PID    PPID  C STIME TTY          TIME CMD
root          83      65  0 06:24 pts/0    00:00:00 bash /challenge/run
root          85      83  0 06:24 pts/0    00:00:00 ps -f

I don't see a second me!

To pass this level, you need to suspend me and launch me again! You can
background me with Ctrl-Z or, if you're not ready to do that for whatever
reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~suspending-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running in
this terminal... Let's check!

UID          PID    PPID  C STIME TTY          TIME CMD
root          83      65  0 06:24 pts/0    00:00:00 bash /challenge/run
root          90      65  0 06:24 pts/0    00:00:00 bash /challenge/run
root          92      90  0 06:24 pts/0    00:00:00 ps -f

Yay, I found another version of me! Here is the flag:
pwn.college{0OHKZLqAWSVrO3P3NsEuuOMyTXk.dVDN4QDLykTN0czW}
```
## Thought Process 
**Resuming Processes** : Now, we suspend processes to be able to resume them in the future, otherwise we could just kill them. <br>
To resume a process and get it back to the foreground of the terminal we use the "fg" command. 
### Solving 
I first suspended the run process so its in the background now and to resume it and get it back into the foreground ill use the fg command.
#### Challenge  5
```bash
hacker@processes~resuming-processes:~$ /challenge/run
Let's practice resuming processes! Suspend me with Ctrl-Z, then resume me with
the 'fg' command! Or just press Enter to quit me!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~resuming-processes:~$ fg
/challenge/run
I'm back! Here's your flag:
pwn.college{UuFJjlV19AeBtnNHAvK0ys1FJjG.dZDN4QDLykTN0czW}
Don't forget to press Enter to quit me!

Goodbye!
```
## Thought Process 
**Backgrounding Processes** : fg resumes the process  in the foreground but we can also resume them in the background itself using the 'bg' command.<br>
So the process will keep running in the background while we can add or process more commands in the terminal.
### Solving 
The run process wants to see another process of itself running but this time, it should not be suspended. So basically again we run it once and suspend it <br>
But then keep it running in the background using bg command and again run it the 2nd time in the terminal.
#### Challenge  6
```bash
Connected!
hacker@processes~backgrounding-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running *and
not suspended* in this terminal... Let's check!

UID          PID STAT CMD
root          82 S+   bash /challenge/run
root          84 R+   ps -o user=UID,pid,stat,cmd

I don't see a second me!

To pass this level, you need to suspend me, resume the suspended process in the
background, and then launch a new version of me! You can background me with
Ctrl-Z (and resume me in the background with 'bg') or, if you're not ready to
do that for whatever reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~backgrounding-processes:~$ bg
[1]+ /challenge/run &



Yay, I'm now running the background! Because of that, this text will probably
overlap weirdly with the shell prompt. Don't panic; just hit Enter a few times
to scroll this text out.
hacker@processes~backgrounding-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running *and
not suspended* in this terminal... Let's check!

UID          PID STAT CMD
root          82 S    bash /challenge/run
root          92 S    sleep 6h
root          93 S+   bash /challenge/run
root          95 R+   ps -o user=UID,pid,stat,cmd

Yay, I found another version of me running in the background! Here is the flag:
pwn.college{As64Xe49pnmapZCn1eyjFwJv1Ip.ddDN4QDLykTN0czW}
```
**EXTRA** :<br>
Viewing differences between suspend and background properties. First we'll suspend a sleep process :<br>
sleep 1337<br>
(ctrl + z)
The sleep process is now suspended in the background. We can see this with ps by enabling the stat column output with the -o option: <br>
```bash
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker       702 Ss   bash
hacker       762 T    sleep 1337
hacker       782 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$ 
```
So the T in front of sleep process as its stat shows that the sleep process is suspended and the S in bash's stat shows that its sleeping while waiting for input<br>. In the ps's stat the R means its running and + means in the foreground.<br>
Lets see when we resume the sleep process :
```bash
hacker@dojo:~$ bg
[1]+ sleep 1337 &
hacker@dojo:~$ ps -o user,pid,stat,cmd
USER         PID STAT CMD
hacker       702 Ss   bash
hacker       762 S    sleep 1337
hacker      1224 R+   ps -o user,pid,stat,cmd
hacker@dojo:~$
```
The S shows that its sleeping but not suspended and since it doesnt have the + sign hence, its in the background. 
## Thought Process 
**Foregrounding Processes** : If a process is in the background and we want to work with it we can bring it back to the foreground again with the fg command.
### Solving 
We first run the /challenge/run process and suspend it using ctrl+Z , then resume it in the background using bg now its running in the background but <br>
to bring it back to the foreground ill just type the fg command and get the flag as it runs in the foreground of the terminal.
#### Challenge  7
```bash
hacker@processes~foregrounding-processes:~$ ps -o stat
STAT
Ss
R+
hacker@processes~foregrounding-processes:~$ fg
ssh-entrypoint: fg: current: no such job
hacker@processes~foregrounding-processes:~$ /challenge/run
To pass this level, you need to suspend me, resume the suspended process in the
background, and *then* foreground it without re-suspending it! You can
background me with Ctrl-Z (and resume me in the background with 'bg') or, if
you're not ready to do that for whatever reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~foregrounding-processes:~$ bg
[1]+ /challenge/run &



hacker@processes~foregrounding-processes:~$ Yay, I'm now running the background! Because of that, this text will probably
overlap weirdly with the shell prompt. Don't panic; just hit Enter a few times
to scroll this text out. After that, resume me into the foreground with 'fg';
I'll wait.
fg
/challenge/run
YES! Great job! I'm now running in the foreground. Hit Enter for your flag!

pwn.college{YunBmUT67b1_y0Y91X9qkTPAnx3.dhDN4QDLykTN0czW}
```
## Thought Process 
**Starting Background Processes** : Instead of starting a process and then suspending it and then running it in the background we can straightup <br>
start a background process by just adding a & sign at the end of the process command. like : <br>
sleep 1337 &

### Solving 
now to start  /challenge/run as a background process i can simply append it with a & sign.
#### Challenge  8
```bash
hacker@processes~starting-backgrounded-processes:~$ /challenge/run &
[1] 82



Yay, you started me in the background! Because of that, this text will probably
overlap weirdly with the shell prompt, but you're used to that by now...

hacker@processes~starting-backgrounded-processes:~$ Anyways! Here is your flag!
pwn.college{EfxL7ijE7K6llZuB0RzCoLcEtjt.dlDN4QDLykTN0czW}

[1]+  Done                    /challenge/run
```
## Thought Process 
**Process Exit Codes** : Every shell command, whether a program or a built-in, ends with an exit code after execution. This exit code allows the shell <br>
or the user to determine if the command succeeded or failed, based on what the process was intended to accomplish.<br>
We can access this exit code of the most recently terminated command by using the special variable '?' and to read it's value we must append it with $ sign.<br>
ex :<br>
touch text_file<br>
echo $? <br>
output = 0<br>
touch /test-file <br>
touch: cannot touch '/test-file': Permission denied <br>
echo $?<br>
output = 1<br>
Commanly if a command succees would return 0 and fails it will return 1 but sometimes can return other values.
### Solving 
Here we had to run the /challenge/get-code process and as it terminates it will generate an exit code, to retrieve and read that exit code i<br>
use the echo command with $? argument where ? gives the exit code and $ reads it , further to get the flag we had to run the /challenge/submit-code with <br>
the exit code of prev process as its argument. 
#### Challenge  9
```bash
hacker@processes~process-exit-codes:~$ /challenge/get-code
Exiting with an error code!
hacker@processes~process-exit-codes:~$ echo $?
204
hacker@processes~process-exit-codes:~$ /challenge/submit-code 204
CORRECT! Here is your flag:
pwn.college{cpoBU34g6R2-Q2eJ_7fS3bnLi1I.dljN4UDLykTN0czW}
```

