From the main OverTheWire (OTW) bandit site, go ahead and click on Level 0 on the left hand side to get started.

The challenge reads:

![screenshot from 2018-12-12 03-34-00](https://user-images.githubusercontent.com/41026969/49857122-b90cba00-fdbf-11e8-8285-f5c19c00ee6c.png)

To secure shell (ssh) into bandit.labs.overthewire.org we:

ssh bandit.labs.overthewire.org 

But we need to add the follwing from the challenge description:
  Port: 2220
  Username: Bandit0

Reading the man page for ssh:

ssh (SSH client) is a program for logging into a remote machine and for executing commands on a remote machine. It is intended to replace rlogin and rsh, and provide secure encrypted communications between two untrusted hosts over an insecure network. X11 connections and arbitrary TCP ports can also be forwarded over the secure channel.

ssh connects and logs into the specified hostname (with optional user name). The user must prove his/her identity to the remote machine using one of several methods depending on the protocol version used (see below). 

  
### Solution:

```
BurntxN00dle@ubuntu:~$ ssh bandit.labs.overthewire.org -p 2220 -l bandit0
```

From the challenge description, we know the password is ```bandit0```

Typing ```bandit0``` when prompted for the password gets you in.
