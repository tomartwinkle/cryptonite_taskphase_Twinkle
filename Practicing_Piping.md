# Practicing Piping
-Twinkle
<br><br>
**Thought Process** : Redirecting output can be done using ">" . ex:<br>
                      echo hello > my_file <br>
                      cat my_file <br>
                      output  : hello<br>
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
                      echo hello > my_file <br>
                      echo hello 1> my_file <br>
                      <br>
                      Similarly, we can use 2> to redirect stderr and standard input using "<" <br>
                      <br>
**Solving** : "Redirecting input" <br>
              so to put the value of COLLEGE in PWN file we can simply redirect the standard output of the command echo COLLEGE :<br>
              echo COLLEGE > PWN <br>
              and then redirect PWN file as the value of standard input that will now read COLLEGE <br>
              /challenge/run < PWN <br>
              I got a bit confused about redirecting input while solving and took me 2-3 tries to understand and i made the mistake of writing college instead of COLLEGE as linux is case sensitive.<br>
<br>
**Thought Process** : We can grep for stored results using the grep command . <br>
<br>
**Solving** : "Grepping for stored results" <br>
              I got to the part /challenge/run > /tmp/data.txt and then tried grep "flag" /tmp/data.txt. but that approach wasn't working as it resulted in many words with the term flag in it instead of the actual flag. I took AI's help and realised the correct and faster approach was to search for the link itself like so:<br>
              grep "pwn.college" /tmp/data.txt.
              <br>
**Thought Process** : Further we can grep for live output as well using the "|" pipe operator. This operator is used to make the program more efficient and faster by cutting the middleman. As in, i wouldn't have to first redirect my standard output to a file and then grep through that file like before, I can simply do this with one line of code like so : <br>
/challenge/run | grep "pwn.college" 
This time instead of grepping for flag i learnt i should grep for the link itself from the prev challenge. The above code is also the solution to the "Grepping live output challenge" .<br>
<br>
Similarly we can grep for errors as well but there is nothing as 2| the way we do for 2>. Instead to grep errors we follow a 2step process by using ">&" that redirects a file descriptor to another file descriptor. So first we will redirect stderr to stdout and now pipe  the combined stdout, stderr.<br>
<br>
**Solving** : /challenge/run 2>&1 | grep "pwn.college" 
              I was able to solve this challenge without much problem as the question made it easier to understand. To explain this code basically I redirected stderr of the file /challenge/run to my stdout and then combining the 2 i grep for the link of the flag that starts as "pwn.college". Basically redirecterd FD(2) to FD(0).
              <br>
  <br>
  **Thought Process** : The "tee" command helps duplicate piped data. It will help see how the data flows through between the commands. Its primary function is to take the output from the previous command, display it on the screen (or pass it to the next command in the pipeline), and simultaneously save it to a file.
  <br> <br>
  **Solving** : "duplicating piped data with tee"
  <br> 
  /challenge/pwn --secret YFQG67s7 | tee my_file | /challenge/college 
  <br> This was the solution basically i first read the pwn file using cat command where i got the value of secret argument YFQG67s7
  <br> Then i used tee to duplicate the output of /challenge/pwn --secret YFQG67s7 , save it as well as pass it onto the next command in the pipeline ie: /challenge/college file. Then it will read and display the /challenge/college file which gave the flag. <br>
  Eventually i went wrong in a lot of ways here , at first i didnt understand the need to use tee in this, i googled and found out the need as explained in the above thought process. <br>
  Secondly i was initially trying /challenge/pwn | tee my_file | /challenge/college <br>
  I realised i didnt intercept the output of pwn file and its secret value to reach to the flag, took me a while to figure out i had to read pwn using cat command. <br>
  I even tried /challenge/pwn | tee my_file --secret  YFQG67s7 | /challenge/college <br>
  but i realised the mistake, tee has a different function, it is not supposed to take any secret argument. The /challenge/pwn file should take the secret arguments and run its outputs that the tee command will duplicate in my_file and pass onto /challenge/college.
  <br><br>
  **Thought Process**  : Writing to multiple programs, Using process substitution. What is process substitution ? <br>
  It is a shell feature that allows output of a command be used as if it were a file. It can be used if we want to provide a command's output directly to another command.<br>
  >(command) (writes to a command)<br>
<(command) (reads from a command)<br>
Process substitution also allows you to send data into a command as if it were a file. For example, you could redirect the output of a program to be processed by a different command:<br>
command1 > >(command2) <br> 
This runs command1, and instead of writing the output to a file, it passes the output directly to command2 for processing.<br>
<br>
**solving** : "writing to multiple programs" <br>
at first i tried using /challenge/hack >  tee /challenge/the | /challenge/planet
but the problem was that i redirected the output before it gets duplicated. tee command is not used properly here. Then i tried doing<br>
/challenge/hack | tee /challenge/the | /challenge/planet <br>
but with the help of inbuilt AI i realised the pipe operator is used to send the output of one command to the input of another, but /challenge/the and /challenge/planet are commands, not files that can be written to directly. <br>
Instead i should use process substitution using tee command that will make everything into a file. <br>
The correct solution was :<br><br>
/challenge/hack | tee >( /challenge/the ) | /challenge/planet
<br><br>
Basically tee is duplicating the output of /challenge/hack and >(/challenge/the) makes it run as a command as if it were reading from a file. Then the /challenge/planet gets the second copy of output via the pipeline, basically tee was used to split the output. <br>
<br>
**solving** : "Split piping stderr and stdout"<br>
at first i tried <br>
/challenge/hack | /challenge/planet 2> /challenge/the<br>
It didnt work and i took a lot of time thinking about this challenge. 

  
