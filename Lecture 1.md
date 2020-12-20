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

