CTF Write-up Rop Emporium Write4

Title: Write4

Category: Binary Exploitation

Challenge Description: 


Initial Analysis:


Tools Used:

    ipython3
    tmux
    pwndbg
    pwntools
    ropper
    readelf
    

Detailed Approach

(The buffer value size turned out to be the same as last challenge so please refer to the ret2win writeup on how to find that i will not be going over that again.)

Step 1: 


Commands used are listed below

``` 
info functions
disass usefulGadgets
disass usefulFunction
```


Step 2: 

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
