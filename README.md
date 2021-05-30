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
5. `auto-doit`: It provides alias management for you with auto-suggestion and color high-lighting.
## Usage

### 1. Auto jump

Download `code/.myautojump.tar.gz` to your Ubuntu home and untar it:

```sh
tar -zxvf .myautojump.tar.gz
```

Open your `~/.bashrc` and put the following command in the last:

```sh
source ~/.myautojump/myautojump.bash
```

Refer to `video/auto-jump.mp4` to see how it works.

### 2. Auto suggestions

Download `code/.auto-suggestions.tar.gz` to your Ubuntu home and untar it:

```sh
tar -zxvf .auto-suggestions.tar.gz
```

```shell
cd ./.auto-suggestions
bash ./install.sh
sudo cp ./as /usr/bin/
source ~/.bashrc
```

Refer to `video/auto-suggestion.mp4` and Auto-Suggestions/README to see how it works.

### 3. Syntax highlighting

Download `code/.syntax-highlighting.tar.xz` to your Ubuntu home and untar it:

```sh
tar -xvJf .syntax-highlighting.tar.xz
```

Open your `~/.bashrc` and put the following command in the last:

```sh
source ~/.syntax-highlighting/syntax-highlighting.sh
```

Refer to `video/syntax-highlighting.mp4` to see how it works.

### 4. Auto Correct

Download `code/.ohsht.tar.gz` to your Ubuntu home and untar it:

```sh
tar -zxvf .ohsht.tar.gz
```

Open your `~/.bashrc` and put the following command in the last:

```sh
source ~/.ohsht/ohsht.bash
```

Refer to `video/ohsht.mp4` to see how it works.

### 5. Auto Do it

Download `code/.auto-suggestions.tar.gz` to your Ubuntu home and untar it:

```sh
tar -zxvf .auto-suggestions.tar.gz
```

Open your `~/.bashrc` and put the following command in the last:

```sh
source ./.auto-suggestions/ad
```

Refer to `video/auto-suggestion.mp4` and Auto-Suggestions/README to see how it works.
