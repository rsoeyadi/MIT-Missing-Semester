Sunday, December 20th at 1:15pm

---

Lecture 1

---

## The Shell

- visual interfaces are limited, so these textual tools have tons of different ways to program things
- **Bash** is a shell (Bourne-Again **SHell**)
- in the shell prompt, you can execute commands
  - can execute with arguments
- ```echo``` is a program that will "echo" the arguments
- ```echo "Hello world"```

---

Your computer has a bunch of programs that it knows how to use

- it will come with terminal-centric programs

- there are also predefined variables in Bash

  - ```$PATH```is one of these variables

    - basically, when we run a command, the computer will look into the path for a program that can run what I am asking the computer to do (ex., it needs to find out where to find the date program)

    - if we want to figure out what is running the command I want to execute, we can do the following

      ```bash
      ryans@Ryans-MacBook-Pro ~ % which echo
      echo: shell built-in command
      ```

```~``` will give you the home directory

```-``` will give you the previous directory you were in, if you want to toggle between different directories

You can use ```ls --help```

You will see **flags and options** I.e., ```-l``` is a flag while ```[OPTIONS]``` is an option

The ```-l``` flag gives you a long-listing format

- you will get a lot more information about files such as permissions

  ```Bash
  missing:~$ ls -l /home
  drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
  
  #look at it in groups of three (I.e., d, rwx, r-x, r-x)
  
  ```

  - First, the `d` at the beginning of the line tells us that `missing` is a directory. Then follow three groups of three characters (`rwx`). These indicate what permissions the owner of the file (`missing`), the owning group (`users`), and everyone else respectively have on the relevant item.
  - To enter a directory, a user must have “search” (represented by “execute”: `x`) permissions on that directory (and its parents). To list its contents, a user must have read (`r`) permissions on that directory. For files, the permissions are as you would expect. Notice that nearly all the files in `/bin` have the `x`permission set for the last group, “everyone else”, so that anyone can execute those programs.

Move a file

```mv dotfiles.md foo.md``` 

Copy a file

```cp dotfiles.md ../food.mc``` 

## Streams

The shell gives us streams:

### Two primary streams

### Input

- what you input to the terminal (keyboard)

```< file```

- means rewire the input of the program (my arguments) to be this file

### Output

- what is printed to the stream (the terminal)

```> file```

- means rewire the output of the program **into this file**

```cat``` **prints the contents of a file**

---

We can do both simultaneously

```cat < hello.txt > hello2.txt```

What this does is it tells ```cat``` to take the contents of ```hello.txt``` as its input, and print it to ```cat```'s output. However, we are rewiring ```cat```'s output, so it will instead use ```hello2.txt``` as its output

```>>``` **is append instead** of **overwrite**

---

tail is a program used to display the tail end of a text file or piped data.

## Pipe "|"

We can use ```|``` to combine programs; ```|``` **takes the output of the program to the left** and **makes it the input of the program to the right**

```ls -l / | tail -n1```

This wires the output of **ls** and makes it the input of **tail**

These redirections of output are not something the programs know about; it's all set up **by the shell** 

---

## Root User

This is the **super** user that can read and write to everything

- if you were this user all the time, you could potentially destroy your computer, which *you don't want*
- however, sometimes you do want the root user privileges
- you can use ```sudo``` to access as the root user instead of the user you actually are
  - "do" as "su", or super
- ```sudo su``` gets you a shell as super user

---

### Sys 

- kernal is the core of your computer
  - ```cd /sys``` to look at core of computer

---

### Open files

```xdf-open lectures.html``` to open files (Linux)

