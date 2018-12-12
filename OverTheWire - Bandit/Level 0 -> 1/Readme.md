Note: You should still be logged in into Bandit0 from the previous challenge.

The challenge reads: 

![level 0 - 1](https://user-images.githubusercontent.com/41026969/49900055-784a8a80-fe2b-11e8-87dd-a317972197d5.png)

We can see it recommends that we read up on the following commands:

```
ls
cd
cat
file
du
find
```

### Solution:

First we will ```ls -la```. To view all the contents of the file:

```
bandit0@bandit:~$ ls -la
```

This will show us the files:

![ls -la](https://user-images.githubusercontent.com/41026969/49900510-c4e29580-fe2c-11e8-8022-d21383b5c4dc.png)

We see that there is a file called ```readme```. Looking into what kind of file it is using the ```file``` command:

```
bandit0@bandit:~$ file readme 
readme: ASCII text
```

To read a ASCII text (essentially a txt file) we use the ```cat``` command:
```
bandit0@bandit:~$ cat readme 
```
This will give us the password for the next level!
Password: ```boJ9jbbUNNfktd78OOpsqOltutMc3MY1```
