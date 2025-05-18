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
