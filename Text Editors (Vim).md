Lecture 3 at 2:06pm on December 23rd, 2020

---

## Editors (Vim)

Vim is a **modal editor** (has multiple operating modes)

When you start up Vim, you are in **normal mode**, for reading and navigating around files

If you press `i`, you will be in **insert mode**, for putting things into the text buffer

- **Normal**: for moving around a file and making edits
- **Insert**: for inserting text
- **Replace**: for replacing text
- **Visual** (plain, line, or block): for selecting blocks of text
- **Command-line**: for running a command

---

`^V` is `Ctrl-V` or `<C-V>`

You can rebind keys if you'd like

---

Vim can take an argument, such as a file name

If you're in normal mode and you press `:`, you will be in **Vim's command shell**

**To save a file**, `:w`, 'w' for **write**

If you need help, `:help :w` for documentation on `:w`

---

Vim's model of buffers and windows is different

- it has a set of open files, and separately from that, you can have tabs, and tabs can have windows
- there isn't a one-to-one correspondence between buffers and windows
  - I.e., you can have the same file open in two different windows if you're working on separate sections of a file

---

Vim's interface **is a programming language**

There are built-in commands, such as movement commands 

### Key Combinations

**To move up down left right**

- use ```hjkl```

**Move forward and back by one word**

- use `w b or e`

****

**In normal mode**

`o` for open a new line **in insert mode**

`O` for **insert mode above**

`d` with a movement key like `w`

`u` for undo

`x` to delete a character

```yw``` to copy a word `p` to paste

---

## Visual Mode

you'd use this to select text; press `v`

`/` to search

---

**Vim RCs** are used to configure certain settings

