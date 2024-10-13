# Practicing Piping
-Twinkle
<br><br>
## Thought Process 
Redirecting output can be done using ">" . ex:<br>
                      - echo hello > my_file <br>
                      - cat my_file <br>
                     -  output  : hello<br>
                      <br>
                      Similarly we can redirect the output of any command. Now, there are 2 ways to redirect <br>
                      Truncate (>) and Append (>>) <br>
                      Truncate redirects output by overwriting ie: replacing the contents of the file if itsd non-empty 
                      and creates a new file if it doesnt exist. <br>
                      Append redirects the output but rather than replacing and overwriting it just adds new content to the file 
                      and creates a new file if it doesnt exist. <br>
                      <br>
                      File Descriptor number (FD) describe standard communication channels in linux : <br>
                      FD(0) = standard Input <br>
                      FD(1) = standard Output <br>
                      FD(2) = standard Error <br>
                      <br>
                      so the following lines of code are equivalent as by default the program considers standard output when using ">" : <br>
                      - echo hello > my_file <br>
                      - echo hello 1> my_file <br>
                      <br>
                      Similarly, we can use 2> to redirect stderr and standard input using "<" <br>
                      <br>
### Solving 

**"Redirecting input"**
<br> <br>
              so to put the value of COLLEGE in PWN file we can simply redirect the standard output of the command echo COLLEGE :<br>
             -  echo COLLEGE > PWN <br>
              and then redirect PWN file as the value of standard input that will now read COLLEGE <br>
             -  /challenge/run < PWN <br>
              I got a bit confused about redirecting input while solving and took me 2-3 tries to understand and i made the mistake of writing college instead of COLLEGE as linux is case sensitive.<br>
<br>
## Thought Process 
We can grep for stored results using the grep command . <br>
<br>
### Solving  
**"Grepping for stored results"** <br>
<br>
              I got to the part /challenge/run > /tmp/data.txt and then tried grep "flag" /tmp/data.txt. but that approach wasn't working as it resulted in many words with the term flag in it instead of the actual flag. I took AI's help and realised the correct and faster approach was to search for the link itself like so:<br>
             -  grep "pwn.college" /tmp/data.txt.
              <br>
              
## Thought Process 

Further we can grep for live output as well using the "|" pipe operator. This operator is used to make the program more efficient and faster by cutting the middleman. As in, i wouldn't have to first redirect my standard output to a file and then grep through that file like before, I can simply do this with one line of code like so : <br>
- /challenge/run | grep "pwn.college" 
This time instead of grepping for flag i learnt i should grep for the link itself from the prev challenge. The above code is also the solution to the "Grepping live output challenge" .<br>
<br>
Similarly we can grep for errors as well but there is nothing as 2| the way we do for 2>. Instead to grep errors we follow a 2step process by using ">&" that redirects a file descriptor to another file descriptor. So first we will redirect stderr to stdout and now pipe  the combined stdout, stderr.<br>
<br>

### Solving 

 /challenge/run 2>&1 | grep "pwn.college" 
              I was able to solve this challenge without much problem as the question made it easier to understand. To explain this code basically I redirected stderr of the file /challenge/run to my stdout and then combining the 2 i grep for the link of the flag that starts as "pwn.college". Basically redirecterd FD(2) to FD(0).
              <br>
  <br>
  
## Thought Process 
  
  The "tee" command helps duplicate piped data. It will help see how the data flows through between the commands. Its primary function is to take the output from the previous command, display it on the screen (or pass it to the next command in the pipeline), and simultaneously save it to a file.
  <br> <br>
  
### Solving 
  
  **"duplicating piped data with tee"**<br>
  <br> 
 -  /challenge/pwn --secret YFQG67s7 | tee my_file | /challenge/college 
  <br> This was the solution basically i first read the pwn file using cat command where i got the value of secret argument YFQG67s7
  <br> Then i used tee to duplicate the output of /challenge/pwn --secret YFQG67s7 , save it as well as pass it onto the next command in the pipeline ie: /challenge/college file. Then it will read and display the /challenge/college file which gave the flag. <br>
  Eventually i went wrong in a lot of ways here , at first i didnt understand the need to use tee in this, i googled and found out the need as explained in the above thought process. <br>
  Secondly i was initially trying /challenge/pwn | tee my_file | /challenge/college <br>
  I realised i didnt intercept the output of pwn file and its secret value to reach to the flag, took me a while to figure out i had to read pwn using cat command. <br>
  I even tried /challenge/pwn | tee my_file --secret  YFQG67s7 | /challenge/college <br>
  but i realised the mistake, tee has a different function, it is not supposed to take any secret argument. The /challenge/pwn file should take the secret arguments and run its outputs that the tee command will duplicate in my_file and pass onto /challenge/college.
  <br><br>
  
  ## Thought Process  
  
  Writing to multiple programs, Using process substitution. What is process substitution ? <br>
  It is a shell feature that allows output of a command be used as if it were a file. It can be used if we want to provide a command's output directly to another command.<br>
  >(command) (writes to a command)<br>
<(command) (reads from a command)<br>
Process substitution also allows you to send data into a command as if it were a file. For example, you could redirect the output of a program to be processed by a different command:<br>
command1 > >(command2) <br> 
This runs command1, and instead of writing the output to a file, it passes the output directly to command2 for processing.<br>
<br>

### solving 

**"writing to multiple programs"**
<br> <br>
at first i tried using /challenge/hack >  tee /challenge/the | /challenge/planet
but the problem was that i redirected the output before it gets duplicated. tee command is not used properly here. Then i tried doing<br>
 /challenge/hack | tee /challenge/the | /challenge/planet <br>
but with the help of inbuilt AI i realised the pipe operator is used to send the output of one command to the input of another, but /challenge/the and /challenge/planet are commands, not files that can be written to directly. <br>
Instead i should use process substitution using tee command that will make everything into a file. <br>
The correct solution was :<br><br>
- /challenge/hack | tee >( /challenge/the ) | /challenge/planet
<br><br>
Basically tee is duplicating the output of /challenge/hack and >(/challenge/the) makes it run as a command as if it were reading from a file. Then the /challenge/planet gets the second copy of output via the pipeline, basically tee was used to split the output. <br>
<br>

### solving 
**"Split piping stderr and stdout"**
<br>
<br>
at first i tried <br>
/challenge/hack > /challenge/planet | 2> /challenge/the<br>
It didnt work and i took a lot of time thinking about this challenge.
The problem here was that this code is redirecting stdout to /challenge/planet and stderr to /challenge/the but it is not separating the stdout and stderr which is the aim of the question, this is where process substitution comes in and the correct ans was : 
<br> - /challenge/hack > >(/challenge/planet) 2> >(/challenge/the) <br>
<br>


  #### Additional resources 
  https://www.youtube.com/watch?v=dR0X0-B9ObA

#### Challenge 1 (Redirecting output)
```bash
hacker@piping~redirecting-output:~$ echo PWN > COLLEGE
Correct! You successfully redirected 'PWN' to the file 'COLLEGE'! Here is your
flag:
pwn.college{cn7MF75pvCy_YqGKfiCYsnEDqXA.dRjN1QDLykTN0czW}
```
#### Challenge 2 (Redirecting more output)
```bash
Connected!
hacker@piping~redirecting-more-output:~$ /challenge/run > myflag
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : myflag
[INFO] - the challenge will output a reward file if all the tests pass : /flag

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /flag file.

[TEST] You should have redirected my stdout to a file called myflag. Checking...

[PASS] The file at the other end of my stdout looks okay!
[PASS] Success! You have satisfied all execution requirements.
hacker@piping~redirecting-more-output:~$ cat myflag

[FLAG] Here is your flag:
[FLAG] pwn.college{stmGw6uhjV_rs5laxVOoWB9DyBu.dVjN1QDLykTN0czW}
```
#### Challenge 3 (Appending Output)
```bash
Connected!
hacker@piping~appending-output:~$ /challenge/run >> ~/the-flag
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : /home/hacker/the-flag

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] Good luck!

[TEST] You should have redirected my stdout to a file called /home/hacker/the-flag. Checking...

[HINT] File descriptors are inherited from the parent, unless the FD_CLOEXEC is set by the parent on the file descriptor.
[HINT] For security reasons, some programs, such as python, do this by default in certain cases. Be careful if you are
[HINT] creating and trying to pass in FDs in python.

[PASS] The file at the other end of my stdout looks okay!
[PASS] Success! You have satisfied all execution requirements.
I will write the flag in two parts to the file /home/hacker/the-flag! I'll do
the first write directly to the file, and the second write, I'll do to stdout
(if it's pointing at the file). If you redirect the output in append mode, the
second write will append to (rather than overwrite) the first write, and you'll
get the whole flag!
hacker@piping~appending-output:~$ cat ~/the-flag
 |
\|/ This is the first half:
 v
pwn.college{QZYVTvFbbE2MC7odrlV5BKVnc8J.ddDM5QDLykTN0czW}
                              ^
     that is the second half /|\
                              |

If you only see the second half above, you redirected in *truncate* mode (>)
rather than *append* mode (>>), and so the write of the second half to stdout
overwrote the initial write of the first half directly to the file. Try append
mode!
```
#### Challenge 4 (Redirecting errors)
```bash
Connected!
hacker@piping~redirecting-errors:~$ /challenge/run > myflag 2> instructions
hacker@piping~redirecting-errors:~$ cat myflag

[FLAG] Here is your flag:
[FLAG] pwn.college{kXRGj1X-v9uViTK114h2pYMOh9E.ddjN1QDLykTN0czW}
```
#### Challenge 5 (Redirecting input)
```bash
Connected!
hacker@piping~redirecting-input:~$ /challenge/run > PWN < echo COLLEGE
hacker@piping~redirecting-input:~$ cat PWN
You must redirect a file called PWN into my standard input! You are currently
redirecting echo, which is not PWN.
hacker@piping~redirecting-input:~$ echo COLLEGE < PWN
COLLEGE
hacker@piping~redirecting-input:~$ /challenge/run > PWN
hacker@piping~redirecting-input:~$ cat PWN
You have not redirected anything to my standard input. Please do so, using '<'.
hacker@piping~redirecting-input:~$ /challenge/run > PWN < COLLEGE
hacker@piping~redirecting-input:~$ cat PWN
You must redirect a file called PWN into my standard input! You are currently
redirecting COLLEGE, which is not PWN.
hacker@piping~redirecting-input:~$ echo "COLLEGE" > PWN
hacker@piping~redirecting-input:~$ /challenge/run < PWN
Reading from standard input...
Correct! You have redirected the PWN file into my standard input, and I read
the value 'COLLEGE' out of it!
Here is your flag:
pwn.college{cCI8sDCfeBZMGfznnAqKpZ_I9Es.dBzN1QDLykTN0czW}
```
#### Challenge 6 (Grepping stored results)
```bash
Connected!
hacker@piping~grepping-stored-results:~$ /challenge/run > /tmp/data.txt
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : /tmp/data.txt
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stdout to a file called /tmp/data.txt. Checking...

[HINT] File descriptors are inherited from the parent, unless the FD_CLOEXEC is set by the parent on the file descriptor.
[HINT] For security reasons, some programs, such as python, do this by default in certain cases. Be careful if you are
[HINT] creating and trying to pass in FDs in python.

[PASS] The file at the other end of my stdout looks okay!
[PASS] Success! You have satisfied all execution requirements.
hacker@piping~grepping-stored-results:~$ grep /tmp/data.txt "flag"
grep: flag: No such file or directory
hacker@piping~grepping-stored-results:~$ grep /tmp/data.txt "pwn.college"
grep: pwn.college: No such file or directory
hacker@piping~grepping-stored-results:~$ grep "flag" /tmp/data.txt
[FLAG] Here is your flag:
flags
persiflage's
flagships
flagellum
camouflage
conflagration
flagellum's
camouflaging
flagpoles
camouflage's
unflagging
flagged
conflagrations
flagellate
conflagration's
flagstaff
flagellating
flagon
flagellums
flagellated
flagpole
flagstone's
flagstaffs
flagrant
flagship's
flagellation's
camouflaged
flagons
camouflages
persiflage
flagstone
flagship
flagellates
flagella
flagellation
flagon's
flagpole's
flagstones
flag's
flagging
flagstaff's
flag
flagrantly
hacker@piping~grepping-stored-results:~$ grep "pwn.college" /tmp/data.txt
pwn.college{AjDXDKOZYI_u2qcBF3s0TBNF-__.dhTM4QDLykTN0czW}
```
#### Challenge 7 (Grepping live output)
```bash
hacker@piping~grepping-live-output:~$ /challenge/run
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge checks for a specific process at the other end of stdout : grep
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stdout to another process. Checking...

[FAIL] You did not satisfy all the execution requirements.
[FAIL] Specifically, you must fix the following issue:
[FAIL]   stdout of this process does not appear to be a pipe!
hacker@piping~grepping-live-output:~$ grep "flag" /challenge/run
$DIR/chio.py --check_stdout_pipe grep --reward /challenge/.data.txt --chio_flag_fd 1
hacker@piping~grepping-live-output:~$ gep "pwn.college" /challenge/run
ssh-entrypoint: gep: command not found
hacker@piping~grepping-live-output:~$ /challenge/run | grep "flag"
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge checks for a specific process at the other end of stdout : grep
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stdout to another process. Checking...
[TEST] Performing checks on that process!

[INFO] The process' executable is /nix/store/xpq4yhadyhazkcsggmqd7rsgvxb3kjy4-gnugrep-3.11/bin/grep.
[INFO] This might be different than expected because of symbolic links (for example, from /usr/bin/python to /usr/bin/python3 to /usr/bin/python3.8).
[INFO] To pass the checks, the executable must be grep.

[PASS] You have passed the checks on the process on the other end of my stdout!
[PASS] Success! You have satisfied all execution requirements.
[FLAG] Here is your flag:
flagstaff's
flagstaffs
flagging
camouflage
persiflage
flagon's
flagellation's
persiflage's
flagellates
conflagration
flagellate
flagstone
conflagration's
flagon
flagellated
flagellating
flagellum's
flagons
flag
flagstaff
camouflaging
flagstones
camouflages
camouflaged
flagstone's
flagellation
flagpoles
flagpole's
flagrantly
flagship's
flagellums
flagship
flagpole
unflagging
flagships
flagged
conflagrations
flagrant
flagellum
camouflage's
flags
flag's
flagella
hacker@piping~grepping-live-output:~$ /challenge/run | grep "pwn.college"
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge checks for a specific process at the other end of stdout : grep
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stdout to another process. Checking...
[TEST] Performing checks on that process!

[INFO] The process' executable is /nix/store/xpq4yhadyhazkcsggmqd7rsgvxb3kjy4-gnugrep-3.11/bin/grep.
[INFO] This might be different than expected because of symbolic links (for example, from /usr/bin/python to /usr/bin/python3 to /usr/bin/python3.8).
[INFO] To pass the checks, the executable must be grep.

[PASS] You have passed the checks on the process on the other end of my stdout!
[PASS] Success! You have satisfied all execution requirements.
pwn.college{0SvlvFPyyy_RnLuqmRmoZVJKXGl.dlTM4QDLykTN0czW}
hacker@piping~grepping-live-output:~$

```
#### Challenge 8 (Grepping Errors)
```bash
/challenge/run 2>&1 | grep "pwn.college"
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge checks for a specific process at the other end of stderr : grep
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stderr to another process. Checking...
[TEST] Performing checks on that process!

[INFO] The process' executable is /nix/store/xpq4yhadyhazkcsggmqd7rsgvxb3kjy4-gnugrep-3.11/bin/grep.
[INFO] This might be different than expected because of symbolic links (for example, from /usr/bin/python to /usr/bin/python3 to /usr/bin/python3.8).
[INFO] To pass the checks, the executable must be grep.

[PASS] You have passed the checks on the process on the other end of my stderr!
[PASS] Success! You have satisfied all execution requirements.
pwn.college{0tIFnzdA034GVb1-NUHCQpAATvP.dVDM5QDLykTN0czW}
```
#### Challenge 9 (Duplicating piped data with tee)
```bash
Connected!
hacker@piping~duplicating-piped-data-with-tee:~$ ls PWN
PWN
hacker@piping~duplicating-piped-data-with-tee:~$ cat PWN
COLLEGE
hacker@piping~duplicating-piped-data-with-tee:~$ cat pwn
Usage: /challenge/pwn --secret [SECRET_ARG]

SECRET_ARG should be "YFQG67s7"
hacker@piping~duplicating-piped-data-with-tee:~$ /challenge/pwn --secret YFQG67s7 | tee /challenge/college
Processing...
You are trying to use 'tee' *instead* of /challenge/college. Use it *between*
/challenge/pwn and /challenge/college!
You must pipe the output of /challenge/pwn into /challenge/college (or 'tee'
for debugging).
hacker@piping~duplicating-piped-data-with-tee:~$ /challenge/pwn --secret YFQG67s7 tee /challenge/college
Processing...
You must pipe the output of /challenge/pwn into /challenge/college (or 'tee'
for debugging).
hacker@piping~duplicating-piped-data-with-tee:~$ /challenge/pwn --secret YFQG67s7 | /challenge/college
Processing...
Correct! Passing secret value to /challenge/college...
Great job! Here is your flag:
pwn.college{YFQG67s7iqbxdKcsQQU2qA15-r9.dFjM5QDLykTN0czW}
hacker@piping~duplicating-piped-data-with-tee:~$ ^C
hacker@piping~duplicating-piped-data-with-tee:~$  /challenge/pwn --secret YFQG67s7 | tee my_file | /challenge/college
Processing...
WARNING: you are overwriting file my_file with tee's output...
Correct! Passing secret value to /challenge/college...
Great job! Here is your flag:
pwn.college{YFQG67s7iqbxdKcsQQU2qA15-r9.dFjM5QDLykTN0czW}
```
#### Challenge 10 (Writing to multiple programs)
```bash
Connected!
hacker@piping~writing-to-multiple-programs:~$ /challenge/hack 0>&1 | tee /challenge/planet | tee /challenge/the
WARNING: it looks like you passed the path /challenge/the, instead of the
substituted process, to tee. This will cause tee to try to write to the
/challenge/the file, rather than have the shell launch the /challenge/the
command and redirect tee's output to it.
WARNING: it looks like you passed the path /challenge/planet, instead of the
substituted process, to tee. This will cause tee to try to write to the
/challenge/planet file, rather than have the shell launch the /challenge/planet
command and redirect tee's output to it.
/usr/bin/tee: /challenge/the: Permission denied
/usr/bin/tee: /challenge/planet: Permission denied
This secret data must directly and simultaneously make it to /challenge/the and
/challenge/planet. Don't try to copy-paste it; it changes too fast.
214122959787931463
hacker@piping~writing-to-multiple-programs:~$ /challenge/hack
You must redirect my output into another command!
hacker@piping~writing-to-multiple-programs:~$ /challenge/hack | >(/challenge/planet) | >(/challenge/the)
ssh-entrypoint: /dev/fd/63: Permission denied
ssh-entrypoint: /dev/fd/63: Permission denied
Are you sure you're properly redirecting input into '/challenge/planet'?
Are you sure you're properly redirecting input into '/challenge/the'?
hacker@piping~writing-to-multiple-programs:~$ /challenge/hack | <(/challenge/planet) | <(/challenge/the)
ssh-entrypoint: /dev/fd/63: Permission denied
ssh-entrypoint: /dev/fd/63: Permission denied
Are you sure you're properly redirecting input into '/challenge/the'?
hacker@piping~writing-to-multiple-programs:~$ /challenge/hack | 0>(/challenge/planet) | 0>(/challenge/the)
ssh-entrypoint: 0/dev/fd/63: No such file or directory
ssh-entrypoint: 0/dev/fd/63: No such file or directory
Are you sure you're properly redirecting input into '/challenge/planet'?
Are you sure you're properly redirecting input into '/challenge/the'?
hacker@piping~writing-to-multiple-programs:~$ /challenge/hack | tee >( /challenge/the ) | /challenge/planet
Congratulations, you have duplicated data into the input of two programs! Here
is your flag:
pwn.college{EBsYKh7HFFKhQCUH9DTF9kCCmjl.dBDO0UDLykTN0czW}

```
#### Challenge 11 (split piping stdout and stderr)
```bash
Connected!
hacker@piping~split-piping-stderr-and-stdout:~$ /challenge/hack | >(/challenge/planet) | 2>(/challenge/the)
ssh-entrypoint: /dev/fd/63: Permission denied
ssh-entrypoint: 2/dev/fd/63: No such file or directory
Are you sure you're properly redirecting /challenge/hack's standard output into
'/challenge/planet'?
Are you sure you're properly redirecting /challenge/hack's standard error into
'/challenge/the'?
You must redirect my standard error into '/challenge/the'!
hacker@piping~split-piping-stderr-and-stdout:~$ /challenge/hack 2>(/challenge/the) >(/challenge/planet)
It looks like you passed something like '>(something)' as an *argument* to
/challenge/hack rather than redirecting /challenge/hack's out/error to
'>(something)'. Remember, 'cmd1 >(cmd2)' does *NOT* redirect output of cmd1;
rather, it'll run cmd2, hook a file up to its standard input, and pass that
file as an argument to cmd1. If you want to redirect cmd1's output into that
file, you will need to do: 'cmd1 > >(cmd2)', which is equivalent to 'cmd1 |
cmd2'.
You must redirect my standard output into '/challenge/planet'!
You must redirect my standard error into '/challenge/the'!
Are you sure you're properly redirecting /challenge/hack's standard output into
'/challenge/planet'?
Are you sure you're properly redirecting /challenge/hack's standard error into
'/challenge/the'?
hacker@piping~split-piping-stderr-and-stdout:~$ /challenge/hack 2> > (/challenge/the) > >(/challenge/planet)
ssh-entrypoint: syntax error near unexpected token `>'
hacker@piping~split-piping-stderr-and-stdout:~$ /challenge/hack > >( /challenge/planet ) 2> >( /challenge/the )
Congratulations, you have learned a redirection technique that even experts
struggle with! Here is your flag:
pwn.college{w5wt2xxQ5uSkBnDmxmpPRb_7RBO.dFDNwYDLykTN0czW}
```
