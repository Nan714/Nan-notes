To assign variables in bash, use the syntax **`foo=bar`** and access the value of the variable with **`$foo`**.
Note that `foo = bar` will not work since it is interpreted as calling the `foo` program with arguments `=` and `bar`  注意space！

Strings in bash can be defined with `'` and `"` delimiters, but they are not equivalent. Strings delimited with `'` are literal strings and will not substitute variable values whereas `"` delimited strings will.
```
foo=bar
echo "$foo"
prints bar
echo '$foo'
prints $foo
 ```
mcd () {
    mkdir -p "$1"
    cd "$1"
}
```
**mcd (){
}**   make and change directory  组合了mkdir和cd的便捷函数

**`-p`** 的作用
- p= parents（父目录）
- 自动创建所有不存在的中间目录
- 如果目录已存在，不会报错（静默跳过）

假设当前目录是空的，你想创建多级目录
mkdir a/b/c/d
错误：mkdir: cannot create directory 'a/b/c/d': No such file or directory
原因：`mkdir` 默认只能创建一级目录，需要父目录 `a/b/c` 已经存在。

**`cd`** 返回主目录 导航

- `$0` - Name of the script
- `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on.
- `$@` - All the arguments
- `$#` - Number of arguments
- `$?` - Return code of the previous command.  return 0 means 命令成功 其他则有人、error
- `$$` - Process identification number (PID) for the current script
- `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!`
- `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.` or `Alt+.`

`&&` (and operator) and `||` (or operator)
both of them are short-circuiting operators
The `true` program will always have a 0 return code and the `false` command will always have a 1 return code.
```
false || echo "Oops, fail"
# Oops, fail
```

**`cmd`** 要执行的命令

_command substitution_. 
Whenever you place `$( CMD )` it will execute `CMD`

_process substitution_, `<( CMD )` will execute `CMD` and place the output in a temporary file and substitute the `<()` with that file’s name. This is useful when commands expect values to be passed by file instead of by STDIN. For example, `diff <(ls foo) <(ls bar)` will show differences between files in dirs `foo` and `bar`

```
#!/bin/bash

echo "Starting program at $(date)" # Date will be substituted

echo "Running program $0 with $# arguments with pid $$"

for file in "$@"; do
    grep foobar "$file" > /dev/null 2> /dev/null
    # When pattern is not found, grep has exit status 1
    # We redirect STDOUT and STDERR to a null register since we do not care about them
    if [[ $? -ne 0 ]]; then
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file"
    fi
done
```
When performing comparisons in bash, try to use double brackets `[[ ]]` in favor of simple brackets `[ ]`.

ls \*.sh    ls project?
1. **`?`** = 正好一个**任意字符
2. **`*`** = **零个或多个**任意字符
`ls` 是 **List Directory Contents**
ls \[选项] \[文件/目录...]

Curly braces **`{}`** - Whenever you have a common substring in a series of commands, you can use curly braces for bash to expand this automatically. This comes in very handy when moving or converting files.

```
convert image.{png,jpg}
# Will expand to
convert image.png image.jpg

cp /path/to/project/{foo,bar,baz}.sh /newpath
# Will expand to
cp /path/to/project/foo.sh /path/to/project/bar.sh /path/to/project/baz.sh /newpath

# Globbing techniques can also be combined
mv *{.py,.sh} folder
# Will move all *.py and *.sh files


mkdir foo bar
# This creates files foo/a, foo/b, ... foo/h, bar/a, bar/b, ... bar/h
touch {foo,bar}/{a..h}
touch foo/x bar/y
# Show differences between files in foo and bar
diff <(ls foo) <(ls bar)
# Outputs
# < x
# ---
# > y
```

<mark style="background: #FFB86CA6;">touch</mark> 创建空文件和修改文件时间戳


The kernel knows to execute this script with a python interpreter instead of a shell command because we included a [shebang](https://en.wikipedia.org/wiki/Shebang_\(Unix\)) line at the top of the script. It is good practice to write shebang lines using the [`env`](https://www.man7.org/linux/man-pages/man1/env.1.html) command that will resolve to wherever the command lives in the system, increasing the portability of your scripts. To resolve the location, `env` will make use of the `PATH` environment variable we introduced in the first lecture. For this example the shebang line would look like `#!/usr/bin/env python`.


## shell tools
#### finding how to use commands
**man
tldr** 更简洁的man

#### finding files
**ls** 列出当前目录下的文件和文件夹
ls -l 详细列表（权限/大小/修改时间都有）

 [`find`](https://www.man7.org/linux/man-pages/man1/find.1.html), a great shell tool to find files
   