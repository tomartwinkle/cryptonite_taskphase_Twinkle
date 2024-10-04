# Pondering Paths 
<br>
-Twinkle 
<br><br>

**Thought Process** : So we always have a root node that consists of every directory inside it. ''/''.A path that starts
                      with the root is called an 'absolute path'. Using / we can give paths to files.
                      example: /challenge/run starts from the root / and goes into the challenge directory and inside that is the 
                      run file. The linux command ''cd'' stands for change directory it can be used to enter or exit into directories. 
                      The command = cd and the path can be its argument, example : cd /challenge 
                     Furthermore, there are implicit and explicit relative paths. If we put absolute paths everywhere, it won't matter
                     which directory we are in, but it does matter if we use relative paths. Relative paths are ones that dont start with / 
                     They are relative to our current working directory. Also, ./challenge , /challenge , /.challenge etc. are all identical paths. 
                     Linux does not automatically look in the current directory for programs when you run a command using a "naked" path 
                     (i.e., a command without specifying its location). So , the command "run" would show an error , rather we are to use an explicit path 
                     like ./run . Every one would have a home directory where most of our work is stored ~. Further more if i have to specify a path 
                     using my home directory in pwd college i can simple say /challenge/run ~/flag which would basically open the flag file inside my 
                     home directory . Basically the command run exists in the /challenge directory which is an absolute path and inside that giving the 
                     argument ~/flag ie the path of the flag file in my directory /home/hacker.
                     <br>
                     
**Solving** : This module was mainly about traversing through directoies, using cd command and understanding about implicit and explicit as well as 
              home directories. I solved it in 1/2 tires as it was well explained in the module questions itself. It took my some time to understand 
              at first but i practiced a little and wrote what i grasped by it in the thought process. 

                     

