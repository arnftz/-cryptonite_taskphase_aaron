# C3 (Crypto)
Flag
picoCTF{adlibs}

Thought process
* opened the code file to realise its a cipher i have to reverse engineer
* there are two lookup strings, from reading the code we can see that the first character of the input string's index in lookup 1 corresponds to the first character of encrypted string from lookup 2, but for the 2nd character the lookup two cipher shifts, which is what i have to reverse engineer.
* i tried to understand the reverse pattern by guessing the first 3 characters of the unencrypted string which turned out to be '#as' and its encrypted counterpart 'DLS'
* so the solution is, reverse the subtraction (cur-prev) to (cur+prev) and change 'prev += cur'
* doing this gives us the decryption code, after running it on our encrypted text we get an output which turns out to be python code in python 2 version as indicated by the print statement without paranthesis  
* this is where i struggled a bit and did a lot of dumb shit because i couldn't figure out the riddles in the first 4 hashtags ( i hate riddles ) but in the end came to the conclusion that maybe #selfinput refers to giving the code as its own input and it worked and i felt dumb and questioned my existence, anyways challenge completed.

Concepts Learned
* how to reverse engineer cyclical ciphers
* how to ignore useless clues in riddles (almost)

Incorrect Paths
* i thought that #asciiorder and #fortychars meant that i had to order some string of 40 characters in ascii order and input it

Steps to Solve
* reverse engineer the cyclical cipher by finding pattern of decoding
* make necessary modifications to the code to decode rest of the string
* take the output which is python code, and feed it back to the same python code to get the flag

# Binary Exploitation

## format string 0

Flag
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_c8362f05}

Thought process
* connected to the system through WSL
* Saw multiple choice options
* did random inputs, somehow worked
* not really certain as to what happened, initial thought was maybe a buffer overflow
* will look into it in detail when i have time

Steps to Solve
* give necessary inputs
* copy the flag and paste

## buffer overflow 0

Flag
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_c8362f05}

Thought process
* i think a similar kind of problem like format string 0
* i saw the challenge name and decided to google what was a buffer overflow
* learned the definition
* when i connected to the challenge, it asked for an input
* according to the definition of buffer overflow i realised if i give a ridiculously large input i can overflow it
* so that what i did and got the flag

Concepts Learned
* basic definition of buffer overflow

Steps to Solve
* connect to challenge
* input hhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhhh
* get flag, ggs





