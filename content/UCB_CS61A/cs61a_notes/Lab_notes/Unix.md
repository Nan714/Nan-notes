The detailed info: <https://cs61a.org/articles/unix/#creating-files>

一种是直接用光标互动，一种是用terminal去manage

### UNIX commands

- Directories:
    
    - `ls`: **l**i**s**t the files and folders inside of the current directory
    - `mkdir`: **m**a**k**e a new **dir**ectory. For example, `mkdir example` creates a directory called `example`
    - `cd`: _c_hange _d_irectories. For example, `cd example` changes directories to `example`
    - `rm -r`: recursively **r**e**m**ove a specified directory. For example, `rm -r example` removes the `example` directory and all files and subdirectories inside it.
- Files:
    
    - `cat`: displays the contents of a file on the screen. For example, `cat unix.txt` shows the contents of the file `unix.txt`
    - `mv`: **m**o**v**es a file/directory to another file/directory. For example, `mv file1 file2` moves the contents of `file1` into a (possibly new) file called `file2`. When moving one file to another, we are effectively renaming the file!
    - `cp`: **c**o**p**ies a file to another file/directory. For example, `cp file1 file2` copies the contents of `file1` into a file named `file2`.
    - `rm`: **r**e**m**oves a file. For example, `rm file1` deletes the file called `file1`.
- Miscellaneous:
    
    - `echo`: displays words on the screen
    - `man`: displays **man**ual pages for a specified command

**unzip** lab00.zip


**echo** "Hello world" 

**ls** : The `ls` command **l**i**s**ts all the files and folders in the current directory. A **directory** is another name for a folder (such as the `Documents` folder).

The **mkdir** command **m**a**k**es a new **dir**ectory (i.e. makes a new folder)

>Some commands always require arguments to work, like `mkdir`. Other commands can work just fine without supplying any arguments, like `ls`.

**cd** command will **c**hange **d**irectories
- Type `cd ..` (two dots). The `..` means "the parent directory". In this case, the parent directory of `example` happens to be our home directory, so we can use `cd ..` to go up one directory. *两点之前有空格！*
- Type `cd ~` (the tilde). Remember that `~` means home directory, so this command tells your terminal to change to the home directory, no matter where you currently are.
- Type `cd` (that is, the `cd` command with no arguments). In UNIX, typing just `cd` is a shortcut for typing `cd ~`.

The **rm** command will **r**e**m**ove files and directories from your filesystem. By itself (that is, without the `-r`) the `rm` command only removes files. However, since we are removing a directory, we need to specify `-r` to **r**ecursively remove the `tmp` directory and any files that `tmp` might contain (the process is called "recursive" because, in order to remove `tmp`, we have to remove everything inside of `tmp`).

>As you've seen, some commands require arguments, like `mkdir`. Other commands do not require any arguments in order to work, like `ls`. In addition, most commands can also be given **flags**, like the `-r` for `rm`. Flags are ways to specify modified behavior for commands -- for example, `rm` by itself only removes files; using `-r` tells `rm` to remove directories.


```
mv unix.txt ~/example
```

The **mv** command **m**o**v**es one file/directory into another file/directory. Here, we are moving the `unix.txt` file into the `example` directory, which is inside the home directory.

The **cat** command prints the contents of a file to the screen.

**cp** 
`cp example/unix_commands.txt .`

The first argument (`example/unix_commands.txt`) tells the terminal to look in the `example` directory to find `unix_commands.txt`.

The second argument `.` tells the terminal to copy `unix_commands.txt` to the directory `.`. Just as two dots (`..`) represents the parent directory, a single dot (`.`) represents the _current_ directory (the directory we're in right now).