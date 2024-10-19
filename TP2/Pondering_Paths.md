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
**Adding Commands** : 

### Solving 

#### Challenge 3
```bash

```
## Thought Process 
** ** : 

### Solving 

#### Challenge 4
```bash

```

