# 2021S-CS302 Project2 Final Report

Group members:

- 11812613 香佳宏
- 11812106 马永煜
- 11812425 张佳雨

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



### 4. ohsh*t



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

