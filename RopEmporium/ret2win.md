CTF Write-up Rop Emporium Ret2Win

Title: ret2win

Category: Binary Exploitation

Challenge Description: 
Locate a method that you want to call within the binary. Call it by overwriting a saved return address on the stack.

Initial Analysis:
The challenge description was pretty straightforward so I had a good idea of what had to be done from the beginning of the challenge.

Tools Used:

    ipython3
    tmux
    pwndbg
    pwntools
    

Detailed Approach
Step 1: Find the Function to be called.
To do this i opened pwndbg (gdb) and used the command listed below.

```
info functions
disass ret2win
x/s 0x400943
```

I then found the function ret2win which struck my curiosity due to the challenge name and description. I decided to disassemble the function to take a look at what it does. Then i found an interesting instruction that moved a value into edi and then made a system call. I examined the value and realized this function moves the value /bin/cat flag.txt and then makes a system call which would print the flag to the shell. A picture showing the  results of the commands is shown below.

![Screenshot from 2023-08-15 15-56-27](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/8631e00c-1687-4eef-a48d-7dfa6b686a7b)

![ret2win3](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/8c06f38a-55db-4ec5-ae48-6e3e906e462e)

Step 2: Find out the buffer size

My next thought was to find the buffer size or how many bytes it will take to overflow the buffer and overwrite the value of the RIP register. I did this by using the cyclic function in pwntools which generates a pattern of bytes. By using a pattern it is easier to find the position of the string that starts to overwrite the RIP register. You can do this by setting a breakpoint at pwnme+109 and examining rsp register to see what part of the string is located in the register. This value will be used as the next rip value as the ret instruction in x86 is just a pop rip. I did this with the following commands. 

***Helpful tip: use the cyclic_find function and pass the value of that will be returned to as a parameter find the position of the string sequence. As you see in the photo i placed the 4 byte sequence of 'kaaa' into the cyclic_find function and it returned to me the position it was located at in the string buf.

``` 
b *pwnme+109
x $rsp
```
Below is a picture of the results:

![ret2win1](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/393def85-8fa5-4984-adac-72ed960a5338)


Step 3: Call Ret2Win Function

The buffer position for me was 40 so now i will append the address of ret2win and go to the instruction that moves /bin/cat/flag into edi to be called by system.
This system call executes the command cat flag wich will display the flag in the shell.

![ret2win2](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/600ea953-6638-4aaa-9cc9-4cdd5834f7c2)


Proof of Concept: Flag & Exploit

![Screenshot_2023-08-15_18-06-25](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/0ec90dc2-e54c-4020-b234-d3d6863999d3)


Conclusion:

This was the first of the rop emporium challenges and a very good introductary challenge. 
