# Shell Variables
## Thought Process 
**Printing variables** : we can simply use the echo command with a argument of a variable starting with $ sign <br>
and it will print out the value of that variable in the shell.In shell terms, the prepending of $ triggers <br>
what is called variable expansion, and is, surprisingly, the source of many potential vulnerabilities<br>
### Solving 
So for the first challenge i did as instructed, used the echo command to print the value of the variable FLAG <br>
using the $ sign with the variable and got the flag.
#### Challenge 1
```bash
hacker@variables~printing-variables:~$ echo $FLAG
pwn.college{ooCBUpFzdBnHsHo-Y3XdXd5rklP.ddTN1QDLykTN0czW}
```
## Thought Process 
**setting variables** : So challenge 1 taught us to access variables and that it required using the $ sign but ,<br>
to set values to variables we dont require the prepending of $ sign (variable expansion) instead, we just use " = " sign without <br>
any spaces. If we use space in between the value we want to set and = sign then bash will just read it as a command rather then variable setting <br>
and might even show an error if the command doesnt exist. 

### Solving 
Here we had to assign the value of COLLEGE to the PWN variable , later we can even use echo $PWN to read the variable and will get the output of COLLEGE
<br> But basically we had to use the " = " sign without any spaces as when the shell sees a space it ends variable assignment.<br>
Also take care of the case sensitivity in linux.
#### Challenge 2
```bash
hacker@variables~setting-variables:~$ PWN=COLLEGE
You've set the PWN variable properly! As promised, here is the flag:
pwn.college{8vRshA0cVIghW6gflJEUG2KVjeR.dlTN1QDLykTN0czW}
```
## Thought Process 
**Multi Word Variables** : Now as mentioned earlies, if we use space then shell will end the variable assignment and it wont work the way we want it to <br>
So, instead if we want to assign more than one value to a variable or we need a space somewhere in the value we can put them inside quotation marks <br>
and shell will read it as a single value.
### Solving 
To assign the value of COLLEGE YEAH to the PWN variable we cant simply say PWN=COLLEGE YEAH instead we have to put this inside { "" } as shown :
#### Challenge 3
```bash
hacker@variables~multi-word-variables:~$ PWN="COLLEGE YEAH"
You've set the PWN variable properly! As promised, here is the flag:
pwn.college{0UHUrmSYqDsuscNGxUISZLjJjtL.dBjN1QDLykTN0czW}
```
## Thought Process
**Exporting variables** : 
### Solving 

#### Challenge 4
```bash

```
#### Challenge 5
```bash

```
#### Challenge 6
```bash

```
#### Challenge 7
```bash

```
#### Challenge 8
```bash

```
