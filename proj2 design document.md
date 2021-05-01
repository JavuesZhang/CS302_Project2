# 2021S-CS302 Project2 Design Document

## Background

Linux, known as GNU/Linux, is a multi-user, multi-tasking, multi-threaded and multi-CPU based POSIX operating system inspired mainly by Minix and Unix ideas. It can run major Unix tools and software, applications and network protocols. 

Bash is a Unix shell and command language for the GNU Project as a free software replacement for the Bourne shell. It has been used as the default login shell for most Linux distributions. 

This project aims to perform enhancements to the bash. We hope to add or improve some features of the current bash so that user experience can be improved during using the shell.

## Description

`bash` is now powerful but still not easy to use compared to `zsh`. We decided to improve `bash` with some features implemented in `zsh` including `auto-jump`, `auto-suggestions`, `syntax-highlighting`, and `screen/tmux`. 

Here's the basic description about these features:
1. `auto-jump`: a faster way to navigate the filesystem. It works by maintaining a database of the directories the user uses the most from the command line.
2. `auto-suggestions`: It suggests commands as the user type based on history and completions.
3. `syntax-highlighting`: It provides syntax highlighting for the shell. It enables highlighting of commands whilst they are typed at a shell prompt into an interactive terminal. This helps in reviewing commands before running them, particularly in catching syntax errors.
4. `screen/tmux`: The most widely used terminal multiplexer.

We divided the project into multiple small goals.

- Learn bash-shell programming
- Learn how to expand bash command with C/C++ program & shell script
- Install these plugins on `zsh` and see how they work
- Go through source code of these plugins
- Reimplement these features on bash-shell

Here we give some photos about these features:

1. `auto-jump`:![image-20210430225012309](https://github.com/JavuesZhang/CS302_Project2/blob/main/photo/image-20210430225012309.png)
2. `auto-suggestions`:![image-20210430225131192](https://github.com/JavuesZhang/CS302_Project2/blob/main/photo/image-20210430225131192.png)
3. `syntax-highlighting`![image-20210430225419967](https://github.com/JavuesZhang/CS302_Project2/blob/main/photo/image-20210430225419967.png)
4. `Screen`
![How to Use 'Tmux Terminal' to Access Multiple Terminals Inside a Single  Console](https://www.tecmint.com/wp-content/uploads/2016/01/Tmux-Manage-Multiple-Linux-Terminals.png)

## Implementation

We refer to source code of the extensions:

- [zsh-users/zsh-autosuggestions: Fish-like autosuggestions for zsh (github.com)](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-users/zsh-syntax-highlighting: Fish shell like syntax highlighting for Zsh. (github.com)](https://github.com/zsh-users/zsh-syntax-highlighting)
- [wting/autojump: A cd command that learns - easily navigate directories from the command line (github.com)](https://github.com/wting/autojump)
- [tmux/tmux: tmux source code (github.com)](https://github.com/tmux/tmux)
- [ohmybash/oh-my-bash: A delightful community-driven framework for managing your bash configuration, and an auto-update tool so that makes it easy to keep up with the latest updates from the community. (github.com)](https://github.com/ohmybash/oh-my-bash)

Also, we have explored an valuable blog:

- [Bash Scripting: Everything you need to know about Bash-shell programming](https://medium.com/sysf/bash-scripting-everything-you-need-to-know-about-bash-shell-programming-cd08595f2fba)

In addition, we may use some third-party libraries and api to simplify the implementation of these features. We plan to implement these features one by one from easy to hard since we still need to learn a lot about linux kernel and shell&.C programming. Due to this consideration, we may first try to implement 3 plugins then have a try of implementing the Split screen feature. 

## Expected goals

In the project, we expect to improve `bash` with the following functions:

- Auto suggestions
- Syntax highlighting
- Auto jump
- Split screen

Detailed progress is listed in **Description**.

## Division of labor

| ID       | Name   | Division                           |
| :------- | :----- | :--------------------------------- |
| 11812613 | 香佳宏 | Auto suggestions & Split screen    |
| 11812106 | 马永煜 | Syntax highlighting & Split screen |
| 11812425 | 张佳雨 | Auto jump & Split screen           |


