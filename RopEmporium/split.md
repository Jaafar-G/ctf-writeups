CTF Write-up Rop Emporium Ret2Win

Title: split

Category: Binary Exploitation

Challenge Description: 
The elements that allowed you to complete ret2win are still present, they've just been split apart.
Find them and recombine them using a short ROP chain.

Initial Analysis:
I completed the last challenge so i knew what this one entailed except this time i knew i had to chain together instructions in order to obtain the flag.

Tools Used:

    ipython3
    tmux
    pwndbg
    pwntools
    

Detailed Approach
Step 1: Find the Function to be called.
To do this i opened pwndbg (gdb) and used the command listed below.

``` info functions```

I then found the function ret2win which struck my curiosity due to the challenge name and description. 
A picture showing the  results of the command is shown below, along with the function name highlighted.



Step 2: Find out the buffer size

My next thought was to find the buffer size or how many bytes it will take to overflow the buffer and overwrite the value of the RIP register. I did this by using the cyclic function in pwntools which generates a pattern of bytes. By using a pattern it is easier to find the position of the string that starts to overwrite the RIP register. You can do this by setting a breakpoint at pwnme+109 and examining rip register to see what part of the string is located in the rip register. I did this with the following commands. 

(Helpful tip: use the cyclic_find function and pass the value of rip as a parameter find the position of the string sequence)

``` 
b *pwnme+109
x $rip
```


Step 3: 




Proof of Concept: Flag & Exploit



Key takeaways:
When using a sys call to execute a command ensure that the bytes of the stack are alinged to 0.

References/Helpful Links:

    [Link to a tool or resource that helped you]
    [Any blogs or forums that guided you]

Conclusion:

This was the first of the rop emporium challenges and a very good introductary challenge. 
