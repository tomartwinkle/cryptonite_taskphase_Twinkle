# Chaining Commands
## Thought Process
**Chaining with Semicolons** : 
; usually works like how we use separate lines to separate commands. <br>
example : <br><br>
echo COLLEGE > PWN<br> 
cat PWN<br>
**CAN ALSO BE WRITTEN AS :** <br>
echo COLLEGE > PWN; cat PWN <br>
### Solving 
To run both /challenge/pwn and /challenge/college by chaining them in one line we can use a semicolon (;) which is the same as entering 2 commands <br>
when we press enter and a prompt occurs just that here there wont be a prompt.
#### Challenge 1
```bash
hacker@chaining~chaining-with-semicolons:~$ /challenge/pwn ; /challenge/college
Yes! You chained /challenge/pwn and /challenge/college! Here is your flag:
pwn.college{glcin_88lq8X1dGjr04Q774jwad.dVTN4QDLykTN0czW}
```
## Thought Process
**Your First Shell Script** : Chaining many commands like this will be not ideal instead we can put these in a file called the shell script and then<br>
execute it to run those commands. By convention, shell scripts end with 'sh' suffix. <br>
Then we can execute it using bash file_name.sh 

### Solving 
So we have to do the same as prev challenge ie: chain 2 commands - running /challenge/pwn and /challenge/college but instead of ; we have to put them in a <br>
shell script and run that. For that, we have to create a file that we can edit so we use nano command and since its a shell script it should have a .sh suffix <br>
then put the commands to run the programs and save it using ctrl x +y and enter then we can run that file. 
#### Challenge 2
```bash
hacker@chaining~your-first-shell-script:~$ nano x.sh
hacker@chaining~your-first-shell-script:~$ bash x.sh
Great job, you've written your first shell script! Here is the flag:
pwn.college{YK5_Eiw1QEkRnWZqAUaX5yulyg0.dFzN4QDLykTN0czW}
```
inside the x.sh shell script : 
```bash
/challenge/pwn
/challenge/college
```
## Thought Process
**Redirecting Script Output** : So if we want to send the output of several programs to one single command we can just put those several programs inside the <br>
shell script like the prev challenge and then redirect its output. Also we consider the script as just another command whose output/input can be redirected.<br>
All the redirection variations work : > , | , 2> , < , >> , 2>> , >& <br>


### Solving 
Here i used the piping method to redirect the output of the shell script that i made using nano script.sh and put in commands to run the /challenge/pwn<br>
and /challenge/college programs into the /challenge/solve file. 
#### Challenge 3
```bash
hacker@chaining~redirecting-script-output:~$ nano script.sh
hacker@chaining~redirecting-script-output:~$ bash script.sh | /challenge/solve
Correct! Here is your flag:
pwn.college{YhmwEeeJ6DTIQlZ96B4PvJhueND.dhTM5QDLykTN0czW}
```
## Thought Process
**Executable Shell Scripts** : Why are we using bash? When we invoke bash with the shell script as argument it tells bash to read commands from the shell script <br>instead of stdin and hence the script is executed. But this need to manually invoke it using bash can be avoided we can just invoke it using the relative<br>
or absolute path 

### Solving 
So to invoke the shell script commands without manually using bash i can simply run it from the relative or absolut path. Hence in this challenge <br>
i used nano z.sh and input the program /challenge/solve in it and saved it. Then to make it executable i used chmod +x z.sh <br>
Then to run it directly without using bash i first tried ~/z.sh as when i cd and ls into ~ the shell script is there but some error occured so i had to use<br>
./z.sh to run it from the current directory and then the file was successfully executed.
#### Challenge 4
```bash
hacker@chaining~executable-shell-scripts:~$ nano z.sh
hacker@chaining~executable-shell-scripts:~$ chmod +x z.sh
hacker@chaining~executable-shell-scripts:~$ ~/z.sh
You must make a shellscript in your home directory that will launch
/challenge/solve, make it executable, and invoke it without explicitly
specifying 'bash'!
hacker@chaining~executable-shell-scripts:~$ ./z.sh
Congratulations on your shell script execution! Your flag:
pwn.college{c_5G0kxw3cWmspNETYpExdvYOR0.dRzNyUDLykTN0czW}
```
