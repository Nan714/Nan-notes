## using the shell

date

echo hello 

~~hello world~~
"hello world" or hello\ world

**$PATH** 
- $PATH 是一堆目录的列表
- Shell 会在这些目录里找你输入的命令

**com+L** 一键清terminal         

## navigating in the shell

 A path that starts with `/` is called an _absolute_ path. Any other path is a _relative_ path. Relative paths are relative to the current working directory, which we can see with the `pwd` command and change with the `cd` command. In a path, `.` refers to the current directory, and `..` to its parent directory
 **`cd ~`** back to home

**`mkdir`** make directory

To see what lives in a given directory, we use the **`ls`** command

```
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```
Then follow three groups of three characters (`rwx`). These indicate what permissions the owner of the file (`missing`), the owning group (`users`), and everyone else respectively have on the relevant item. A `-` indicates that the given principal does not have the given permission.

Some other handy programs to know about at this point are **`mv`** (to rename/move a file), **`cp`** (to copy a file), and `mkdir` (to make a new directory).
`mv`
mv notes.txt notes_final.txt    rename
mv report.pdf Documents/    move
mv report.pdf Documents/report_final.pdf      move+rename


If you ever want _more_ information about a program’s arguments, inputs, outputs, or how it works in general, give the `man` program a try
**`man ls` ** manual
press `q` to quit
## connecting programs

```
missing:~$ echo hello > hello.txt
missing:~$ cat hello.txt
hello
missing:~$ cat < hello.txt
hello
missing:~$ cat < hello.txt > hello2.txt
missing:~$ cat hello2.txt
hello
```

The **`|`** operator lets you “chain” programs such that the output of one is the input of another
ls | grep ".txt"

## A versatile and powerful tool
```
$ echo 3 | sudo tee brightness
```
**sudo**: superuser do
**tee**:一边把内容显示在屏幕上，一边写进文件
