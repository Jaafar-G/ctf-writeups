CTF Write-up Rop Emporium Ret2Win

Title: split

Category: Binary Exploitation

Challenge Description: 
The elements that allowed you to complete ret2win are still present, they've just been split apart. Find them and recombine them using a short ROP chain. I'll let you in on a secret: that useful string "/bin/cat flag.txt" is still present in this binary, as is a call to system(). It's just a case of finding them and chaining them together to make the magic happen. 

Initial Analysis:
I completed the last challenge so i knew what this one entailed except this time i knew i had to chain together instructions in order to obtain the flag.

Tools Used:

    ipython3
    tmux
    pwndbg
    pwntools
    

Detailed Approach

(The buffer value size turned out to be the same as last challenge so please refer to the ret2win writeup on how to find that i will not be going over that again.)

Step 1: Find the Function to be called. And find the useful String.
To do this i opened pwndbg (gdb) and used the commands listed below to find the useful string and useful functions.

``` 
info functions
search "/bin/cat flag.txt"
disass usefulFunction
```

I then found the function usefulFunction which contains the same mov edi and call to system as the last challenge. However this this time the mov to edi is just /bin/ls and not the useful string.
After finding the address of the useful string i knew that i knew which function to call and what value had to be in edi at the time of the system call i just had to find a way to place that value into edi for the system call. A picture showing the results of the commands is shown below. 



Step 2: Find out how to place the usefulstring value into edi.

My next thought was to find a gadget that would allow me to place the value of usefulstring into edi and then return to the system call to cat the flag into the shell. To find the gadget i used a tool called ropper this tool is recommended by ROP emporium and worked pretty well for me during the challenge. The command i used along with a picture of the results is shown below for referrence.


``` 
ropper --file split --search  "pop rdi"
```

![split2](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/c0ba9c2c-1806-46d7-a45c-edda28167840)


Step 3: Form the Chain
Now that i know what to do and have all the tools necessary to do it, i will form the rop chain. First i will add the buf, then the address of the pop rdi gadget, the address of the usefulstring to be popped into rdi, and then the address of the system call. The order of the payload is important because you must first fill the buffer return to the gadget with the value you would like directly after it in the payload so it is popped correctly then when it is in edi the call to system so that it is the argument passed to the function call.

``` 
b *pwnme+89
```

![split3](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/adc35956-385e-4f9d-b087-453a2d166efd)

Proof of Concept: Flag & Exploit

![Screenshot_2023-08-15_18-09-19](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/da1a20a0-6ed0-44a4-a17a-d4b8de6b4866)


Conclusion:

This was the second of the rop emporium challenges and a very good challenge displaying how to modify the edi register which holds the first parameter of a function. This is very useful to know and i recommend this challenge to everyone. 
