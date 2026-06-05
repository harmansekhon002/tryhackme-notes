there are different types of shells in linux 
- the default linux shell is bash
- to see the shell type you type 
```echo$Shell
```

-  You can also list down the available shells in your Linux OS.
- The file `/etc/shells` contains all the installed shells on a Linux system. 
- You can list down the available shells in your Linux OS by typing `cat /etc/shells` in the terminal
- To switch between these shells, you can type the shell name that is present on your OS, and it will open for you, as can be seen below:

Switch Shell

```shell-session
user@tryhackme:~$ zsh
tryhackme% 
```

If you want to permanently change your default shell, you can use the command: `chsh -s /usr/bin/zsh`. This will make this shell as the default shell for your terminal.

## Bourne Again Shell

Bourne Again Shell (Bash) is the default shell for most Linux distributions. When you open the terminal, bash is present for you to enter commands. Before bash, some shells like sh, ksh, and csh had different capabilities. Bash came as an enhanced replacement for these shells, borrowing capabilities from all of them. This means that it has many of the features of these old shells and some of its unique abilities. Some of the key features provided by bash are listed below:

- Bash is a widely used shell with scripting capabilities.
- It offers a tab completion feature, which means if you are in the middle of completing a command, you can press the tab key on your keyboard. It will automatically complete the command based on a possible match or give you multiple suggestions for completing it.
- Bash keeps a history file and logs all of your commands. You can use the up and down arrow keys to use the previous commands without typing them again. You can also type `history` to display all your previous commands.

## Friendly Interactive Shell

Friendly Interactive Shell (Fish) is also not default in most Linux distributions. As its name suggests, it focuses more on user-friendliness than other shells. Some of the key features provided by fish are listed below:

- It offers a very simple syntax, which is feasible for beginner users.
- Unlike bash, it has auto spell correction for the commands you write.
- You can customize the command prompt with some cool themes using fish.
- The syntax highlighting feature of fish colors different parts of a command based on their roles, which can improve the readability of commands. It also helps us to spot errors with their unique colors.
- Fish also provides scripting, tab completion, and command history functionality like the shells mentioned in this task.

## Z Shell

Z Shell (Zsh) is not installed by default in most Linux distributions. It is considered a modern shell that combines the functionalities of some previous shells. Some of the key features provided by zsh are listed below:

- Zsh provides advanced tab completion and is also capable of writing scripts.
- Just like fish, it also provides auto spell correction for the commands.
- It offers extensive customization that may make it slower than other shells.
- It also provides tab completion, command history functionality, and several other features.

|Feature|Bash|Fish|Zsh|
|---|---|---|---|
|Full Name|The full form of Bash is Bourne Again Shell.|The full form of Fish is Friendly Interactive Shell.|The full form of Zsh is Z Shell.|
|Scripting|It offers widely compatible scripting with extensive documentation available.|It has limited scripting features as compared to the other two shells.|It offers an excellent level of scripting, combining the traditional capabilities of Bash shell with some extra features.|
|Tab completion|It has a basic tab completion feature.|It offers advanced tab completion by giving suggestions based on your previous commands.|Its tab completion capability can be extended heavily by using plugins.|
|Customization|Basic level of customization.|It offers some good customization through interactive tools.|Advanced customization through oh-my-zsh framework.|
|User friendliness|It is less user-friendly, but being a traditional and widely used shell, its users are quite familiar and comfortable with it.|It is the most user-friendly shell.|It can be highly user-friendly with proper customization.|
|Syntax highlighting|The syntax highlighting feature is not available in this shell.|The syntax highlighting is built-in to this shell.|The syntax highlighting can be used with this shell by introducing some plugins.|
## shellscripting
- it refers to the act of writing of a set of commands in a script and executing that script for the sake of automation
- first step is the creation of a shell file 
- for bash shell file we use the extension .sh
- Every script should start from shebang. 
- Shebang is a combination of some characters that are added at the beginning of a script, starting with `#!` followed by the name of the interpreter to use while executing the script. 
- As we are writing our script in bash, let’s define it as the interpreter in the shebang.
### an example script
- The script below displays a string on the screen: "Hey, what’s your name?” 
- This is done by `echo` command. 
- The second line of the script contains the code `read name`. `read` is used to take input from the user, and `name` is the variable in which the input would be stored. 
- The last line uses `echo` to display the welcome line for the user, along with its name stored in the variable.

```shell
# Defining the Interpreter 
#!/bin/bash
echo "Hey, what’s your name?"
read name
echo "Welcome, $name"
```
Now, save the script by pressing `CTRL+X`. Confirm by pressing `Y` and then `ENTER`.  
To execute the script, we first need to make sure that the script has execution permissions. To give these permissions to the script, we can type the following command in our terminal:

Execution Permission to Script

```shell-session
user@tryhackme:~$ chmod +x first_script.sh
```

Now that the script has execution permissions use `./` before the script name to execute it. We use `./` before the script to run rather than typing the script name directly because `./` tells the shell to execute the file that is present in the current directory. If you don't define `./` before the script name, the shell will search the script in the PATH environment variable (that contains all the directories except the current one), and it will not find the defined script in any of those directories and generate an error. The below terminal shows the script in which we utilized the variables:

Script Execution

```shell-session
user@ubuntu:~$ ./first_script.sh
Hey, What's your name?
John
Welcome, John
```

loop script

```shell
# Defining the Interpreter 
#!/bin/bash
for i in {1..10};
do
echo $i
done
```

conditional script 

```shell
# Defining the Interpreter 
#!/bin/bash
echo "Please enter your name first:"
read name
if [ "$name" = "Stewart" ]; then
        echo "Welcome Stewart! Here is the secret: THM_Script"
else
        echo "Sorry! You are not authorized to access the secret."
fi
```

comments

```shell
# Defining the Interpreter
#!/bin/bash

# Asking the user to enter a value.
echo "Please enter your name first:"

# Storing the user input value in a variable.
read name

# Checking if the name the user entered is equal to our required name.
if [ "$name" = "Stewart" ]; then

# If it equals the required name, the following line will be displayed.
echo "Welcome Stewart! Here is the secret: THM_Script"

# Defining the sentence to be displayed if the condition fails.
else
        echo "Sorry! You are not authorized to access the secret."
fi
```
