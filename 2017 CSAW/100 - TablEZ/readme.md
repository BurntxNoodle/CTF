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

Scrolling down a bit futher, we see some more stuff happening. Here's the graph view:

![image](https://user-images.githubusercontent.com/41026969/56452868-9fe3bf00-6305-11e9-9a09-bae9b72fd55c.png)

Here's the pseudocode that IDA creates:

![image](https://user-images.githubusercontent.com/41026969/56452885-e1746a00-6305-11e9-853f-31d76e79602f.png)

So from the above, we can see that for every char that we inputted it gets replaced from whatever ```get_tbl_entry``` returns. Then after all the chars we inputted gets replaced from the ```get_tbl_entry``` function, the program first checks if the resulting string has a length of 37, if so, it string compares with ```s2```.

##### note: ```s2``` is seen in hex form on the graph view of ida. It's those interesting hex variables being intialized in the first picture

##### also note: since we are dealing with x86-64 bit (as seen from the ```file``` check) we are dealing with little endian. This means those hex values mentioned above, are stored backwards. 

IDA has a cool feature that you can do that will display the hex values in order, so that it is easier to read, just highlight the hex string, right click, and one of the options shows the hex string backwards, click the one. Here it is shown below:

![image](https://user-images.githubusercontent.com/41026969/56453251-b68e1400-630d-11e9-8646-f000eed5d87a.png)

Doing the above, we see the hex version of ```s2``` which is what it's comparing our input by.

Let's check out the ```get_tbl_entry``` function:

![image](https://user-images.githubusercontent.com/41026969/56453284-2b614e00-630e-11e9-8900-2a091353215d.png)

So this function takes in a char and returns a char. It takes the char passed in, looks it up in ```trans_tbl``` (using a for loop, with ```i``` incrementing) and when found, returns the char found at the array ```byte_201281``` at index ```[2 * i]``` (i being the variable that's incrementing the for loop).

Let's check out those variables:


### The solution
So that now we know how to program works, and the string that it's comparing it by, we can reverse engineer the process to get the flag.

I wrote a python program to do this for me.

