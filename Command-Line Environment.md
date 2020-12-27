Lecture 5 on December 27th, 2020 at 2:30pm

---

## Command-line Environment

`^C` - what the shell is doing for us is sending `SIGINT` which tells the program to stop itself

`man signal` to learn about the different signals

you can control these exceptions in your program if you'd like; you would write a ```handler``` 

there are certain ```SIG```s that can't be captured

---

### Jobs

You can kill jobs

-  To send this signal we can use the [`kill`](https://www.man7.org/linux/man-pages/man1/kill.1.html) command, with the syntax `kill -TERM <PID>`.

We can then continue a paused job in the foreground or in the background using [`fg`](https://www.man7.org/linux/man-pages/man1/fg.1p.html) or [`bg`](http://man7.org/linux/man-pages/man1/bg.1p.html), respectively.

---

## Terminal Multiplexer

- different workspaces at the same time, like two or more terminal windows open at the same time

you might want to run your editor and your program side by side, but this can be achieved by opening new terminal windows, using a terminal multiplexer is a more versatile solution.

it has the form `<C-b> x` where that means (1) press `Ctrl+b`, (2) release `Ctrl+b`, and then (3) press `x`

## `tmux`

- sessions 
  - windows (tabs)
    - panes

### Sessions

a session is an independent workspace with one or more windows

- `tmux` starts a new session.
- `tmux new -s NAME` starts it with that name.
- `tmux ls` lists the current sessions
- Within `tmux` typing `<C-b> d` detaches the current session
- `tmux a` attaches the last session. You can use `-t` flag to specify which

## Windows

 Equivalent to tabs in editors or browsers, they are visually separate parts of the same session

- `<C-b> c` Creates a new window. To close it you can just terminate the shells doing `<C-d>`
- `<C-b> N` Go to the *N* th window. Note they are numbered
- `<C-b> p` Goes to the previous window
- `<C-b> n` Goes to the next window
- `<C-b> ,` Rename the current window
- `<C-b> w` List current windows

## Panes

Like vim splits, panes let you have multiple shells in the same visual display.

- `<C-b> "` Split the current pane horizontally
- `<C-b> %` Split the current pane vertically
- `<C-b> <direction>` Move to the pane in the specified *direction*. Direction here means arrow keys.
- `<C-b> z` Toggle zoom for the current pane
- `<C-b> [` Start scrollback. You can then press `<space>` to start a selection and `<enter>` to copy that selection.
- `<C-b> <space>` Cycle through pane arrangements.

---

## Dot Files

aliases - remap commands using `alias` command

Many programs are configured using plain-text files known as *dotfiles*(because the file names begin with a `.`, e.g. `~/.vimrc`, so that they are hidden in the directory listing `ls` by default).

For `bash`, editing your `.bashrc` or `.bash_profile` will work in most systems. Here you can include commands that you want to run on startup, like the alias we just described or modifications to your `PATH` environment variable.

Some other examples of tools that can be configured through dotfiles are:

- `bash` - `~/.bashrc`, `~/.bash_profile`
- `git` - `~/.gitconfig`
- `vim` - `~/.vimrc` and the `~/.vim` folder
- `ssh` - `~/.ssh/config`
- `tmux` - `~/.tmux.conf`

---

## Symlinks

 They should be in their own folder, under version control, and **symlinked** into place using a script. This has the benefits of:

- **Easy installation**: if you log in to a new machine, applying your customizations will only take a minute.
- **Portability**: your tools will work the same way everywhere.
- **Synchronization**: you can update your dotfiles anywhere and keep them all in sync.
- **Change tracking**: you’re probably going to be maintaining your dotfiles for your entire programming career, and version history is nice to have for long-lived projects.

---

## Remote Machines

`ssh` - a secure shell that takes the responsibility of reaching where you want to go and opening up a session

To `ssh` into a server you execute a command as follows

```bash
ssh foo@bar.mit.edu
```

Here we are trying to ssh as user `foo` in server `bar.mit.edu`. The server can be specified with a URL (like `bar.mit.edu`) or an IP (something like `foobar@192.168.1.42`). Later we will see that if we modify ssh config file you can access just using something like `ssh bar`.

### Executing commands

An often overlooked feature of `ssh` is the ability to run commands directly.`ssh foobar@server ls` will execute `ls` in the home folder of foobar. 

