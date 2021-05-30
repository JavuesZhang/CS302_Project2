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
- Auto Doit

However, during the development process we found that each of them was much more difficult and would take much more time than we thought, so we abandoned the goal of split screen, which is far more complicated than the other three functions. Instead, we found thefuck, a very useful tool that helps automatically repair commands, and we implemented a similar tool, named ohsh*t, by referring to the source code.

So the final goals we achieved are:

- Auto suggestions
- Syntax highlighting
- Auto jump
- ohsh*t
- Auto Doit

## Implementation

### 1. Auto suggestions
1. Feature 1: check list history CMDs and list 50 suggestion commands

   First I get the user input command by `${COMP_LINE}` and we replace the " "(whitespace) with `blank="\\\\\\ "` for escape. Then remove the header command  `as `  from the curr command.

   With the help of `complegen` , we input the history 1000 lines into it by command `fc -nl -1000` and get the suggestions back. If the suggestion list is too much, we just thin this list.

   Finally return the suggestions to the complete method for user to <TAB>

```shell
os@ubuntu:/data/auto-suggestion$ as c
c                                        cd\ /data/auto-suggestion/
cat\ \ ~/.inputrc\                       cd\ /root/
cat\ ~/.inputrc\                         cd\ -r\ os-proj\ /data/auto-suggestion/
cat\ /root/.inputrc                      chmod\ 777\ /usr/bin/as
cd\ ..                                   complete
cd\ ~                                    complete\ |\ grep\ as
cd\ auto-suggestion/                     
os@ubuntu:/data/auto-suggestion$ as cat\ 
cat\ \ ~/.inputrc\   cat\ ~/.inputrc\     cat\ /root/.inputrc
```

2. Feature 2: give auto-suggestions when user typing:

   When user press <TAB>, we get the interrupt here and the args list are convert into the string, in this means we can auto-compelte the user input from history.

```shell
os@ubuntu:/data/auto-suggestion$ as s
s                                              source\ ./install.sh\ 
source\ ad                                     sudco\ cat\ /root/.inputrc
source\ ad\                                    sudo\ 
source\ ~/.bash                                sudo\ bash\ ./install.sh\                            
os@ubuntu:/data/auto-suggestion$ as sud
sudco\ cat\ /root/.inputrc                     sudo\ cd\ /root/
sudo\                                          sudo\ chmod\ 777\ /usr/bin/as
sudo\ bash\ ./install.sh\                      sudo\ cp\ /data/auto-suggestion/as\ /usr/bin/
os@ubuntu:/data/auto-suggestion$ as sudo\ 
```

3. Feature 3:  exec the command that auto-suggestion

   In this feature, we pass the auto-suggestion cmd to `as` , which will be exec in shell. And here we change the bash config with `- i` since we want to show the result into user's shell.

```shell
os@ubuntu:/data/auto-suggestion$ as ls 
auto-suggestions.tar.gz  bash_completion  cd.bash  dothis.bkp  goto.bash  readin.bash  test.bash
```
Well, although only 50 lines of code, I search revalent information everywhere and it cost nearly 10 days for me to implement it. (also broken 3 virtue machines)

And actually it is impossible. This is zsh auto-suggestions

We can see that the suggestions auto appears in user command line and if we press :arrow_right:, the suggestions will auto go into your input command line.

But:

	1. Bash do not support user customized command input, not to say show color in input buffer.
	2. Bash do not give built-in method to interrupt when user press the keyboards.
	3. Bash still has lots of strange behavior and bugs.

After trying & knowing all of these, I decided to use Bash built-in **complete** method which relies on Tabs to auto-complement.

So:

1. First I install my script **as** into /usr/bin folder
2. Then source the **as-completion** method
3. completion method will interrupt when user typed "as <Tab>"
4.  **as-completion** get the user input find similar cmd in history
5. return the suggestions to complete method

But problem is the bash complete only support single word completion, which means it cannot complete a whole command. So I choose to add "\\" to transfer whitespace. But some known bugs stop me: (a discussion with overwriting about complete method):

This is because bash complete method directly replace the last word when trying to complete, so sometimes it remove my last command or just appending them

After several days wasting on this by searching and doing all kinds of experiment, I think there is no way to fix the build-in method bugs. So I escape the cmd several times as I show above with "\\ "replace " "and it finally work successfully.


### 2. Syntax highlighting

Different from zshell, a newer shell to bash, bash cannot get the user input easily before users click the 'enter' and it is also hard to change the color of input text. Therefore, I decided to try to get the input shell command after clicking 'enter' and output the syntax highlighting of this command after the normal output of command. This function is implemented at `bash-preexec.sh` which is from https://github.com/rcaloras/bash-preexec?tdsourcetag=s_pcqq_aiomsg.

In any computer languages, their syntax are corresponding to not only their before and later contexts, and so as Bash. In my preliminary design, the program should scan each word of the input sentences one by one and confirm their highlighting styles. Since for each word, its syntax-highlighting is based on last words,  which can be seen as a kind of state, I think it is necessary to design a state machine used for scanning each word. When the scanned word changed, the state changed.

Generally, there're 4 states:

- `:start:`. It represents that it is command word.
- `:sudo_opt:`. It represents a leading-dash option to a pre-command, whether it takes an argument or not. (Example: `sudo`'s `-u` or `-i`.)
- `:sudo_arg:`. It represents the argument to a pre-command's leading-dash option, when given as a separate word. (Example: "foo" in `-u foo`.)
- `:regular:`. It represents that it is not a command word, which may be command delimiters or other strings.

In the beginning, the state of a word is not yet known. The state of this word and next word may contain multiple states. For example, after `sudo -i`, the next word may be either another leading-dash flags or a command name, hence the state would include both `:start:` and `:sudo_opt:`. 

The judgement principle is very complex and comprehensive syntax implementation is difficult to make progress. Hence, the implemented case of syntax highlighting is mainly as below.

1. **Highlighting for all kinds of commands**. 

   For the word which is seems to be a command, the program will check all kinds of command list, which is obtained from command `compgen`. Built-in command list from `compgen -b`, global function list from `compgen -A function` and all commands are from `compgen -A command`. For each list, the color of highlighting is different.

2. **Highlighting for all kinds of brackets**.

   For the brackets in total, I used simple stack to store and judge each brackets' level.

   For the brackets followed with other reserved word like `$` or `=`, I highlight them separately.

3. **Highlighting for all kinds of command separators**.

   For special words like ` '|' '||' ';' '&' '&&' $'\n' '|&' '&!' '&|'`, each of them works as different jobs in different cases. For example, for `'|'`, when it used as command separators, it is necessary to highlight it out. But when it is used in `(( ))` or `&{ }`, the case will be quite different. I use light-white for these special words.

4. **Highlighting for assignment**.

   For simple assignment of an integer or a string, I set their color be the default one since there is no need for users to know something about these simple assignment. But for assignment of an array or other more complex situation, for example, `a=( "1" "2" )`, I will make the brackets brighter or even yellow.

5. **Highlighting for some common keywords**.

   For some common keywords like `'case' 'do' 'done' 'elif' 'else' 'esac' 'fi' 'in' 'if' 'for' 'function' 'select' 'then' 'time' 'until' 'while'`, each of them will be highlighted as color brown. What is the most important is that I use a stack to store the state of words, for example, when I meets `do`, I will push character `D` into stack; when I meets done, I will check if the top of stack is "D". If not, I will mark this words.

   ![keyword_highlighting](G:\大三下\OS\project2\CS302_Project2\photo\keyword_highlighting.png)

6. **Highlighting for error**.

   For some obviously syntax error, such as command not found, brackets not match, I will make the place of error red.

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

 When typing fast, `ls` may be mistyped as `sl` so this rule is produced. When command matches this rule, a new command fixed from the previous command will be returned. Other rules are similar.

`CorrectedCommand` represents a fixed command. It necessarily needs a priority because sometimes the fix will conflict with each other such as fixing `sl` to `ls` or `sl.py`.

In `ohsht`, rules under `.ohsht/rules` are all imported and used to check whether the command to be fixed matches the rules. If matched, a fixed command will be produces by `get_new_command` from this rule. Eventually the command which has the highest priority will be outputed as the result. Also, the program does not run the fixed command immediately, but it still needs user's confirmation which in result keeps the system safe relatively.

Moreover, you can still add your own rules in `.ohsht/rules`. It should meets these following requirements:

1. Its suffix is `.py`;
2. It has a function `match(command:Command) -> bool` to determine whether the command to be fixed matches this situation or rule;
3. It has a function `get_new_command(command:Command) -> str` to fix the command with your own strategies.

### 5. Auto Doit
1. Feature 1: register do-this alias
   
   In this feature we first check if there is `AD_DB`, if not just init one. Then we check the alias and add into the DB.
```shell
os@ubuntu:/data/auto-suggestion$ ad -r yeah echo "沈学姐最帅"
Alias 'yeah' registered successfully.
```

2. Feature 2: do the alias with auto-complete.

   In this feature we exec the alias with auto-complete.

```shell
os@ubuntu:/data/auto-suggestion$ ad y <Tab here>
os@ubuntu:/data/auto-suggestion$ ad yeah 
沈学姐最帅
```

3. Feature 3: remove the alias with auto-complete.
   
   In this feature we can remove the alias we set.

```shell
os@ubuntu:/data/auto-suggestion$ ad -u yeah 
Alias 'yeah' unregistered successfully.
```

4. Feature 4. list all alias with color:

```shell
os@ubuntu:/data/auto-suggestion$ ad -l
   xzc  echo 太帅了
  home  cd /home/os
    hs  history
 autoS  cd /data/auto-suggestion
  data  cd /data/
```

5. Feature 5: push current directory into stack and jump

```shell
os@ubuntu:/data/auto-suggestion$ ad -p data 
os@ubuntu:/data$ 
```

6. Feature 6: pop the stack and jump back

```shell
os@ubuntu:/data$ ad -o
os@ubuntu:/data/auto-suggestion$ 
```

7. Feature 7: clean all alias

```shell
os@ubuntu:/data/auto-suggestion$ ad -c
os@ubuntu:/data/auto-suggestion$ ad -l
  
os@ubuntu:/data/auto-suggestion$ 
```

8. Feature 8: help

```shell
 os@ubuntu:/data/auto-suggestion$ ad -h
usage: ad [<option>] <alias> [<task>]

default usage:
  ad <alias> - changes to the task registered for the given alias

OPTIONS:
  -r, --register: registers an alias
    ad -r|--register <alias> <task>
  -u, --unregister: unregisters an alias
    ad -u|--unregister <alias>
  -p, --push: pushes the current task onto the stack, then performs ad
    ad -p|--push <alias>
  -o, --pop: pops the top task from the stack, then changes to that task
    ad -o|--pop
  -l, --list: lists aliases
    ad -l|--list
  -c, --cleanup: cleans up all alias
    ad -c|--cleanup
  -h, --help: prints this help
    ad -h|--help
os@ubuntu:/data/auto-suggestion$ 
```

First I listen the args from command line and:

1. Check if it is an options or the alias, if not just print error msg (if user is freshman we humanized auto print help)
2. If it is an options, matching it and go into its function!
   - -r:
      - First check if the  *AD_DB* exists, it not, just create one in ~/.config/ad.
      - Check if the name of new alias exist and legal.
      - Add the alias into *AD_DB*
   - -u:
      - First check if the alias in *AD_DB*, if not, throw error msg
      - remove the item in *AD_DB*
   - -p:
      - check the alias append in args
      - Push current directory into stack
      - exec the alias
   - -o:
      - If stack is not null, cd in the directory push before
   - -l:
      - List all alias in colors
   - -c:
      - overwriting the *AD_DB* with empty str.
   - -h:
      - Print help info into command line.
3. If it is alias, then pick it out from *AD_DB* and exec it!



And the structure of this plugins is well designed, we subtract every operation as function and do the error&warning handling.

What's more, with the experience in **Auto-Suggestions**, I add appropriate auto-suggestions into this plugins. When user press <Tab>, then the alias which satisfied will show below.
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
| 11812613 | 香佳宏 | Auto suggestions & auto-doit    |
| 11812106 | 马永煜 | Syntax highlighting & ohsh*t |
| 11812425 | 张佳雨 | Auto jump & ohsh*t           |

> ### References
> https://github.com/zsh-users/zsh-syntax-highlighting  
> https://github.com/wting/autojump  
> https://github.com/nvbn/thefuck  
> https://github.com/iridakos/goto/blob/master/goto.sh
> https://stackoverflow.com/questions/10528695/how-to-reset-comp-wordbreaks-without-affecting-other-completion-script
> https://iridakos.com/programming/2018/03/01/bash-programmable-completion-tutorial
