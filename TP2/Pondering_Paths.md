# Pondering Paths 

## Thought Process 
**The Path Variable** : How does the shell find ls command ? So there's a special shell variable called PATH it stores directory paths where the shell will <br>
search the programs from corresponding to the commands. If we blank out the path variable we cannot find the command anymore. 

### Solving 
The first attempt i did i got the flag but i basically removed all commands including rm that were in the directory by setting PATH="" <br>
#### Challenge 1
```bash
hacker@path~the-path-variable:~$ $rm
hacker@path~the-path-variable:~$ PATH=""
hacker@path~the-path-variable:~$ rm
ssh-entrypoint: rm: No such file or directory
hacker@path~the-path-variable:~$ /challenge/run
Trying to remove /flag...
/challenge/run: line 4: rm: No such file or directory
The flag is still there! I might as well give it to you!
pwn.college{ctAWrkbfm-lMe7ntzc0eCu-Wub8.dZzNwUDLykTN0czW}
```
## Thought Process 
**Setting PATH** :  
To add a directory to your PATH so that you can run scripts or programs from that directory without specifying the full path, you can modify the PATH variable.<br>


### Solving 
Since the /challenge/run command doesn't have 'win' in it we need to overwrite the path to the directory where the 'win' program is and then launch it using <br>
/challenge/run again.Basically the win command that /challenge/run executed was stored in /challenge/more_commands.
#### Challenge 2
```bash
hacker@path~setting-path:~$ PATH="/challenge/more_commands/"
hacker@path~setting-path:~$ /challenge/run
Invoking 'win'....
Congratulations! You properly set the flag and 'win' has launched!
pwn.college{AfQ1UBZwR0gbfQMzLp2l8zHovP8.dVzNyUDLykTN0czW}
```
## Thought Process 
**Adding Commands** : We can add our own scripts and commands in the path variables.
### Solving 
This challenge took me a while as the hint confused me into using an absolute path for cat commnand inside the script shell and i put the path wrong aswell,<br>
the solution was to first create a shell script using nano command called win.sh and inside that put the commands cat /flag to read the flag when win is executed.<br> Then we need to give executing permissions to win.sh as well which is inside our ~ directory using chmod command and adding +x argument. <br>
Then overwriting the path variable to the home hacker directory so that when we run /challenge/run the win program is invoked which in turn will read the flag file contents.
#### Challenge 3
```bash
hacker@path~adding-commands:~$ nano win.sh
hacker@path~adding-commands:~$ chmod +x ~/win.sh
hacker@path~adding-commands:~$ export PATH="$PATH:/home/hacker/"
hacker@path~adding-commands:~$ /challenge/run
Invoking 'win'....
pwn.college{o5WB2f0rhKm6sq5w2ggfv_1QmsP.dZzNyUDLykTN0czW}
```
## Thought Process 
**Hijacking Commands** 

### Solving 
This was tough to solve, it took me some time and initially i tried many ways involving creating a shell script using nano for a fake rm command etc. <br>
I took some friend's help discussed and found a solution. First created a file named rm using touch in the current directory. Then added the command cat /flag<br>
to the rm script using echo "cat /flag" > rm, which means that when rm is executed, it will instead output the contents of /flag. Then modified the path to <br>
make sure that the system will first search for the rm command in the home directory rather than the system directories which is why the rm i created will be <br>
executed and hence read the flag rather than the defualt system rm command which would have delete the flag instead. 
#### Challenge 4
```bash
hacker@path~hijacking-commands:~$ touch rm
hacker@path~hijacking-commands:~$ echo "cat /flag" > rm
hacker@path~hijacking-commands:~$ PATH="$HOME:$PATH"
hacker@path~hijacking-commands:~$ /challenge/run
Trying to remove /flag...
Found 'rm' command at /home/hacker/rm. Executing!
pwn.college{0DKUjJS_4JGvED8wHI30wdmn8tb.ddzNyUDLykTN0czW}
```

