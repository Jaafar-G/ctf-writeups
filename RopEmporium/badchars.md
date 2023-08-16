CTF Write-up Rop Emporium Badchars

Title: Badchars

Category: Binary Exploitation

Challenge Description: 
An arbitrary write challenge with a twist; certain input characters get mangled as they make their way onto the stack. Find a way to deal with this and craft your exploit. You'll still need to deal with writing a string into memory, similar to the write4 challenge, that may have badchars in it. Once your string is in memory and intact, just use the print_file() method to print the contents of the flag file, just like in the last challenge. Think about how we're going to overcome the badchars issue; should we try to avoid them entirely, or could we use gadgets to change our string once it's in memory?


Initial Analysis:

As usual the goal was pretty straightforward in what it would like and the description did a great job of informing what was needed to be done.

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


write41

Step 2: 


Step 3: 

Proof of Concept: Flag & Exploit

write4proof

Conclusion:

This was the fifth of the rop emporium challenges and one of my favorites, i really enjoyed this challenge because it reminds me of a lab i once did about bitwise operations. I didnt use the method suggested but the description semmed to suggest that a little deviation was find. I beleive that a little creativity in solving the problem may inspire other to be more creative in finding different solutions that accomplish the same objective. Either way a flag is a flag.
