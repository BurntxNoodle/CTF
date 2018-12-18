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
