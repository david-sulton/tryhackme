![image](https://github.com/user-attachments/assets/efe86537-1785-4776-bdbe-90f98dfc8575)

# Intro
- Consider the scenario where you just downloaded a 6 GB file and want to know whether the copy you downloaded is identical to the original file, bit for bit. How would you do that? Or if a good Samaritan handed you this 6 GB file on a USB memory drive, how can you be sure it is identical to the file you want to download?
- The answer to both of the above questions lies in comparing the hash values of the two files; if two hash values are equal, you can say with very high certainty that the two files are identical. But what is a hash value?
- A hash value is a fixed-size string or characters that is computed by a hash function. A hash function takes an input of an arbitrary size and returns an output of fixed length, i.e., a hash value. We will cover various exciting and clever uses of hash functions and values in this room.
- Note about terminology: We favoured the terms hash function and hash value. However, we occasionally use the word hash as a verb to mean calculate the hash value; moreover, we occasionally use the word hash by itself as a noun to refer to the hash value.

# Hash Functions
- Hash functions are different from encryption. There is no key, and it’s meant to be impossible (or computationally impractical) to go from the output back to the input.
- A hash function takes some input data of any size and creates a summary or digest of that data. The output has a fixed size. It’s hard to predict the output for any input and vice versa. Good hashing algorithms will be relatively fast to compute and prohibitively slow to reverse, i.e., go from the output and determine the input. Any slight change in the input data, even a single bit, should cause a significant change in the output.
- Let’s check an example. In the terminal below, we can see two files; the first contains the letter T, while the second contains the letter U. If you check T and U in an ASCII table or using hexdump, you will notice that the two letters differ by a single bit.

```
    The letter T is 54 in hexadecimal, i.e., 01010100 in binary.
    The letter U is 55 in hexadecimal, i.e., 01010101 in binary.
```
```
sha256sum *.txt
```

## Why is Hashing Important?

- Hashing plays a vital role in our daily use of the Internet. Like other cryptographic functions, hashing remains hidden from the user. Hashing helps protect data’s integrity and ensure password confidentiality.

- Consider this example of hashing being used to protect your cyber security. When you log into TryHackMe, the server uses hashing to verify your password. In fact, as per good security practices, a server does not record your password; it records the hash value of your password. Whenever you want to log in, it will calculate the hash value of the password you submitted with the recorded hash value. Similarly, when you log into your computer, hashing plays a role in verifying your password. You interact more indirectly with hashing than you would think, and almost daily in the context of passwords.

## What’s a Hash Collision?

- A hash collision is when two different inputs give the same output. Hash functions are designed to avoid collisions as best as possible. Furthermore, hash functions are designed to prevent an attacker from being able to create, i.e., engineer, a collision intentionally. However, because the number of inputs is practically unlimited and the number of possible outputs is limited, this leads to a pigeonhole effect.

- As a numeric example, if a hash function produces a 4-bit hash value, we only have 16 different hash values. The total number of possible hash values is 2number_of_bits = 24 = 16. The probability of a collision is relatively very high.

- The pigeonhole effect states that the number of items (pigeons) is more than the number of containers (pigeonholes); some containers must hold more than one item. In other words, in this context, there are a fixed number of different output values for the hash function, but you can give it any size input. As there are more inputs than outputs, some inputs must inevitably give the same output. If you have 21 pigeons and 16 pigeonholes, some of the pigeons are going to share the pigeonholes. Consequently, collisions are unavoidable. However, a good hash function ensures that the probability of a collision is negligible.

- MD5 and SHA1 have been attacked and are now considered insecure due to the ability to engineer hash collisions. However, no attack has yet given a collision in both algorithms simultaneously, so if you compare the MD5 and SHA1 hash, you will see that they’re different. You can view the MD5 collision example on the MD5 Collision Demo page; furthermore, you can read the details of the SHA1 collision attack at Shattered. Due to these, you shouldn’t trust either algorithm for hashing passwords or data.

# Insecure Password sotrage for authentication
# Using hashing for secure password storage
# Recognising password hashes
# Password Cracking
# Hashing for Integrity checking 
# Conclusion
