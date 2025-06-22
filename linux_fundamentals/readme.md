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

    Tr replaces certain characters from a line with characters defined by us.

1. Practice questions:

    The file we will need to work with is the /etc/passwd file on our target and we can use any shown command above. Our goal is to filter and display only specific contents. Read the file and filter its contents in such a way that we see only:
    
    - A line with the username cry0l1t3.

        `cat /etc/passwd | grep cry0l1t3`

    - The usernames.
    
        `cat /etc/passwd | tr ":" " " | awk '{print $1}'`

        tr replaces all colons with space. awk prints only the first column. 

        Alternatively we can also do: 
        
        `cat /etc/passwd | cut -d":" -f1`. 

        cut delimits lines based on colons and prints the first field alone. 

    - The username cry0l1t3 and his UID.

        `cat /etc/passwd | grep "cry0l1t3" | cut -d":" -f 1,3`

    - The username cry0l1t3 and his UID separated by a comma (,)

        `cat /etc/passwd | grep "cry0l1t3" | cut -d":" -f 1,3 | tr ":" ","`

    - The username cry0l1t3, his UID, and the set shell separated by a comma (,).

        `cat /etc/passwd | grep "cry0l1t3" | cut -d":" -f 1,3,7 | tr ":" ","`

    -	All usernames with their UID and set shells separated by a comma (,).

        `cat /etc/passwd | cut -d":" -f 3,7 | tr ":" ","`

    -	All usernames with their UID and set shells separated by a comma (,) and exclude the ones that contain nologin or false.

        `cat /etc/passwd | grep -v "false\|nologin" | cut -d":" -f 3,7 | tr ":" ","`


    -	All usernames with their UID and set shells separated by a comma (,) and exclude the ones that contain nologin and count all lines of the filtered output.

        `cat /etc/passwd | grep -v "nologin" | cut -d":" -f 3,7 | tr ":" "," | wc -l`

1. Questions

    1. How many services are listening on the target system on all interfaces? (Not on localhost and IPv4 only)

        - Explaining the question:

            Key Terms Explained: 
            1. Services

            In Linux, a service is a program or process that runs in the background and provides some functionality, often over a network. Common examples include web servers (like Apache), SSH servers, and FTP servers.

            2. Listening

            When a service is listening, it means it is waiting for incoming network connections on a specific port. Think of it as a receptionist waiting for phone calls on a certain phone line.

            3. Target System

            This refers to the computer or server you are checking. It's the machine whose network services you want to inspect.

            4. All Interfaces

            A network interface is like a doorway for network traffic on your computer (e.g., Ethernet, Wi-Fi). When a service is listening on all interfaces, it means it will accept connections from any network source, not just from itself or a specific network card. In technical terms, this is often represented by the IP address 0.0.0.0 for IPv4.

            5. Not on localhost

            localhost is a special network address (127.0.0.1 for IPv4) that refers to the computer itself. If a service is listening only on localhost, it can only be accessed from the same machine, not from other computers on the network. The question specifically wants to exclude these.

            6. IPv4 only

            IPv4 is one version of the Internet Protocol, using addresses like 192.168.1.1. The question wants to count only services listening for IPv4 connections, not the newer IPv6 (which uses addresses like ::1).
            
        - What is the question asking?

            The question is asking:

            On the target Linux system, how many different services (programs) are currently set up to accept network connections from anywhere (not just from itself), using IPv4 addresses?

            In other words, if you were on another computer in the same network, how many different services could you try to connect to on this system, using IPv4?
        
        - Solution:

            `ss -l -4 | grep -v "127\.0\.0" | grep "LISTEN" | wc -l`

    1. Determine what user the ProFTPd server is running under. Submit the username as the answer.

        - Solution: 

            `ps aux | grep proftpd`

            - ps: Stands for "process status" and is used to list running processes.

            - aux: These are grouped BSD-style options:

                - a: Shows processes for all users, not just those belonging to the current user.

                - u: Displays the process's user/owner and shows output in a user-oriented format, including extra columns like CPU and memory usage.

                - x: Includes processes that are not attached to a terminal (i.e., background services and daemons)

            
        - Do processes run under a username?

            Yes, every Linux process runs under a specific username, which determines its permissions and access rights on the system. When a user starts a process, that process inherits the user's identity, known as the effective user ID (UID), and runs with the same permissions as that user. This means the process can access files, resources, and perform actions allowed by that user's account.

            This user-based ownership applies to all processes, including system services and background daemons, which often run under dedicated or restricted user accounts to limit their access for security reasons.

    1. Use cURL from your Pwnbox (not the target machine) to obtain the source code of the "https://www.inlanefreight.com" website and filter all unique paths (https://www.inlanefreight.com/directory" or "/another/directory") of that domain. Submit the number of these paths as the answer.

        - `curl https://www.inlanefreight.com | tr " " “\n” | cut -d"‘" -f2 | cut -d’"’ -f2 | grep www.inlanefreight.com | sort -u | wc -l`


## Regular Expressions
- Practice questions

    - Show all lines that do not contain the # character.

        `grep -v "#" /etc/ssh/sshd_config`

    - Search for all lines that contain a word that starts with Permit.

        `grep -Ei '\bPermit\w*' /etc/ssh/sshd_config`

        Output:
        ```
        #PermitRootLogin prohibit-password
        #PermitEmptyPasswords no
        # the setting of "PermitRootLogin prohibit-password".
        #PermitTTY yes
        #PermitUserEnvironment no
        #PermitTunnel no
        #	PermitTTY no
        ```

        The command is able to match successfully because \b means word boundary i.e. "Permit" should be at the start of a word boundary. 

        The \b anchor matches a position where a "word character" (letters, digits, or underscore) is next to a "non-word character" (anything else, such as whitespace, punctuation, or the start/end of a line). 
        
        In your example, the # character is a non-word character, and the P in Permit is a word character. 
        
        The boundary between # and P is thus a word boundary, so \bPermit matches at that position.

        \w matches a word character (letters, digits, or underscore).

    - Search for all lines that contain a word ending with "Authentication".

        `grep -Ei "\w*Authentication\b" /etc/ssh/sshd_config`

    - Search for all lines containing the word Key.

        `grep -i "key" /etc/ssh/sshd_config`

    - Search for all lines beginning with Password and containing yes.

        `cat /etc/ssh/sshd_config | grep -iE "^\W*password.*yes.*"`

        \w is for word characters. \W is for non-word characters.

        OUTPUT:
        `#PasswordAuthentication yes`

    - Search for all lines that end with "yes".

        `cat /etc/ssh/sshd_config | grep -iE "yes$"`

## Permission management

- To "traverse" or navigate into a directory, the user must first have the right key—this key is the _execute_ permission on the directory. Without it, even if the contents of the directory are visible to the user, they won't be able to enter or move through it.

    The _execute_ permission doesn't allow you to see or modify what's inside, but it does grant you the ability to step inside and explore the directory's structure. Without this permission, the user cannot access the directory's contents and will instead be presented with a “Permission Denied" error message.

- Execute permission on a directory vs executing a file in the directory.

    It is important to note that execute permissions are necessary to traverse a directory, no matter the user's level of access. Also, execute permissions on a directory do not allow a user to execute or modify any files or contents within the directory, only to traverse and access the content of the directory.

    To execute files within the directory, a user needs execute permissions on the corresponding file. To modify the contents of a directory (create, delete, or rename files and subdirectories), the user needs write permissions on the directory.

- The permission system in Linux systems

    - Based on the Octal number system. There are three different types of permissions a file or directory can be assigned: read (r), write (w) and execute (x).

    - `chmod` - change permissions.

    - `chown` - change owner.

    - SUID & SGID:

        The Set User ID (SUID) and Set Group ID (SGID) bits function like temporary access passes, enabling users to run certain programs with the privileges of another user or group. For example, administrators can use SUID or SGID to grant users elevated rights for specific applications, allowing tasks to be performed with the necessary permissions, even if the user themselves doesn’t normally have them. The presence of these permissions is indicated by an _s_ in place of the usual x in the file's permission set.

        
# System management
## User management
- /etc/shadow file is a critical system file that stores encrypted password information for all user accounts.

- sudo command

    To perform tasks that require elevated privileges, users can utilize the sudo command. The sudo command, short for "superuser do," allows permitted users to execute commands with the security privileges of another user, typically the superuser or root.

- Questions
    - Which option needs to be set to create a home directory for a new user using "useradd" command?

        -m

    - Which option needs to be set to lock a user account using the "usermod" command? (long version of the option)

        --lock

    - Which option needs to be set to execute a command as a different user using the "su" command? (long version of the option)

        --command

## Package management

- Different package management programs

    - dpkg: For Debian packages. 
    - apt: High level command line interface to the package manager.
    - aptitude: Alternative to apt. Also a high-level command line interface to the package manager. 
    - gem: Package manager for Ruby. 
    - pip: For python packages. 
    - git

- Advanced package manager (APT)

    APT (Advanced Package Tool) and DPKG (Debian Package Manager) are interconnected components of Debian-based Linux package management systems. APT acts as a high-level front-end that leverages DPKG's low-level functionality while adding critical features like dependency resolution and repository management.

    - DPKG
    
        - Installs, removes, and queries individual .deb packages.
        
        - Operates at a low level without handling dependencies automatically.
        
        - Example command: dpkg -i package.deb (installs a local .deb file).

    - APT

        - Manages dependencies, retrieves packages from repositories, and upgrades systems.

        - Uses DPKG internally to perform package installations/removals.

        - Example command: apt install package (resolves dependencies and uses DPKG).

    Linux gets its software from online repositories. These repositories can be stable, testing, or unstable. Most systems use stable repositories for safety. You can see which repositories you use by looking at /etc/apt/sources.list.

## Service and Process Management

- What are services / daemons?

    Services, also known as daemons, are fundamental components of a Linux system that run silently in the background "without direct user interaction". They perform crucial tasks that keep the system operational and provide additional functionalities.

- Types of services

    - System Services

    - User-Installed Services

- Identifying daemons

    Daemons are often identified by the letter d at the end of their program names, such as sshd (SSH daemon) or systemd.

- systemd, PIDs and PPIDs

    - systemd: 
    
        Most modern Linux distributions have adopted systemd as their initialization system (init system). It is the first process that starts during the boot process and is assigned the Process ID (PID).

        - systemctl and systemd

            Systemctl is a command-line utility used to interact with and manage systemd, the system and service manager in Linux operating systems. It serves as the primary tool for controlling system services, units, and the overall system state. Systemd itself is responsible for initializing the system during boot and managing running services and daemons throughout the system's operation.

    - PIDs & PPIDs:
        All processes in a Linux system are assigned a PID and can be viewed under the /proc/ directory, which contains information about each process. ProcessA maybe created by ProcessB which means ProcessA is the parent of ProcessB. ProcessA would have a parent PID (PPID).

    - Run a service after startup using systemctl
        `systemctl enable ssh`

- Kill a process

    A process can be in the following states: Running, Waiting (waiting for an event or system resource), Stopped, Zombie (stopped but still has an entry in the process table).

    Processes can be controlled using kill, pkill, pgrep, and killall. To interact with a process, we must send a signal to it. We can view all signals with the following command: `kill -l`.

- Backgrounding and foregrounding a process

    Processes can be sent to the background (pressing Ctrl+Z) or to the foreground (fg command).

- Execute Multiple Commands

    There are three possibilities to run several commands, one after the other. These are separated by: 
    
    -   Semicolon (\;)
    -   Double ampersand characters (&&)
    -   Pipes (|)

- Question

    1. Use the "systemctl" command to list all units of services and submit the unit name with the description "Load AppArmor profiles managed internally by snapd" as the answer.

        `systemctl list-units --all | grep "Load AppArmor"`

## Task scheduling

We can automatically schedule tasks in Linux to run at the end of a timer or at regular intervals automatically. We use systemd for this. 

There are three steps in this: 
- Create a timer (schedules when your mytimer.service should run)
- Create a service (executes the commands or script)
- Activate the timer

Systemd and Cron are both tools that can be used in Linux systems to schedule and automate processes. The key difference between these two tools is how they are configured. With Systemd, you need to create a timer and services script that tells the operating system when to run the tasks. On the other hand, with Cron, you need to create a crontab file that tells the cron daemon when to run the tasks.

## Network Services

- SSH
    Secure Shell (SSH) is a network protocol that allows the secure transmission of data and commands over a network. It is widely used to securely manage remote systems and securely access remote systems to execute commands or transfer files. In order to connect to our or a remote Linux host via SSH, a corresponding SSH server (software) must be available and running.

- Network File System (NFS)

    Network File System (NFS) is a network protocol that allows us to store and manage files on remote systems as if they were stored on the local system. It enables easy and efficient management of files across networks. For example, administrators use NFS to store and manage files centrally (for Linux and Windows systems) to enable easy collaboration and management of data. 

    - `/etc/exports`

        /etc/exports is the main configuration file for NFS exports on Linux.

        Each line defines a directory to share, the clients allowed to access it, and the permissions/options for each client.

        Example line:

        `/shared/directory client1(options) client2(options)`

    - Server and client roles

        The server mentions the directory it intends to share with the client in /etc/exports. Included in this are dir name, client hostname/IP, access rights.

        The client is responsible for mounting the shared (exported) directories. This process makes the remote directory appear as part of the local filesystem on the client machine.

- 

    
