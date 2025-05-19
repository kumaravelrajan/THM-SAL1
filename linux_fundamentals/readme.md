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
1. `touch <name>`: Create empty file. 

## Editing files
1. /etc/passwd file: 

    Really important file that contains essential information about the users on the system, such as their usernames, user IDs (UIDs), group IDs (GIDs), and home directories.

    - Historically /etc/passwd also stored password hashes. However, now those hashes are typically stored in /etc/shadow which has stricter permissions. 

    - If the permissions on /etc/passwd or other critical files are not set correctly, it may expose sensitive information or lead to privilege escalation opportunities.

1. `vimtutor`: Command that opens a tutorial to vim editor. 

## File descriptors and Redirections

1. What are file descriptors?: 

    A file descriptor in Linux is a non-negative integer that uniquely identifies an open file or other input/output resource (like a pipe, socket, or device) within a process. When a process opens a file or I/O resource, the Linux kernel assigns it a file descriptor and tracks it in a per-process file descriptor table. This integer acts as a handle that the process uses to perform operations such as reading, writing, or closing the resource.

    By convention, every process starts with three standard file descriptors:

    0: Standard input (stdin)

    1: Standard output (stdout)

    2: Standard error (stderr)

    File descriptors are fundamental to how Linux manages all kinds of I/O, treating everything-from files to network connections-as files accessible through these integer handles.

1. STDOUT and STDERR

    ```
    htb-student@nixfund:~$ find /etc -name shadow
    find: ‘/etc/dovecot/private’: Permission denied #STDERR
    /etc/shadow #STDOUT
    find: ‘/etc/ssl/private’: Permission denied #STDERR
    find: ‘/etc/polkit-1/localauthority’: Permission denied #STDERR

    htb-student@nixfund:~$ find /etc -name shadow 2>/dev/null #Redirect STDERR to /dev/null i.e. discard data.
    
    /etc/shadow #STDOUT
    ```

    We can also redirect STDOUT to a file without STDERR featuring in it. 

    ```
    find /etc -name shadow 2>/dev/null > results.txt
    ```

    Alternatively, we can also have 

    ```
    find /etc -name shadow 2>stderr.txt 1>stdout.txt
    ```

1. Redirect STDIN

    For redirecting STDOUT and STDERR we use > (greater than sign). For redirecting STDIN we use < (lesser than sign).

    Example:
    `cat < stdout.txt` uses the contents of the file stdout.txt as STDIN for cat command. 

1. To redirect STDOUT and append to a file 

    Using > means the a new file is created or if a file of the same name already exists, it is overwritten. To append to the given file use >>. 

    `find /etc -name passwd >> stdout.txt 2>/dev/null`

1. Redirect STDIN Stream to a File

    Consider: `cat << EOF > stream.txt`
    Here, The "<< EOF" tells the shell to read input until it encounter a line containing only the delimiter you specify, in this case EOF. Everything typed between the command and the ending EOF line is treated as input

    Then, once input as ended, the STDOUT of cat (which basically regurgitates the entered input) is directed to be stored in stream.txt.

    Note that "EOF" itself is not stored in stream.txt.

1. Pipes

    Another way to redirect STDOUT is to use pipes (|). These are useful when we want to use the STDOUT from one program to be processed by another. One of the most commonly used tools is grep, which we will use in the next example. Grep is used to filter STDOUT according to the pattern we define. In the next example, we use the find command to search for all files in the "/etc/" directory with a ".conf" extension. Any errors are redirected to the "null device" (/dev/null). Using grep, we filter out the results and specify that only the lines containing the pattern "systemd" should be displayed.

    `find /etc/ -name *.conf 2>/dev/null | grep systemd`

## Filter contents
1. More and Less

    Use `more` for basic, forward-only paging or when working on minimal systems.

    Use `less` for interactive, feature-rich navigation, searching, and handling large files.

    More typically leaves output on terminal post exit. Less typically does not. However, less can also be instructed to leave output at the terminal similar to more with the use of apt options. 

1. Head

    By default, head prints the first ten lines of the given file or input, if not specified otherwise.

    `head /etc/passwd`

1. Tail

    Returns the last ten lines.

    `tail /etc/passwd`

1. Sort

    Eg: `cat /etc/passwd | sort`

1. Cut

    ```
    htb-student@nixfund:~$ cat /etc/passwd | grep -v "false\|nologin"
    root:x:0:0:root:/root:/bin/bash
    sync:x:4:65534:sync:/bin:/bin/sync
    mrb3n:x:1000:1000:mrb3n:/home/mrb3n:/bin/bash
    cry0l1t3:x:1001:1001::/home/cry0l1t3:/bin/bash
    htb-student:x:1002:1002::/home/htb-student:/bin/bash
    ```

    ```
    htb-student@nixfund:~$ cat /etc/passwd | grep -v "false\|nologin" | cut -d ":" -f 1
    root
    sync
    mrb3n
    cry0l1t3
    htb-student
    ```

    The cut command says treat ":" as the delimiter in each line. Also, only display the 1st field (column) from each line. 

1. Tr

    