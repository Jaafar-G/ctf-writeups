CTF Write-up Rop Emporium Badchars

Title: Badchars

Category: Binary Exploitation

Challenge Description: 
An arbitrary write challenge with a twist; certain input characters get mangled as they make their way onto the stack. Find a way to deal with this and craft your exploit. You'll still need to deal with writing a string into memory, similar to the write4 challenge, that may have badchars in it. Once your string is in memory and intact, just use the print_file() method to print the contents of the flag file, just like in the last challenge. Think about how we're going to overcome the badchars issue; should we try to avoid them entirely, or could we use gadgets to change our string once it's in memory?


Initial Analysis:

As usual the goal was pretty straightforward in what it would like and the description did a great job of informing what was needed to be done. It was nice that the bad chars were listed so that we knew what to avoid.

Tools Used:
```
ipython3
tmux
pwndbg
pwntools
ropper
readelf
```

Detailed Approach

(The buffer value size turned out to be the same as last challenge so please refer to the ret2win writeup on how to find that i will not be going over that again.)

Step 1: Find bad chars and also inspect functions.

Firstly, as mention in the description i ran the binary to find out which chars were bad and found that the 'a', 'g', 'x', and '.' chars were forbiden. Immediatley a problem arose in my head since these chars are used in the string flag.txt.

I ran my go to command in gdb info functions to see what was going on and what i found is pictured below. This binary included a usefulFunction function as well as a usefulGadgets function. After seeing that i quickly disassembled them to get a good feel of what the challenge was going for and what was needed.

![badchars1](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/907dc1f8-1aea-4bb3-b329-375ff2cd640b)

After disassembling the usefulFunction i realized that, similar to the last challenge, a call to printfile is being made which will most likely be used to print the flag to stdout. Now that i know i must use the flag.txt string as an argument to the printfile function i have a pretty good idea of what must be done but the overarching problem is the bad chars so i disassembled the usefulGadgets function to see if anything of use could be used. As suspected i found gadgets that would XOR each byte of a string so this could definitely be used in bitwise operations to get the correct byte value. 
( pictures of disassebled funcitons below )

![badchars2](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/03956bf7-f713-41aa-bbc6-ae08f506508a)

As always the commands i used are below.

```
info functions
disass usefulFunction
disass usefulGadgets
```

Step 2: My workaround

Although the challenge suggested using xor operations to alter the byte values in memory my intial approach was to avoid the bytes all together and instead use bytes that were one value less than the correct and using an addition operation to increase the byte values by one so that they would be the correct value and then write this to memory.


This idea came to me as I ran the ropper --file badchars --search "add" command to see what gadgets were present in the file. After running this command i noticed the addition gadgets and this led me to the idea of avoiding the bytes and then manipulating them in memory to acheive the correct string value.

![badchars3](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/fc1abf29-7e60-4260-98de-c39a15b5bd7d)

Commands used :
```
ropper --file badchars ---search "add"
```

Step 3: How to change the bytes into the correct values

Knowing the above knowledge, I decided to take the altered string and place it into the correct memory value. I used the same method as the last challenge so please refer to my write4 writeup to see how that is done. After doing this i used the add gadget at address 0x40062c to alter the string fl`f-twt at the byte positions ( shown below in full exploit ) that were altered from flag.txt. This resulted in me obtaining the correct string 'flag.txt'. I then popped this into rdi and then received the flag.


Proof of Concept: Flag & Exploit 

![badcharsproof](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/5348547e-950d-4f53-a2a7-ffe712113492)


Conclusion:

This was the fifth of the rop emporium challenges and one of my favorites, i really enjoyed this challenge because it reminds me of a lab i once did about bitwise operations. I didnt use the method suggested but the description semmed to suggest that a little deviation was find. I beleive that a little creativity in solving the problem may inspire other to be more creative in finding different solutions that accomplish the same objective. Either way a flag is a flag.
