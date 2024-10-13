# Pondering Paths 
<br>
-Twinkle 
<br><br>

## Thought Process 
So we always have a __root node__ that consists of every directory inside it ''/''.A path that starts
                      with the root is called an **'absolute path'**. 
                      Using / we can give paths to files, example:
                      <br><br>
                      /challenge/run 
                      <br><br>
                      starts from the root / and goes into the challenge directory and inside that is the 
                      run file.<br><br> 
                 The linux command "cd"
                 stands for __change directory__ it can be used to enter or exit into directories. 
                      The command = cd and the path can be its argument, example : <br><br>
                      cd /challenge <br><br>
                     Furthermore, there are __implicit and explicit relative paths__.<br> If we put absolute paths everywhere, it won't matter
                     which directory we are in, but it does matter if we use relative paths. Relative paths are ones that dont start with / 
                     They are relative to our current working directory. Also,<br> ./challenge <br> /challenge <br> /.challenge<br> etc. are all identical paths. 
                     Linux does not automatically look in the current directory for programs when you run a command using a "naked" path 
                     (i.e., a command without specifying its location). So , the command "run" would show an error , rather we are to use an explicit path 
                     like ./run . Every one would have a home directory where most of our work is stored ~. Further more if i have to specify a path 
                     using my home directory in pwd college i can simple say <br><br>/challenge/run ~/flag <br><br>which would basically open the flag file inside my 
                     home directory . Basically the command run exists in the /challenge directory which is an absolute path and inside that giving the 
                     argument ~/flag ie the path of the flag file in my directory /home/hacker.
                     <br>
                     

                     
### Solving
This module was mainly about traversing through directoies, using cd command and understanding about implicit and explicit as well as 
              home directories. I solved it in 1/2 tries as it was well explained in the module questions itself. It took my some time to understand 
              at first but i practiced a little and wrote what i grasped by it in the thought process.
#### challenge 1 (The root)
```bash
Connected!
hacker@paths~the-root:~$ /pwn
BOOM!!!
Here is your flag:
pwn.college{c1eqYDMLA2rwuOuvZNOZEHtCsMB.dhzN5QDLykTN0czW}
hacker@paths~the-root:~$
```
#### challenge 2 (Program and absolute paths)
```bash
Connected!
hacker@paths~program-and-absolute-paths:~$ /challenge
ssh-entrypoint: /challenge: Is a directory
hacker@paths~program-and-absolute-paths:~$ /challenge/run
Correct!!!
/challenge/run is an absolute path! Here is your flag:
pwn.college{QnVoYCJo26TwR4bFfeJ888LFXeY.dVDN1QDLykTN0czW}
hacker@paths~program-and-absolute-paths:~$
```
#### challenge 3 (Position Thy self)
```bash
Connected!
hacker@paths~position-thy-self:~$ cd /challenge
hacker@paths~position-thy-self:/challenge$ /challenge/run
Incorrect...
You are not currently in the /usr/share/build-essential directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-thy-self:/challenge$ cd  /usr/share/build-essential
hacker@paths~position-thy-self:/usr/share/build-essential$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{Y_Nk8IHNF9x5h--k09yqVZBOnmm.dZDN1QDLykTN0czW}
```
#### challenge 4 (Position elsewhere)
```bash
hacker@paths~position-elsewhere:/$ cd /challenge
hacker@paths~position-elsewhere:/challenge$ cat /challenge/run
#!/opt/pwn.college/bash

RANDOM=$(cksum /flag  | awk '{print $1}')
CANDIDATES=(
        "/"
        "/tmp"
        "/var"
        "/etc"
        "/sys"
        "/proc/$PPID"
        "/proc/$PPID/fd"
        "/home"
        "/usr/bin"
        "/var/log"
        "/sys/kernel"
        "/etc/apt/sources.list.d"
        "/usr/include"
        "/usr/share/zoneinfo/posix/Asia"
        "/usr/aarch64-linux-gnu/include/gnu"
        "/usr/share/doc"
        "/usr/share/doc/fontconfig"
        "/usr/share/build-essential"
        "/var/lib/apt/lists"
)
NEEDED=${CANDIDATES[ $RANDOM % ${#CANDIDATES[@]} ]}

while [[ ! -d "$NEEDED" ]]
do
        NEEDED=${CANDIDATES[ $RANDOM % ${#CANDIDATES[@]} ]}
done

if [ "$PWD" != "$NEEDED" ]
then
        echo -e "${COLOR_RED}Incorrect...${COLORLESS}"
        echo "You are not currently in the $NEEDED directory."
        echo 'Please use the `cd` utility to change directory appropriately.'
        exit 1
fi

if [ "${0:0:1}" != "/" ]
then
        echo -e "${COLOR_RED}Incorrect...${COLORLESS}"
        echo "You did not call this challenge using an absolute path!"
        echo "An absolute path is anchored at the root of the filesystem, so it starts with /"
        exit 1
fi

echo -e "${COLOR_GREEN}Correct!!!${COLORLESS}"
echo "$0 is an absolute path, invoked from the right directory!"
echo "Here is your flag:"
/bin/cat /flag
hacker@paths~position-elsewhere:/challenge$ cd /
hacker@paths~position-elsewhere:/$ cd /challenge
hacker@paths~position-elsewhere:/challenge$ ls
DESCRIPTION.md  run
hacker@paths~position-elsewhere:/challenge$ /challenge/run
Incorrect...
You are not currently in the /usr/share/zoneinfo/posix/Asia directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-elsewhere:/challenge$ cd /usr/share/zoneinfo/posix/Asia
hacker@paths~position-elsewhere:/usr/share/zoneinfo/posix/Asia$ cat /challenge/run
#!/opt/pwn.college/bash

RANDOM=$(cksum /flag  | awk '{print $1}')
CANDIDATES=(
        "/"
        "/tmp"
        "/var"
        "/etc"
        "/sys"
        "/proc/$PPID"
        "/proc/$PPID/fd"
        "/home"
        "/usr/bin"
        "/var/log"
        "/sys/kernel"
        "/etc/apt/sources.list.d"
        "/usr/include"
        "/usr/share/zoneinfo/posix/Asia"
        "/usr/aarch64-linux-gnu/include/gnu"
        "/usr/share/doc"
        "/usr/share/doc/fontconfig"
        "/usr/share/build-essential"
        "/var/lib/apt/lists"
)
NEEDED=${CANDIDATES[ $RANDOM % ${#CANDIDATES[@]} ]}

while [[ ! -d "$NEEDED" ]]
do
        NEEDED=${CANDIDATES[ $RANDOM % ${#CANDIDATES[@]} ]}
done

if [ "$PWD" != "$NEEDED" ]
then
        echo -e "${COLOR_RED}Incorrect...${COLORLESS}"
        echo "You are not currently in the $NEEDED directory."
        echo 'Please use the `cd` utility to change directory appropriately.'
        exit 1
fi

if [ "${0:0:1}" != "/" ]
then
        echo -e "${COLOR_RED}Incorrect...${COLORLESS}"
        echo "You did not call this challenge using an absolute path!"
        echo "An absolute path is anchored at the root of the filesystem, so it starts with /"
        exit 1
fi

echo -e "${COLOR_GREEN}Correct!!!${COLORLESS}"
echo "$0 is an absolute path, invoked from the right directory!"
echo "Here is your flag:"
/bin/cat /flag
hacker@paths~position-elsewhere:/usr/share/zoneinfo/posix/Asia$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{gWmqRKKRKngRfq0VrTYW08qrlsA.ddDN1QDLykTN0czW}
```
#### challenge 5 (Position yet elsewhere)
```bash
Connected!
hacker@paths~position-yet-elsewhere:~$ cd /challenge
hacker@paths~position-yet-elsewhere:/challenge$ /challenge/run
Incorrect...
You are not currently in the /usr/aarch64-linux-gnu/include/gnu directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-yet-elsewhere:/challenge$ cd /usr/aarch64-linux-gnu/include/gnu
hacker@paths~position-yet-elsewhere:/usr/aarch64-linux-gnu/include/gnu$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
pwn.college{Q0AgJutICg4r9OV4gxVg116tWMC.dhDN1QDLykTN0czW}
```
#### challenge 6 (Implicit relative paths , from /)
```bash
Connected!
hacker@paths~implicit-relative-paths-from-:~$ cd /
hacker@paths~implicit-relative-paths-from-:/$ cat challenge/run
#!/opt/pwn.college/bash

NEEDED="/"

if [ "$PWD" != "$NEEDED" ]
then
        echo -e "${COLOR_RED}Incorrect...${COLORLESS}"
        echo "You are not currently in the $NEEDED directory."
        echo 'Please use the `cd` utility to change directory appropriately.'
        exit 1
fi

if [ "${0:0:1}" == "/" ]
then
        echo -e "${COLOR_RED}Incorrect...${COLORLESS}"
        echo "You invoked this challenge with an absolute path. This challenge needs a relative path!"
        exit 1
fi

if [ "${0:0:1}" == "." ]
then
        echo -e "${COLOR_RED}Incorrect...${COLORLESS}"
        echo "This challenge must be invoked with a relative path that starts with a letter!"
        exit 1
fi

echo -e "${COLOR_GREEN}Correct!!!${COLORLESS}"
echo "$0 is a relative path, invoked from the right directory!"
echo "Here is your flag:"
/bin/cat /flag
hacker@paths~implicit-relative-paths-from-:/$ challenge/run
Correct!!!
challenge/run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{cZj5brjZe1yQWxBqzpJmgQtK2gZ.dlDN1QDLykTN0czW}
```
#### challenge 7 (Explicit relative paths , from /)
```bash
Connected!
hacker@paths~explicit-relative-paths-from-:~$ cd /
hacker@paths~explicit-relative-paths-from-:/$ .challenge/run
ssh-entrypoint: .challenge/run: No such file or directory
hacker@paths~explicit-relative-paths-from-:/$ ./challenge/run
Correct!!!
./challenge/run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{M9berce50zj7Y2_GiXwAcsWune0.dBTN1QDLykTN0czW}
```
#### challenge 8 (Implicit relative paths)
```bash
Connected!
hacker@paths~implicit-relative-path:~$ cd /
hacker@paths~implicit-relative-path:/$ /challenge/run
Incorrect...
You are not currently in the /challenge directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~implicit-relative-path:/$ cd /challenge
hacker@paths~implicit-relative-path:/challenge$ /challenge/run
Incorrect...
You invoked this challenge with an absolute path. This challenge needs a relative path!
hacker@paths~implicit-relative-path:/challenge$ /run
ssh-entrypoint: /run: Is a directory
hacker@paths~implicit-relative-path:/challenge$ ./run
Correct!!!
./run is a relative path, invoked from the right directory!
Here is your flag:
pwn.college{YZcSOoaTAkflqwAoHAPNfosTlNt.dFTN1QDLykTN0czW}
```
#### challenge 9 (Home Sweet Home)
```bash
Connected!
hacker@paths~home-sweet-home:~$ cd /
hacker@paths~home-sweet-home:/$ /challenge/run ~/my_file
The argument you provided must not have been longer than 3 characters (it's
currently 9 characters long)!
hacker@paths~home-sweet-home:/$ /challenge/run ~/c
Writing the file to /home/hacker/c!
... and reading it back to you:
pwn.college{Ez5Xu2gePAnpJOGRHhD-8P0V8fv.dNzM4QDLykTN0czW}
```

                     

