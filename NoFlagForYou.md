#San Diego CTF
## No flag for you

Category | Points 
--- | --- 
Misc| 460

![screen](public/noflag.png)

- Connect to server with `nc noflag.sdc.tf 1337`

- Let's try to find out something about content with `ls`

![screen](public/noflag_1.png)

- Let's try to find out content of a bin with `ls`

![screen](public/noflag_3.png)

- Ok. `ls` is powerless here. We also have README. Let's try it with `cat`

![screen](public/noflag_4.png)

- Let's try `cat` just in case

![screen](public/noflag_5.png)

- Nothing. Ok we have one more option `echo *`. Let's try it

![screen](public/noflag_6.png)

- Bingo! Let's get it out with `echo`

![screen](public/noflag_8.png)

flag=sdctf{1t'5_7h3_sh3ll_1n_4_shEll}