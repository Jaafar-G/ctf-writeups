CTF Write-up Rop Emporium Write4

Title: Write4

Category: Binary Exploitation

Challenge Description: 
Our first foray into proper gadget use. A useful function is still present, but we'll need to write a string into memory somehow. On completing our usual checks for interesting strings and symbols in this binary we're confronted with the stark truth that our favourite string "/bin/cat flag.txt" is not present this time. Although you'll see later that there are other ways around this problem, such as resolving dynamically loaded libraries and using the strings present in those, we'll stick to the challenge goal which is learning how to get data into the target process's virtual address space via the magic of ROP. In this challenge we won't be using built-in functionality since that's too similar to the previous challenges, instead we'll be looking for gadgets that let us write a value to memory such as mov [reg], reg. Perhaps the most important thing to consider in this challenge is where we're going to write our "flag.txt" string. Use rabin2 or readelf to check out the different sections of this binary and their permissions. Learn a little about ELF sections and their purpose. Consider how much space each section might give you to work with and whether corrupting the information stored at these locations will cause you problems later if you need some kind of stability from this program.  



Initial Analysis:

This challenge was pretty straightforward in what it would like and the description did a great job of informing what was needed to be done and what sort of challenges a person was 
expected to face while trying to complete this challenge. I knew that i had to write the string flag.txt into memory but the challenge was to figure out how and where to do it.

Tools Used:

    ipython3
    tmux
    pwndbg
    pwntools
    ropper
    readelf
    

Detailed Approach

(The buffer value size turned out to be the same as last challenge so please refer to the ret2win writeup on how to find that i will not be going over that again.)

Step 1: Inspect funtions
As always i start with inspecting the functions listed in gdb from this binary to get a good idea of what is present for use and how i should approach this challenge. After running the info functions i found a usefulFuntion and usefulGadgets function so similar to the last challenge i disassebled them to take a look at what they do.
I found that the useful funtion is similar to the earlier challenges in that it includes a call to print file. Knowing this and the fact that the challenge description mentions that i have to write flag.txt to memory i knew
that i had to pass that string as an argument to the printfile function. Then by disassembling the usefulGadgets
funtion i found that it included a mov [r14], r15 which moves the value 

Commands used are listed below Along with a picture of results.

``` 
info functions
disass usefulGadgets
disass usefulFunction
```
![write41](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/55b62c55-a896-419b-9f28-3738453df305)


Step 2: Finding out what address to use in order write the string to memory.

``` 
ropper --file callme --search  "ret"
```



Step 3: Form the Chain
Now that i know what to do and have all the tools necessary to do it, i will form the rop chain. 


Proof of Concept: Flag & Exploit

![write4proof](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/1dc30b1a-87e6-4777-a6ab-2ffa387655ac)


Conclusion:

This was the fourth of the rop emporium challenges and definitely one of my favorites as i learned some important assembly intructions from it. The basis of this challenge
was to learn how to write a value into memory with ROP which is an extremely powerful tool and i thoroughly enjoyed this challenge although it took me longer than the others.
