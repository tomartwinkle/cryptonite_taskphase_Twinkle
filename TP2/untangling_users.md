# Untangling Users 
hacker:x:1000:1000:Hacker User:/home/hacker:/bin/bash <br>
hacker: Username<br>
x: Placeholder for password (actually stored in /etc/shadow)<br>
1000: User ID (UID)<br>
1000: Group ID (GID)<br>
Hacker User: User details (can be empty)<br>
/home/hacker: Home directory<br>
/bin/bash: Default shell<br>
Passwords used to be stored in /etc/passwd, but this was insecure because the file needs to be readable by all users. Now, actual passwords are encrypted and <br>stored in /etc/shadow, which is readable only by root and highly privileged users. This adds an extra layer of protection.<br>
## Thought Process 
**Becoming Root with su** : su is an old command that is the 'switch user' command , its not used to elevate to root access anymore. <br>
su is a setuid binary this means that it has the SUID bit set, allowing it to run with the privileges of its owner (typically root), <br>
regardless of which user executes it.<br>
Because it has the SUID bit set, su runs as root. Running as root, it can start a root shell but before allowing the user to elevate privileges to root, it checks to make sure that the user knows the root password.This is also a reason why su is not used anymore there are other modern authentication methods <br>
that dont ask for passwords.

### Solving :
Here, normally su isnt used anymore and neither are root passwords but this challenge has one. We just had to invoke the su command and then enter the pass to <br>
get the root access so that we can read the flag file using cat /flag due to our root user abilities.
#### Challenge 1
```bash
hacker@users~becoming-root-with-su:~$ su
Password:
root@users~becoming-root-with-su:/home/hacker# cat /flag
pwn.college{c0we1iOJCLsxDKl0-jz2fTmnUrc.dVTN0UDLykTN0czW}
```
## Thought Process 
**Other Users with su** : Without any argument, su will start a root shell and if given the right password it will elevate to root user privilages and access but<br> if we give some user's username as its argument it will run that user's shell instead of root shell basically **it will swtich to that user**. 

### Solving :
Here i basically used the usernmae 'zardus' as argument for su command hence swtiching to zardus user and entering the password to be able to run the <br>
/challenge/run program as zardus.
#### Challenge 2
```bash
hacker@users~other-users-with-su:~$ su zardus
Password:
zardus@users~other-users-with-su:/home/hacker$ /challenge/run
Congratulations, you have become Zardus! Here is your flag:
pwn.college{8Q9LsFCJlInysrYhrkdWUgtQ_mj.dZTN0UDLykTN0czW}
```
## Thought Process 
**Cracking Passwords** : When we enter a password for the su command, it is compared from the stored password and these passwords were stored <br>
in the /etc/password before but that is a globally readable file which is not safe enough so it got changed to /etc/shadow which encrypts the pass ,br>
and is not readble by everyone only by root or users with elevated privilages can read it, making it safer.<br>
The structure of /etc/shadow is similar to /etc/passwd, but the second field is used for storing hashed passwords. Fields are separated by colons (:).<br>
The first field of each line is the username and the second is the password. A value of * or ! functionally means that password login for the account <br>
is disabled, a blank field means that there is no password and example : <br>
<br>
```bash
zardus:$6$bEFkpM0w/6J0n979$47ksu/JE5QK6hSeB7mmuvJyY05wVypMhMMnEPTIddNUb5R9KXgNTYRTm75VOu1oRLGLbAql3ylkVa5ExuPov1.:19921:0:99999:7:::
```
username - zardus <br>
password - (hashed password) long string of characters as shown and it is the result of one-way-encrypting (hashing) <br> 
The other fields are related to password aging, expiration, and other account-specific settings. <br>
When you input a password into su, it one-way-encrypts (hashes) it and compares the result against the stored value. <br>
If the result matches, su grants you access to the user.<br>
If an attacker manages to get a copy of /etc/shadow (or a backup), they can attempt to crack the hashed passwords.<br>

### Solving :
At first i read the /challenge/shawdow-leak file but i realised that got me hashed (one way encrypted) passwords that can directly be used as password for <br>
the su authentication. Instead, I needed to use the john program to attempt to crack the hashed password by guessing what the original password could be. Once I used john to crack the hash, it provided the original password,got it and put it as the password for su command so i can get user access <br>
and all permissions as zardus to run the /challenge/run file to get the flag.
#### Challenge 3
```bash
hacker@users~cracking-passwords:~$ cat /challenge/shadow-leak
root:*:19873:0:99999:7:::
daemon:*:19873:0:99999:7:::
bin:*:19873:0:99999:7:::
sys:*:19873:0:99999:7:::
sync:*:19873:0:99999:7:::
games:*:19873:0:99999:7:::
man:*:19873:0:99999:7:::
lp:*:19873:0:99999:7:::
mail:*:19873:0:99999:7:::
news:*:19873:0:99999:7:::
uucp:*:19873:0:99999:7:::
proxy:*:19873:0:99999:7:::
www-data:*:19873:0:99999:7:::
backup:*:19873:0:99999:7:::
list:*:19873:0:99999:7:::
irc:*:19873:0:99999:7:::
gnats:*:19873:0:99999:7:::
nobody:*:19873:0:99999:7:::
_apt:*:19873:0:99999:7:::
hacker::20000:0:99999:7:::
zardus:$6$e1axNmRMJpNxlWpC$xIh3P90kuWuNyvOcDA23ehO6X1xj8q2NOZ0i..ihEb/p7LFtK/vCa.nmAJ.NLdiffOSBUUFQl/W2EKDUgf94t1:20014:0:99999:7:::
hacker@users~cracking-passwords:~$ su zardus
Password:
su: Authentication failure
hacker@users~cracking-passwords:~$ john ./challenge/shadow-leak
Created directory: /home/hacker/.john
stat: ./challenge/shadow-leak: No such file or directory
hacker@users~cracking-passwords:~$ john /challenge/shadow-leak
Loaded 1 password hash (crypt, generic crypt(3) [?/64])
Press 'q' or Ctrl-C to abort, almost any other key for status
0g 0:00:00:12 0% 2/3 0g/s 281.2p/s 281.2c/s 281.2C/s brenda..keith
0g 0:00:00:17 1% 2/3 0g/s 282.8p/s 282.8c/s 282.8C/s 1234qwer..babygirl
0g 0:00:00:19 1% 2/3 0g/s 283.5p/s 283.5c/s 283.5C/s ncc1701d..1022
aardvark         (zardus)
1g 0:00:00:20 100% 2/3 0.04873g/s 283.7p/s 283.7c/s 283.7C/s Johnson..buzz
Use the "--show" option to display all of the cracked passwords reliably
Session completed
hacker@users~cracking-passwords:~$ su zardus
Password:
zardus@users~cracking-passwords:/home/hacker$ /challenge/run
Congratulations, you have become Zardus! Here is your flag:
pwn.college{wBgIfqjyF1XZofPkAr-G78i1-ue.ddTN0UDLykTN0czW}
```
## Thought Process 
**Using sudo** : Since there are many issues with using the su command it got outdated and replaced by the sudo command (superuser do). <br>
Unlike su, which defaults to launching a shell as a specified user, sudo defaults to running a command as root. <br>
sudo: Rather than logging in as root, sudo allows you to run a specific command as root, without needing to know the root password.<br>
Instead, sudo checks a policy file (usually /etc/sudoers) to determine if a user is authorized to run commands as root.<br>
```bash
whoami 
(output - hacker)
sudo whoami
(output - root)
```

### Solving :
simply using sudo i can get root privilages to run a command and using that i ran the cat /flag command to read the flag getting root privilages.
#### Challenge 4
```bash
hacker@users~using-sudo:~$ sudo cat /flag
pwn.college{YJthcKFHS7uFwbs4JrWwuIl9Qay.dhTN0UDLykTN0czW}
```

