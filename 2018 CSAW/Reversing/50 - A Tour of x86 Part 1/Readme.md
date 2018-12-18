The challenge reads:  
![tour of x86 part 1](https://user-images.githubusercontent.com/41026969/50127555-a7079d00-023f-11e9-922f-d5880475e4fe.png)

You can find the files given in the challenge above the Readme.  

The ```nc``` will prompt the user and ask several questions regarding the ```stage-1.asm``` file. Users can either use ```cat```  to read the file or open with a text editor (I personally prefer sublime text). Upon entering the question correct, it will lead to the next question until the end. When all the answers were answered correctly, the flag will be given.

So throughout the ```stage-1.asm``` file there are examples of assembly instructions. Most of which have to do with memory addresses. Reading it, there are several parts with an arrow and question number, ex: ```<- Question 1```. This will denote what
the question refers to when asked from the ```nc``` command (not open anymore).

Note: All answers will start with ```0x``` followed by the adress asked.

### Question 1
Upon doing the ```nc``` command, the first question will ask: ```what is the value of dh after line 129 executes (One byte)``` 

When I first did the challenge, I was held back (time wise) on the formatting of the answers: specifically how many bytes are
represented per hex digit. According to the [hexidemical wiki](https://en.wikipedia.org/wiki/Hexadecimal) ```Each hexadecimal digit represents four binary digits, also known as a nibble, which is half a byte.``` So since each hex digit is half a byte, we'll have to input two digits after ```0x``` (according to the question, the answer is one byte).

Reading line 129 in the ```stage-1.asm```, the assembly line is ```xor dh, dh```. This means we'll do an exclusive-or (also denoted as XOR) comparison with the higher half of register ```d``` (this is what the ```h``` means in ```dh``` - that we're referring to the higher half of d's address).

When we compare using [XOR](https://en.wikipedia.org/wiki/Exclusive_or) we check each bit and if it's the same (1 and 1, or 0 and 0) then it will result in 0. Though if it's different (1 and 0, or 0 and 1), then it will result in 1. (this is where 'exclusive' comes from, basically the bits have to be different).

So since we are XOR'ing the same address, it doens't matter what the actual address is, all the digits will be the same. And as explained above, when we compare two of the same addresses, it will result in 0. Thus, after line 129 executes, the value of dh will simply be all 0's.  

And since the question specifies that we will have the answer in one byte (and we know each hex digit is half a byte), the resulting answer is: ```0x00```

### Question 2  
Upon answering the first question correctly, the second question will be prompted: ```What is the value of gs after line 145 executes? (one byte)```

Line 145 reads: ```mov gs, dx```

So there are a couple hints regarding how to solve this question.

So the first way is that looking a bit above: specifically on line 134, it says ```cmp dx, 0```. ```cmp``` stands for compare
and it will compare the two numerical data fields. Depending on the result, the next line will execute or not. In this case, the 
next line reads: ```jne .death```. First, ```jne``` means "jump not equal." Meaning if the comparison is not equal, it will jump to the address/function named ```.death```. Assuming from the name, this will execute the program. Thus, putting together:

```
cmp dx, 0
jne .death
```

Translated into English: We compare register ```dx``` with 0, and we will jump, if they're not equal, to the function ```.death```

So if the program continues and doesn't die, we'll get to the line in question 2 (145) which reads: ```mov gs, dx```. 
```mov``` stands for "move." It basically means we'll copy the address stored (in this case) ```dx``` to ```gs```. Since we  
know dx is 0 (if it wasn't 0, the program would die/exit from the assembly code above), we're moving the address stored in
```dx``` (all 0's) into ```gs```. So ```gs``` will have all 0s.

Since it is a one byte answer format, we'll write out two hex digits (each hex digit equals one half of a byte as explained
above). Thus the answer for question 2 is: ```0x00```.

A second hint is the comment on the side of the x86 assembly code block (lines 142 - 146). It reads ```Oh yea so this since
the other registers are already 0, I'm just going to use them to help me clear these registers out.``` From that comment
you can assume that by ```clear these registeres out``` the author means he'll set everything to 0. Thus yielding the answer
of: ```0x00```.

### Question 3
When answering the second question correctly: ```What is the value of si after line 151 executes? (two bytes)```

So this question requires a tad bit of reversing. So first line 151 says: ```mov si, sp ;```. And the question is asking
what is the value of ```si```. And to know what value was moved into ```si``` we need to find the value of ```sp```. 
Traversing up the code to investigate the value of ```sp``` we get to line 149 that says ```mov sp, cx```. So now we know
that the value of ```cx``` was moved into ```sp```, and the value moved into ```sp``` was moved into ```si```. So now we need to 
figure out what ```cx``` is equal to. And we know that whole code block from line 142-146 is just setting all the reigsters to 0
because of the comment on the side: ```Oh yea so this since the other registers are already 0, I'm just going to use them to help me
clear these registers out.``` And in those registers is ```cx``` so it's safe to assume that ```cx``` is simply equal to 0. 

If that doesn't convince you, we can also see that on line 107 that the program moves 0 into ```cx```. It reads: ```mov cx, 0```.

Since the answer format needs to be in two bytes, the answer is: ```0x0000```

### Question 4
