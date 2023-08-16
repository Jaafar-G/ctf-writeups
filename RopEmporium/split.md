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
```

I then found the function usefulFunction which contains the same mov edi and call to system as the last challenge. However this this time the mov to edi is just /bin/ls and not the useful string.
After finding the address of the useful string i knew that i knew which function to call and what value had to be in edi at the time of the system call i just had to find a way to place that value into edi for the system call. A picture showing the  results of the command is shown below, along with the function name highlighted.



Step 2: Find out the buffer size

My next thought was to find the buffer size or how many bytes it will take to overflow the buffer and overwrite the value of the RIP register. I did this by using the cyclic function in pwntools which generates a pattern of bytes. By using a pattern it is easier to find the position of the string that starts to overwrite the RIP register. You can do this by setting a breakpoint at pwnme+109 and examining rip register to see what part of the string is located in the rip register. I did this with the following commands. 

(Helpful tip: use the cyclic_find function and pass the value of rip as a parameter find the position of the string sequence)

``` 
b *pwnme+89


```


Step 3: 




Proof of Concept: Flag & Exploit

![Screenshot_2023-08-15_18-09-19](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/da1a20a0-6ed0-44a4-a17a-d4b8de6b4866)



Key takeaways:


References/Helpful Links:

    [Link to a tool or resource that helped you]
    [Any blogs or forums that guided you]

Conclusion:

This was the first of the rop emporium challenges and a very good introductary challenge. 
