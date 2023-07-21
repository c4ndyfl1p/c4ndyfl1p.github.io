+++
author = "c4ndy"
title = "ECB cut and paste/profile forging"
date = "2023-07-21"


description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
    "aes-ecb",

"aes-cbc", 
]
categories = [
    "cryptography",    
]



+++

Cryptopals Challenge 13 - Profile Forging/ECB cut and paste

I solved [challenge 13](https://cryptopals.com/sets/2/challenges/13) of cryptopals, this is the writeup for it
Relevent code can be found here: [github](https://github.com/c4ndyfl1p/matasano-cryptopals/blob/main/set2/13.ipynb)

In the Cryptopals challenge number 13,  function called profile_for() is provided. This function is capable of producing valid ciphertexts.

The `profile_for()` function takes an email address as its input and then encrypts the corresponding encoded profile. For example, if the email is `foo@bar.com`, the encoded profile will look like this:


```
email=foo@bar.com&uid=10&role=user
```

This encoded profile corresponds to the following JSON representation:

```
{
	"email": "foo@bar.com",
	"uid": 10,
	"role": "user"
}
```

The main objective of this challenge is to create a specially crafted profile that has the role of "admin" using the profile_for() function. However, there's a catch – the function will remove any ampersands (&) or equal signs (=) from the user input.

Let's look at how the plaintext is chopped into blocks and encrypted without us doing anything

"foo@bar.co" ".&uid=10&role=us" "er--------------"
 
Since we can't ourselves intrtoduce "=", our hope is to get the workd "user" at the start of it's own block
 
pt1 = "foo@bar12." "com&uid=10&role=" "user-----------"

ct1 = "ct1_0"            "ct1_1"                       "ct1_2"
 
We can encrypt the above payload and get out first 2 blocks of ciphertext.

`final_ct = [ ct1_0 , ct1_1 ]`

Now we only need the encryption of "admin-----------"
 
we can get that by including that in the email ID
 
pt2 = "foo@bar12." "admin----------" "&uid=10&role=user"
 
ct2 = "ct2_0"            "ct2_1"              "ct2_2"
 
 The second block of the encrypted ciphertext of pt2, gives us the encryption of "admin-----------"
 
`final_ct = [ ct1_0 , ct1_1 , ct2_1]`
  
  
 
 
 