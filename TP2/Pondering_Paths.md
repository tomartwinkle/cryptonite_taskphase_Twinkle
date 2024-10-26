# Pondering Paths 

Resource : https://www.youtube.com/watch?v=hk0RwVC6uts
## Thought Process  
**The Path Variable** : How does the shell find ls command ? So there's a special shell variable called PATH it stores directory paths where the shell will <br>
search the programs from corresponding to the commands. If we blank out the path variable we cannot find the command anymore. 

### Solving 
The first attempt i did i got the flag but i basically removed all commands including rm that were in the directory by setting PATH="" <br>
#### Challenge 1
```bash
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
or we can overwrite path without removing the directories that were already there by adding the new directory to the existing PATH variable inside the new PATH 
```bash
hacker@path~setting-path:~$ PATH=$PATH:/challenge/more_commands/
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
Here using the echo command im redirecting 'cat /flag' to the rm command in my home directory and then using chmod making it executable. Then setting a new path variable im updating it so that it will first search for the commands in my home directory rather than the directories where default rm exists. When the /challenge/run program invokes the rm command it will look for the command in the directories in the PATH variable but since i set it that way it will look into the home directory first hence, it will execute the rm command i set which will read cat /flag hence giving the contents of the flag.
#### Challenge 4
```bash
hacker@path~hijacking-commands:~$ echo 'cat /flag' > ~/rm
hacker@path~hijacking-commands:~$ chmod +x ~/rm
hacker@path~hijacking-commands:~$ PATH="$HOME:$PATH" /challenge/run
Trying to remove /flag...
Found 'rm' command at /home/hacker/rm. Executing!
pwn.college{0DKUjJS_4JGvED8wHI30wdmn8tb.ddzNyUDLykTN0czW}
```
I further tried solving this a few times and i think a better way would be to create a script instead like so :
```bash
hacker@path~hijacking-commands:~$ echo '#!/bin/sh\ncat /flag' > ~/rm
hacker@path~hijacking-commands:~$ chmod +x ~/rm
hacker@path~hijacking-commands:~$ PATH="$HOME:$PATH" /challenge/run
Trying to remove /flag...
Found 'rm' command at /home/hacker/rm. Executing!
pwn.college{0DKUjJS_4JGvED8wHI30wdmn8tb.ddzNyUDLykTN0czW}
```
The command echo '#!/bin/sh\ncat /flag' > ~/rm is used to create a shell script in the home directory. #!/bin/sh: This is called a shebang. It tells the system that the file should be executed using the /bin/sh shell, which is essential for making the script executable.
cat /flag: This command, written on the second line, tells the shell to display the contents of the /flag file.


