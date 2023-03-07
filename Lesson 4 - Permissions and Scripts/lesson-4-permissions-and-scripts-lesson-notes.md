(The following information is partially from William Shott's book, The Linux Command Line) - https://www.amazon.com/Linux-Command-Line-2nd-Introduction/dp/1593279523/ and UNIX and Linux System Administration Handbook by Evi Nemeth, Garth Snyder, Trent Hein, Ben Whaley, Dan Mackin - https://www.amazon.com/UNIX-Linux-System-Administration-Handbook/dp/0134277554/

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
umask will remove the permissions for created files, for example, 
```
0002 is 
000 000 000 010 

which applied to the symbolic names would mean removing the write permission for others.
the default is
default: 000 110 110 110
umask:   000 000 000 010 (w is set to remove for others)
result:  000 110 110 100 (w removed for others)
```

umask 0000 won't have any effect.

### special permissions

- setuid bit (4000), (chmod u+s program) when applied to an executable file, it changes the effective user ID from that of the real user to that of the program's owner. When an ordinary user runs a program that is setuid root, the program runs with the effective privileges of the superuser. This allows the program to access files and directories that an ordinary user would normally be prohibited from accessing. 
- setgid bit (2000), (chmod g+s dir) changes the effective group ID from the real group ID of the real user to that of the file owner. If the setgid bit is set on a directory, newly created files in the directory will be given the group ownership of the directory rather the group ownership of the file's creator. 
- sticky bit (1000), (chmod +t dir) a holdover from ancient Unix, where it was possible to mark an executable file as "not swappable." On files, Linux ignores the sticky bit, but if applied to a directory, it prevents users from deleting or renaming files unless the user is either the owner of the directory, the owner of the file, or the superuser. Often used to control access to a shared directory, such as /tmp.

Special permissions might look like the following

```
-rwsr-xr-x (setuid program)
drwxrwsr-x (setgid directory)
drwxrwxrwt (sticky bit directory)
```

## Managing Users

### /etc/passwd

It's a list of users recognized by the system. Historically, each user's encrypted password was also stored in the /etc/passwd file, now moved to a separated file, /etc/shadow. Now you can see the `x` in place of the password.

### login names

Login names (usernames) must be unique and Linux currenlty limits logins to 32 characters. Ubuntu allows logins starting with or entirely consisting of numbers and other special characters. Login names are case sensitive, while the lowercase names are traditional. 


Passwords were encrypted with DES, then MD5-based, and finally moved to salted SHA-512-based password hashes. 
Encrypted passwords are of constant length (86 characters for SHA-512, 34 characters for MD5, and 13 characters for DES).

MD5-encrypted password fields in the shadow password file always start with `$1$` or `$md5$`. Blowfish passwords start with `$2$`, SHA-256 passwords start with `$5$` and SHA-512 passwords with `$6$`.

### UID and GID

By definition, root has UID 0. Most systems also define pseudo-users such as bin and daemon to be the owners of commands or configuration files. Such users also have fake shells, /bin/false or /usr/sbin/nologin.

GID 0 is reserved for the group called root, system, or wheel. The system uses several predefined groups for its own housekeeping. In ancient times, when computing power was expensive, groups were used for accounting puproses so that the right department could be charged for your seconds of CPU time, minutes of login time, and kilobytes of disk used. Today, groups are used primarily to share access to files.

The `/etc/group` file defines the groups. To facilitate collaboration, you can set the setgid bit (2000) on a directory or mount filesystems with the grpid option. 

### home directory

Home directories are where login shells look for account-specific customizations such as shell aliases and environment variables, as well as SSH keys, server fingerprints, and other program state.

### login shell

The login shell is normally a command interpreter, but can be any program. bash is the default for Linux. A sysadmin can always change a user's shell by editing the passwrd file with vipw. 

### /etc/shadow

Shadow password file is readable only by the superuser and serves to keep ecnrypted passwords safe. Each line of /etc/shadow contains:
- Login name, the same as in /etc/passwd. 
- Encrypted password.
- Date of last password change, filled in by the `passwd` command.
- Min. number of days between password changes.
- Max. number of days between password changes.
- Number of days in advanced to warn users about password expiration.
- Days after password expiration that account is disabled.
- Account expiration date. Never expires if left blank.
- A field reserved for future use which is currently always empty.

### /etc/group

Lines described
- Group name
- Ecnrypted password or a placeholder
- GID number
- List of members, separated by commas

It's possible to set a group password that allows arbitrary users to enter the group with the newgrp command. The feature rarely used. The group password can be set with gpasswd, which under Linux stores the encrypted password in the /etc/gshadow file.

If a user defaults to a particular group in /etc/passwd but does not appear to be in that group according to /etc/group, /etc/passwd wins the argument. The group memberships granted at login time are the union of those found in the passwd and group files.

Group membership can also serve as a marker for other contexts or privileges. You can configure sudo so that everyone in the "admin" group automatically has sudo privileges. 

Linux supplies the **groupadd**, **groupmod**, and **groupdel** commands to create, modify, and delete groups.

### manually creating a user
1. Edit the `passwd` and `shadow` files to define the user's account.
2. Add the user to the `/etc/group` file (not really necessary).
3. Set an initial password
4. Create, chown, and chmod the user's home directory.
5. Configure roles and permissions.

```sudo passwd username``` sets the password for the username.

Startup files traditionally begin with a dot and end with the letters **rc**, short for "run command," a relic of the CTSS operating system.

`Sample startup files are traditionally kept in /etc/skel`. If you customize your systems' startup file examples, `/usr/local/etc/skel` is a reasonable place to put the modified copies. 

Setting home directory permissions and ownerships
```
sudo chown -R newuser:newgroup ~newuser
```

Scripts to help ease the process of user creation
- useradd
- adduser

deluser deletes the user, although the files/folders won't be deleted unless specified in /etc/deluser.conf

`usermod -L user` locks the user, and `usermod -U user` unlocks the user.

## Changing identities

- The `su` command allows you to assume the identity of another user and either start a new shell session with that user's ID or issue a single command as that user.
- The `sudo` command allows an administrator to set up a configuration file called `/etc/sudoers` and define specific commands that particular users are permitted to execute under an assumed identity.

### su - substitute user

Used to start a shell as another user. 

```
su [-[l]] [user]
```

If the `-l` option is included, the resulting shell session is a login shell for the specified user, aka, the user's environment is loaded and the working directory is changed to the user's home directory. If the user is not specified, the superuser is assumed. 

```
su knuth
su - knuth
su knuth -c 'ls -l'
```

### sudo - execute a command as another user

The admin can configure sudo to allow an ordinary user to execute commands as a different user (usually the superuser) in a controlled way. 
**Note.** The use of `sudo` does not require access to the superuser's password. Authenticating using sudo requires the user's own password. 

```
sudo command_name
```

Sudo doesn't start a new shell nor does it load another user's environment. `sudo -i` will start an interactive superuser session. To see the granted privileges, use `sudo -l`.

By default, Ubuntu disables logins to the root account (by failing to set a password for the account) and instead uses sudo to grant superuser privileges. The initial user account is granted full access to superuser privileges via sudo and may grant similar powers to subsequent user accounts.

### chown - change file owner and group

```
chown [owner][:[group]] file...
```

```
chown bob file - changes the ownership of the file from its current owner to user bob.
chown bob:users file - changes the ownership to bob and the group to users.
chown :admins file - changes the group owner to the group admins.
chown bob: file - changes the file owner to user bob and changes the group owner to the login group of user bob.
```

An old chgrp may be used for group ownership change, as well.

Adding new groups
```
groupadd new_group
```

Adding user to a new group (appending)
```
usermod -a -G new_group username
```

See user's groups ```groups username```

Changing password is done by the `passwd` command.
