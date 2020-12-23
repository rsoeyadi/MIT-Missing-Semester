## Lecture 2 at 3:40pm on December 21st, 2020

---

### Shell Tools and Scripting

Most shells have their own scripting language with variables, control flow and its own syntax. 

- spaces are important

```bash
foo=bar
echo $foo
# prints bar
foo = bar
# this will not work; it is calling "foo" and giving it the argument of "="
```

- Strings delimited with `'` are literal strings and will not substitute variable values whereas `"` delimited strings will.

---

In Bash, you can create functions that take arguments. For example:

```bash
mcd () {
    mkdir -p "$1"
    cd "$1"
}

# $1 is the first argument to the script/function
```

Unlike other languages, Bash uses a variety of special variables to refer to the arguments, error codes, and other relevant variables

- `$0` - Name of the script
- `$1` to `$9` - Arguments to the script. `$1` is the first argument and so on.
- `$@` - All the arguments
- `$#` - Number of arguments
- `$?` - Return code of the previous command
- `$$` - Process identification number (PID) for the current script
- `!!` - Entire last command, including arguments. A common pattern is to execute a command only for it to fail due to missing permissions; you can quickly re-execute the command with sudo by doing `sudo !!`
- `$_` - Last argument from the last command. If you are in an interactive shell, you can also quickly get this value by typing `Esc` followed by `.`

---

## Short-circuit evaluation

Exit codes can be used to conditionally execute commands using `&&` (and operator) and `||` (or operator)

- **the second argument is executed or evaluated** *only* **if the first argument does not suffice to determine the value of the expression**
  - when the first argument of the ```AND``` function evaluates to ```false```, the overall values is ```false```; when the first argument of the ```OR``` function evalutes to ```true```, the overall value must be ```true```

```bash
false || echo "Oops, fail"
# evalues the left side (which will give a 1 return code), and if that fails, then it tries the right side
# Oops, fail

true || echo "Will not be printed"
#

true && echo "Things went well"
# Things went well

false && echo "Will not be printed"
#

true ; echo "This will always run"
# This will always run

false ; echo "This will always run"
# This will always run
```

---

### Command Substitution

```$(CMD)``` will execute ```CMD```, get the output of it, and substitute it in place.

A lesser known similar feature is *process substitution*

```<( CMD )```

- will execute `CMD` and place the output in a temporary file and substitute the `<()` with that file’s name. 

---

```diff <(ls foo) <(ls bar)```

- will show the differences between ```foo``` and ```bar```

---

### grep

- this command will search any given input files, selecting lines that match one or more patterns.

---

```bash
#!/bin/bash

## iterate through the arguments we provide, `grep` for the string `foobar`, and append it to the file as a comment if it’s not found.

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

- ```-ne``` is *not equals*
- When performing comparisons in bash, try to use double brackets `[[ ]]` in favor of simple brackets `[ ]`. Chances of making mistakes are lower although it won’t be portable to `sh`.

---

When launching scripts, you will often want to provide arguments that are similar. Bash has ways of making this easier, expanding expressions by carrying out filename expansion. These techniques are often referred to as shell *globbing*.

- Wildcards - Whenever you want to perform some sort of wildcard matching, you can use `?` and `*` to match one or any amount of characters respectively. For instance, given files `foo`, `foo1`, `foo2`, `foo10` and `bar`, the command `rm foo?` will delete `foo1` and `foo2` whereas `rm foo*` will delete all but `bar`.
- Curly braces `{}` - Whenever you have a common substring in a series of commands, you can use curly braces for bash to expand this automatically. This comes in very handy when moving or converting files.

```Bash
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

---

## Errors

There are tools like [shellcheck](https://github.com/koalaman/shellcheck) that will help you find errors in your sh/bash scripts.

---

### find instead of ls

```bash
find . -name src -type d
```

`find` will recursively search for files matching some criteria. Some examples:

```bash
# Find all directories named src
find . -name src -type d
# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'
```

---

### locate vs. find

`locate` uses a prebuilt database, which should be regularly updated, while `find` iterates over a filesystem to locate files.

Thus, `locate` is much faster than `find`, but can be inaccurate if the database -can be seen as a cache- is not updated (see `updatedb` command).

Also, `find` can offer more granularity, as you can filter files by every attribute of it, while `locate`uses a pattern matched against file names.

---

### grep

`grep` has many flags that make it a very versatile tool. Some I frequently use are `-C` for getting **C**ontext around the matching line and `-v` for in**v**erting the match, i.e. print all lines that do **not** match the pattern. For example, `grep -C 5` will print 5 lines before and after the match. When it comes to quickly searching through many files, you want to use `-R`since it will **R**ecursively go into directories and look for files for the matching string.

`grep -R` can be improved in many ways, such as ignoring `.git` folders, using multi CPU support, &c. Many `grep` alternatives have been developed, including [ack](https://beyondgrep.com/), [ag](https://github.com/ggreer/the_silver_searcher) and [rg](https://github.com/BurntSushi/ripgrep). All of them are fantastic and pretty much provide the same functionality. For now I am sticking with ripgrep (`rg`), given how fast and intuitive it is. Some examples:

```
# Find all python files where I used the requests library
rg -t py 'import requests'
# Find all files (including hidden files) without a shebang line
rg -u --files-without-match "^#!"
# Find all matches of foo and print the following 5 lines
rg foo -A 5
# Print statistics of matches (# of matched lines and files )
rg --stats PATTERN
```

Note that as with `find`/`fd`, it is important that you know that these problems can be quickly solved using one of these tools, while the specific tools you use are not as important.

---

## History of Commands in Bash

Typing the up arrow will give you back your last command, and if you keep pressing it you will slowly go through your shell history.

The `history` command will let you access your shell history programmatically. It will print your shell history to the standard output. If we want to search there we can pipe that output to `grep` and search for patterns.`history | grep find` will print commands that contain the substring “find”.

If you start a command with a leading space it won’t be added to your shell history. This comes in handy when you are typing commands with passwords or other bits of sensitive information. If you make the mistake of not adding the leading space, you can always manually remove the entry by editing your `.bash_history` or `.zhistory`.

---

## Extra Notes for Myself

```bash
while /bin/bash three.sh > error.sh; do :; done;
#will execute until error is thrown
```

```:``` is equivalent to ```true```



