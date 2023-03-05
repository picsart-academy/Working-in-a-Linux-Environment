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

