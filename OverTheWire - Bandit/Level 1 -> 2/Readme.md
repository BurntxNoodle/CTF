You should be able to exit out of the ssh by just typing ```quit```

Now that you have the password for the next level, you can connect to the next level, except
that you have to change the username to specification ```-l banditX``` where x is the level you are
accessing. 

Reading the challenge:

![screenshot from 2018-12-14 00-15-06](https://user-images.githubusercontent.com/41026969/49984282-6b19c280-ff35-11e8-84ce-70be47325c2a.png)

To connect to bandit level 1:
```
BurntxN00dle@ubuntu:~$ ssh bandit.labs.overthewire.org -p 2220 -l bandit1
```

Googling on how to cat a dashed filename, we discover that there a couple work arrounds.

# Solution

After we connect into the first level of the wargame, we do a quick ```ls -la``` to view all files:
```
bandit1@bandit:~$ ls -la
```

We see that there is a filename called ```-```
