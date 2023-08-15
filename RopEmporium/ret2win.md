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

![Screenshot from 2023-08-15 15-36-25](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/5dcbe791-0d5f-40a9-a330-f1dc29ef0135)

Step 2: Find out buffer size
My next thought was to find the buffer size or how many bytes it will take to overflow the buffer and overwrite the value of the RIP register. I did this by using the cyclic function in pwntools which generates a pattern of bytes. By using a pattern it is easier to find the position of the string that starts to overwrite the RIP register. You can do this by setting a breakpoint at pwnme+109 and examining rip register to see what part of the string is located in the rip register. I did this with the following commands.

``` 
b *pwnme+109
x $rip
```

Step 3:
Step 4:
Step 5:
Step 6:

Flag: FLAG-VALUE-GOES-HERE

Key Takeaways/Learnings:

    [Key learning or takeaway 1]
    [Key learning or takeaway 2]
    ...
    [Key learning or takeaway N]

References/Helpful Links:

    [Link to a tool or resource that helped you]
    [Any blogs or forums that guided you]

Conclusion:

[Your final thoughts on the challenge, its difficulty level, and any tips for others attempting a similar challenge.]

I hope this template serves you well for your CTF write-ups! Adjust as needed for the specifics of each challenge and your personal style. Remember, a good write-up not only provides a solution but also teaches and guides its readers. Happy hacking!
.
