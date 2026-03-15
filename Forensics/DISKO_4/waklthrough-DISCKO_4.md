#### Walkthrough 

### Description

Can you find the flag in this disk image? This time I deleted the file! Let see you get it now! Download the disk image "disko-4.dd.gz".

### Hints

How would you look for deleted files?

- "disko-4.dd.gz" is a zip file so we have to unzip so get the file 

**Run** 

`gunzip disko-4.dd.gz`

- This will unzip the image file now you will see a unziped file "disko-4.dd"

NOW, lets read to know what is inside

`cat disko-4.dd `

you will get a lot of giberiss code so we will use a strings to sww the text

`strings disko-4.dd | grep -i 'pico'`

well you will get a lot of usless text which is no use to us 

> We have a hint saying "How would you look for deleted files?"

We will use the same sleuthkit command

`fls -r -d disk-4.dd`

- `-d` is used to show the deleted file in the img file

you will see something like

r/r * 522629:	log/messages
r/r * 532021:	log/dont-delete.gz

our second zip file look suspicious

`icat disko-4.dd 532021 > mystery.gz `

now,

`ls`
disko-4.dd  mystery.gz
 
Unzip the mystery.gz
`gunzip mystery.gz `

Again,
`ls`


you will see a ziped file "mystery"

`cat mystery`
you will have your flag
---
***Alish***
