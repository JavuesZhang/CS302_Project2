# 2021S-CS302 Project2 Design Document

## Background

Linux, known as GNU/Linux, is a multi-user, multi-tasking, multi-threaded and multi-CPU based POSIX operating system inspired mainly by Minix and Unix ideas. It can run major Unix tools and software, applications and network protocols. 

Bash is a Unix shell and command language for the GNU Project as a free software replacement for the Bourne shell. It has been used as the default login shell for most Linux distributions. 

This project aims to perform enhancements to the bash. We hope to add or improve some functions of the current bash so that user experience can be improved during using the shell.

## Description

`bash` is now powerful but still not easy to use compared to `zsh`. We decided to improve `bash` with some functions implemented in `zsh` including `auto-jump`, `auto-suggestions`, `syntax-highlighting`, and `screen/tmux`. We divided the project into multiple small goals.

- Learn bash-shell programming
- Learn how to expand bash functions with shell script
- Install these plugins on `zsh` and see how they work
- Go through source code of these plugins
- Transfer these functions to bash

## Implementation

We refer to source code of the extensions:

- [zsh-users/zsh-autosuggestions: Fish-like autosuggestions for zsh (github.com)](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-users/zsh-syntax-highlighting: Fish shell like syntax highlighting for Zsh. (github.com)](https://github.com/zsh-users/zsh-syntax-highlighting)
- [wting/autojump: A cd command that learns - easily navigate directories from the command line (github.com)](https://github.com/wting/autojump)
- [tmux/tmux: tmux source code (github.com)](https://github.com/tmux/tmux)
- [ohmybash/oh-my-bash: A delightful community-driven framework for managing your bash configuration, and an auto-update tool so that makes it easy to keep up with the latest updates from the community. (github.com)](https://github.com/ohmybash/oh-my-bash)

Also, we have explored an valuable blog:

- [Bash Scripting: Everything you need to know about Bash-shell programming](https://medium.com/sysf/bash-scripting-everything-you-need-to-know-about-bash-shell-programming-cd08595f2fba)

## Expected goals

In the project, we expect to improve `bash` with the following functions:

- Auto suggestions
- Syntax highlighting
- Auto jump
- Split screen

Detailed progress is listed in **Description**.

## Division of labor

| ID       | Name   | Division |
| :------- | :----- | :------- |
| 11812613 | 香佳宏 |          |
| 11812106 | 马永煜 |          |
| 11812425 | 张佳雨 |          |

