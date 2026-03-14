### Description
This file doesn't look like much... just a bunch of 1s and 0s. But maybe it's not just random noise. Can you recover anything meaningful from this? Download the file "digits.bin".
----
### Walkthrough

We hava a file "digits.bin" which if you have downloaded  the file, you will see the file is fill of 1's and 0's write 

If you do `cat digits.bin` you will see the same digits in your terminal 

So, now lets see what kinf of file you will see

`file digits.bin`
- you will see a file is  a ASCII text ,

Now we have two method to solve it 

*** 1st method ***
 
Copy the 'digits.bin' full of (0's AND 1-s) to an **Binary to ASCII Online Convertor** you will get a ASCII converted text 

- if you look  closely you will see a "JFIF with other geberiss code" this tell us that this is a image code so we have to save the decoded ASCII text in a .jpg file and see the flag.

- save the code to "recovered.jpg" 

-  now, run `xdg-open recovered.jpg`

- all window will pop with a image and there you will see a flag.

---

*** 2nd method***

We will convert the bianry text to ASCII with bash scripting and rest is same step we did in 1st method

**Run**

`perl -lape '$_=pack"B*", join "",@F' digit.bin`

you will see a converted ASCII text  in your terminal 

**Explation of a command**

- `perl` : is a powefull,gereral purpose scripting langauage mostly used in a UNIX-like system, inluding linux. It is mostly used in a text procssing ,web dvelopment and sytem administartive dur to its strong regualr expression .

- `e` (Execute): This is the most important one. It tells Perl, "The next thing you see is actual code to run, not a filename."

- `p` (Loop & Print): This wraps your code in a while loop that reads the input file line-by-line, runs your code on each line, and then automatically prints the result.

- `l` (Line-ending): This automatically handles newlines. It "chops" the newline off the input so it doesn't mess up your binary conversion and adds it back to the output if needed.

- `a` (Autosplit): This splits each line into an array called @F. By default, it splits by whitespace. If your digit.bin has spaces between groups of 1s and 0s, this switch catches them. 
    
1. `@F and join("", @F)`

As mentioned, -a puts your 1s and 0s into the "Field" array (@F).

    If your file looks like 0100 0101, @F holds ('0100', '0101').

    join("", @F) glues them back together into one solid string: 01000101. This ensures no spaces break the binary-to-text conversion.

2. `pack("B*", ...)`

This is the "magic" function. pack takes a list of values and packs them into a binary structure.

    "B": This is a template character. It tells Perl: "Interpret the input as a string of bits (1s and 0s), starting with the high-order bit first (big-endian)."

    "*": This means "do this for all the bits you find in the string."

    The Result: It takes 8 bits (e.g., 01000001), turns them into a single byte (65), and converts that to its ASCII character (A).

3. `$_ = ...`

In Perl, $_ is the "default variable."

now `perl -lape '$_=pack"B*", join"",@F' digits.bin  > recoverd.jpg `

*Run*

`xdg-open recoverd.jpg` 

you will get a flag as image will apper in your screen

----
**Alish**
