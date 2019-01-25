# Gametime writeup - 50 points

This is what the challenge description says:

![gametime](https://user-images.githubusercontent.com/41026969/51548489-6569a380-1e36-11e9-85a1-b46d35d94fe3.png)

First I played around with the executable, though since I'm on a linux system I have to use [wine](https://www.winehq.org/) to run it since it's a windows executable ```gametime.exe: PE32 executable (console) Intel 80386, for MS Windows```.

![gametime](https://user-images.githubusercontent.com/41026969/51547995-59c9ad00-1e35-11e9-8f06-083972a1d4a8.png)

Basically how this game works is that you have to press a specific character when one is printed out, if you fail to press the character at a certain timing, the game will exit and you won't get the flag. 

### Reverse engineering the executable 

I open up IDA 32 bit (since this executable is 32 bit) and start reversing.

First thing I like to do when doing these reverse engineering chellanges is to open up the strings page. This is similar to using the ```strings``` command on Linux except you can see addresses and cross reference them. To open a strings page, on the top click view > open subviews > strings.

![gametime](https://user-images.githubusercontent.com/41026969/51716876-9e5b7100-200c-11e9-87ec-fc1ca89ef7c9.PNG)

Looking at the strings page, I see the failure message occur two separate times. We can click the string and then again click the ```DATA XREF SUB...``` to go to the function in proximity view. Below is a picture of the two functions with the error messages:

![gametime](https://user-images.githubusercontent.com/41026969/51717048-4a9d5780-200d-11e9-9a0b-52886c9f7c3b.PNG)

![gametime](https://user-images.githubusercontent.com/41026969/51717095-7f111380-200d-11e9-953d-8438236f1035.PNG)

Both these functions have a ```jnz``` command before the ```UDDER FAILURE!``` message. We can change this jump not zero command to jump zero so that we never get the failure message.


### coming soon - in progress
