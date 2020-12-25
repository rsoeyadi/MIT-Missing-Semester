Lecture 4 at 1:27pm on 12/25/2020

---

## Data Wrangling

One piece of data to another representation of data = **data wrangling**

Whenever you use the pipe `|` symbol, you are doing data wrangling since you're taking output and making it input

```sed``` is a stream editor, and this programming language lets use edit the stream that is given

- one of the most common things to do is to run replacement expressions on an input stream

  

## Regular Expressions

a powerful way to match text, most commonly

a number of special characters to use; essentially generates a program to search

Regular expressions are usually (though not always) surrounded by `/`

Exactly which characters do what vary somewhat between different implementations of regular expressions, which is a source of great frustration. Very common patterns are:

- `.` means “any single character” except newline
- `*` zero or more of the preceding match
- `+` one or more of the preceding match
- `[abc]` any one character of `a`, `b`, and `c`
- `(RX1|RX2)` either something that matches `RX1` or `RX2`
- `^` the start of the line
- `$` the end of the line

 ex.

```bash
echo 'aba' | sed 's/[ab]//'
```

we are telling the pattern to replace any character that is either 'a' or 'b' with nothing

regular expressions will match once and run the replacement **once** unless you use the `g` modifier to apply it to everything matching

**therefore, in this expression above, it looks at the input of 'aba' and looks to see if there is an 'a' or a 'b'. If there is, then it gets replaced by nothing**

---

## Capture Groups

If you want to remember a value and reuse it later using `( )`

---

## Regex Debuggers

Debuggers can tell you what everything is doing, and how it's affecting your inputs

