# Linux Workshop
Mark Hagemann, Oliver Spiro  
October 26, 2016  







## What is Linux
- Just another operating system.
    - Created by Linus Torvalds ("Lin" as in Linus)
    - based on Unix OS ("ux" as in Unix)
- Open source/free
- Widely used on servers
- Has graphical interface, but often used mainly from CLI
    - This workshop will focus on CLI (Bash)


## Why might I need Linux?

- Interacting with computing cluster (e.g., AWS, GHPCC)
- Using software only available for Linux 
- Economy (it's free!)
- Hacker street cred

![](https://i.kinja-img.com/gawker-media/image/upload/s--sKzFn87E--/c_fit,fl_progressive,q_80,w_636/1311302556276352036.jpg)

## Goals of this workshkop

- Get familiar with
    - Linux file structure
    - Command-line basics (Bash terminal)
    
- But we'll only be scratching the surface...

![tux](https://upload.wikimedia.org/wikipedia/commons/a/af/Tux.png)

Section 1 - file structure
========================================================

## Working directory

Log into your AWS instance, and try the command below.

```r
pwd
```
This stands for "Print Working Directory," it gives the name of the folder (directory in Linux jargon) you are currently in.

## What directories exist by default?

- The root directory, known only as /
  - This is where everything on the computer is stored
- /tmp, for temporary files. The contents of this directory is deleted after a reboot!
- /usr, programs installed here, often in subdirectories.


All of these begin with "/", because they are subdirectories of root.

Unless you are installing something, you won't have to worry about these.

## Home folder

Your **home folder** is where most of your work is done. In here, you can create files, edit and execute them without Linux complaining.

Just like **/** is the shorthand for root, **~** means your home directory.

## Navigating

You can move around between these directories using the **cd** (as in "change directory") command. Try this:


```r
cd /usr/bin
```

The argument for this command "/usr/bin" is called a **path**. Paths are the location directories or files on the computer. This means we are in **bin** inside **/usr**.
To see what's in here use this command:

```r
ls
```
You will see a list of all files and subdirectories in /usr/bin.

## Go home 

To navigate back to your home folder, you can do 

```r
cd ~
```

but just `cd` with nothing after it works too.

**~** is a special nickname for the home directory. Other special nicknames include 

- **..** for the parent directory
- **.** for the current directory
- **-** for the previous directory

Section 2: file manipulation
===============

## Topics covered

- Making files
- moving, renaming, removing
- permissions

## wget

The `wget` command pulls a file from a server (like on the internet)


```r
wget
```

The following URL links to a csv file with US Geological Survey parameter codes: http://bit.ly/2eA7j2p

- To download it to your current working directory, use `wget http://bit.ly/2eA7j2p`
- run `ls` to see if that worked.

## File permissions

Files can be *read*, *written*, and/or *executed*. Not every user can do all these things to every file. 

To see the permissions for a file, type `ls -l [file name]`.


```bash
ls -l
```

The `-l` is an option given to the `ls` command, telling it to list files in *long* format, i.e. with more information. These options (flags) are associated with many different commands and options.

## Changing file permissions

The **chmod** function sets permissions for a specified file.

- There are 3 permissions (read, write, execute)
   -denoted by r, w, x
-There are three categories that can have permissions: user, group, other
   -"user" is the files owner, "group" refers to other members of a specific group of users, and "other" is anyone else.

To set these permissions, you can simply run the command *chmod <permissions> <file>* where <file> the specified file, and <permissions> is a pattern like the below:

-To give the file's owner execute permission, **chmod u+x <file>**
-To remove write access for people in the category "other", **chmod o-x <file>**
-To give full permissions to everyone on the system, **chmod ugo+rwx <file>**

## Changing file permissions, continued

The previous slide showed one way to set file permissions. Here is another.

- Permissions can be encoded as 3-digit binary converted to integer, for example:
    - (*read* and *write* and *execute*) = (111) = 7
    - (*not read* and *write* and *not execute*) =  (010) = 2
- To set the 3 permissions for the 3 user categories smash the 3 permission integers together into one 3-digit integer. For example:
    - `chmod 777 [filename]` gives everyone read, write, and execute permissions
    - `chmod 744 [filename]` gives the "user" read, write, and execute permissions, but everyone else ("group" and "other") can only read.


## Making files--3 ways

1. You can make an empty file using the `touch` command. For example, `touch foo.csv` would make an empty file with the name "foo.csv"

2. You can also create a file and fill it at the same time using the `>` operator `echo hi there > hi.txt` creates a file called `hi.txt` with the text "hi there"

    - `>` is called a **redirect**. It redirects output of a function to a file. 

3. Use a built-in command-line text editor. **vi** is a popular one, but it's not easy to use. **nano** is much easier. `nano foo.csv` allows you to edit the file "foo.csv".

## Task

- Create a file called changeprompt.sh
- change the permissions to make it executable
- Using `nano`, put the following text in the file:


```r
#! /bin/bash
export OLDPS=$PS1
export PS1="GRiD_"$OLDPS
```

- Execute the file by running `./changeprompt.sh`

- Your command prompt should now have "GRiD_" at the beginning.

- Change it back by running `export PS1=$OLDPS`

## Other file operations

- **cp** copy a file:   `cp [file name] [copy name]`
- **mv** move/rename a file:   `mv [file name] [new name]`
- **rm** remove a file: ` rm [file name]`
- **mkdir** make a new directory: ` mkdir [directory name]`

## Task

Rename the file you downloaded "usgsCodes.csv"

Section 3: Probing files, folders, and contents
=======

## Useful functions

- **find** to look for files with a particular name 
- **grep** to look for text that matches a pattern
- **head** to print first few lines of a file
- **tail** same, for last few lines
- **cat** to print the entire file contents

- Look at the first few lines of the usgsCodes.csv file.

## The pipe operator

You'll often need to chain commands together, "piping" the output of one into another as input. This is accomplished using the vertical bar symbol, **|**

- look for the string "needle" in the file "haystack.txt"
    - `cat haystack.txt | grep needle`
- sort all occurrences of the string "needle" alphabetically by the first character in the line
    - `cat haystack.txt | grep needle | sort`

## Task

Find all the lines in the file **usgsCodes.csv** that contain the text "Trihalo". What are their associated codes (column 1)?

Section 4: Packages and processes
========


## Running a program

We can run Python from the command line, since it comes preinstalled with our AWS instance. 


```r
python
```

should bring up a new and different command prompt. You are now in a python console and your bash commands won't work. But Python code will.


```r
print 'hello!'
3 + 4 * 6
```

`quit()` will return you to the bash terminal.

## The `sudo` (super-user do) command

Linux doesn't let just anybody play around with its guts. Only someone with root access, a "super user" can do things like install new packages or delete unwanted ones.

- Playing around with Linux guts is dangerous and can harm your machine.
- But you can run a command as a super user by prepending it with `sudo`. 


```r
touch /this # doesn't work
sudo touch /this # works
```


## Getting a new package using apt-get

Linux programs (*packages*) can be obtained using a package management tool. For Ubuntu (the linux distribution you're using), the package manager is called **apt**. The two most important commands are **apt-get install** and **apt-get update**. 

- Try obtaining R using `apt-get install r-base`. Does it work? How can you make it work?



## Terminating/killing runaway programs

- `top` to see top resource-using processes
- `ps -aux` to see all running processes
    - pipe into `grep` to get process ID of the process you want to kill
- `kill [process ID]` will kill the process with a specified ID

## Process termination exercise

1. `wget` the following file: http://bit.ly/2dXDYfy
2. `mv` it to have the name "blabber.sh"
3. `chmod` permissions so you can execute it.
4. Run it using `./blabber.sh &` (note the ampersand)

## Process termination exercise

You should now have a file called "blah.txt". Monitor its contents with `tail -f blah.txt`.

- Your runaway process, blabber.sh, is writing text in an infinite loop!
- Kill it! KILL IT!!!!
- Is it dead?

## Learning more

Function documentation 

- Manual pages, `man [function]`
    - often verbose
- help flag, often `-h` or `--help`
    - e.g. `python -h`
- Google, stackexchange

## Other utilities and things we like

<!--html_preserve--><div id="htmlwidget-b5b1fdba266d6efdc681" style="width:100%;height:300px;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-b5b1fdba266d6efdc681">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11"],["scp","rsync","more/less","ln","du","df","~/.profile","environment variables","export","alias","PATH"],["file transfer, e.g. to/from server","more intelligent file tranfer/syncing","preview text file. Less is a bit better than more.","hard and soft (symbolic) links","disk usage - how much space a directory is using","how much free space is left on your file system","(file in home folder) customizes your shell experience","handy variables you can access from anywhere","create an environment variable","Give a command a nickname you can use to call it in the future","special environment variable telling linux where to look for executable files"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>command.thing\u003c/th>\n      <th>what.it.does\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"scrollY":300,"paging":false,"order":[],"autoWidth":false,"orderClasses":false,"columnDefs":[{"orderable":false,"targets":0}]}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

