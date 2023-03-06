(The following information is from William Shott's book, The Linux Command Line) - https://www.amazon.com/Linux-Command-Line-2nd-Introduction/dp/1593279523/

# Lesson 3 - Redirection, Expansion, Quoting, and Tricks

## Redirection

Programs such as ls send their results to a special file called standard output (stdout) and their status messages to standard error (stderr).
Many programs take input from a facility called standard input (stdin).

```
ls -l /usr/bin > output.txt
```

Redirecting the output to the output.txt file.

A trick to quickly empty the contents of a file.
```
> output.txt
```

Use >> to append instead of overwriting.

Files have descriptors, numbers that help differentiate them.
input - 0, output - 1, error - 2.

To redirect standard error
```
ls -l /bin/usr 2> error.txt
```

To capture all
```
ls -. /bin/usr > output.txt 2>&1
```

The second approach
```
ls -l /bin/usr &> output.txt
```

To suppress error messages from a command
```
ls -l /bin/usr 2> /dev/null
```

### cat - concatenate files
`cat filename` - reads one or more files and copies them to standard output.
It can also be used to join files together.
`cat movie.mpeg.0* > movie.mpeg`

`Tip:` Wildcards expand in sorted order.

`cat` with no arguments, reads from standard input (keyobard). `cat > res.txt` redirects the standard input into res.txt.

## Pipelines

Using the pipe, `|`, the standard output of one command can be piped into the standard input of another.

```
ls -l /usr/bin | less
```

It's possible to put several commands together into a pipeline. The commands used this way are referred to as filters. 

```
ls /bin /usr/bin | sort | less
```

### uniq - report or omit repeated lines

`uniq` accepts a sorted list of data from either standard input or a single filename argument and, by default, removes any duplicates from the list. 

To see the the list of duplicates, use `-d` option.

```
ls /bin /usr/bin | sort | uniq | less
ls /bin /usr/bin | sort | uniq -d | less
```

### wc - print line, word, and byte counts

The `wc` (word count) command is used to display the number of lines, words, and bytes contained in files.

```
wc output.tx
```

The `-l` option limits the output to report only lines.

To see the number of items we have in the sorted list:

```
ls /bin /usr/bin | sort | uniq | wc -l
```

### grep - print lines matching a pattern

```
grep pattern filename
```

Suppose we wanted to find all the files in our list of programs that had the word **zip** embedded in the name.

```
ls /bin /usr/bin | sort | uniq | grep zip
```

`-i` option causes grep to ignore case when performing the search (normally searches are case sensitive).
`-v` telling grep to print only those lines that do not match the pattern.

### head/tail - print first/last part of files

The `head` command prints the first 10 lines of a file, and the `tail` command prints the last 10 lines.  The number of lines can be adjusted with the `-n` option.

```
head -n 5 output.txt
tail -n 5 output.txt
```

Also can be used in pipelines
```
ls /usr/bin | tail -n 5
```

tail has an option that allows you to view files in real time. Useful, for example, when watching the progress of log files as they are being written. 

```
tail -f /var/log/messages
```

### tee - read from stdin and output to stdout and files

The program reads standard input and copies it to both standard output (allowing the data to continue down the pipeline) and to one or more files. Useful for capturing a pipeline's contents at an intermediate stage of processing. 

```
ls /usr/bin | tee ls.txt | grep zip
```

## Expansion

With expansion, we enter something, and it is expanded into something else before the shell acts upon it. For example, `echo *` doesn't print the star `(*)`, but echoes the contents of the folder. The shell expands the star into the names of the files in the current working directory before the `echo` command is executed. 

### Pathname expansion

The mechanism by which wildcards work is called pathname expansion. Examples:

```
echo D*
echo *s
echo [[:upper:]]*
```

### Tilde expansion

`~` used at the beginning of a word expands into the name of the home directory of the named user or, if no user is named, the home directory of the current user.

### Arithmetic expansion

Arithmetic expansions allow us to use the shell prompt as a calculator: `echo $((2 + 2))`. Arithmetic expansion uses the form `$((expression))`, where you can use `+, -, *, /, $, **`.

### Brace expansion

With **brace expansion** you can create multiple text strings from a pattern containing braces.

```
echo Front-{A,B,C}-Back
```

prints

```
Front-A-Back Front-B-Back Front-C-Back
```

Patterns to be brace expanded may contain a leading portion called a preamble and a trailing portion called a postscript. The brace expression itself may contain either a comma-separated list of strings or a range of integers or single characters.

```
echo Number_{1..3}
prints Number_1 Number_2 Number_3

echo {Z..A}
prints Z Y X W V U T S R Q P O N M L K J I H G F E D C B A
```

Brace expansions may be nested

```
echo a{A{1,2},B{3,4}}b
prints aA1b aA2b aB3b aB4b
```

Usefulness examples
```
mkdir {2012..2023}-{01..12}
```

### Parameter expansion

Parameter expansion includes the dollar sign ($) put in front of the variable.
```
echo $USER
```

To see the list of available variables, try `printenv | less`

### Command substitution

Allows us to use the output of a command as an expansion.

```
echo $(ls)
```

```
ls -l $(which cp)
```

```
file $(ls -d /usr/bin/* | grep zip)
```

Older shell programs use bacquotes instead of the dollar sign and parentheses (bash also supports this syntax). 

### Quoting

If we place text inside double quotes, all the special characters used by the shell lose their special meaning and are treated as ordinary characters.
```
ls -l "two words.txt"
```

**Note.** Parameter expansion, arithmetic expansion, and command substitution still take place within double quotes.

Also note the difference between `echo $(cal)` and `echo "$(cal)".

**To suppres all expansions, use single quotes**.

To escape characters, use backslash
```
echo "The balance is \$5.oo"
```

Note. Within single quotes, the backslash loses its special meaning and is treated as an ordinary character.

In addition to its role as the escape character, the backslash is used as part of a notation to represent certain special characters called control codes. The first 32 characters in the ASCII coding scheme are used to transmit commands to Teletype-like devices. The idea behind this representation using the backslash originated in the C programming language and has been adopted by many others, including the shell.

 Adding `-e` option to echo will enable interpretation of escape sequences. 


## Keyboard Tricks

- `Ctrl-A`, move cursor to the beginning of line.
- `Ctrl-E`, move cursor to the end of the line.
- `Ctrl-F`, same as the right arrow key.
- `Ctrl-B`, same es the left arrow key.
- `Alt-F`, move cursor forward one word.
- `Alt-B`, move cursor backward one word.
- `Ctrl-L`, same as the clear command (although might behave slightly differently for some shells).

### modifying text

- `Ctrl-D`, delete the character at the cursor location.
- `Ctrl-T`, transpose (exchange) the character at the cursor location with the one preceding it.
- `Alt-T`, transpose the word at the cursor location with the one preceding it.
- `Alt-L`, convert the characters from the cursor location to the end of the word to lowercase.
- `Alt-U`, convert the characters from the cursor location to the end of the word to uppercase.

### cut and paste (kill and yank)

Items that are cut are stored in a buffer (a temporary storage area in memory) called the kill-ring.

- `Ctrl-K`, kill text from the cursor location to the end of line.
- `Ctrl-U`, kill text from the cursor location to the beginning of the line.
- `Alt-D`, kill text from the cursor location to the end of the current word.
- `Alt-Backspace`, kill text from the cursor location to the beginning of the current word. If the cursor is at the beginning of a word, kill the previous word.
- `Ctrl-Y`, yank text from the kill-ring and insert it at the cursor location.

### completion

Completion occurs when you press the `TAB` key while typing a command.

Completion will also work on variables (if the beginning of the word is a $), usernames (if the word begins with ~), commands (if the word is the first word on the line), and hostnames (if the beginning of the word is @). Hostname completion works only for hostnames listed in `/etc/hosts`.

- `Alt-?`, display a list of possible completions. On most systems pressing the TAB a second time will do the same.
- `Alt-*`, insert all possible completions.

### history

To view the contents, type `history'.

To use the line in the history, use the exclamation point: `!88` to run the 88th command.

To start incremental search, press `Ctrl-R`.

- `Alt-<`, move to the beginning (top) of the history list.
- `Alt->`, move the the end.
- `Ctrl-O`, execute the current item in the history list and advance to the next one.

### history expansion

- `!!`, repeat the last command
- `!number`, repeat history list item number.
- `!string`, repeat last history list item starting with string.
- `!?string`, repeat last history list item containing string.
