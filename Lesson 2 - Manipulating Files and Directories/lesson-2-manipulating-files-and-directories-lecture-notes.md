# Lesson 2 - Manipulating Files and Directories

## Wildcards
The shell provides special characters, called wildcards, to help you rapidly specify groups of filenames.
Using wildcards is also known as globbing.

\* Matches any characters

? Matches any single character

[characters] Matches any character that is a member of the set characters

[!characters] Matches any character that is not a member of the set characters

[[:class:]] Matches any character that is a member of the specified class

**Classes**

[:alnum:] alphanumeric character

[:alpha:] alphabetic character

[:digit:] any numeral

[:lower:] any lowercase

[:upper:] any uppercase letter

**Examples**

```
* All files

g* Any file beginning with g

b*.txt Any file beginning with b followed by any characters and ending with .txt

Data??? Any file beginning with Data followed by exactly three characters

[abc]* Any file beginning with either a, b, or c

BACKUP.[0-9][0-9][0-9] Any file beginning with BACKUP. followed by exactly three numerals

[[:upper]]* Any file beginning with an uppercase letter

[![:digit:]]* Any file not beginning with a numeral

*[[:lower:]123] Any file ending with a lowercase letter or the numerals 1, 2, 3
```

## mkdir - create directories

```
mkdir dir1

mkdir dir1 dir2
```

## cp - copy files and directories

```
cp item1 item2

cp item1 item2 directory

-a, --archive. copy the files and directories and all of their attributes, including ownerships and permissions. 

-i, --interactive, before overwriting an existing file, prompt the user for confirmation.

-r, --recursive, recursively copy directories and their contents.

-u, --update, when copying files from one directory to another, only copy files that either don't exist or are newer than the existing corresponding files in the destination directory.

-v, --verbose, display informative messages as the copy is performed.
```

## mv - move/rename files

```
mv item1 item2

mv item1 item2 directory

-i, --interactive, before overwriting an existing file, prompt the user for confirmation.

-u, --update, when moving files from one directory to another, only move files that either don't exist or are newer than the existing.

-v, --verbose
```

## rm - remove files and directories

```
rm item1 item2

-i, -r, -v

-f, --force, ignore nonexistent files and do not prompt.
```

## ln - create links

Create either hard or soft (symbolic) links.

```
ln file link #create hard link

ln -s item link #create symbolic link
```

* a hard link cannot reference a file outside its own file system.
* a hard link may not reference a directory

Symbolic links work by creating a special type of file that contains a text pointer to the referenced file or directory. If you write something to the symbolic link, the referenced file is written to. On deletion only the symbolic link is deleted. If the file is deleted before the symbolic link, the link will continue to exist but will point to nothing (a broken link). 

## Commands

A command can be one of four different things.
* An executable program.
* A command buil into the shell itself (the cd command).
* A shell function.
* An alias.

- **type**, display a command's type.
- **which**, display an exe's location (works only for executables).
- **help**, get help for shell builtins.
- **--help**, display usage information.
- **man**, display a program's manual page

Man page organization
- 1 User commands
- 2 Programming interfaces for kernel system calls
- 3 Programming interfaces to the C library
- 4 Special files such as device nodes and drivers
- 5 File formats
- 6 Games and amusements such as screen savers
- 7 Miscellaneous
- 8 System administration commands

```
man 5 passwd
```

- **appropos**, display appropriate commands.
- **whatis**, display one-line manual page descriptions.
- **info**, display a program's info entry.
- **alias**, create aliases for commands.
- **unalias**, remove an alias.

To see all the aliases defined in the environment, use the alias command without arguments.
