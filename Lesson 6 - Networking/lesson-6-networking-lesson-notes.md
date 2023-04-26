# Lesson 6 - Networking

The ping command sends a special network packet called an ICMP ECHO_REQUEST to a specified host. Most network devices receiving this packet will reply to it, allowing the network connection to be verified. Although, it's possible to configure most network devices (including Linux hosts) to ignore these packets.

The traceroute program lists all the "hops" network traffic takes to get from the local system to a specified host. 

wget is useful for downloading content from both web and FTP. 

```
wget http://linuxcommand.org/index.php
```

SSH (secure shell) solves the two basic problems of secure communication with a remote host.
• It authenticates that the remote host is who it says it is (thus preventing so-called man-in-the-middle attacks).
• It encrypts all of the communications between the local and remote hosts.

SSH consists of two parts. An SSH server runs on the remote host, listening for incoming connections, by default, on port 22, while an SSH client is used on the local system to communicate with the remote server.

Besides opening a shell session on a remote system, ssh allows us to execute a single command on a remote system.

It’s possible to use this technique in more interesting ways, such as the following example in which we perform an ls on the remote system and redirect the output to a file on the local system:

```
ssh remote-sys 'ls *' > dirlist.txt
```

scp (secure copy), is used much like the familiar cp program to copy files. The most notable difference is that the source or destination pathnames may be preceded with the name of a remote host, followed by a colon char- acter. For example, if we wanted to copy a document named document.txt from our home directory on the remote system, remote-sys, to the current working directory on our local system, we could do this:

```
scp remote-sys:document.txt .
```

As with ssh, you may apply a username to the beginning of the remote host’s name if the desired remote host account name does not match that of the local system.

```
scp bob@remote-sys:document.txt .
```

