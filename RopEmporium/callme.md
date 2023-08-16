CTF Write-up Rop Emporium Callme

Title: callme

Category: Binary Exploitation

Challenge Description: 
Reliably make consecutive calls to imported functions. Use some new techniques and learn about the Procedure Linkage Table. How do you make consecutive calls to a function from your ROP chain that won't crash afterwards?
If you keep using the call instructions already present in the binary your chains will eventually fail, especially when exploiting 32 bit binaries. Consider why this might be the case. 
You must call the callme_one(), callme_two() and callme_three() functions in that order, each with the arguments 0xdeadbeef, 0xcafebabe, 0xd00df00d e.g. callme_one(0xdeadbeef, 0xcafebabe, 0xd00df00d) to print the flag. For the x86_64 binary double up those values, e.g. callme_one(0xdeadbeefdeadbeef, 0xcafebabecafebabe, 0xd00df00dd00df00d)

Initial Analysis:
By now i know how to return to different functions however i am curious to see how this challenge differs fro the rest and what obstacles will be in place. 
We are given very useful hints as what to do in the challenge page description so lets see if we can combine the things we have learned to solve this challenge.

Tools Used:

    ipython3
    tmux
    pwndbg
    pwntools
    ropper
    

Detailed Approach

(The buffer value size turned out to be the same as last challenge so please refer to the ret2win writeup on how to find that i will not be going over that again.)

Step 1: Find out how to load the arguments into the respective resgisters and where to call the functions.
I first decided to see what kind of functions are present in this binary as i have recognized a pattern of usefulFunctions/strings/gadgets that are provided.
After running info functions I found the three callme functions; however what seemed more interesting were the usefulGadgets and usefulfunctions functions.

![callme1](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/e4154130-2885-4165-930c-837259ca2501)

I then decided to disassmble the usefulGadgets function to determine how it would be useful, in which I found the gadgets needed to load the arguments into the correct registers.
( Results shown in picture below )

![callme2](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/a22fd9a3-eb75-4575-a590-46d6bfb1a6ec)

I then disassembled the usefulFunction and found the addresses of the functions to be called in order to get the flag.
( Results shown in picture below )

![callme4](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/b048acd2-bd2c-49c6-80df-5e690946e234)

Now using the above information I crafted a payload with the buffer size, then the usefulgadgets address along with the arguments to be passed in the correct order
and finally the callme_one address to test my concept.

Commands used are listed below

``` 
info functions
disass usefulGadgets
disass usefulFunction
```


Step 2: Find out how to call functions properly

My next thought was to find a way to correctly call the functions since this is mentioned in the challenge description. 
After trying to return to call me after usefulGadgets i kept running into problems so instead i decided to use another ret gadget and then return to callme_one
which worked. This obstacle is shown in the picture below. 

 ![callme5](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/40356a60-1ad4-4684-acdc-52c386965f9f)

 I was running into this issue I beleive due to stack alignment or because the function was not setting the stack frame correctly I am not entirely sure; 
 however by adding an extra ret gadget before callme_one i seemed to have resolved the issue and the function call has happened correctly. The description 
 of the challenge also mentions incorrect function calls present in the binary so this also might be the issue. Nevertheless i found a ret gdaget
using the command demonstrated below and was able to get around this issue. 

``` 
ropper --file callme --search  "ret"
```

![callme3](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/6de244cd-e59a-4bba-a320-00c003245dd4)


Step 3: Form the Chain
Now that i know what to do and have all the tools necessary to do it, i will form the rop chain. First i will add the buf, then the address of the usefulGadgets function, followed by the
arguments placed in the correct order, and the ret gadget, and finally callme_one. The rest of the payload was just the usefulgadgets, the repeated arguments, and the next callme function
to call in the order specified by the challenge description. I have my exploit shown below as proof of concept and i hope you all enjoyed my explanation.


Proof of Concept: Flag & Exploit

![callmeproof](https://github.com/Jaafar-G/ctf-writeups/assets/120587992/f5785e2f-10fb-4372-8b53-d4143f83fd0a)


Conclusion:

This was the third of the rop emporium challenges and a very good challenge displaying how to pass arguments to functions, return to specific functions, and ultimately 
control the flow of a program by using ROP chains. It was a nice challenge and it was very interesting to watch the controlflow of a program be manipulated.
