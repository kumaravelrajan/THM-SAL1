# Room 1 - Linux fundamentals part 1

## Searching for files

1. `find`

    We can **search for files** using the `find` command. 

    1. `find -name passwords.txt`

        If we already know the name of the file we are looking for. This commands looks for the file in the current working directory. 

    1. `find -name *.txt`

        Find all files in current directory that have .txt extension. 

1. `grep`

    The grep command allows us to **search the contents of files** for specific values that we are looking for.

## Intro to shell operators

1. &

    This operator allows us to execute commands in the background.

1. &&

    we can use "&&" to make a list of commands to run for example `command1 && command2`. However, it's worth noting that `command2` will only run if `command1` was successful.

1. \>

    Redirection operator. `echo hey > welcome` puts "hey" inside a file named "welcome". If welcome file already exists, it will be overwritten. 

    ![redirection operator](images/redirection-operator.png)

1. \>\>

    Also a redirection operator but appends content rather than overwriting. 

    In the previous example we overwrote a welcome file with content "hey". We can append to this file the word "hello" using the >> operator. 

    ![double redirection operator](images/double-redirection-operator.png)