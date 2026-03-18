### Description

Can you find the flag in this disk image? Download the disk image here.

### Hints

How can you checkout the files of a previous commit?

### Wlakthrough 

All the process is same from our "forensic-git-0" which is in this repo 

> Please know that "recoverd_files" are important which we recoverd from partation  from the "forensic-git-0" 

As, from the hint we kanow that we have to look for a file or flag in a previous commit,

> We know that we have .git which is in thr img file on our recoverd_files

Now **Run**

`git log --online --all`

We are checeking what activites happend which we can see through log 

you will see something like this,

5fb8194 (HEAD -> master) Remove flag
177789a Add flag

This is the timeline of the flag life and death

The commit 177789a is where the flag was added, and 5fb8194 is where it was deleted. Because you are currently at HEAD (the most recent version), the folder looks empty because the "Remove flag" action is the current reality.

To check the content what is inside of add flag 

**Run**
`git 177789a show`

you will see the content and the flag too
----
***Alish***

