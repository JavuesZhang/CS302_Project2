# 2021S-CS302 Project2 Final Report

> Group members:
>
> - 11812613 香佳宏
> - 11812106 马永煜
> - 11812425 张佳雨
>

## Background

Linux, known as GNU/Linux, is a multi-user, multi-tasking, multi-threaded and multi-CPU based POSIX operating system inspired mainly by Minix and Unix ideas. It can run major Unix tools and software, applications and network protocols. 

Bash is a Unix shell and command language for the GNU Project as a free software replacement for the Bourne shell. It has been used as the default login shell for most Linux distributions. 

This project aims to perform enhancements to the bash. We hope to add or improve some functions of the current bash so that user experience can be improved during using the shell.

## Result Analysis

In the project, we expect to improve `bash` with the following functions:

- Auto suggestions
- Syntax highlighting
- Auto jump
- Split screen

However, during the development process we found that each of them was much more difficult and would take much more time than we thought, so we abandoned the goal of split screen, which is far more complicated than the other three functions. Instead, we found thefuck, a very useful tool that helps automatically repair commands, and we implemented a similar tool, named ohsh*t, by referring to the source code.

So the final goals we achieved are:

- Auto suggestions
- Syntax highlighting
- Auto jump
- ohsh*t

## Implementation

### 1. Auto suggestions



### 2. Syntax highlighting



### 3. Auto jump

Auto jump uses a mix of shell and python. 

Directory tree:

```
.myautojump
├── bin
│   ├── data.txt
│   └── myautojump
└── myautojump.bash
```

`myautojump.bash ` is sourced every time a new terminal is opened, which defines some aliases of option:

```
 - aj_add_to_data()  :  automatically add directory to data file during shell use
 
 - aj()              :  myautojump main function
 - ajc()             :  use myautojump to jump to child directory
 - ajo()             :  use file explorer to open the directory aj() will jump to
 - ajoc()            :  use file explorer to open the directory ajc() will jump to
```

In the functions, if there are any arguments it will run no-output-command, while if there is no arguments it will give an output representing a path you may want to jump to, then the script will change directory on bash or on file explorer as required.

`myautojump` is in python and suffix `.py` is omitted for convenience. All logics are defined here. 

The main logic is about finding paths with abbreviated or incorrect path arguments. You may want to:

```sh
cd ~/OS/Labs/Lab1
```

Unluckily you input one of the following commands but you still want to get the correct outcome:

```sh
cd ~/os/lqbs/lab1
cd ~/OS/labs/lan1
cd os labs lab1
cd o l 1
```

All these can be solved if `~/OS/Labs/Lab1` is in the data. Data is located in `.myautojump/bin/data.txt` and follows the format:

```
/home/username/OS	21
/home/username/CS302	8
/home/username/Videos	8
/home/username/Music	4
/home/username/.myautojump	2
/home/username/.myautojump/bin	0
/home/username/foo/bar	0
```

Each line represents an entry, the first element is a recorded path and the second is its weight, which would be used to evaluted how this path is possible to be chosen. When bash works on a new directory, it will also add the working directory automatically to this data file with the help of shell file.

Also, you can use other arguments to add directories, increase or decrease the weight of current working directory, show current data or version. For more use `myautojump -h` to see how it works.

For details of finding paths, some functions are defined in `myautojump`. There are three functions defined to find paths with different mechanisms:

1. ```
   find_matches()
   ```

   Use regex to perform relatively precise search. For example, if input is `aj o l` and the separator is `/`, the regex would be `o[^/]*/[^/]*l[^/]*$`, which means that `o` and `l` are located in two consecutive directories and `l` is located in the last directory.

2. ```
   find_matches_fuzzy()
   ```

   Use `fuzz` from `fuzzywuzzy` to perform fuzzy search. `fuzzywuzzy` is an easy-to-use fuzzy string matching toolkit. 5K stars on github, it calculates the difference between two sequences based on the Levenshtein Distance algorithm. 

   This function takes the last several directories in each path in data to compare with the query string. For example, if input is `aj myautpjumo bun`, there are two arguments representing path so search in data for whose last two directories is more matched with `myautpjumo/bun`. Obviously, `.myautojump/bin` is matched so `/home/username/.myautojump/bin` is one answer.

3. ```
   find_matches_fuzzy2()
   ```

   Similar to 2. but more relaxed. No number of directories is limited so just compare the paths in data with input path to see whether an path is one answer.

Finally, `myautojump` integrates the results of these three functions and evaluates a best one to output.

### 4. ohsh*t

 ohsh*t uses a mix of shell and python. 

Directory tree:

```
.ohsht
├── bin
│   ├── classes.py
│   ├── ohsht
│   └── rules
│       ├── sl_ls.py
│       ├── sudo.py
|        .
|        .
└── ohsht.bash
```

`ohsht.bash` defines an alias for `ohsht` --> `sht`:

```sh
sht() {
    cmd=$(fc -ln -1)
    out=$(ohsht ${@} "$cmd")
    echo -e "\033[32mDon't worry! Calm down plz. I'll try to fix it for u~\033[0m"
    echo -e "\033[42;37m[Corrected command] $out \033[0m"
    if [[ $out == '' ]]; then
        echo "Sorry, there seems no fix..."
    else
        perl -e 'require "sys/ioctl.ph"; ioctl(STDIN, &TIOCSTI, $_) for split "", join " ", @ARGV' $out
    fi
}
```

Here we use `fc -ln -1` to get the last command run. Then put arguments and the command which needs to be fixed into `sht`. Also, we put the output into the input buffer with the help of **TIOCSTI** so that user will easily run the fixed command by just press `Enter`. **TIOCSTI** is an ioctl, which allows the injection of terminal commands.

In `classes.py`, we defined some classes to do object-oriented programming. 

```python
class Command(object):
    def __init__(self, script, output)
    
class Rule(object):
    def __init__(self, match, get_new_command, priority)
    
class CorrectedCommand(object):
    def __init__(self, script, priority):
```

`Command` represents a command we need to fix. Sometimes its output contains information which help us to fix so output is also an important attribute of this class.

`Rule` represents a rule which is followed when fixing a command. It can be self-defined in `.ohsht/rules`. A simple example is given (`sl_ls.py`):

```python
def match(command):
    return command.script == 'sl'

def get_new_command(command):
    return 'ls'
```

 When typing fast, `ls` may be mistyped as `sl` so this rule is produced. When command matches this rule, a new command fixed from the previous command will be returned.



## Future direction

### 1. Function Expansion

The project we have made is still only implementing some basic functions compared to the original project, and the completeness is far from comparable to the original project. 

For example, within **thefuck**, which is **ohsh*t**'s original body, it provides the ability to use the keyboard to control and view other commands that may be correct after repairing them, and the ui aspect is quite well designed, whereas in our project the repaired commands are simply placed into the input buffer. There are many features like this in the project that enhance the user experience and are worth being studied and learned.

### 2. Platform compatibility

Expect for **Auto suggestions** and **Syntax highlighting**, which are specifically implemented on **zsh** before, other two pluggins are both multi-platform. That means, both are avaliable on multiple platforms such as OS X, Ubuntu, ChromeOS, Windows. A large part of their implementation is to solve the multi-platform compatibility problems, such as path separators, environment variables, etc., and have good robustness to ensure that the program has the same effect on multiple platforms.

Also for these two python based projects, they guarantee performance on both python2 and python3. For example, python2 needs encoding and decoding strings on the communication between program and terminal but python3 does not. Our project only support python3 and Ubuntu currently, not guaranteeing platform compatibility. Thus it is a future direction we need to work on.

### 3. Contributes to source projects 

What we have achieved so far is only a reproduction of the basic functionality of the listed project, which is far from the original project developed by the team, and it is relatively meaningless to reproduce their previous results. It would be more meaningful to fix bugs or add features to their existing projects than to reproduce their previous work, both to improve our own coding skills and to make their projects benefit more programmers.

## Summary

Throughout the project, we made a qualitative leap in our familiarity with the shell language, and it was no longer a difficult task to write programs in the shell language and understand the scenarios and businesses for which the shell language is suitable. In addition, we also gained some understanding of the workflow of zsh and bash, how to add additional operations before and after the command execution, and how to have a series of operations after the command execution, so it became possible to implement plugins on **zsh **and **bash**. Also, I learned some tricks, such as how to change the theme of the terminal and how to change the font color, all these tricks help to improve the user experience and are very useful.

At the same time, even though teamwork is not a new thing, it is a new experience when unfamiliar teammates start doing the same thing together. In addition, this team project is characterized by relatively independent functions, but there are many things that are connected at the bottom, and we all learned a lot from the communication of teammates and improved our ability to write code independently.

## Division of labor

| ID       | Name   | Division                     |
| :------- | :----- | :--------------------------- |
| 11812613 | 香佳宏 | Auto suggestions & ohsh*t    |
| 11812106 | 马永煜 | Syntax highlighting & ohsh*t |
| 11812425 | 张佳雨 | Auto jump & ohsh*t           |

