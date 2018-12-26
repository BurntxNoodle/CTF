This writeup will contain an in-depth guide for the challenge "WarmUp" from the CSAW 2016 qualifiers. I did not do this challenge when it was live but instead learned by using their archive of challenges at [365 CSAW](https://365.csaw.io/). Follow the instructions on the website if you want to register and try out these challenges yourself. It contains challenges from previous years that you can do yourself! Link is also found on the my main CTF repo.

This is what the challenge says:

![warmup](https://user-images.githubusercontent.com/41026969/50432982-6644ff00-08a3-11e9-8620-b93ddd847117.png)

### Note: the nc address will not work for you. You would have to register on the website, download your VPN keys and you will be able to connect to your own address. 

Doing a ```file warmup``` check to see what type of filetype it is, we see it is an 64 bit ELF (executable linux file). The executable is linked above if you wish to download it yourself. This is how the program looks like when executed (after first giving it executable permission using ```chmod +x warmup```):

![warmup output](https://user-images.githubusercontent.com/41026969/50433220-a062d080-08a4-11e9-97ba-78a678393f16.png)

So when we execute the program, we see there's a header that just says ```-Warm Up-```. But the next line is interesting, we see something interesting: ```WOW: 0x40060d```. That's an interesting address...


Let's pop this executable into IDA pro and take a look into the ```main``` function.

![warmup ida](https://user-images.githubusercontent.com/41026969/50433411-069c2300-08a6-11e9-8f56-ef023af9ce54.png)

### in progress
