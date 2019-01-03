# fd writeup

First let's take a look at the challenge description says:

![fd](https://user-images.githubusercontent.com/41026969/50621778-35ce1800-0ed6-11e9-9ce3-e36e9487ec01.PNG)

So we know from the challenge description that fd also stands for file descriptor (more on this later).

So we open up a terminal and ssh into the address and enter the password ```guest```

First we do a quick ```ls -la``` to check all the files in the system. Doing so will reveal 3 files: an executable named ```fd```, a c source filed named ```fd.c``` and some text named ```flag```. We see that we do NOT have permission to read the flag text. (If you do not know how/need a refreser on how to read the linux permissions, here's a good [link to learn it](https://www.pluralsight.com/blog/it-ops/linux-file-permissions))

I like to play around with the executable before first inspecting it. Doing ```./fd``` reveals that we need to also pass in an argument, and trying out any integer/string will spit out the code ```learn about Linux file IO```...

Let's look at the source code:

![fd](https://user-images.githubusercontent.com/41026969/50622005-0c15f080-0ed8-11e9-93e2-4edf8681c6e9.PNG)
