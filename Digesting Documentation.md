# Digesting Documentation 
<br>
-Twinkle 
<br><br>

**Thought Process**: Reading documentation helps figure out the correct arguments required and get help from the environment. 
                     It guides us through the program. Apart from this, we can access a whole manual using the "man" command and hit "q" 
                     to come out of the man command.
                     We can even search a manual using / and scroll up and down using n and N , we can also use ? to search backwards.
<br>
**Solving**: To solve the "searching manuals" i had to use / and find the argument by which ill get the flag. So i figured if i just use 
              /A neat argument, but not the right one 
              Then it will find all the arguments leaving one out and i just had to find which one was left out. 
              or by searching for /flag as a keyword. Both ways could work out. 
              Then using that argument as 
              /challenge/challenge --rutc
              i got the flag.
<br>
**Thought Process**: We can also see an advanced use to manuals by searching for manuals using man man. 
                     Some programs dont have man pages but we can invoke help using --help or -h.
                     Then we can also look up builtins if man or help pages arent provided directly.
<br>
**Solving** : To solve "Help for builtins" basically we had to run the builtin help that opened a page showing how to proceed further. 
              Inside that the shell builtin of challenge gave the argument --secret but it was incomplete. We had to find the argument for --secret
              using the help challenge ie: opening the challenge builtin where i recieved the arguements. 
              help 
              help challenge 
              challenge --secret EbzwavPB
              gave the flag. 
              
              
