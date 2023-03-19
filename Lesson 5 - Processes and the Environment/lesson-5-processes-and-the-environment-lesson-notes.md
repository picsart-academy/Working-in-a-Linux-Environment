(The following information is partially from William Shott's book, The Linux Command Line) - https://www.amazon.com/Linux-Command-Line-2nd-Introduction/dp/1593279523/

# Lesson 5 - Processes and the Environment

When a system starts up, the kernel initiates a few of its own activities as processes and launches a program called init.
init, in turn, runs a series of shell scripts (in /etc) called init scripts, which start all the system services. Many of these services are implemented as daemon programs.

Each process is assigned a number called a process ID (PID). PIDs are assigned in ascending order, with init always getting PID 1. The kernel also keeps track of the memory assigned to eac process, as well as the processes' readiness to resumse execution.
Processes also have owners and user IDs, effective user IDs, and so on.

`ps` to view processes.
 - TTY is short for "teletype" and refers to the controlling terminal for the process. 
 - The TIME field is the amount of CPU time consumed by the process.
 
 Adding the `x` option tells `ps` to show all of our processes regardless of what terminal (if any) they are controlled by.
 The presence of `?` in the TTY indicates no controlling terminal.
The option shows every process we own.

Note for practice: Pipe the output with less.
 
STAT is short for state and reveals the current status of the process.
- R: Running. 
- S: Sleeping. Waiting for an event, such as a keystroke or network packet.
- D: Uninterruptible sleep. Waiting for I/O such as a disk drive.
