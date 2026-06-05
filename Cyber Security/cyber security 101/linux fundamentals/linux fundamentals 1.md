# intro

- the name linux is actually an umbrella term for multiple os that are UNIX 
- linux is an open source os that can customised in any shape or form
- For example, Ubuntu & Debian are some of the more commonplace distributions of Linux because it is so extensible. 
- I.e. you can run Ubuntu as a server (such as websites & web applications) or as a fully-fledged desktop.
## Terminal
- echo can be used to print words on the display 
- we can use head to show the outputs from the starting
- we can use -n 20 to limit the number of outputs
- a single word can be printed as is but for multiple words we need to use ""
- whoami to see who you are logged in as
- ls to list the files or dir or folders
- cat to view a files contents
- cd to change the dir
- ls <folder name> to view the content of the folder
-  cat<full path> to view content of a file without navigating to the path
- pwd to know the path of the current working dir
- find to locate the files of whom we only know the name of 
- find -name *.fileextension "*" to find all the files with that extension
- grep can be used to search a file for specific entries or words
- for ex lets say we need to find the ip 8.8.8.8 from a total of 350 entries we use the command
```
grep "8.8.8.8" filename
```
- grep -R can be used to search for content in the current dir and its subdirs and its available files

## operators 
At an overview, I'm going to be showcasing the following operators:

|   |   |
|---|---|
|Symbol / Operator|Description|
|&|This operator allows you to run commands in the background of your terminal.|
|&&|This operator allows you to combine multiple commands together in one line of your terminal.|
|>|This operator is a redirector - meaning that we can take the output from a command (such as using cat to output a file) and direct it elsewhere.|
|>>|This operator does the same function of the `>` operator but appends the output rather than replacing (meaning nothing is overwritten).|

Let's cover these in a bit more detail.

## Operator "&"

This operator allows us to execute commands in the background. For example, let's say we want to copy a large file. This will obviously take quite a long time and will leave us unable to do anything else until the file successfully copies.

The "&" shell operator allows us to execute a command and have it run in the background (such as this file copy) allowing us to do other things!

## Operator "&&"

This shell operator is a bit misleading in the sense of how familiar is to its partner "&". Unlike the "&" operator, we can use "&&" to make a list of commands to run for example `command1 && command2`. However, it's worth noting that `command2` will only run if `command1` was successful.

##   
Operator ">"

This operator is what's known as an output redirector. What this essentially means is that we take the output from a command we run and send that output to somewhere else.

A great example of this is redirecting the output of the `echo` command that we learned in Task 4. Of course, running something such as `echo howdy` will return "howdy" back to our terminal — that isn't super useful. What we can do instead, is redirect "howdy" to something such as a new file!

Let's say we wanted to create a file named "welcome" with the message "hey". We can run `echo hey > welcome` where we want the file created with the contents "hey" like so:

Using the > Operator

```shell-session
tryhackme@linux1:~$ echo hey > welcome
```

Using cat to output the "welcome" file

```shell-session
tryhackme@linux1:~$ cat welcome
hey
```

_Note: If the file i.e. "welcome" already exists, the contents will be overwritten!_

## Operator ">>"

This operator is also an output redirector like in the previous operator (`>`) we discussed. However, what makes this operator different is that rather than overwriting any contents within a file, for example, it instead just puts the output at the end.

Following on with our previous example where we have the file "welcome" that has the contents of "hey". If were to use echo to add "hello" to the file using the `>` operator, the file will now only have "hello" and not "hey".

The `>>` operator allows to append the output to the bottom of the file — rather than replacing the contents like so:

Using the >> Operator

```shell-session
tryhackme@linux1:~$ echo hello >> welcome
```

Using cat to output the "welcome" file

```shell-session
tryhackme@linux1:~$ cat welcome
hey
hello
```

