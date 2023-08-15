CTF Write-up Rop Emporium Ret2Win

Title: ret2win

Category: Binary Exploitation

Challenge Description: 
Locate a method that you want to call within the binary. Call it by overwriting a saved return address on the stack.

Initial Analysis:
The challenge description was pretty straightforward so i knew exactly what had to be done from the beginning of the challenge.

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

![Screenshot from 2023-08-15 15-56-27](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/8631e00c-1687-4eef-a48d-7dfa6b686a7b)


Step 2: Find out the buffer size

My next thought was to find the buffer size or how many bytes it will take to overflow the buffer and overwrite the value of the RIP register. I did this by using the cyclic function in pwntools which generates a pattern of bytes. By using a pattern it is easier to find the position of the string that starts to overwrite the RIP register. You can do this by setting a breakpoint at pwnme+109 and examining rip register to see what part of the string is located in the rip register. I did this with the following commands. 

(Helpful tip: use the cyclic_find function and pass the value of rip as a parameter find the position of the string sequence)

``` 
b *pwnme+109
x $rip
```


Step 3: Call Ret2Win Function

The buffer position for me was 40 so now i will append the address of ret2win and a return address to the payload and examine in gdb to see the outcome.



Proof of Concept: Flag & Exploit

![Screenshot from 2023-08-15 16-13-39](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/a6445804-c313-4a8d-803c-bdf72a2c7e72)


Key takeaways:
When using a sys call to execute a command ensure that the bytes of the stack are alinged to 0.

References/Helpful Links:

    [Link to a tool or resource that helped you]
    [Any blogs or forums that guided you]

Conclusion:

This was the first of the rop emporium challenges and a very good introductary challenge. 
