### Description
Can you find the flag in this disk image? Download the disk image "disk.img.gz".

### Walkthrough

The file is in ziped file we have to unzip the file first,

`gunzip disk.img.gz`

Once unziped you will have a unziped file "disk.img"

Now,

`cat disk.img`
When you try to read the file using a cat command you will get gibberish code.

So Now we will try to list out the files and directory using a fls command

`fls -FD disk.img`
You will get a line saying cannot determine file system type

> It means "disk.img" contains a full disk image contaning a partion table(like a real hard drive), rather than a single partition , fls is confused as it is looking at a very beginning of the Maste Boot Recorder , rather then the actual file system where the acutal file lives

### Step by Step Walkthrough

1. To check disk layout 

**Run**

`mmls disk.img`
- `mmls` is a command used to check the disk layout, in this case partition table .

you will see a partition table like,

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000616447   0000614400   Linux (0x83)
003:  000:001   0000616448   0001140735   0000524288   Linux Swap / Solaris x86 (0x82)
004:  000:002   0001140736   0002097151   0000956416   Linux (0x83)

Here, you can see a  two linux partition table in number 2 and 4 number 3 is say which is basically a dummy so it is usless to us.


2. File System Exploration

We know the partition starting point, we have to look inside without mounting the drive 

***Run**

`fls -o 2048 -r -F disk.img`

- `-o` : jumps to the correct starting point (leaves the boot config file)
- `-r` : Recursive (look inside every folder staring from root directory)
- `-F` : Shows the file inside the img file

> Here we found only the system config file, which was usless for us

Now, check the second partation

**Run**

`fls -o 1140736 -r -FD disk.img`

We get thousand of files we will specifically search for directoy d/d.

d/d 64770:	home
+ d/d 64771:	ctf-player
++ d/d 65663:	Code
+++ d/d 65664:	secrets
++++ d/d 65665:	.git

These are the hint that we are moving to right direction.
The "secrect"  is a give away and .git is the golden egg

3. Data Extraction

We are given a hint that "extract the directory from the disk img"

We are gona extract to out local machine

**Run**

`tsk_recover -e -r -o 1140736 disk.img ./recovered_files `

Now the second partition is recovered in folder "recoverd_files"

4. Now go to 

`/recovered_files/home/ctf-player/Code/secrets$ `

and run `ls`

- you will get a "note.txt"

`cat note.txt`

You will get a "The picoCTF flag format is 'picoCTF{}' where there is some leetspeak phrase in between the curly braces
" which is not the flag but just a txt to bait us.

now,

5. Git Forensis

We saw a '.git' directory in out second partition  so we are gona find a flag from there because .git lets us see all the commit that was made bt developer

**Run**

`git log --oneline --all --graph`

`--online` : simplified output so we can see message clearly

`--all`: shows all the commit at evey branch

`--graph` : shows if there are diffferent branch

you will get a flag but you will have to wrap it in picoCTF{}

> Flag picoCTFg17_1n_7h3_d15k_041217d8"
---
***Alish***
