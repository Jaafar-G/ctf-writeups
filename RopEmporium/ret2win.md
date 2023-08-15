CTF Write-up Template

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
    

Detailed Approach:

    Step 1: I knew that i had to locate the function that needed to be called to i first wanted to locate the adress of it. To do this i opened gdb and used the info functions to find the address of ret2win, as seen in the photo below.

    Step 2: The next thing i did was find out how many bytes it took to overflow. 

N. Final Step: [Your breakthrough moment when you captured the flag. Describe in detail.]

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
