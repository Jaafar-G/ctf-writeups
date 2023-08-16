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

I found that the useful funtion is similar to the earlier challenges in that it includes a call to print file. Knowing this and the fact that the challenge description mentions that i have to write flag.txt to memory i knew that i had to pass that string as an argument to the printfile function. Then by disassembling the usefulGadgets funtion i found that it included a mov [r14], r15 which moves the value of r15 into to the memory location pointed to by r14. This is what allows us to write to the memory, after the r14 register will be a pointer to the value that was moved into it 
by r15.

Commands used are listed below Along with a picture of results.

``` 
info functions
disass usefulGadgets
disass usefulFunction
```
![write41](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/55b62c55-a896-419b-9f28-3738453df305)


Step 2: Finding out what address to use in order write the string to memory.

First i used the recommended tool readelf to veiw the segments of memory and the persimissions associated with each segment. This allowed me to view where i could write to memory.
I have a picture demonstrating what i did below for referrence.

*** 
Side note - Trying to find the address to pop into r14 was a huge obstacle for me. I had never done this before and this proved to be the most difficult part of the challenge by far.
I experiemented with countless different addresses for numerous hours and I was constantly doing trial and error until i noticed a memory address in the gadgets that looked viable. 
Although this obstacle  was a little frustrating, this made getting the flag much sweeter.
***

![write42](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/0e0ef250-d046-49ae-a8ed-05ac65e74db7)


Then I searched for gadgets with the ropper tool in order to find a pop r14 and pop r15 so that i may start trying to write to memory and i found them in the pictures below. However,
as mentioned above I had difficulties with finding an address to write to until i noticed a gadget that moved a value into to edi. This gadget caught my eye and it was in a segment
of memory that had the correct write permissions so i decided to give it a shot, and low and behold it ended up working and allowed me to write to memory. 
( Picture below sowing the gadget with the memory address mention as well as the gadgets used )

![write44](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/426df81d-c5cd-4b01-ac78-a730d0a87f00)

Below is a picture of how to search for the specific gadget with ropper.

![write43](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/66e5acdf-836e-4a24-af89-8809caa712f3)


As always the commands i used are listed here below for copy and paste.

```
readelf -l write4
ropper --file write4
ropper --file write4 --search  "ret"
```



Step 3: Form the Chain
Now that i know what to do and have all the tools necessary to do it, i will form the rop chain. 


Proof of Concept: Flag & Exploit

![write4proof](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/1dc30b1a-87e6-4777-a6ab-2ffa387655ac)


Conclusion:

This was the fourth of the rop emporium challenges and definitely one of my favorites as i learned some important assembly intructions from it. The basis of this challenge
was to learn how to write a value into memory with ROP which is an extremely powerful tool and i thoroughly enjoyed this challenge although it took me longer than the others.
