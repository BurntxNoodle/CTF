# TablEZ writeup - 100 point RE challenge

The challenge description:

![image](https://user-images.githubusercontent.com/41026969/56436169-f1119580-62a8-11e9-9355-3be5ce621652.png)

### Starting off

So from the challenge description it looks like we'll be dealing with some sort of table. From the last sentence is sounds like there'll be some tables that'll translate our input to make it encoded...

Downloading the binary and doing a ```./file``` check shows that it is a 64 bit linux file:

![image](https://user-images.githubusercontent.com/41026969/56436319-63827580-62a9-11e9-8e01-8f39672516bd.png)

Running the executable/playing around with it, this is what it outputs:

![image](https://user-images.githubusercontent.com/41026969/56436380-a2b0c680-62a9-11e9-8157-e853882e5869.png)

So at a first glance, we see that the program prompts us to enter a flag, and then checks it.

### Reverse engineering with IDA pro

Popping the progrma into IDA Pro 64 this is the main function:

![image](https://user-images.githubusercontent.com/41026969/56436646-92e5b200-62aa-11e9-90fb-31e06431c2d6.png)

We see some interesting hex variables being initialized and stored... we then see when the program prompts us to ```Please enter the flag"``` and when it calls ```fgets()```.

Scrolling down a bit futher, we see some more stuff happening:

