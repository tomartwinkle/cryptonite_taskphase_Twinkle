# Shell Variables
## Thought Process 
**Printing variables** : we can simply use the echo command with a argument of a variable starting with $ sign <br>
and it will print out the value of that variable in the shell.In shell terms, the prepending of $ triggers <br>
what is called variable expansion, and is, surprisingly, the source of many potential vulnerabilities<br>
### Solving 
So for the first challenge i did as instructed, used the echo command to print the value of the variable FLAG <br>
using the $ sign with the variable and got the flag.
#### Challenge 1
```bash
hacker@variables~printing-variables:~$ echo $FLAG
pwn.college{ooCBUpFzdBnHsHo-Y3XdXd5rklP.ddTN1QDLykTN0czW}
```
## Thought Process 
**setting variables** : So challenge 1 taught us to access variables and that it required using the $ sign but ,<br>
to set values to variables we dont require the prepending of $ sign (variable expansion) instead, we just use " = " sign without <br>
any spaces. If we use space in between the value we want to set and = sign then bash will just read it as a command rather then variable setting <br>
and might even show an error if the command doesnt exist. 

### Solving 
Here we had to assign the value of COLLEGE to the PWN variable , later we can even use echo $PWN to read the variable and will get the output of COLLEGE
<br> But basically we had to use the " = " sign without any spaces as when the shell sees a space it ends variable assignment.<br>
Also take care of the case sensitivity in linux.
#### Challenge 2
```bash
hacker@variables~setting-variables:~$ PWN=COLLEGE
You've set the PWN variable properly! As promised, here is the flag:
pwn.college{8vRshA0cVIghW6gflJEUG2KVjeR.dlTN1QDLykTN0czW}
```
## Thought Process 
**Multi Word Variables** : Now as mentioned earlies, if we use space then shell will end the variable assignment and it wont work the way we want it to <br>
So, instead if we want to assign more than one value to a variable or we need a space somewhere in the value we can put them inside quotation marks <br>
and shell will read it as a single value.
### Solving 
To assign the value of COLLEGE YEAH to the PWN variable we cant simply say PWN=COLLEGE YEAH instead we have to put this inside { "" } as shown :
#### Challenge 3
```bash
hacker@variables~multi-word-variables:~$ PWN="COLLEGE YEAH"
You've set the PWN variable properly! As promised, here is the flag:
pwn.college{0UHUrmSYqDsuscNGxUISZLjJjtL.dBjN1QDLykTN0czW}
```
## Thought Process
**Exporting variables** : So when we set a variable in a shell it will exist and be accessible to only that shell and other commands or programs wont be <br>
able to access that variable hence, shell variables are local to that shell process. Ex: <br>
VAR=137<br>
echo Var is: $VAR<br>
<br>
output : var is : 137
<br>
I set VAR=137, and it exists in that shell.<br>
sh<br>
$ echo "VAR is: $VAR" <br>
VAR is: <br>
If I open a new shell (a child process), it doesn’t know about VAR because variables aren’t automatically shared between shells.<br>
So , we need to export the variables .  When we export our variables, they are passed into the environment variables of child processes.
<br>(Environment variables are special variables that are passed down to programs or processes that you start from your shell. These variables contain information that both the system and programs can use.)<br>
<br>
VAR=137<br>
export VAR<br>
( we can also do : export VAR=137)<br>
sh<br>
echo var is: $VAR<br>
<br>
output - var is: 137<br>
Now since we exported the variable VAR so it is now passed on in the environment variables of the shell , we can now access its value.
### Solving 
So, we had to export the PWN value to the shell and also input its value as COLLEGE and set the value of COLLEGE as PWN without exporting.
#### Challenge 4
```bash
hacker@variables~exporting-variables:~$ COLLEGE=PWN
You've set the COLLEGE variable to the proper value!
hacker@variables~exporting-variables:~$ export PWN=COLLEGE
You've set the PWN variable to the proper value!
You've set the COLLEGE variable to the proper value!
hacker@variables~exporting-variables:~$ /challenge/run
CORRECT!
You have exported PWN=COLLEGE and set, but not exported, COLLEGE=PWN. Great
job! Here is your flag:
pwn.college{4pFq0yzTLQlpX46by7jMKnz1fWe.dJjN1QDLykTN0czW}
You've set the PWN variable to the proper value!
You've set the COLLEGE variable to the proper value!
```
## Thought Process 
**Printing Exported Variables** : We can access variables in plenty of ways in the shell like echo command as before,<br>
another command is "env" that will print all the exported variables set in the shell.
### Solving 
Using the env command to access all exported variables in the environment of the shell i can find where the flag is. 
#### Challenge 5
```bash
hacker@variables~printing-exported-variables:~$ env
SHELL=/run/dojo/bin/bash
HOSTNAME=variables~printing-exported-variables
PWD=/home/hacker
DOJO_AUTH_TOKEN=101854ec25ef973e265ed1468a6a274cf77364e89c93583ac29087b4726f7fd0
HOME=/home/hacker
LANG=C.UTF-8
LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=00:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.7z=01;31:*.ace=01;31:*.alz=01;31:*.apk=01;31:*.arc=01;31:*.arj=01;31:*.bz=01;31:*.bz2=01;31:*.cab=01;31:*.cpio=01;31:*.crate=01;31:*.deb=01;31:*.drpm=01;31:*.dwm=01;31:*.dz=01;31:*.ear=01;31:*.egg=01;31:*.esd=01;31:*.gz=01;31:*.jar=01;31:*.lha=01;31:*.lrz=01;31:*.lz=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.lzo=01;31:*.pyz=01;31:*.rar=01;31:*.rpm=01;31:*.rz=01;31:*.sar=01;31:*.swm=01;31:*.t7z=01;31:*.tar=01;31:*.taz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tgz=01;31:*.tlz=01;31:*.txz=01;31:*.tz=01;31:*.tzo=01;31:*.tzst=01;31:*.udeb=01;31:*.war=01;31:*.whl=01;31:*.wim=01;31:*.xz=01;31:*.z=01;31:*.zip=01;31:*.zoo=01;31:*.zst=01;31:*.avif=01;35:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.webp=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:*~=00;90:*#=00;90:*.bak=00;90:*.crdownload=00;90:*.dpkg-dist=00;90:*.dpkg-new=00;90:*.dpkg-old=00;90:*.dpkg-tmp=00;90:*.old=00;90:*.orig=00;90:*.part=00;90:*.rej=00;90:*.rpmnew=00;90:*.rpmorig=00;90:*.rpmsave=00;90:*.swp=00;90:*.tmp=00;90:*.ucf-dist=00;90:*.ucf-new=00;90:*.ucf-old=00;90:
FLAG=pwn.college{w9bOOJhdnL1w63uhBHUmkuYy6LN.dhTN1QDLykTN0czW}
LESSCLOSE=/usr/bin/lesspipe %s %s
TERM=xterm
LESSOPEN=| /usr/bin/lesspipe %s
SHLVL=1
LC_CTYPE=C.UTF-8
PATH=/run/challenge/bin:/run/workspace/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
_=/run/workspace/bin/env
```
## Thought Process 
**Storing command output** : Using command substitution we can store the output of any command into a variable. <br>
FLAG=$(cat /flag)<br>
echo "$FLAG"<br>
<br>
output - pwn.college{...}
<br>

### Solving 
We had to store the output of reading /challenge/run into the PWN variable using command substitution and then<br>
read out the PWN variable using echo command.
#### Challenge 6
```bash
hacker@variables~storing-command-output:~$ PWN=$(/challenge/run)
Congratulations! You have read the flag into the PWN variable. Now print it out
and submit it!
hacker@variables~storing-command-output:~$ echo $PWN
pwn.college{YFqq9c-Fsd-rH2kwQ42ej09YtLt.dVzN0UDLykTN0czW}
hacker@variables~storing-command-output:~$
```
## Thought Process
**Reading Input** : The read builtin command reads input or file from the user.It assigns input to one or more variables.<br>
The -p argument lets us specify a prompt .So, the -p option allows you to provide a prompt message before taking input. <br>
This way, instead of just waiting for input with no context, the user will see a message.
### Solving 
Here we had to read and input the value of COLLEGE into the PWN variable , we can do it using the read command and i additionally used <br>
a prompt aswell using the -p argument.
#### Challenge 7
```bash
hacker@variables~reading-input:~$ read -p "Input a value to PWN variable " PWN
Input a value to PWN variable COLLEGE
You've set the PWN variable properly! As promised, here is the flag:
pwn.college{otTjpxcvlK3kczPhfVaSBsRPMQL.dhzN1QDLykTN0czW}
```
## Thought Process 
**Reading Files** : Now if want a user to read a file in the environment variables we can combine the redirection and read process and commands like so:<br>
echo "test" > some_file    (Redirecting the output of the string test into the file "some_file)<br>
read VAR < some_file       (Redirecting the contents of the file some_file as input to VAR and then reading it)<br>
echo $VAR                  (using echo to read the value of VAR variable)<br>
<br>
output - test
### Solving 
Since the value of /challenge/read_me will keep changing we need to read it in one line so we can simply redirect the standard input of /challenge/read_me file <br>into the PWN variable and read it.
#### Challenge 8
```bash
hacker@variables~reading-files:~$ read PWN < /challenge/read_me
You've set the PWN variable properly! As promised, here is the flag:
pwn.college{YsA6i70X0Ko6nftNA5HQ1m-d8Gw.dBjM4QDLykTN0czW}
```
