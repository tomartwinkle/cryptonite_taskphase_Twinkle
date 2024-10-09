# Comprehending Commands 
<br>
-Twinkle 
<br>
<br>
<p align=centre>
  
## Thought Process
This module taught some linux commands like cat and ls. Cat is a command used to read out files. To use the cat command 
                      we need to provide the argument to it ie the path of the file. It could also be an absolute path. cat will concatenate 
                      multiple files if provided multiple arguments eg: cat my_file flag_file then cat will read both the files provided as its arguments 
                      So if the flag file for example is in an unknown directory, we can find it out and without using the cd command we can cat the absolute path 
                      itself. <br><br>
                      Secondly, the "grep" command. Its useful when the contents are too long to traverse throught so we can just type what we need to find 
                      and grep it.<br><br>
                      Third we learn the listing through files in a directory using "ls" command. In the other modules we're using ls -a also to even list the hidden files. 
                      It will basically list everything present in a directory.<br><br>
                      Fourth , the "touch" command. Its used to create a new blank file in the current working directory eg: 
                      cd /challenge 
                      touch new_file 
                      so i'll have created a new file called new_file in my /challenge directory and i can also verify it by using the ls command. 
                      Fifth, we can remove files using "rm" command.
<br>
<br>
### Solving
The " epic filesystem quest" used a bit of solving using these commands itself. so i first cd into / as asked then used ls and it was tough at first and i got stuck a little 
                even hit the trap once but i figured it out on my own after a few tries. Basically we had to cd and ls to find files that could contain clues but then in between there were files
                that were trapped so we had to read them without cd so i used cat and wrote the absolute path as its argument,**The key here was to use ls command to find the file inside the directory and then read it**. Then there were delayed clues ie: not readable if we dont use cd 
                then there were hidden clues as well so we had to use ls -a and those files start from a "." so to read it we have to use cat .SECRET rather than cat SECRET. 
                it was a fun challenge i learned from the mistakes a lot.
<br>
<br>
## Thought process
(continued) Another command "mkdir" is used to create directories. We can create a directory using mkdir and then use cd to enter that directory and make changes.
                      "find" command is used to find any file by providing selective arguments ie: location or criteria. If we dont specify a location then it will find all those files in the 
                      current working directory(.). We can find inside the entire home directory as well. The format can be like 
                      find -name my_file 
                      so it will find that name in the file my_file. 
<br>
<br>
### Solving
In the "finding files" challenge i got a bit stuck and made an error in the find syntax as well. i was doing find -name "flag" ~ but not only is the syntax wrong but also 
              i should cover all locations not just the home directory so i should use / instead of ~. Finally , it should be 
              find / -name "flag"
              after it finds many flag terms and paths i can hit and trial by using cat for an absolute path to read directly and thats how i found the flag. The hint was the correct usage of syntax for find command and usage of / instead of ~ . 
<br>

## Thought process
Linking of files . There are 2 ways to link ie hard and soft(symbolic) linking. Hard link is basically another name for the same file on the file system its easier to understand but 
                      symbolic links are better to use . Soft links give the file path but not the contents directly so much safer and indirect acess, they are also breakable ie they will not longer be accessable 
                      if the original files are deleted. Syntax is 
                      ln -s /path/original_file /path/new_file 
                      or 
                      ln -s original_file new_file 
                      Also if we use the find command on a new sym linked file it will show that the file is symbollically linked. 
<br>
<br>
### Solving
The last challenge to link files ie: /challenge/flag would run the other file but not give us the flag. We need to trick it by using sym linking. 
             I did ln -s /challenge/catflag ~/new 
              and then run ~/new file 
            basically symbolically linking the contents of /challenge/catflag to a new file ~/new 
            and then running the ~/new file. 
              
  </p>                    

#### Challenge 1 (Cat: not the pet, but the command!)
```bash
hacker@commands~cat-not-the-pet-but-the-command:~$ cat flag
pwn.college{o1Ne9pvG-3h9WUxSrlwoDxfiM-N.dFzN1QDLykTN0czW}
```
#### Challenge 2 (Catting absolute paths)
```bash
hacker@commands~catting-absolute-paths:~$ cat /flag
pwn.college{s1XFKsVbJVZvM_DcHYVcNIEch0h.dlTM5QDLykTN0czW}
```
#### Challenge 3 (More casting practice)
```bash
Connected!
You cannot use the 'cd' command in this level, and must retrieve the flag by
absolute path. Plus, I hid the flag in a different directory! You can find it
in the file /lib/python3.8/curses/flag. Go cat it out **without** cding into
that directory!
hacker@commands~more-catting-practice:~$ cd /lib/python3.8/curses/flag
You used 'cd'! In this level, I don't allow you to change the working directory
--- you MUST chase pass 'cat' the absolute path of where I put it on the
filesystem (which is /lib/python3.8/curses/flag).
hacker@commands~more-catting-practice:~$ cat /lib/python3.8/curses/flag
pwn.college{o8koAZgNTt0HQFLWuhD9k2yPIPV.dBjM5QDLykTN0czW}
```
#### Challenge 4 (Grepping for a needle in a haystack)
```bash
cd /challenge
cat /challenge/data.txt
....(a bunch of data)
hacker@commands~grepping-for-a-needle-in-a-haystack:/challenge$ grep flag /challenge/data.txt
flagellating
flagellums
flagellum's
flagellum
camouflage's
flagon's
flagpole
flagship's
flagella
flagpole's
camouflaging
flagstone's
flagstones
conflagration
flagstone
flagellated
flagpoles
camouflaged
flagellation
conflagration's
flagged
flagstaff
flagrantly
flagons
conflagrations
camouflage
flagellates
persiflage's
camouflages
flagon
flagship
flagellation's
persiflage
flagging
flagellate
flagships
flags
flag
flagrant
unflagging
flagstaff's
flagstaffs
flag's
hacker@commands~grepping-for-a-needle-in-a-haystack:/challenge$ grep pwn.college /challenge/data.txt
pwn.college{wtXFqwGDaqBXrPoFPl7iOOPCenF.ddTM4QDLykTN0czW}
```
#### Challenge 5 (Listing Files)
```bash
Connected!
hacker@commands~listing-files:~$ cd /challenge
hacker@commands~listing-files:/challenge$ ls
10783-renamed-run-25736  DESCRIPTION.md
hacker@commands~listing-files:/challenge$ cat /challenge/10783-renamed-run-25736
#!/opt/pwn.college/bash

echo "Yahaha, you found me! Here is your flag:"
cat /flag
hacker@commands~listing-files:/challenge$ cat /flag
cat: /flag: Permission denied
hacker@commands~listing-files:/challenge$ /challenge/10783-renamed-run-25736
Yahaha, you found me! Here is your flag:
pwn.college{YeOJGCSvrPaRMi-yN6-DorXHBJr.dhjM4QDLykTN0czW}
hacker@commands~listing-files:/challenge$
```
#### Challenge 6 (Touching Files)
```bash
Connected!
hacker@commands~touching-files:~$ cd /challenge
hacker@commands~touching-files:/challenge$ touch /tmp/pwn
hacker@commands~touching-files:/challenge$ touch /tmp/college
hacker@commands~touching-files:/challenge$ ls
DESCRIPTION.md  run
hacker@commands~touching-files:/challenge$ /challenge/run
Success! Here is your flag:
pwn.college{EWf9TIlEeIIavcIpRnSF-PUrasb.dBzM4QDLykTN0czW}
```
#### Challenge 7 (Removing Files)
```bash
Connected!
hacker@commands~removing-files:~$ cd /challenge
hacker@commands~removing-files:/challenge$ rm delete_me
rm: cannot remove 'delete_me': No such file or directory
hacker@commands~removing-files:/challenge$ cd /
hacker@commands~removing-files:/$ rm delete_me
rm: cannot remove 'delete_me': No such file or directory
hacker@commands~removing-files:/$ rm /challenge/delete_me
rm: cannot remove '/challenge/delete_me': No such file or directory
hacker@commands~removing-files:/$ rm ~/delete_me
hacker@commands~removing-files:/$ /challenge/check
Excellent removal. Here is your reward:
pwn.college{M9-mRWZYFLaxu5CKgL5YVG59NDJ.dZTOwUDLykTN0czW}
```
#### Challenge 8 (Hidden Files)
```bash
Connected!
hacker@commands~hidden-files:~$ cd /
hacker@commands~hidden-files:/$ ls -a
.   .dockerenv             bin   challenge  etc   lib    lib64   media  nix  proc  run   srv  tmp  var
..  .flag-291742059813062  boot  dev        home  lib32  libx32  mnt    opt  root  sbin  sys  usr
hacker@commands~hidden-files:/$ cat .flag-291742059813062
pwn.college{076qGxqk0TR4yrQIMaMNwPhOW61.dBTN4QDLykTN0czW}
```
#### Challenge 9 (An Epic Filesystem Quest)
```bash
twinkletomar@LAPTOP-5QO5H0LO:~$ ssh -i ./key hacker@dojo.pwn.college
Connected!
hacker@commands~an-epic-filesystem-quest:~$ cd
hacker@commands~an-epic-filesystem-quest:~$ cd /
hacker@commands~an-epic-filesystem-quest:/$ ls
ALERT  boot       dev  flag  lib    lib64   media  nix  proc  run   srv  tmp  var
bin    challenge  etc  home  lib32  libx32  mnt    opt  root  sbin  sys  usr
hacker@commands~an-epic-filesystem-quest:/$ cat ALERT
Yahaha, you found me!
The next clue is in: /usr/lib/python3/dist-packages/sage/matrix/__pycache__
hacker@commands~an-epic-filesystem-quest:/$ cd  /usr/lib/python3/dist-packages/sage/matrix/__pycache__
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/sage/matrix/__pycache__$ ls
SECRET                           compute_J_ideal.cpython-38.pyc                  matrix_space.cpython-38.pyc
__init__.cpython-38.pyc          docs.cpython-38.pyc                             operation_table.cpython-38.pyc
all.cpython-38.pyc               matrix_integer_dense_hnf.cpython-38.pyc         special.cpython-38.pyc
benchmark.cpython-38.pyc         matrix_integer_dense_saturation.cpython-38.pyc  symplectic_basis.cpython-38.pyc
berlekamp_massey.cpython-38.pyc  matrix_misc.cpython-38.pyc                      tests.cpython-38.pyc
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/sage/matrix/__pycache__$ cat SECRET
Tubular find!
The next clue is in: /opt/capstone/arch
hacker@commands~an-epic-filesystem-quest:/usr/lib/python3/dist-packages/sage/matrix/__pycache__$ cd /opt/capstone/arch
hacker@commands~an-epic-filesystem-quest:/opt/capstone/arch$ ls
AArch64  Alpha      BPF  HPPA       M680X  MOS65XX  PowerPC  SH     SystemZ     TriCore  X86
ARM      BLUEPRINT  EVM  LoongArch  M68K   Mips     RISCV    Sparc  TMS320C64x  WASM     XCore
hacker@commands~an-epic-filesystem-quest:/opt/capstone/arch$ cat BLUEPRINT
Tubular find!
The next clue is in: /usr/local/lib/python3.8/dist-packages/defusedxml

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.
hacker@commands~an-epic-filesystem-quest:/opt/capstone/arch$ cd /usr/local/lib/python3.8/dist-packages/defusedxml
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/defusedxml$ ls
ElementTree.py  __init__.py  cElementTree.py  expatbuilder.py  lxml.py     pulldom.py  xmlrpc.py
WHISPER         __pycache__  common.py        expatreader.py   minidom.py  sax.py
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/defusedxml$ cat WHISPER
Tubular find!
The next clue is in: /opt/busybox/busybox-1.33.2/include/config/feature/install/long

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/defusedxml$ cat /opt/busybox/busybox-1.33.2/include/config/feature/install/long
cat: /opt/busybox/busybox-1.33.2/include/config/feature/install/long: Is a directory
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/defusedxml$ ls /opt/busybox/busybox-1.33.2/include/config/feature/install/long
POINTER-TRAPPED  options.h
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/defusedxml$ cat /opt/busybox/busybox-1.33.2/include/config/feature/install/long/POINTER-TRAPPED
Tubular find!
The next clue is in: /usr/local/lib/python3.8/dist-packages/pip/_vendor/truststore/__pycache__

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/defusedxml$ cd /usr/local/lib/python3.8/dist-packages/pip/_vendor/truststore/__pycache__
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/pip/_vendor/truststore/__pycache__$ ls
TEASER                   _api.cpython-38.pyc    _openssl.cpython-38.pyc        _windows.cpython-38.pyc
__init__.cpython-38.pyc  _macos.cpython-38.pyc  _ssl_constants.cpython-38.pyc
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/pip/_vendor/truststore/__pycache__$ cat
TEASER
Lucky listing!
The next clue is in: /usr/lib/python3/dist-packages/scipy/odr/tests/__pycache__

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/pip/_vendor/truststore/__pycache__$ ls /usr/lib/python3/dist-packages/scipy/odr/tests/__pycache__
INFO-TRAPPED  __init__.cpython-38.pyc  test_odr.cpython-38.pyc
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/pip/_vendor/truststore/__pycache__$ cat
/usr/lib/python3/dist-packages/scipy/odr/tests/__pycache__/INFO-TRAPPED
Lucky listing!
The next clue is in: /usr/share/locale/tl

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/pip/_vendor/truststore/__pycache__$ ls  /usr/share/locale/tl
LC_MESSAGES  MESSAGE-TRAPPED
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/pip/_vendor/truststore/__pycache__$ cat
 /usr/share/locale/tl/MESSAGE-TRAPPED
Great sleuthing!
The next clue is in: /opt/linux/linux-5.4/Documentation/devicetree/bindings/sifive

The next clue is **hidden** --- its filename starts with a '.' character. You'll need to look for it using special options to 'ls'.
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/pip/_vendor/truststore/__pycache__$ cd /opt/linux/linux-5.4/Documentation/devicetree/bindings/sifive
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/Documentation/devicetree/bindings/sifive$ ls -a
.  ..  .SNIPPET  sifive-blocks-ip-versioning.txt
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/Documentation/devicetree/bindings/sifive$ cat SNIPPET
cat: SNIPPET: No such file or directory
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/Documentation/devicetree/bindings/sifive$ cat .SNIPPET
CONGRATULATIONS! Your perserverence has paid off, and you have found the flag!
It is: pwn.college{0kG97eZXeGR2QcvRJp6A6By-rb_.dljM4QDLykTN0czW}
```
#### Challenge 10 (Making directories)
```bash
Connected!
hacker@commands~making-directories:~$ mkdir /tmp/pwn
hacker@commands~making-directories:~$ cd /tmp/pwn
hacker@commands~making-directories:/tmp/pwn$ touch college
hacker@commands~making-directories:/tmp/pwn$ /challenge/run
Success! Here is your flag:
pwn.college{ULoouZumRGoL_6ruK90Ef6hpX0M.dFzM4QDLykTN0czW}
```
#### Challenge 11 (Finding Files)
```bash
Connected!
hacker@commands~finding-files:~$ find -name flag ~
find: paths must precede expression: `/home/hacker'
find: possible unquoted pattern after predicate `-name'?
hacker@commands~finding-files:~$ find ~ -name "flag"
hacker@commands~finding-files:~$ cat flag
cat: flag: No such file or directory
hacker@commands~finding-files:~$ cd /
hacker@commands~finding-files:/$ ls
bin   challenge  etc   lib    lib64   media  nix  proc  run   srv  tmp  var
boot  dev        home  lib32  libx32  mnt    opt  root  sbin  sys  usr
hacker@commands~finding-files:/$ find / -name "flag"
find: ‘/root’: Permission denied
find: ‘/etc/ssl/private’: Permission denied
find: ‘/tmp/tmp.G9qthVCks5’: Permission denied
/usr/local/share/radare2/5.9.5/flag
/usr/local/lib/python3.8/dist-packages/pwnlib/flag
find: ‘/var/cache/apt/archives/partial’: Permission denied
find: ‘/var/cache/ldconfig’: Permission denied
find: ‘/var/cache/private’: Permission denied
find: ‘/var/log/private’: Permission denied
find: ‘/var/log/apache2’: Permission denied
find: ‘/var/log/mysql’: Permission denied
find: ‘/var/lib/apt/lists/partial’: Permission denied
find: ‘/var/lib/mysql-keyring’: Permission denied
find: ‘/var/lib/php/sessions’: Permission denied
find: ‘/var/lib/private’: Permission denied
find: ‘/var/lib/mysql-files’: Permission denied
find: ‘/var/lib/mysql’: Permission denied
find: ‘/run/mysqld’: Permission denied
find: ‘/run/sudo’: Permission denied
find: ‘/proc/tty/driver’: Permission denied
find: ‘/proc/1/task/1/fd’: Permission denied
find: ‘/proc/1/task/1/fdinfo’: Permission denied
find: ‘/proc/1/task/1/ns’: Permission denied
find: ‘/proc/1/fd’: Permission denied
find: ‘/proc/1/map_files’: Permission denied
find: ‘/proc/1/fdinfo’: Permission denied
find: ‘/proc/1/ns’: Permission denied
find: ‘/proc/7/task/7/fd’: Permission denied
find: ‘/proc/7/task/7/fdinfo’: Permission denied
find: ‘/proc/7/task/7/ns’: Permission denied
find: ‘/proc/7/fd’: Permission denied
find: ‘/proc/7/map_files’: Permission denied
find: ‘/proc/7/fdinfo’: Permission denied
find: ‘/proc/7/ns’: Permission denied
/opt/linux/linux-5.4/Documentation/features/perf/flag
/opt/pwndbg/.venv/lib/python3.8/site-packages/pwnlib/flag
/opt/radare2/libr/flag
/nix/store/pmvk2bk4p550w182rjfm529kfqddnvh3-python3.11-pwntools-4.12.0/lib/python3.11/site-packages/pwnlib/flag
/nix/store/1yagn5s8sf7kcs2hkccgf8d0wxlrv5sz-radare2-5.9.0/share/radare2/5.9.0/flag
hacker@commands~finding-files:/$ cat /usr/local/share/radare2/5.9.5/flag
cat: /usr/local/share/radare2/5.9.5/flag: Is a directory
hacker@commands~finding-files:/$ cat ^C
hacker@commands~finding-files:/$ cat /usr/local/lib/python3.8/dist-packages/pwnlib/flag
cat: /usr/local/lib/python3.8/dist-packages/pwnlib/flag: Is a directory
hacker@commands~finding-files:/$ cat /opt/linux/linux-5.4/Documentation/features/perf/flag
pwn.college{4LIAkemy-dX2OllaGhiLnfnkKLR.dJzM4QDLykTN0czW}
```      
#### Challenge 12 (Linking Files)
```bash
Connected!
hacker@commands~linking-files:~$ cd /challenge
hacker@commands~linking-files:/challenge$ ln -s /challenge/catflag ~/new
hacker@commands~linking-files:/challenge$ ~/new
About to read out the /home/hacker/not-the-flag file!
pwn.college{oMjPnSEwGXQMpS_kbeoCZui_pjB.dlTM1UDLykTN0czW}
```
