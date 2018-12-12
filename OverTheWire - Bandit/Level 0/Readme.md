From the main OverTheWire (OTW) bandit site, go ahead and click on Level 0 on the left hand side to get started.

The challenge reads:

![screenshot from 2018-12-12 03-34-00](https://user-images.githubusercontent.com/41026969/49857122-b90cba00-fdbf-11e8-8285-f5c19c00ee6c.png)

To secure shell (ssh) into bandit.labs.overthewire.org we would normally:

ssh bandit.labs.overthewire.org 

But... we need to add the follwing from the challenge description:
  Port: 2220
  Username: Bandit0

Reading the man page for ssh:
```
ssh connects and logs into the specified hostname (with optional user name). The user must prove 
his/her identity to the remote machine using one of several methods depending on 
the protocol version used... 
```

To specifiy the user (from the ssh manpage):
```
-l login_name
Specifies the user to log in as on the remote machine.
```

To specificy the port (from the ssh manpage):
```
-l login_name
Specifies the user to log in as on the remote machine. 
```
Combining the two, we get the solution:

### Solution:

```
BurntxN00dle@ubuntu:~$ ssh bandit.labs.overthewire.org -p 2220 -l bandit0
```
You may be asked ```Are you sure you want to continue connecting``` Just type ```yes```

Afterwards, we will be prompted for the password, and we know from the challenge description the password
is simply ```bandit0``` typing that in gets us in!
