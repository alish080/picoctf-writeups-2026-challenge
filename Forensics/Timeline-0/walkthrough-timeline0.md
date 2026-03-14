### Description

Can you find the flag in this disk image? Wrap what you find in the picoCTF flag format. Download the disk image "partition4.img.gz".

Hint: Create a Sleuthkit MAC timeline!
 
The Sleuth Kit (TSK) enables MAC (Modified, Accessed, Created) timeline analysis to reconstruct file system events using fls to generate a "body file" and mactime to turn it into a chronological, comma-separated report. It reveals file activities (Modified, Accessed, Changed, Created), essential for forensic investigations into system changes, file deletion, and timestomping. 

### Walkthrough

1. Decompress the image

The file is a zip file so we have to unzip it 

**Run**

`gunzip partition4.img.gz`

Once you have unziped the img file you will get "partition4.img" only 

2. Extract metadata of img file 

Now, we have to create a file (body file) to store the metadata of a files in this img disk image

**Run**

`fls -r -m / partition4.img > body.txt` 

- `-r`- is a recurisve flag,looks at evey directoty from / , if we don't use `-r` then we `fls` only looks at a root directory and only shows the  top-directory and not sub-diretory

- `-m`- /When you run fls -r -m / partition4.img, the -m / tells the tool:

    "Pretend this disk is currently mounted at the root directory (/)."

     If you didn't use -m /, your timeline would just show file names like bcab. By adding -m /, it makes the path look like /bin/bcab. It adds the Path to the metadata so it looks like a real system.

- `body.txt`- This is a file where we sort the medata into.

Now,

Gemini said

This is a perfect way to solidify what you've learned for your documentation. This workflow moves from raw data to evidence discovery to final decoding.

Here is the step-by-step breakdown of your forensic investigation.
Step 1: Decompress the Image

Before any tools can read the disk, you have to "inflate" it back to its original size.
Bash

gunzip partition4.img.gz

    gunzip: The "Gzip" decompression utility.

    No flags needed: By default, it takes the .gz file, extracts the contents, and removes the compressed version to save space.

 2. Extract File Metadata (The "Body" File)

We need to "crawl" the disk and record every file's timestamps into a format Sleuthkit understands.
Bash

fls -r -m / partition4.img > body.txt

    fls: List file and directory names in a file system.

    -r (Recursive): Tells the tool to go inside every folder and sub-folder. Without this, you only see the root level.

    -m / (Mount Point): This provides a "dummy" prefix for the file paths (like /bin/bcab) so the timeline looks like a real Linux system.

    >: A bash redirect that saves the output into body.txt instead of printing it all to your screen.

3. Create the MAC Timeline

Now we turn that raw metadata into a chronological "history book" of the disk.
Bash

`mactime -b body.txt > timeline.csv`

-   ` mactime`: Takes the metadata and sorts it by date and time.

-  `-b` (Body file): Tells the tool which metadata file to read.

-  ` MAC`: Stands for Modified, Accessed, and Changed—the three types of timestamps tracked.

4.  Step 4: Analyze and Identify the Outlier

You searched for dates that didn't make sense.
Bash

`cat timeline.csv | sort -n | head -n 50`

-    `cat`: Reads the file.

-    `sort -n`: Ensures the dates are in numerical order (oldest first).

-    `head -n 50`: Shows only the first 50 lines so you aren't overwhelmed by data.

>    The Discovery: You found the date 1985, which pointed to Inode 4945.

5. : Extract the Hidden Data

Once you have the Inode (the file's "ID number"), you pull the data directly from the disk blocks.
Bash

`icat partition4.img 4945`

-   ` icat`: "Image Cat"—it opens a file based on its Inode number rather than its name. This is useful if a file is deleted or hidden in a system directory.

-   4945: The specific address you found in your timeline analysis.

6. Decoding 

The data is in base64 encoded we must decode it

` icat partition4.img 4945 | base64 -d`
- 71m311n3_0u7113r_h3r_43a2e7af

Now that us your flag,
> Add pic0{ 71m311n3_0u7113r_h3r_43a2e7af}

---
***Alish**
