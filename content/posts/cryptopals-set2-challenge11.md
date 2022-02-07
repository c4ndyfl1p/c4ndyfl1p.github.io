+++
author = "c4ndy"
title = "Aes in CBC mode"
date = "2022-01-26"
katex = true
mathjax = true
description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
    "aes-ecb",

"aes-cbc", 
]
categories = [
    "cryptography",    
]
series = ["Themes Guide"]
aliases = ["migrate-from-jekyl"]

+++


# AES-CBC | I still see penguins 

I think this was a very experimental exercise.

edit: I did something dumb, spent more time than i'd care to admit in agony before realising the dumb thing, hence this post exists. Scroll to the bottom to save yourself some pain.

**Task ([cryptopals set2 challenge 11](https://cryptopals.com/sets/2/challenges/11)):**
1. Write an encryption oracle with encrypts a plaintext with AES in ECB mode half the time and CBC mode other half.

2. Write a detection oractle, which given a ciphertext, detects if it's been encrypted with ECB mode with CBC mode.

Easy. Both tasks are pretty simple. Detection of ECB works on the principle that ECB is stateless and deterministic; the same 16 byte plaintext block will always produce the same 16 byte ciphertext.

**Extras:**
I wrote a function that will keep track of contents and positions(indices) of any repetetive blocks when given a string of bytes. Really helpful to see what's happening to the actual bytes.

**Let's test it it now.**
First, I need to feed my encryption oracle with something that is somewhat random but also has a lot of repetetive bits. Pop Songs! Pop music is repetitive, both in musicality and lyrics. Harmony is made of 4 chords(often 1st, 5th, 6th, 4th of a scale) playing in a repetetive manner forming the rythm. Lyrically, you hear catchy chorus lyrics after every short while. I decide to feed it bits of "Elle Me dit"- MIKA (Je ne parle pas très bien le français.. MIKA est très mignonne) to see what the results are:

- here is the part I fed in as plaintext:[Elle me dit](https://github.com/c4ndyfl1p/matasano-cryptopals/blob/main/set2/11.txt)

- code: [main]([This](https://github.com/c4ndyfl1p/matasano-cryptopals/blob/main/set2/11.txt))

- code : [helper functions ](https://github.com/c4ndyfl1p/matasano-cryptopals/blob/main/set2/aes.py)

  





| repetetive blocks:        |                    |
| ------------------------- | ------------------ |
| Plaintext Block content   | Plaintext Block ID |
| b'tu g\xc3\xa2ches ta vi' | 51                 |
| b'e?\nPourquoi tu g'      | 52                 |
| b'tu g\xc3\xa2ches ta vi' | 97                 |
| b'e?\nPourquoi tu g'      | 98                 |
| b'Danse, danse, da'       | 103                |
| b'Danse, danse, da'       | 112                |
| b'e me dit danse, '       | 147                |





**Encryption under: CBC mode**

Cipher Block Chaining mode, essentially, XORs the last block's cipher text with the current block's plaintext before running it through an encryption function. Voila.

 For the first block it XOR's the plaintext with the Initialisation Vector(IV)(My IV is random 16 bytes)

CBC:
$$
C_i = E_k(P_i \oplus C_{i-1} )\\
C_0 = IV
$$

Since I'm encrypting half of the times in ECB and the other half in CBC, correctness of my decryption oracle will be indicated by it detecting exactly that.

*drum roll for the results*

My detection oracle detects ECB about 80-90% of times.

What did I do wrong? I look under the hood:

PENGUINS EVERYWHERE

> *pengwings everywhere*



CBC mode churning out identical ciphertexts? 

Let's investigate.

Repeated blocks in Ciphertext:
| CT Block content                                           | CT Block ID |
| ---------------------------------------------------------- | ----------- |
| b'\xaf\x90\xee\xe5\x84&i\xad\x0e\x81&\xc6\xd2\xb5\x11\xc0' | 52          |
| b'\xaf\x90\xee\xe5\x84&i\xad\x0e\x81&\xc6\xd2\xb5\x11\xc0' | 98          |



This means, 

<div>$$ C_{52} = E_k(P_{52} \oplus C_{51}</div> \\
C_{98} = E_k(P_{98} \oplus C_{97})	\\
\\
$$
Since,
$$ C_{52} = C_{98}\\
\Rightarrow E_k(P_{52} \oplus C_{51})  = E_k(P_{98} \oplus C_{97})
$$

This would be possible only if,
$$
P_{52} \oplus C_{51} = P_{98} \oplus C_{97}
$$

We can see,
$$  P_{52}=P_{98}  $$



We also see a pattern: identical plaintexts produce identical ciphertexts only if the preceding plaintext blocks were also identical. i.e if plaintext happen to have blocks in form: 
$$
Plaintext: A, B , ...., A,B \\
then,\\
Ciphertext: ... ,B ,... , B
$$
I saw a similar pattern where when 3 consecutive plaintext blocks produced the same 3 consecutive blocks later on:
$$
Plaintext: A, B , C, ...., A,B, C \\
then,\\
Ciphertext: ... ,B ,C ,... , B, C,...
$$

XOR MAGIC??
XOR is associative. That means:
$$
A \oplus (B \oplus C) =( A \oplus B)  \oplus C
$$

It's also commutative:

$$
A = B \oplus C \\
B = A \oplus C\\
C = B \oplus A \\
$$

My intuition declares it's because of this property of XOR, but i'm a peasant CS major 

> takes math courses next sem :)

Edit: WHY IS THIS HAPPENING. This should not be happening. I admit, I'm not the best at math, but this *reallly* should not be happening.

.

.

Edit: so here's the dumb thing:

```python
def encrypt_aes_ecb(plaintext, key):
    cipher = AES.new(key, AES.MODE_ECB)
    ciphertext = cipher.decrypt(plaintext)
    return ciphertext

def encrypt_aes_cbc(plaintext,key,iv):
    cipher = AES.new(key, AES.MODE_CBC, iv = iv)
    ciphertext = cipher.decrypt(plaintext)
    return ciphertext
```

Yea, so my encryption functions were decrypting. 

I will sleep now. Goodnight
