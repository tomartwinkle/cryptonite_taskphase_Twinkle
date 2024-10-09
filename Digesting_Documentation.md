# Digesting Documentation 
<br>
-Twinkle 
<br><br>

## Thought Process
**Reading documentation** <br>
It helps figure out the correct arguments required and get help from the environment. 
                     It guides us through the program. Apart from this, we can access a whole manual using the "man" command and hit "q" 
                     to come out of the man command.
                     We can even search a manual using / and scroll up and down using n and N , we can also use ? to search backwards.
<br>
<br>
### Solving
 **searching manuals** <br>
 To solve, i had to use / and find the argument by which i'll get the flag. So i figured if i just use 
              /A neat argument, but not the right one 
              Then it will find all the arguments leaving one out and i just had to find which one was left out. 
              or by searching for /flag as a keyword. Both ways could work out. 
              Then using that argument as 
              /challenge/challenge --rutc
              i got the flag.
<br>
<br>
## Thought Process
We can also see an advanced use to manuals by searching for manuals using man man. 
                     Some programs dont have man pages but we can invoke help using --help or -h.
                     Then we can also look up builtins if man or help pages arent provided directly.
<br>
<br>
### Solving
 **Help for builtins** <br>
 To solve, we had to run the builtin help that opened a page showing how to proceed further. 
              Inside that the shell builtin of challenge gave the argument --secret but it was incomplete. We had to find the argument for --secret
              using the help challenge ie: opening the challenge builtin where i recieved the arguements. 
            
 #### Challenge 1 (Learning from Documentation)
 ```bash
hacker@man~learning-from-documentation:~$ /challenge/challenge --giveflag
Correct argument! Here is your flag:
pwn.college{QR_oW-34ppymgZFRSSlaPrwyYdq.dRjM5QDLykTN0czW}
```
 #### Challenge 2 (Learning Complex Usage)
 ```bash

```
 #### Challenge 3 (Reading Manuals)
 ```bash
hacker@man~reading-manuals:~$ man challenge

CHALLENGE(1)                                      Challenge Commands                                     CHALLENGE(1)

NAME
       /challenge/challenge - print the flag!

SYNOPSIS
       challenge OPTION

DESCRIPTION
       Output the flag when called with the right arguments.

       --fortune
              read a fortune

       --version
              output version information and exit

       --cbjdxs NUM
              print the flag if NUM is 172

AUTHOR
       Written by Zardus.

REPORTING BUGS
       The repository for this dojo: <https://github.com/pwncollege/linux-luminarium/>

SEE ALSO
       man(1) bash-builtins(7)
hacker@man~reading-manuals:~$ /challenge/challenge --cbjdxs 172
Correct usage! Your flag: pwn.college{cMbjdC17_ZEHMxskEkGhhU-e2Dm.dRTM4QDLykTN0czW}
```
 #### Challenge 4 (Searching Manuals)
 ```bash
hacker@man~searching-manuals:~$ man challenge
(did /flag in the challenge manual page)
hacker@man~searching-manuals:~$ /challenge/challenge --rutc
Initializing...
Correct usage! Your flag: pwn.college{EuJTUlzeiVQLXoCKlmjQAwMQOlF.dVTM4QDLykTN0czW}
```
 #### Challenge 5 (Searching for Manuals)
 ```bash
acker@man~searching-for-manuals:~$ man man
(whole manual page opens)
hacker@man~searching-for-manuals:~$ man -k challenge
lkzfpltelc (1)       - print the flag!
hacker@man~searching-for-manuals:~$ /challenge/challenge  --lkzfpl 109
Correct usage! Your flag: pwn.college{Ulkz10LEfK9plFteRlc4Mz-dBzj.dZTM4QDLykTN0czW}
```
 #### Challenge 6 (Helpful Programs)
 ```bash
twinkletomar@LAPTOP-5QO5H0LO:~$ ssh -i ./key hacker@dojo.pwn.college
Connected!
hacker@man~helpful-programs:~$ help
GNU bash, version 5.2.32(1)-release (x86_64-pc-linux-gnu)
These shell commands are defined internally.  Type `help' to see this list.
Type `help name' to find out more about the function `name'.
Use `info bash' to find out more about the shell in general.
Use `man -k' or `info' to find out more about commands not in this list.

A star (*) next to a name means that the command is disabled.

 job_spec [&]                                                history [-c] [-d offset] [n] or history -anrw [filename]>
 (( expression ))                                            if COMMANDS; then COMMANDS; [ elif COMMANDS; then COMMAN>
 . filename [arguments]                                      jobs [-lnprs] [jobspec ...] or jobs -x command [args]
 :                                                           kill [-s sigspec | -n signum | -sigspec] pid | jobspec .>
 [ arg... ]                                                  let arg [arg ...]
 [[ expression ]]                                            local [option] name[=value] ...
 alias [-p] [name[=value] ... ]                              logout [n]
 bg [job_spec ...]                                           mapfile [-d delim] [-n count] [-O origin] [-s count] [-t>
 bind [-lpsvPSVX] [-m keymap] [-f filename] [-q name] [-u >  popd [-n] [+N | -N]
 break [n]                                                   printf [-v var] format [arguments]
 builtin [shell-builtin [arg ...]]                           pushd [-n] [+N | -N | dir]
 caller [expr]                                               pwd [-LP]
 case WORD in [PATTERN [| PATTERN]...) COMMANDS ;;]... esa>  read [-ers] [-a array] [-d delim] [-i text] [-n nchars] >
 cd [-L|[-P [-e]] [-@]] [dir]                                readarray [-d delim] [-n count] [-O origin] [-s count] [>
 command [-pVv] command [arg ...]                            readonly [-aAf] [name[=value] ...] or readonly -p
 compgen [-abcdefgjksuv] [-o option] [-A action] [-G globp>  return [n]
 complete [-abcdefgjksuv] [-pr] [-DEI] [-o option] [-A act>  select NAME [in WORDS ... ;] do COMMANDS; done
 compopt [-o|+o option] [-DEI] [name ...]                    set [-abefhkmnptuvxBCEHPT] [-o option-name] [--] [-] [ar>
 continue [n]                                                shift [n]
 coproc [NAME] command [redirections]                        shopt [-pqsu] [-o] [optname ...]
 declare [-aAfFgiIlnrtux] [name[=value] ...] or declare -p>  source filename [arguments]
 dirs [-clpv] [+N] [-N]                                      suspend [-f]
 disown [-h] [-ar] [jobspec ... | pid ...]                   test [expr]
 echo [-neE] [arg ...]                                       time [-p] pipeline
 enable [-a] [-dnps] [-f filename] [name ...]                times
 eval [arg ...]                                              trap [-lp] [[arg] signal_spec ...]
 exec [-cl] [-a name] [command [argument ...]] [redirectio>  true
 exit [n]                                                    type [-afptP] name [name ...]
 export [-fn] [name[=value] ...] or export -p                typeset [-aAfFgiIlnrtux] name[=value] ... or typeset -p >
 false                                                       ulimit [-SHabcdefiklmnpqrstuvxPRT] [limit]
 fc [-e ename] [-lnr] [first] [last] or fc -s [pat=rep] [c>  umask [-p] [-S] [mode]
 fg [job_spec]                                               unalias [-a] name [name ...]
 for NAME [in WORDS ... ] ; do COMMANDS; done                unset [-f] [-v] [-n] [name ...]
 for (( exp1; exp2; exp3 )); do COMMANDS; done               until COMMANDS; do COMMANDS-2; done
 function name { COMMANDS ; } or name () { COMMANDS ; }      variables - Names and meanings of some shell variables
 getopts optstring name [arg ...]                            wait [-fn] [-p var] [id ...]
 hash [-lr] [-p pathname] [-dt] [name ...]                   while COMMANDS; do COMMANDS-2; done
 help [-dms] [pattern ...]                                   { COMMANDS ; }
hacker@man~helpful-programs:~$ /challenge/challenge --help
usage: a challenge to make you ask for help [-h] [--fortune] [-v] [-g GIVE_THE_FLAG] [-p]

optional arguments:
  -h, --help            show this help message and exit
  --fortune             read your fortune
  -v, --version         get the version number
  -g GIVE_THE_FLAG, --give-the-flag GIVE_THE_FLAG
                        get the flag, if given the correct value
  -p, --print-value     print the value that will cause the -g option to give you the flag
hacker@man~helpful-programs:~$ /chhallenge/challenge -p
ssh-entrypoint: /chhallenge/challenge: No such file or directory
hacker@man~helpful-programs:~$ /challenge/challenge -g
usage: a challenge to make you ask for help [-h] [--fortune] [-v] [-g GIVE_THE_FLAG] [-p]
a challenge to make you ask for help: error: argument -g/--give-the-flag: expected one argument
hacker@man~helpful-programs:~$ /challenge/challenge -p
The secret value is: 404
hacker@man~helpful-programs:~$ /challenge/challenge -g 404
Correct usage! Your flag: pwn.college{IeroeF4KkG0gzsJ4_Y7fghj1bMg.ddjM4QDLykTN0czW}
```
 #### Challenge 7 (Help for Builtins)
 ```bash
Connected!
hacker@man~help-for-builtins:~$ help
GNU bash, version 5.2.32(1)-release (x86_64-pc-linux-gnu)
These shell commands are defined internally.  Type `help' to see this list.
Type `help name' to find out more about the function `name'.
Use `info bash' to find out more about the shell in general.
Use `man -k' or `info' to find out more about commands not in this list.

A star (*) next to a name means that the command is disabled.

 job_spec [&]                                                history [-c] [-d offset] [n] or history -anrw [filename]>
 (( expression ))                                            if COMMANDS; then COMMANDS; [ elif COMMANDS; then COMMAN>
 . filename [arguments]                                      jobs [-lnprs] [jobspec ...] or jobs -x command [args]
 :                                                           kill [-s sigspec | -n signum | -sigspec] pid | jobspec .>
 [ arg... ]                                                  let arg [arg ...]
 [[ expression ]]                                            local [option] name[=value] ...
 alias [-p] [name[=value] ... ]                              logout [n]
 bg [job_spec ...]                                           mapfile [-d delim] [-n count] [-O origin] [-s count] [-t>
 bind [-lpsvPSVX] [-m keymap] [-f filename] [-q name] [-u >  popd [-n] [+N | -N]
 break [n]                                                   printf [-v var] format [arguments]
 builtin [shell-builtin [arg ...]]                           pushd [-n] [+N | -N | dir]
 caller [expr]                                               pwd [-LP]
 case WORD in [PATTERN [| PATTERN]...) COMMANDS ;;]... esa>  read [-ers] [-a array] [-d delim] [-i text] [-n nchars] >
 cd [-L|[-P [-e]] [-@]] [dir]                                readarray [-d delim] [-n count] [-O origin] [-s count] [>
 challenge [--fortune] [--version] [--secret SECRET]         readonly [-aAf] [name[=value] ...] or readonly -p
 command [-pVv] command [arg ...]                            return [n]
 compgen [-abcdefgjksuv] [-o option] [-A action] [-G globp>  select NAME [in WORDS ... ;] do COMMANDS; done
 complete [-abcdefgjksuv] [-pr] [-DEI] [-o option] [-A act>  set [-abefhkmnptuvxBCEHPT] [-o option-name] [--] [-] [ar>
 compopt [-o|+o option] [-DEI] [name ...]                    shift [n]
 continue [n]                                                shopt [-pqsu] [-o] [optname ...]
 coproc [NAME] command [redirections]                        source filename [arguments]
 declare [-aAfFgiIlnrtux] [name[=value] ...] or declare -p>  suspend [-f]
 dirs [-clpv] [+N] [-N]                                      test [expr]
 disown [-h] [-ar] [jobspec ... | pid ...]                   time [-p] pipeline
 echo [-neE] [arg ...]                                       times
 enable [-a] [-dnps] [-f filename] [name ...]                trap [-lp] [[arg] signal_spec ...]
 eval [arg ...]                                              true
 exec [-cl] [-a name] [command [argument ...]] [redirectio>  type [-afptP] name [name ...]
 exit [n]                                                    typeset [-aAfFgiIlnrtux] name[=value] ... or typeset -p >
 export [-fn] [name[=value] ...] or export -p                ulimit [-SHabcdefiklmnpqrstuvxPRT] [limit]
 false                                                       umask [-p] [-S] [mode]
 fc [-e ename] [-lnr] [first] [last] or fc -s [pat=rep] [c>  unalias [-a] name [name ...]
 fg [job_spec]                                               unset [-f] [-v] [-n] [name ...]
 for NAME [in WORDS ... ] ; do COMMANDS; done                until COMMANDS; do COMMANDS-2; done
 for (( exp1; exp2; exp3 )); do COMMANDS; done               variables - Names and meanings of some shell variables
 function name { COMMANDS ; } or name () { COMMANDS ; }      wait [-fn] [-p var] [id ...]
 getopts optstring name [arg ...]                            while COMMANDS; do COMMANDS-2; done
 hash [-lr] [-p pathname] [-dt] [name ...]                   { COMMANDS ; }
 help [-dms] [pattern ...]
hacker@man~help-for-builtins:~$ challenge
Incorrect usage! Please read the help page for the challenge builtin!
hacker@man~help-for-builtins:~$ cat challenge
cat: challenge: No such file or directory
hacker@man~help-for-builtins:~$ ls
COLLEGE  PWN  c     error_output.txt  instructions  my_file   my_flag  myflag  new_backup  not-the-flag  tee
Desktop  a    echo  f                 leap          my_fille  myfile   new     new_file    pwn           the-flag
hacker@man~help-for-builtins:~$ cat the-flag
TvFbbE2MC7odrlV5BKVnc8J.ddDM5QDLykTN0czW}
                              ^
     that is the second half /|\
                              |

If you only see the second half above, you redirected in *truncate* mode (>)
rather than *append* mode (>>), and so the write of the second half to stdout
overwrote the initial write of the first half directly to the file. Try append
mode!
hacker@man~help-for-builtins:~$ cat myflag
hacker@man~help-for-builtins:~$ cat my_flag
SUPERSECRET:7iqb
hacker@man~help-for-builtins:~$ cat not-the-flag
cat: not-the-flag: Permission denied
hacker@man~help-for-builtins:~$ cat instructions
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : myflag
[INFO] - the challenge will check that error output is redirected to a specific file path : instructions
[INFO] - the challenge will output a reward file if all the tests pass : /flag

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /flag file.

[TEST] You should have redirected my stdout to a file called myflag. Checking...

[PASS] The file at the other end of my stdout looks okay!

[TEST] You should have redirected my stderr to instructions. Checking...

[PASS] The file at the other end of my stderr looks okay!
[PASS] Success! You have satisfied all execution requirements.
hacker@man~help-for-builtins:~$ /challenge/challenge -p
ssh-entrypoint: /challenge/challenge: No such file or directory
hacker@man~help-for-builtins:~$ help challenge
challenge: challenge [--fortune] [--version] [--secret SECRET]
    This builtin command will read you the flag, given the right arguments!

    Options:
      --fortune         display a fortune
      --version         display the version
      --secret VALUE    prints the flag, if VALUE is correct

    You must be sure to provide the right value to --secret. That value
    is "EbzwavPB".
hacker@man~help-for-builtins:~$ challenge --secret EbzwavPB
Correct! Here is your flag!
pwn.college{EbzwavPBLCJHFTSBIFoLdRFJmsb.dRTM5QDLykTN0czW}
```


              
