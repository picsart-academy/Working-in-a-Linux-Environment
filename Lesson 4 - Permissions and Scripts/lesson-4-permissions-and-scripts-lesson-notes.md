(The following information is from William Shott's book, The Linux Command Line) - https://www.amazon.com/Linux-Command-Line-2nd-Introduction/dp/1593279523/

# Lesson 4 - Permissions and Scripts

## Owners, group members, and everybody else

In the UNIX security model, a user may **own** files and directories. When a user owns a file or directory, the user has control over its access. Users can belong to a **group** consisting of one or more users who are given access to files and directories by their owners. An owner may also grant some set of access rights to everybody. To find out information about yout identity, use the `id` command.

Users are assigned a number called a user ID (uid), a group ID (gid) and may belong to additional groups. Ubuntu starts its numbering of regular user accounts at 1000. 

User accounts are defined in the `/etc/passwd` file, and groups are defined in the `/etc/group` file. When user accounts and groups are created, these files are modified along with `/etc/shadow`, which holds information about the user's password. 

For each user account, the `/etc/passwd` file defines 
- the user (login) name, 
- uid, 
- gid, 
- account's real name, 
- home directory, 
- login shell. 

## Reading, writing, and executing

```
-rw-r--r--
```

The first 10 characters of the listing are the file attributes. The first of these characters is the file type. 
- `-`, a regular file.
- `d`, a directory.
- `l`', a symbolic link. Notice that with symbolic links, the remaining file attributes are always rwxrwxrwx and are dummy values. The real file attributes are those of the file the symbolic link points to.
- `c`, a character special file. This file type refers to a device that handles data as a stream of bytes, such as a terminal or /dev/null.
- `b`, a block special file. This file type referes to a device that handles data in blocks, such as a hard drove or DVD drive.

The remaining nine characters of the file attributes, called the file mode, represent the read, write, and execute permissions for the file's owner, the file's group owner, and everybody else.

- `r` allows a file to be opened and read. Allows a directory's contents to be listed if the execute attribute is also set. 
- `w` allows a file to be written to or truncated (doesn't allow files to be renamed or deleted). The ability to delete or rename files is determined by directory attributes. For directories, allows files within a directory to be created, deleted, and renamed if the execute attribute is also set. 
- `x` allows a file to be treated as a program and executed. Program files written in scripting languages must also be set as readable to be executed. For directories, allows a directory to be entered, e.g. cd directory. 

### chmod - change file mode

Note. Only file's owner or the superuser can change the mode of a file or directory. 

Octal, binary, and file mode pattern

```
0 000 ---
1 001 --x
2 010 -w-
3 011 -wx
4 100 r--
5 101 r-x
6 110 rw-
7 111 rwx
```

`chmod 600 foo.txt` sets the foo.txt's permissions to rw-------

chmod also supports a symbolic notation for specifying file modes. 
- `u` user
- `g` group
- `o` others
- `a` all

If no character is specified, "all" will be assumed. 

```
u+x Add execute permission for the owner.
o-rw remove the read and write permissions from anyone besides the owner and group owner.
go=rw set the group owner and anyone besides the owner to have read and write permissions.
u+x,go=rw add execute to the user, and read/write to group and other.
```

### umask - set default permissions

Controls the default permission given to a file when it is created. 

- setuid bit (4000), (chmod u+s program) when applied to an executable file, it changes the effective user ID from that of the real user to that of the program's owner. When an ordinary user runs a program that is setuid root, the program runs with the effective privileges of the superuser. This allows the program to access files and directories that an ordinary user would normally be prohibited from accessing. 
- setgid bit (2000), (chmod g+s dir) changes the effective group ID from the real group ID of the real user to that of the file owner. If the setgid bit is set on a directory, newly created files in the directory will be given the group ownership of the directory rather the group ownership of the file's creator. 
- sticky bit (1000), (chmod +t dir) a holdover from ancient Unix, where it was possible to mark an executable file as "not swappable." On files, Linux ignores the sticky bit, but if applied to a directory, it prevents users from deleting or renaming files unless the user is either the owner of the directory, the owner of the file, or the superuser. Often used to control access to a shared directory, such as /tmp.

