# WarmUp - 50 PWN

### Introduction/First look at the program
This writeup will contain an in-depth guide for the challenge "WarmUp" from the CSAW 2016 qualifiers. I did not do this challenge when it was live but instead learned by using their archive of challenges at [365 CSAW](https://365.csaw.io/). Follow the instructions on the website if you want to register and try out these challenges yourself. It contains challenges from previous years that you can do yourself! Link is also found on the my main CTF repo.

This is what the challenge says:

![warmup](https://user-images.githubusercontent.com/41026969/50432982-6644ff00-08a3-11e9-8620-b93ddd847117.png)

##### Note: the nc address will not work for you. You would have to register on the website, download your VPN keys and you will be able to connect to your own address. 

Doing a ```file warmup``` check to see what type of filetype it is, we see it is an 64 bit ELF (executable linux file). The executable is linked above if you wish to download it yourself. This is how the program looks like when executed (after first giving it executable permission using ```chmod +x warmup```):

![warmup output](https://user-images.githubusercontent.com/41026969/50433220-a062d080-08a4-11e9-97ba-78a678393f16.png)

So when we execute the program, we see there's a header that just says ```-Warm Up-```. But the next line is interesting, we see something interesting: ```WOW: 0x40060d```. That's an interesting address...

### Inspecting program with IDA pro
Let's pop this executable into IDA pro and take a look into the ```main``` function.

![warmup ida](https://user-images.githubusercontent.com/41026969/50433411-069c2300-08a6-11e9-8f56-ef023af9ce54.png)

We see that there are two variables, one being ```char s``` that is size [128] and we also have a ```char v5``` that is of size [64]. The values that are shown (80 and 40 respectively) show the values in hex, you can right click them in IDA and display them as a decimal number.

So let's take a look of what each call does:

```write()``` in c takes in 3 arguments: the file descriptor (0 for stdin, 1 stdout, 2 stderror), the buffer variable, and the number of bytes to write.

So basically all these write functions are printing things out to us.

The ```sprintf()``` function is basically the ```printf``` function except it saves what would be printed in the buffer variable.

The last function we see is ```gets()```... THIS FUNCTION IS EXTREMELY VULNERABLE

```gets()``` does NOT check if the input is larger than the buffer size. For example, if you write a program and declare a variable ```name[5]```, and you use ```gets()``` for the user to input their name, the program will NOT check if the name they inputted was over 5 letters long and it WILL overwrite into other memory.

### The exploit/Stacks/Registers
We can use this fact to exploit the program using what's called a buffer overflow. Remember that interesting function's memory address we saw before (that was printed to the screen)? Looking into what it does in IDA we see that it will give us the flag! So knowing all the information above, we can write an exploit so that the program will instantly return to that function that will print us the flag. Though it's not as easy as inputting 64 characters... 

First off, you should read about stacks in x86 and how they behave, specifically the [function prologue](https://en.wikipedia.org/wiki/Function_prologue) [(another resource here)](https://stackoverflow.com/questions/14765406/function-prologue-and-epilogue-in-c). Here's a great video on x86 in general: [x86 Assembly Crash Course](https://www.youtube.com/watch?v=75gBFiFtAb8&).

To put in simply terms with some visual aid, when a function is called, the function prologue starts:

1. The original function (before jumping to the function called) is stored onto the stack.
2. The Base Pointer is pushed onto the stack 
3. The Stack Pointer is made equal to the Base Pointer and is decremented to make room for variables.

As the function runs and is finished, the stack pointer is set back to equal the base pointer freeing all memory. Then the base/stack pointer is popped. Then it returns to the previous function by popping the original function's address and jumping to it.

Here's how it looks visually:

![buffer_overflow1](https://user-images.githubusercontent.com/41026969/50466083-68719100-0969-11e9-830d-73210aaec353.jpg)

Since this program is 64 bit, this means that the addresses will be 8 bytes, hence the base pointer address being stored as 8 bytes long in the stack. 

If we want to execute the buffer overflow attack to go into that specific funciton (which the address was given to us conveniently) we need to overwrite the buffer, through the base pointer, and inject an address that's 8 bytes long; because once the function is done it will instead return to the address we overwrote it in.

So we can input 72 characters (doesn't matter what character, we'll make it 'Z'). We get 72 because we need to overwrite the buffer, which is 64 bytes, and we also need to overwirte through the base pointer, which is 8 bytes (64 + 8 = 72). Then the next 8 bytes we write HAS to be the address we want the function to exit into.

Also note: since this is popped like a stack, we need to write the address backwards as seen:

