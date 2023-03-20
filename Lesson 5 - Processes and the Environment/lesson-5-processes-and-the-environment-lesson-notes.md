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
- T: Stopped. 
- Z: Zombie. This is a child process that has terminated but has not been cleaned up by its parent.
- <: A high-priority process. This property of a process is called **niceness**. A process with high priority is said to be less nice because it's taking more of the CPU's time, which leaves less for everybody else.
- N: A low-priority process. A process with low priority (a nice process) will get processor time only after other processes with higher priority have been serviced.

`ps aux` displays the processes belonging to every user. 
- USER: User ID, the owner
- %CPU: CPU usage in percent.
- %MEM: Memory usage in percent.
- VSZ: Virtual memory size.
- RSS: Resident set size. Amount of physical memory (RAM) the process is using in kilobytes.
- START: Time when process started.

To see a more dynamic view of the machine's activity, we use the `top` command.

Row 1:
- top, the name of the program
- 14:59:20, the current time of day
- up 6:30, uptime
- 2 users, two logged in users
- load average: the number of processes that are waiting to run; that is, the number of processes that are in a runnable state and are sharing the CPU. Three values: the first is the average for the last 60 seconds, the next the previous 5 minutes, and finally the previous 15 minutes. Values less than 1.0 indicate that the machine is not busy.

Row 2:
- Tasks: Summarizes the number of processes and their various process states.

Row 3:
- Cpu(s): This row describes the character of the activities that the CPU is performing.
- 0.7%us, 0.7 percent of the CPU is being used for user processes.
- 1.0sy, 1.0 percent of the CPU is being used for system (kernel) processes.
- 0.0%ni, 0.0 percent of the CPU is being used by "nice" processes.
- 98.3%id, 98.3 percent of the CPU is idle.
- 0.0%wa, 0.0 percent is waiting for I/O.
- hi, time spent processing hardware interrupts.
- si, time spent in these softirqs (if long or complex processing needs to be done, these tasks are deferred using a mechanism called softirqs, which are scheduled independently, can run on any CPU, can even run concurrently).
- st, steal time; relevant in virtualized environments. Time when the real CPU was not available to the current virtual machine - it was stolen from the VM by the hypervisor.

Row 4, 5:
- Mem: Shows how physcial RAM is being used
- Swap: how swap space (virtual memory) is being used.

Note: a mebibyte (MiB) is a unit of measurement. mebi comes from the binary system of data measurement that is based on powers of two. A mebibyte equals 2^20 or 1,048,576 bytes. One mebibyte equals 1024 kibibytes, and 1024 mebibytes equal one gibibyte. 

Columns
- PID: Process ID.
- USER: The owner of the process.
- PR: Process priority.
- NI: The nice value of the process.
- VIRT: Amount of virtual memory used by the process.
- RES: Amount of resident memory used by the process.
- SHR: Amount of shared memory used by the process.
- S: Status of the process. 
- %CPU: The share of CPU time used by the process since the last update.
- %MEM: The share of physical memory used.
- TIME+: Total CPU time used by the task in hundredths of a second.
- COMMAND: The command name or command line (name + options).

## interrupting, background, foreground

Interrupting a process: Ctrl-C

Putting in background
`xlogo &`
[1] 28236
The shell is telling us that we have started job number 1 and that it has PID 28236. If we run ps, we can see our process.

`jobs` lists the jobs

A process in the background is immune from terminal keyboard input, including any attempt to interrupts with Ctrl-C. To return a process to the foreground, use the `fg` command:

```
fg %1
```

Followed by the percent and the number of the job. If we have only one background job, the jobspec is optional. 

Ctrl-Z stops the process. We can further use fg/bg.

## signals

```
kill 28401
```
kills process with the specified PID.

It sends signals to processes.

- Ctrl-C: INT (interrupt) signal is sent.
- Ctrl-Z: TSTP (terminal stop)

```
kill -signal PID
```

- 1 (HUP): Hang up. Old days. Almost similar to closing a terminal session. The signal is also used by many daemon programs to cause reinitialization. This means that when a daemon is sent this signal, it will restart and reread its configuration file. 
- 2 (INT): Interrupt. 
- 9 (KILL): Kills is never actually sent to the target program, rather the kernel immediately terminates the process. 
- 15 (TERM): Terminate. The default signal sent by the kill command. If a program is still alive enough to receive signals, it will terminate.
- 18 (CONT): Continue. Will restore a process after a STOP or signal. This signal is sent by bg/fg.
- 19 (STOP): Causes a process to pause without terminating. Can't be ignored.
- 20 (TSTP): Terminal stop. May be ignored.

```
kill -9 1111
kill -INT 111
kill -SIGINT 111
```

sending signals to multiple processes
```
killall programname
```

```
pstree - Outputs a process list arranged in a tree-like pattern.
vmstat - Outputs a snapshot of system resource usage including memory, swap, and disk i/o.
xload - A graphical program that draws a graph showing system load over time.
tload - Draws the graph in terminal.
```
