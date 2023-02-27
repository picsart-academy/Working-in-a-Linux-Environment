This file is a lecture note of the **"Working in a Linux Environment"** module taught at Picsart Academy's [Sandbox department](https://picsart.academy/sandbox).
For more info, visit https://picsart.academy/

# Lesson 1. Exploring the System (lesson notes)
(The following information is from Wikipedia)
Linux is a family of open-source Unix-like operating systems based on the Linux kernel, an operating system kernel first released on September 19, 1991, by Linus Torvalds. Linux is typically packaged as a Linux distribution, which includes the kernel and supporting system software and libraries, many of which are provided by the GNU Project. 

The Linux kernel is a free, open-source, monolithic, multitasking, Unix-like operating system kernel. It is provided under the GNU GPL v2 only but contains files under other compatible licenses. 

![Kernel Dev Summit](https://sites.google.com/site/kernelsummit2012/_/rsrc/1326506421196/home/ks-prague-display.jpg)
![Kernel Maintainers, 2022](https://static.lwn.net/images/conf/2022/lpc/ms-group-sm.png)

The Linux kernel repository: https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
General link: https://kernel.org/

The kernel is written in C, Rust, and the assembly languages.

Linux is deployed on a wide variety of computer systems, such as embedded devices, mobile devices (including its use in the Android OS), personal computers, servers, mainframes, and supercomputers. The list includes routers, automation controls, smart home devices, video game consoles, televisions, automobiles (Tesla, Audi, Mercedes-Benz, Hyundai, and Toyota), and spacecraft (Falcon 9 rocket, Dragon crew capsule and the Perseverance rover). 

The Linux development discussions take place on the Linux kernel mailing list (LKML), http://vger.kernel.org/vger-lists.html 

Popular Linux distributions include Debian, Fedora Linux, and Ubuntu. Commercial distributions include Red Hat Enterprise Linux and SUSE Linux Enterprise. 

Over 96.4% of the top 1 million web servers' operatings systems are Linux.

The GNU Project is a free software, mass collaboration project announced by Richard Stallman on September 27, 1983. Its goal is to give computer users freedom and control in their use of their computers and computing devices by collaboratively developing and publishing software that gives everyone the rights to freely run the software, copy and distribute it, study it, and modify it. GNU software grants these rights in its license.

(The following information is from William Shott's book, The Linux Command Line) - https://www.amazon.com/Linux-Command-Line-2nd-Introduction/dp/1593279523/

## The Shell
The shell is a program that takes keyboard commands and passes them to the operating system to carry out. Almost all Linux distributions spply a shell program from the GNU Project called bash
The name is an acronym for bourne-again shell, a reference to the fact that bash is an enhanced replacement for sh, the original Unix shell program written by Steve Bourne.

If the last character of the prompt is a hash mark (#) rather than a dollar sign, the terminal session has superuser privileges. 

Commands
- **date**, displays the current time and date.
- **cal**, displays a calendar of the current month.
- **df**, displays the current amount of free space on disk drives.
- **free**, displays the amount of free memory.
- **exit**, ending a terminal session.

## File System Tree
The first directory in the file system is called the root directory. Unlike Windows, which has a separate file system tree for each storage device, Unix-like systems such as Linux always have a single file system tree, regardless of how many drives or storage devices are attached to the computer.

- **pwd**, displays the current working directory.

When we first log in to our system (or start a terminal emulator session), our current working directory is set to our home directory.

- **ls**, lists the files and directories in the current working directory.
- **cd**, changes the working directory.

Pathnames can be specified as absolute pathnames or as relative pathnames.
The . notation refers to the working directory, and the .. notation refers to the working directory's parent directory.

Filenames that begin with a period character are hidden. ls -a lists them. 
**cd -**, changes the working directory to the previous working directory.
**cd**, changes the working directory to your home directory.
**cd ~user_name**, changes the working directory to the home directory of user_name.

## Exploring the System
We can specify multiple directories with the ls command: ls /dir1 /dir2
**ls -l**, the long format.

Commands are often followed by one or more options that modify their behavior and by one or more arguments, the items upon which the commands act. 
command -options arguments

### ls options
**-a** --all List all files
**-A** --almost-all List all files, except . and ..
**-d** --directory List the directory itself, not its contents.
**-F** --classify Append an indicator character to the end of each listed name.
**-h** --human-readable Display file sizes in human-readable format rather than in bytes.
**-l** Display in long format.
**-r** --reverse Display the results in reverse order.
**-S** sort the results by file size.
**-t** Sort by modification time.

- **file**, display file type

- **less**, see the contents of a text file

- **/**, the root
- **/bin**, binaries (programs)
- **/boot**, contains the linux kernel, initial RAM disk image, and the boot loader. 
- **/dev**, contains device nodes. Here is where the kernel maintains a list of all the devices it understands.
- **/etc**, contains all the system-wide configuration files. It also contains a collection fo shell scripts that start each of the system services at boot time. **/etc/crontab** defines when automated jobs will run; **/etc/fstab** is a table of storage devices and their associated mount points; **/etc/passwd**, a list of the user accounts.
- **/home**, the home directory.
- **/lib**, contains shared library files.
- **/lost+found**, it is used in the case of a partial recovery from a file system corruption event. 
- **/media**, contains the mount points for removable media such as USB drives, CR-ROMs, and so on.
- **/mnt**, On older Linux systems, the directory contains mount points for removable devices that have been mounted manually.
- **/opt**, used to install optional software. Used to hold commercial software products that might be installed on the system.
- **/proc**, a virtual file system maintained by the kernel. The "files" it contains are peepholes into the kernel itself.
- **/root**, home dir for root.
- **/sbin**, contains system binaries. 
- **/tmp**, storage of temporary, transient files created by various programs.
- **/usr**, contains all the programs and support files used by regular users.
- **/usr/bin**, contains the executable programs installed by your Linux distribution. 
- **/usr/lib**, the shared libraries for the programs in /usr/bin.
- **/usr/local**, programs that are not included with your distribution but are intended for system-wide use are installed. Programs compiled from source code are normally installed in /usr/local/bin. 
- **/usr/sbin**, more system administration programs.
- **/usr/share**, shared ddata used by programs in /usr/bin (default config files, icons, screen backgrounds, sound files, and so on).
- **/usr/share/doc**, documentation files organized by package.
- **/var**, data that is likely to change is stored here. Various databases, spool files, user mail, and so on.
- **/var/log**, log files, records of various system activity. 

