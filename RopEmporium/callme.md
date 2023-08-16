CTF Write-up Rop Emporium Split

Title: split

Category: Binary Exploitation

Challenge Description: 
Reliably make consecutive calls to imported functions. Use some new techniques and learn about the Procedure Linkage Table. How do you make consecutive calls to a function from your ROP chain that won't crash afterwards?
If you keep using the call instructions already present in the binary your chains will eventually fail, especially when exploiting 32 bit binaries. Consider why this might be the case. 
You must call the callme_one(), callme_two() and callme_three() functions in that order, each with the arguments 0xdeadbeef, 0xcafebabe, 0xd00df00d e.g. callme_one(0xdeadbeef, 0xcafebabe, 0xd00df00d) to print the flag.
For the x86_64 binary double up those values, e.g. callme_one(0xdeadbeefdeadbeef, 0xcafebabecafebabe, 0xd00df00dd00df00d)

Initial Analysis:
By now i know how to return to different functions however i am curious to see how this challenge differs fro the rest and what obstacles will be in place. We are given very useful hints as what to do 
in the challenge page description so lets see if we can combine the things we have learned to solve this challenge.

Tools Used:

    ipython3
    tmux
    pwndbg
    pwntools
    ropper
    

Detailed Approach

(The buffer value size turned out to be the same as last challenge so please refer to the ret2win writeup on how to find that i will not be going over that again.)

Step 1: 
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



Step 3: Form the Chain
Now that i know what to do and have all the tools necessary to do it, i will form the rop chain. First i will add the buf, then the address of the pop rdi gadget, the address of the usefulstring to be popped into rdi, and then the address of the system call. The order of the payload is important because you must first fill the buffer return to the gadget with the value you would like directly after it in the payload so it is popped correctly then when it is in edi the call to system so that it is the argument passed to the function call.

``` 
b *pwnme+89
```


Proof of Concept: Flag & Exploit



Conclusion:

This was the second of the rop emporium challenges and a very good challenge displaying how to modify the edi register which holds the first parameter of a function. This is very useful to know and i recommend this challenge to everyone. 
