# 2021S-CS302 Project2 Bash Enhancement

> Group members:
>
> - 11812613 香佳宏
> - 11812106 马永煜
> - 11812425 张佳雨

## Background

Linux, known as GNU/Linux, is a multi-user, multi-tasking, multi-threaded and multi-CPU based POSIX operating system inspired mainly by Minix and Unix ideas. It can run major Unix tools and software, applications and network protocols. 

Bash is a Unix shell and command language for the GNU Project as a free software replacement for the Bourne shell. It has been used as the default login shell for most Linux distributions. 

This project aims to perform enhancements to the bash. We hope to add or improve some functions of the current bash so that user experience can be improved during using the shell.

`bash` is now powerful but still not easy to use compared to `zsh`. We decided to improve `bash` with some features implemented in `zsh` including `auto-jump`, `auto-suggestions`, `syntax-highlighting`, and `ohsh*t`. 

Here's the basic description about these features:

1. `auto-jump`: a faster way to navigate the filesystem. It works by maintaining a database of the directories the user uses the most from the command line.
2. `auto-suggestions`: It suggests commands as the user type based on history and completions.
3. `syntax-highlighting`: It provides syntax highlighting for the shell. It enables highlighting of commands whilst they are typed at a shell prompt into an interactive terminal. This helps in reviewing commands before running them, particularly in catching syntax errors.
4. `ohsh*t`: It provides auto fix for your last error command. Only need to input `sht` and it will fix command for you.

## Usage

### 1. Auto jump

Download `code/.myautojump.tar.gz` to your Ubuntu home and untar it:

```sh
tart -zxvf .myautojump.tar.gz
```

Open your `~/.bashrc` and put the following command in the last:

```sh
source ~/.myautojump/myautojump.bash
```

Refer to `video/auto-jump.mp4` to see how it works.

### 2. Auto suggestions

Refer to `video/auto-suggestion-1.mp4` and `video/auto-suggestion-2.mp4` to see how it works.

### 3. Syntax highlighting

Refer to `video/syntax-highlighting.mp4` to see how it works.

### 4. Auto jump

Download `code/.ohsht.tar.gz` to your Ubuntu home and untar it:

```sh
tart -zxvf .ohsht.tar.gz
```

Open your `~/.bashrc` and put the following command in the last:

```sh
source ~/.ohsht/ohsht.bash
```

Refer to `video/ohsht.mp4` to see how it works.
