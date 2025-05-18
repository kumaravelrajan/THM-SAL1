# Introduction

## Introduction to shell

1. Difference between terminal and shell

    The terminal is where you type; the shell is what understands and runs what you type. The terminal can exist without the shell, displaying output from some other program. The shell can exist without the terminal, running scripts in the background. 

    A shell runs inside a terminal. 

    Relation: User <=> Terminal <=> Shell <=> Kernel

    There are different kinds of shells in Linux like zsh, bash etc. Each shell has been developed for a specialized purpose like more customizability or feature richness and so on. A terminal emulator program like GNOME Terminal can run different types of shell within it. 

# The Shell

## Prompt description

1. $ for user. # for root.

    user: \<username\>@\<hostname\>[~]$

    root: root@\<hostname\>[~]#

1. The PS1 variable in Linux systems controls how your command prompt looks in the terminal.

1. Each shell program has its own configuration file. For the bash shell, .bashrc is the config file. In this file, PS1 variable determines how the prompt looks. 

## System information
1. /var/mail has collection of all mails sent to each user on the device. 

# Workflow
## Navigation

1. ls command

    - Typing `ls -l` gives us : 
        ```
        total 32
        drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:37 Desktop
        drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:34 Documents
        drwxr-xr-x 3 cry0l1t3 htbacademy 4096 Nov 15 03:26 Downloads
        .
        .   
        ```
    
        - `total 32` means all files and directories in the current directory occupy 32 KB space (32 blocks where 1 block = 1024 bytes).

        - `drwxr-xr-x 2 cry0l1t3 htbacademy 4096 Nov 13 17:37 Desktop` has the following meaning:

            - `drwxr-xr-x`: Types and permissions
            - `2`: This number shows how many hard links point to the file or, for directories, how many subdirectories (including . and ..)
            - `cry0l1t3`: Owner of the file/directory
            - `htbacademy`: Group owner of the file/directory
            - `4096`: The size of the file in bytes. For directories, this is typically the size of the directory metadata (often a multiple of the filesystem block size, e.g., 4096 bytes)
            - `Nov 13 17:37`: Date and time
            - `Desktop`: Directory name

    - To also see hidden files type `ls -la`.
    - If we cd from dirA to dirB, `cd -` in dirB brings us back to dirA. 
    - `[Ctrl] + [L]`: Shortcut to clear terminal. 
    - `[Ctrl] + [R]`: Search through the command history.

## Working with files and directories



    



    