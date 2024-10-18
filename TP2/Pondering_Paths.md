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

### Solving 

#### Challenge 2
```bash

```
## Thought Process 
** ** : 

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

