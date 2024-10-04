# Comprehending Commands 
<br>
-Twinkle 
<br>
<br>
<p align=centre>
  
**Thought Process** : This module taught some linux commands like cat and ls. Cat is a command used to read out files. To use the cat command 
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
**Solving** : The " epic filesystem quest" used a bit of solving using these commands itself. so i first cd into / as asked then used ls and it was tough at first and i got stuck a little 
                even hit the trap once but i figured it out on my own after a few tries. Basically we had to cd and ls to find files that could contain clues but then in between there were files
                that were trapped so we had to read them without cd so i used cat and wrote the absolute path as its argument. Then there were delayed clues ie: not readable if we dont use cd 
                then there were hidden clues as well so we had to use ls -a and those files start from a "." so to read it we have to use cat .SECRET rather than cat SECRET. 
                it was a fun challenge i learned from the mistakes a lot.
<br>
<br>
**Thought process** : (continued) Another command "mkdir" is used to create directories. We can create a directory using mkdir and then use cd to enter that directory and make changes.
                      "find" command is used to find any file by providing selective arguments ie: location or criteria. If we dont specify a location then it will find all those files in the 
                      current working directory(.). We can find inside the entire home directory as well. The format can be like 
                      find -name my_file 
                      so it will find that name in the file my_file. 
<br>
<br>
**Solving** : In the "finding files" challenge i got a bit stuck and made an error in the find syntax as well. i was doing find -name "flag" ~ but not only is the syntax wrong but also 
              i should cover all locations not just the home directory so i should use / instead of ~. Finally , it should be 
              find / -name "flag"
              after it finds many flag terms and paths i can hit and trial by using cat for an absolute path to read directly and thats how i found the flag. For this one i did use AI help thats 
              builtin the pwn college for the find syntax and usage of / instead of ~.
<br>

**Thought process** : Linking of files . There are 2 ways to link ie hard and soft(symbolic) linking. Hard link is basically another name for the same file on the file system its easier to understand but 
                      symbolic links are better to use . Soft links give the file path but not the contents directly so much safer and indirect acess, they are also breakable ie they will not longer be accessable 
                      if the original files are deleted. Syntax is 
                      ln -s /path/original_file /path/new_file 
                      or 
                      ln -s original_file new_file 
                      Also if we use the find command on a new sym linked file it will show that the file is symbollically linked. 
<br>
<br>
**Solving** : The last challenge to link files ie: /challenge/flag would run the other file but not give us the flag. We need to trick it by using sym linking. 
              I got stuck at first as i did ln -s /challenge/catflag ~/new 
              but it didnt work as the file already existed so i had to rm the file and then run the same command 
              after that i used cat to run ~/new but it didnt work and i realised cat will only give me the text of what the file contains but wont give me the actual contents inside the file 
              so , i figured i had to simple run it 
              ~/new 
              and i got the flag. It took me a while to figure stuff out as im new to linux but it made sense overall. 
              
  </p>                    

                      
