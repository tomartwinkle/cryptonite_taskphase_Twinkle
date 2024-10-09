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
