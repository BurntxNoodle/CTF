First let's read the challenge description:

![bof](https://user-images.githubusercontent.com/41026969/50720410-e2e19580-107a-11e9-8c4e-760fdb4f0d87.PnG)

So we can tell we'll be working on a buffer overflow challenge. Before we begin, there's a few things you need to understand before getting into how buffer overflow works:

1. x86 Assembly
2. Function Prologue/Epilogue
3. General knowledge of stacks

So for this challenge, we are given the source code, let's open that first (source file also provided above):

![bof](https://user-images.githubusercontent.com/41026969/50780253-78924600-1270-11e9-85e0-b19fc6ca36d8.png)

So we see that this function uses ```gets()``` which is extremely vulnerable since it doesn't check if what you inputted goes past the buffer. To explain this better: in C programs (like this one) the variable is only supposed to contain 32 chars, but ```gets()``` does not check if what you inputted was more than 32 characters, and it will continue writing whatever you input into memory.

Unfortunately, since ```overflowme``` is in a local function, it's on some negative offset. Also, the argument passed (```0xdeadbeef1```) is a function parameter so it also starts on some negative offsets. This means that we can't really figure out (yet) how much bytes to feed.

There are a couple workarounds to find the exact number of bytes we need to give ```gets()``` :

### The first workaround

### The second workaround
