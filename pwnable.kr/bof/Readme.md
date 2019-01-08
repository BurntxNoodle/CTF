First let's read the challenge description:

![bof](https://user-images.githubusercontent.com/41026969/50720410-e2e19580-107a-11e9-8c4e-760fdb4f0d87.PnG)

So we can tell we'll be working on a buffer overflow challenge. Before we begin, there's a few things you need to understand before getting into how buffer overflow works:

1. x86 Assembly [Good Video Here](https://www.youtube.com/watch?v=75gBFiFtAb8)
2. Function Prologue/Epilogue [Wiki Here](https://en.wikipedia.org/wiki/Function_prologue)
3. General knowledge of stacks (combination of both of the above)

So for this challenge, we are given the source code, let's open that first (source file also provided above):

![bof](https://user-images.githubusercontent.com/41026969/50780253-78924600-1270-11e9-85e0-b19fc6ca36d8.png)

So we see that this function uses ```gets()``` which is extremely vulnerable since it doesn't check if what you inputted goes past the buffer. To explain this better: in C programs (like this one) the variable is only supposed to contain 32 chars, but ```gets()``` does not check if what you inputted was more than 32 characters, and it will continue writing whatever you input into memory.

Unfortunately, since ```overflowme``` is in a local function, it's on some negative offset. Also, the argument passed (```0xdeadbeef1```) is a function parameter so it also starts on some negative offsets. This means that we can't really figure out (yet) how much bytes to feed.

There are a couple workarounds to find the exact number of bytes we need to give ```gets()``` :

### The first workaround
So first we can fire up gdb debugger. This allows us to view registers, view values stored in those registers, memory addresses, disassembly of function and more.

First, to open gdb and run the ```bof``` executable, we just type in ```gdb bof```. Then I just run through the program so it can examine everything as seen below:

![bof](https://user-images.githubusercontent.com/41026969/50835773-9d94c080-1325-11e9-9eab-8df30d59d310.png)

```r``` is just the command to run the program and the rest runs like normal. 

Next, since we are given the source code we know that the function we want to look at is named ```func```. We can disassemble the function by doing ```disass func``` as shown below:

![bof](https://user-images.githubusercontent.com/41026969/50836056-44795c80-1326-11e9-99e0-4301b56a7cb6.png)

So looking at the assembly

### The second workaround
