# Reverse Engineering Challenges

## Transformation (Easy)
### Flag
* Flag : picoCTF{16_bits_inst34d_of_8_0ddcd97a}
### Thought Process
* First opened the file, saw that they were all unreadable chinese characters
* then went to the challenge, saw the code they used to encode the flag
* realised that they added two characters together to form the chinese character after bitwise shifting the first char to the left by 8 bits and adding the next char
* the pairs of characters are subsequent, like the first two then the next two and so on
* so to solve this would mean to reverse engineer the main code and send in the chinese characters as input
* the chinese characters are a 16 bit character created by combining two 8 bit characters, so to get the flag we must separate these 2 byte chinese characters into two 1 byte character pairs
* now there are two ways to achieve this, the dumb method would be to convert the chinese character to binary and manually separate the 8 bit pairs, convert them induvidually back to characters and add them.
* but since this is the most basic reverse engineering we go the elegant way of reverse engineering the code using the same bitwise shift and also we'll be adding an & operation with 0xFF which is 11111111 in binary, this & operation will help us separate the bits by only isolating the first 8 bits of the 16
* onto execution
### Steps Taken
* Write the reverse engineering code
* for this the main logic would be to split a 16 bit character 灩 which is in UTF-16 01101001 01110000 binary
* to take the higher byte, or the first 8 bits, we right shift by 8 bits making it 00000000 01101001
* and to perfectly isolate the only last 8 bits we use an & operation with 0xFF which is 11111111 in binary, giving us our hgher byte
* to get the lower byte or the second byte we just directly use the & operation to eliminate the higher (first) byte
* then we add them both to get back our character pair
* iterate through all characters to get final flag
### Code
```py
encoded = "灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸弰摤捤㤷慽"
result = []
for c in encoded:
    result.append(chr((ord(c)>>8) & 0xFF) + chr(ord(c) & 0xFF))

decoded = ''.join(result)
print(decoded)
```

## GDB Baby Step 1 (Medium)
### Flag
* Flag : picoCTF{549698}
### Thought Process
* this might be the dumbest solve i've had, and idk if this is borderline cheating
* so i remember that debugger files can be debugged by gdb, hence i went n googled how do i debug and saw two commands
* gdb 'file' folowed with disass main
* the first obviously opened the debugger file and the second one disassembled the main function
* when i did that i found the eax register along with a value being pushed into it (i think)
* i convert that hexadecimal notation and voila, found a number
* put that in the flag and onto the next challenge we go
### Steps Taken
* open the debugger file in gdb
* disassemble the main function
* find the hexadecimal being pushed into an eax register
* convert it
* put in flag
### Code
gdb debugger0_a
disass main

## ARMassembly1 (Medium)
### Flag
* Flag : picoCTF{0000001b}
### Thought Process
* now there are two ways i could do this, either learn assembly or convert the assembly language into c and reverse engineer from there
* since im lazy and kinda short on time we're gonna go with the latter for now
* upon converting i find two functions, the main and the func
* its evident that whatever is returned from the func to main (stack[7]) should be equal to 0, so that must be the parameter
* to find what gives the output 0, we reverse engineer the entire function 'func'
* upon doing all the steps in the function i find that 27 is the value that is required in as a parameter
* now since the main function is using i need to give the input as "27", also another note is that argv[1] means they are taking the second argument
* so input can be something like "aaaa", "27"
* now we need to write "27" in a 32 bit format without 0x all lowercase, which is 0000001b
### Steps Taken
* open the assembly file
* convert it to c code
* do calculations in the 'func' to find what value gives 0 as output
```
    stk3 = in
    stk4 = 83
    stk5 = 0
    stk6 = 3
    w0 = 0
    w1 = 83 
    w0 = 83 << 0
    stk7 = w0 = 83
    w1 = 83
    w0 = 3
    w0 = 83/3 = 27
    stk7 = w0 = 27
    w1 = 27
    27 - input = 0
    hence input = 27
```
* convert 27 into hex 32 bit no 0x all lowercase
* put in flag
### Code
```c
#include <stdio.h>
#include <stdlib.h>

int func(int input) {
    int stack[8] = {0}; // Simulating stack allocation
    stack[3] = input; // str w0, [sp, 12]
    stack[4] = 83;    // str w0, [sp, 16]
    stack[5] = 0;     // str wzr, [sp, 20]
    stack[6] = 3;     // str w0, [sp, 24]

    int w0 = stack[5]; // ldr w0, [sp, 20]
    int w1 = stack[4]; // ldr w1, [sp, 16]
    w0 = w1 << w0;     // lsl w0, w1, w0
    stack[7] = w0;     // str w0, [sp, 28]

    w1 = stack[7];     // ldr w1, [sp, 28]
    w0 = stack[6];     // ldr w0, [sp, 24]
    w0 = w1 / w0;      // sdiv w0, w1, w0
    stack[7] = w0;     // str w0, [sp, 28]

    w1 = stack[7];     // ldr w1, [sp, 28]
    w0 = stack[3];     // ldr w0, [sp, 12]
    w0 = w1 - w0;      // sub w0, w1, w0
    stack[7] = w0;     // str w0, [sp, 28]

    return stack[7];   // ldr w0, [sp, 28]
}

int main(int argc, char *argv[]) {
    int input;
    if (argc > 1) {
        input = atoi(argv[1]); // Convert argument to integer
    } else {
        input = 0; // Default value if no argument is provided
    }

    int result = func(input);
    if (result == 0) {
        puts("You win!");
    } else {
        puts("You Lose :(");
    }

    return 0;
}
```

## Vault Door 3 (Medium)
### Flag
* Flag : picoCTF{}
### Thought Process
* downloaded to java file, opened it in vscode
* its evident that there is a password checking function and the first thing i notice is that the password has to be 32 characters
* i also noticed that the input password goes through a series of transformations to end up being 'jU5t_a_sna_3lpm18gb41_u_4_mfr340' this text because they compare at the bottom
* so to solve this challenge i gotta reverse engineer the transformations, let it be noted that i dont know java
* we start by creating a sring variable of length 32 called buffer
* the first for loop makes me think that the first 8 characters of the password are the same without transformation
* from the 9th to 16th character we subtract the index from 23 and find the character at that new index
* similar transformation in the next loop except we increment by 2 values and also subtract the index from 46
* then at last from the 32 to 18th character with decrements of 2 we replace the same value as seen in the password at those indexes
* to make life easier i converted the code to python
* the first and last forloops are easy to decode since they are directly the same indexes from the string
* the second forloop is the reverse of the characters from 8 to 16 index
* the third forloop is the reverse of the next set of 8 characters
* the ideal way to reverse engineer this is the same order as it was encoded
* upon writing the code i get the password as jU5t_a_s1mpl3_an4gr4m_4_u_1fb380, ggs
### Steps Taken
* open java file
* convert password decoder into python
* reverse engineer the password decoder
* use the encoded string to get the password using the decoder
* put the password in the picoCTF and call it a day
### Code
```py
encoded = "jU5t_a_sna_3lpm18gb41_u_4_mfr340"
decoded = ['\0']*32
for i in range(8):
    decoded[i] = encoded[i]
for i in range(8, 16):
    decoded[i] = encoded[23 - i]
for i in range(16, 32, 2):
    decoded[i] = encoded[46 - i]
for i in range(31, 16, -2):
    decoded[i] = encoded[i]
print(''.join(decoded))
```

## Vault Door Training
### Flag
* Flag : picoCTF{w4rm1ng_Up_w1tH_jAv4_eec0716b713}
### Thought Process
* kinda dumb, i just did read the file onto the console
### Steps Taken
* cat the file

## Vault Door 1
### Flag
* Flag : picoCTF{d35cr4mbl3_tH3_cH4r4cT3r5_f6daf4}
### Thought Process
* so basically i just have to rearrange the passwords in index order
### Steps Taken
* i rearranged the letters according to their indexes

## Vault Door 4
### Flag
* Flag : picoCTF{jU5t_4_bUnCh_0f_bYt3s_8f4a6cbf3b}
### Thought Process
* so its just an array with multiple different types of encoding, including ascii, hex, octal
* convert it all to ascii
### Steps Taken
* convert everything to ascii

## Vault Door 5
### Flag
* Flag : picoCTF{c0nv3rt1ng_fr0m_ba5e_64_e3152bf4}
### Thought Process
* base 64 encoded converts to url encoded, decode both
### Steps Taken
* put in cyberchef, eat 5star do nothing

## Vault Door 6
### Flag
* Flag : picoCTF{n0t_mUcH_h4rD3r_tH4n_x0r_95be5dc}
### Thought Process
* hate java so took the entire code to python
* realised if i just xor every byte with the `0x55` i find the password 
### Steps Taken
* xor every byte in `my_bytes` with `0x55`
```py
my_bytes = [
    0x3b, 0x65, 0x21, 0x0a, 0x38, 0x00, 0x36, 0x1d,
    0x0a, 0x3d, 0x61, 0x27, 0x11, 0x66, 0x27, 0x0a,
    0x21, 0x1d, 0x61, 0x3b, 0x0a, 0x2d, 0x65, 0x27,
    0x0a, 0x6c, 0x60, 0x37, 0x30, 0x60, 0x31, 0x36,
]

password = ''
for i in range(32):
    char = chr(my_bytes[i] ^ 0x55)     
    password += char

print(password)
```

## Vault Door 7
### Flag
* Flag : picoCTF{A_b1t_0f_b1t_sh1fTiNg_dc80e28124}
### Thought Process
* i mean, it was obviously long to bytes
### Steps Taken
```py
longs = [
    1096770097, 1952395366, 1600270708, 1601398833, 
    1716808014, 1734304867, 942695730, 942748212
]

# Convert each long integer to a sequence of 4 bytes (big-endian order)
byte_arrays = [long.to_bytes(4, byteorder='big') for long in longs]

# Flatten the list of byte arrays into a single byte string
byte_string = b''.join(byte_arrays)

# Convert the byte string to a readable string
decoded_string = byte_string.decode('utf-8', errors='ignore')
print(decoded_string)
```

## Vault Door 7
### Flag
* Flag : picoCTF{A_b1t_0f_b1t_sh1fTiNg_dc80e28124}
### Thought Process
* did cat on the file but couldnt read properly cause it wasnt beautified
* so thats exactly what i did
* i read the code, finally its fun
* took a pen and book
* 32 char passwd, each character scrambled through multiple transformations, they are all independently transformed
* have to reverse the switchbits function
* 
### Steps Taken
```py
longs = [
    1096770097, 1952395366, 1600270708, 1601398833, 
    1716808014, 1734304867, 942695730, 942748212
]

# Convert each long integer to a sequence of 4 bytes (big-endian order)
byte_arrays = [long.to_bytes(4, byteorder='big') for long in longs]

# Flatten the list of byte arrays into a single byte string
byte_string = b''.join(byte_arrays)

# Convert the byte string to a readable string
decoded_string = byte_string.decode('utf-8', errors='ignore')
print(decoded_string)
```

