# File Gobbling 
<br>
-Twinkle 
<br>
<br>

## Thought process
<p align=centre>
                     first we come across "*" which basically replaces the argument with any files that match the pattern for eg:
                     <br><br>
                     touch file_a<br>
                     ls <br>
                     output- file_a <br>
                     echo file_
                      <br><br>
                     output-<br> 
                    file_a*<br>
                     <br><br>
                     "?" argument also works like "*" but it will match just one character . Ex:
                     <br><br>
                     touch file_a<br>
                     touch file_cc<br>
                     ls <br><br>
                     output : 
                     <br>
                     file_a file_cc
                     <br><br>
                     echo file_? <br><br>
                     output : 
                      <br>
                     file_a <br>
                     <br>
                     or if it was this instead :
                     <br><br>
                     echo file_??<br><br>
                     output :
                     <br>
                     file_cc*<br>
                     <br>
                     Furthermore "[ ]" are similar to "?" for a single character but instead of matching any characters they 
                     are a subset of potential characters ex:
                     <br><br>
                     touch file_a<br>
                     touch file_b<br>
                     echo file_[ab]<br><br>
                     output : <br>
                     file_a file_b
                      <br>
                     <br>
                     so gobbling happens on a path basis hence we can even expand whole paths in it.
  <br>
  <br>
  
### Solving
  The "mixing globs" challenge was one in which we have to match files using 6 characters or less. I solved it 
              by using [cep]* 
              basically searching for characters c e and p using "[]" and * matches any sequence of characters that follow. 
<br>
<br>
## Thought Process
"^" helps match all files except the ones we've listed ex:
                       touch file_a
                       touch file_b
                       touch file_c
                       echo file_[^ab]
                       output - file_c
                       we can also use "!" for the same but it has its disadvantagees so less preferable . </p>
#### Challenge 1 (Matching with *)
```bash
hacker@globbing~matching-with-:/challenge$ cd /c*
hacker@globbing~matching-with-:/challenge$ /challenge/run
You ran me with the working directory of /challenge! Here is your flag:
pwn.college{0hlH99M_4EpfJQi5vCEC1w-EKUp.dFjM4QDLykTN0czW}
```
#### Challenge 2 (Matching with ?)
```bash
Connected!
This challenge resets your working directory to /home/hacker unless you change
directory properly...
hacker@globbing~matching-with-:~$ cd /?ha??enge
hacker@globbing~matching-with-:/challenge$ /challenge/run
You ran me with the working directory of /challenge! Here is your flag:
pwn.college{MACLO3y21lU_2g6R3HIh_9AArS-.dJjM4QDLykTN0czW}
```
#### Challenge 3 (Matching with [ ])
```bash
hacker@globbing~matching-with-:/challenge$ cd /challenge/files
hacker@globbing~matching-with-:/challenge/files$ /challenge/run file_[absh]
You got it! Here is your flag!
pwn.college{I3YR3uTU2VKNNOXQgbWqP_RYFB6.dNjM4QDLykTN0czW}
```
#### Challenge 4 (Matching paths with [ ])
```bash
Connected!
hacker@globbing~matching-paths-with-:~$ cd /challenge/files
hacker@globbing~matching-paths-with-:/challenge/files$ /challenge/run/file_[absh]
ssh-entrypoint: /challenge/run/file_[absh]: Not a directory
hacker@globbing~matching-paths-with-:/challenge/files$ /challenge/run ~/file_[absh]
Error: please run with a working directory of /home/hacker!
hacker@globbing~matching-paths-with-:/challenge/files$ cd ~
hacker@globbing~matching-paths-with-:~$ /challenge/run /challenge/files/file_[absh]
You got it! Here is your flag!
pwn.college{8eAssfdlun1UP_rSE-kPaoEQKWc.dRjM4QDLykTN0czW}
```
#### Challenge 5 (Mixing Globs)
```bash
hacker@globbing~mixing-globs:/challenge/files$ cd /challenge/files
hacker@globbing~mixing-globs:/challenge/files$ ls
amazing      delightful   great       jovial    magical     pwning   splendid   victorious  youthful
beautiful    educational  happy       kind      nice        queenly  thrilling  wonderful   zesty
challenging  fantastic    incredible  laughing  optimistic  radiant  uplifting  xenial
hacker@globbing~mixing-globs:/challenge/files$ [cep]*
ssh-entrypoint: challenging: command not found
hacker@globbing~mixing-globs:/challenge/files$ ls [cep]*
challenging  educational  pwning
hacker@globbing~mixing-globs:/challenge/files$ /challenge/run [cep]*
You got it! Here is your flag!
pwn.college{Yg2cI_OGqtLPo6JVrTHE2ReWQHb.dVjM4QDLykTN0czW}
```
#### Challenge 6 (Exclusionary Globbing)
```bash
Connected!
hacker@globbing~exclusionary-globbing:~$ cd /challenge/files
hacker@globbing~exclusionary-globbing:/challenge/files$ /challenge/run [^pwn]
Your expansion did not expand to the requested files (amazing beautiful
challenging delightful educational fantastic great happy incredible jovial kind
laughing magical optimistic queenly radiant splendid thrilling uplifting
victorious xenial youthful zesty).
Instead, it expanded to:
[^pwn]
hacker@globbing~exclusionary-globbing:/challenge/files$ /challenge/run [^pwn]*
You got it! Here is your flag!
pwn.college{YVwut873iLgYQtcaF6Fl6cm4VOj.dZjM4QDLykTN0czW}
```
