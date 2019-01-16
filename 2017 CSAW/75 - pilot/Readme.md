# pilot Writeup

![pilot](https://user-images.githubusercontent.com/41026969/50910626-2e3dd000-13fc-11e9-8ec9-58243f541a8e.png)

Ok so the description doesn't really give us much of anything, just an executable to work with and a ```nc``` address.
##### note: this ```nc``` address won't work for you since it's unique to each individual and requires vpn, check out links in my CTF page and look at the 365 CSAW link.

First, I did a few checks:

![pilot](https://user-images.githubusercontent.com/41026969/50911285-86290680-13fd-11e9-97aa-da7c1e90efe5.png)

To quickly explain what I did for every command I did:
```chmod +x pilot``` gives pilot permission to execute (to run). If we didn't, if we tried to run it it'd say "Permission Denied." ```file pilot``` checks what kind of file pilot is. We learn that this is a 64 bit executable which means 8 byte addresses (which may be handy to us further down). ```checksec pilot``` checks the properties of an executable. We see that the program does not utilize a [stack canary](https://en.wikipedia.org/wiki/Stack_buffer_overflow#Stack_canaries) (which essential checks to see if the stack has been changed for buffer overflow possibility). We then just test the program by passing a bunch of letters into it.

### Reverse engineering time
So I opened the file in IDApro 64 bit. In IDA, you can generate pseudocode by clicking ```view -> open subviews -> generate pseudocode```. Looking at the ```main``` function, I see this interesting piece of code:

![pilot](https://user-images.githubusercontent.com/41026969/51258592-56877a80-1978-11e9-844a-75a29572e2b8.png)

So from the code above, we can see a few things. 
1) The read() instruction is called
2) The function will take in 64 bytes (or characters) into standard input (in IDA, you can right click and convert the hex to decimal)
3) if what we inputted is less than or equal to 4 bytes, we see that ```[*]There are no commands....``` and ```[*]Mission Failed....``` will get printed out - that's not what I want...

I looked at the disassembly x86 instructions next, specifically looked at the main function again. I saw this:

![pilot](https://user-images.githubusercontent.com/41026969/51260034-60f74380-197b-11e9-8d90-c6202fc5fe3e.png)

### in progress
