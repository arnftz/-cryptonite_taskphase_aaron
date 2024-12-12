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
