# Cryptography

## C3 (Crypto)
### Flag
picoCTF{adlibs}

### Thought process
* opened the code file to realise its a cipher i have to reverse engineer
* there are two lookup strings, from reading the code we can see that the first character of the input string's index in lookup 1 corresponds to the first character of encrypted string from lookup 2, but for the 2nd character the lookup two cipher shifts, which is what i have to reverse engineer.
* i tried to understand the reverse pattern by guessing the first 3 characters of the unencrypted string which turned out to be '#as' and its encrypted counterpart 'DLS'
* so the solution is, reverse the subtraction (cur-prev) to (cur+prev) and change 'prev += cur'
* doing this gives us the decryption code, after running it on our encrypted text we get an output which turns out to be python code in python 2 version as indicated by the print statement without paranthesis  
* this is where i struggled a bit and did a lot of dumb shit because i couldn't figure out the riddles in the first 4 hashtags ( i hate riddles ) but in the end came to the conclusion that maybe #selfinput refers to giving the code as its own input and it worked and i felt dumb and questioned my existence, anyways challenge completed.

### Concepts Learned
* how to reverse engineer cyclical ciphers
* how to ignore useless clues in riddles (almost)

### Incorrect Paths
* i thought that #asciiorder and #fortychars meant that i had to order some string of 40 characters in ascii order and input it

### Steps to Solve
* reverse engineer the cyclical cipher by finding pattern of decoding
* make necessary modifications to the code to decode rest of the string
* take the output which is python code, and feed it back to the same python code to get the flag

# Cryptography

## Custom Encryption
### Flag
* flag: picoCTF{custom_d2cr0pt6d_e4530597}
### Throught Process
* from preliminary analysis i realise that the enc_flag file has the encrypted flag which has to be decoded
* after a few tests, i understand that every element in the array corresponds to one encrypted letter in the flag
* after reading the test function, we can see that the code up until semi_cipher doesnt matter since values are the same everytime, which means what needs to be decoded must be in the dynamic xor encrypt and encrypt function
* after reading both functions and developing a basic understanding of the workflow, i begin by decrypting the encrpyt function
* so we basically divide every integer in the array by the key value and 311
* then we decrypt the xor function
* from reading the xor function i understand that first the flag is reversed then something called a keychar is found out of the word 'trudeau' basically every letter of the reversed flag for example 'abcdefg' --> 'gfedcba' so t will be xor'd with g, f with r, e ^ u, etc.
* i wish there was a better way of explaining this through plain text
* putting all of it together in a program in python, i have decoded the flag
### Steps Taken
* decode the flag using a custom script using understanding of the program
* decoding the encrypt function by dividing every integer by 311 and 54
* decoding the xor function by passing it again through the xor
```py
x = [151146, 1158786, 1276344, 1360314, 1427490, 1377108, 1074816, 1074816, 386262, 705348, 0, 1393902, 352674, 83970, 1141992, 0, 369468, 1444284, 16794, 1041228, 403056, 453438, 100764, 100764, 285498, 100764, 436644, 856494, 537408, 822906, 436644, 117558, 201528, 285498]
y = []
t = 'trudeau'
c = ''
for i in x:
    y.append(chr(int(i/(311*54))))
for i, char in enumerate(y):
    key_char = t[i % 7]
    encrypted_char = chr(ord(char) ^ ord(key_char))
    c += encrypted_char
print(c[::-1])
```

## miniRSA
### Flag
* flag: picoCTF{n33d_a_lArg3r_e_d0cd6eae}
### Throught Process
* skimmed the concept behind rsa
* after analysing what all they have provided, i see that we need the 'd' value to decrypt the message
* now how to find the d value is the question
* N is multiplictaion of two primes p and q, which cant be prime factorized here cause of its large value
* the totient function can also be writen as (N + 1) - (p + q), i dont really know if that is useful
* the public exponent is 3, and is less than the totient, and has to be a coprime with the totient, which means we could iterate all possible coprimes of 3 till N itself since totient is less than N
* and that phi value can be used to derive the new d value and be put into the decryption equation
* this is gonna be the most ugliest, computationally intensive algo i've ever written
* okay scratch everything above cause this was clearly stupid, so i googled few of the hints and realised with e value 3 its vulnerable to a few attacks, and due to shortage of time i mayy have pussied out a little bit and chatgpt'd the code for the Low exponent attack code, which is basically cube root of the cipher text, normal numpy or cube rooting methods werent working.
so yeah, finally found the flag
### Steps Taken
* run low exponent attack (cube root) the cipher text
```py
def integer_cube_root(n):
    """Compute the integer cube root of a number."""
    low = 0
    high = n
    while low < high:
        mid = (low + high) // 2
        if mid**3 < n:
            low = mid + 1
        else:
            high = mid
    return low

# Example: Low exponent attack
e = 3    # Public exponent

# Compute the plaintext
m = integer_cube_root(c)
print(f"Recovered plaintext: {m}")
flag = m.to_bytes((m.bit_length() + 7) // 8, 'big').decode('utf-8')
print(flag)
```

## b00tl3gRSA2
### Flag
* flag: picoCTF{bad_1d3a5_2438125}
### Throught Process
* saw the hint and the challenge description
* shifted my focus to e and d rather than tryna prime factorize n like usual
* i realised e is almost the same size as n, which means the d value which is the mod inverse would be very small
* googled attacks possible on small d, came across weiner's attack, funny name
* now, i could have gone the long way and learnt the concept and written the code myself
* but i was feeling lazy and conveiniently i found [this](https://cryptohack.gitbook.io/cryptobook/untitled/low-private-component-attacks/wieners-attack) link with a custom module attached to it
* so i just used the module and easily found the flag
### Steps Taken
* put the values in the code :)
```py
def long_to_bytes(n):
    length = (n.bit_length() + 7) // 8
    return n.to_bytes(length, byteorder='big')

#!/usr/bin/env python3
import owiener

#--------Wiener's attack--------#

d = owiener.attack(e, n)

if d:
    m = pow(c, d, n)
    flag = long_to_bytes(m).decode()
    print(flag)
else:
    print("Wiener's Attack failed.")
```

## oracle_rsa
### Flag
* flag: 
### Throught Process
* used CPA attack given in the hints
* found the encrypted c for 2, multiplied with the ciphertext, then decrypted it
* then took the decrypted hex, divided it by hex 32, took the divided hex, converted it
* used to found code ```da099``` in the openssl command and decrypted secret
### Steps Taken
* basically the same shit above

## No Padding, No Problem
### Flag
* flag: picoCTF{m4yb3_Th0se_m3s54g3s_4r3_difurrent_4005534}
### Throught Process
* same shit as oracle
* used the n and e given my the netcat to encrypt the value of 2
* took the encrypted value and multiplied with the ct
* decrypted the multiplied value, divided it by 2 and converted to bytes and voila
### Steps Taken
* copy n, e and ct
* use n and e to encrypt the value `2`
* multiply the encrypted value with the ct
* decrypt the multiplied value of ct's
* then take the decrypted value, divide it by 2
* convert from long to bytes
* you got the flag
```py
def long_to_bytes(n, length=None):
    length = (n.bit_length() + 7) // 8
    return n.to_bytes(length, byteorder='big')

m = 2
c2 = pow(m,e,n)
C = c2*ct

print(long_to_bytes(C//2))
```

## triple-secure
### Flag
* flag: picoCTF{1_gu3ss_tr1pl3_rs4_1snt_tr1pl3_s3cur3!!!!!!}
### Throught Process
* this one is interesting, and i can kinda already see the vulnerability because they use 3 primes and multiply them among each other
* which means i basically have 3 equations for 3 variables, if i can find the p q and r, im set
* after finding p q r, i just gotta find the d values for the respective n values and then reverse the decryption
* istg i wrote the code and found the flag on the first try
### Steps Taken
* find p q r
* find phi_n for all n
* find d for all n
* reverse encryption
* find flag
```py
import math
from sympy import mod_inverse

def long_to_bytes(n, length=None):
    length = (n.bit_length() + 7) // 8
    return n.to_bytes(length, byteorder='big')

def find_pqr(n1, n2, n3):
    product = n1 * n2 * n3
    pqr = math.isqrt(product)
    p = pqr // n3
    q = pqr // n2
    r = pqr // n1
    return p, q, r

p1, q1, r1 = find_pqr(n1, n2, n3)

def phi(n, p, q):
    return (p - 1) * (q - 1)

phi_n1 = phi(n1, p1, q1)
phi_n2 = phi(n2, p1, r1) 
phi_n3 = phi(n3, q1, r1) 

# Find the modular inverse of e mod φ(n) for each n
d1 = mod_inverse(e, phi_n1)
d2 = mod_inverse(e, phi_n2)
d3 = mod_inverse(e, phi_n3)

d = [d3,d2,d1]
n = [n3,n2,n1]

for i in range(len(d)):
    c = pow(c, d[i], n[i])

print(long_to_bytes(c))
```

## sum-o-primes
### Flag
* flag: picoCTF{1_gu3ss_tr1pl3_rs4_1snt_tr1pl3_s3cur3!!!!!!}
### Throught Process
* similar to last instead they have given the n value and sum of p and q
* using basic math i will find p and q
* and the rest is childs play, and i was right
* found p and q, found d, reversed ct
### Steps Taken
* find p q using basic math really
* find phi_n
* find d
* reverse encryption
* find flag
```py
import math
from sympy import mod_inverse

def find_p_q(n, s):
    discriminant = s**2 - 4 * n
    root = math.isqrt(discriminant) 
    if root * root != discriminant:
        return None
    p = (s + root) // 2
    q = (s - root) // 2
    if p * q == n:
        return p, q
    else:
        return None 
    
def long_to_bytes(n, length=None):
    length = (n.bit_length() + 7) // 8
    return n.to_bytes(length, byteorder='big')

p, q = find_p_q(n, x)
d = mod_inverse(65537, (p-1)*(q-1))
print(long_to_bytes(pow(c,d,n)))
```

