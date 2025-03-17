![image](https://github.com/user-attachments/assets/efe86537-1785-4776-bdbe-90f98dfc8575)

# Intro (Task1)
- Consider the scenario where you just downloaded a 6 GB file and want to know whether the copy you downloaded is identical to the original file, bit for bit. How would you do that? Or if a good Samaritan handed you this 6 GB file on a USB memory drive, how can you be sure it is identical to the file you want to download?
- The answer to both of the above questions lies in comparing the hash values of the two files; if two hash values are equal, you can say with very high certainty that the two files are identical. But what is a hash value?
- A hash value is a fixed-size string or characters that is computed by a hash function. A hash function takes an input of an arbitrary size and returns an output of fixed length, i.e., a hash value. We will cover various exciting and clever uses of hash functions and values in this room.
- Note about terminology: We favoured the terms hash function and hash value. However, we occasionally use the word hash as a verb to mean calculate the hash value; moreover, we occasionally use the word hash by itself as a noun to refer to the hash value.

# Hash Functions (Task2)
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

# Insecure Password sotrage for authentication (Task3)
Hashing has many uses in Cyber Security. In this room, we will focus on two uses: password storage and data integrity. We refer to password storage when used for authentication.

- It is important to note that this does not apply to password managers, where you must retrieve your password in cleartext. On the other hand, authentication mechanisms only need to confirm that the user knows the password so they can be granted access to the resource; therefore, this problem differs from password managers.
Stories of Insecure Password Storage for Authentication

- Most web applications need to verify a user’s password at some point. Storing these passwords in plaintext is a very insecure security practice. You’ve probably seen news stories about companies that have had their database leaked. Knowing that many people use the same password on their various accounts, including their online banking, leaking the password from one account jeopardises the security of all other accounts.

We will visit three insecure practices when it comes to passwords:

    Storing passwords in plaintext
    Storing passwords using a deprecated encryption
    Storing passwords using an insecure hashing algorithm

## Storing Passwords in Plaintext

Quite a few data breaches have leaked plaintext passwords. You’re probably familiar with the “rockyou.txt” password list on Kali Linux, among many other offensive security distributions. This password list came from RockYou, a company that developed social media applications and widgets. They stored their passwords in plaintext, and the company had a data breach. The text file contains over 14 million passwords. You can find rockyou.txt in the /usr/share/wordlists directory.
Terminal

           
    strategos@g5000 /usr/share/wordlists> wc -l rockyou.txt 
    14344392 rockyou.txt      
    strategos@g5000 /usr/share/wordlists> head rockyou.txt 
    123456
    12345
    123456789
    password
    iloveyou
    princess
    1234567
    rockyou
    12345678
    abc123

        

## Using an Insecure Encryption Algorithm

- Adobe’s notable data breach was slightly different. Instead of using a secure hashing function to store the hash values of the passwords, the company used a deprecated encryption format. Furthermore, password hints were stored in plain text, sometimes containing the password itself.
Consequently, the plaintext password could be retrieved relatively quickly.

## Using an Insecure Hash Function

- LinkedIn also suffered a data breach in 2012. LinkedIn used an insecure hashing algorithm, the SHA-1, to store user passwords. Furthermore, no password salting was used. Password salting refers to adding a salt, i.e., a random value, to the password before it is hashed.


# Using hashing for secure password storage (Task4)
- This is where hashing comes in. What if, instead of storing the password, you just stored its hash value using a secure hashing function? This process means you never have to store the user’s password, and if your database is leaked, an attacker will have to crack each password to find out what the password was.

- There’s just one problem with this. What if two users have the same password? As a hash function will always turn the same input into the same output, you will store the same password hash for each user. That means if someone cracks that hash, they gain access to more than one account. It also means someone can create a Rainbow Table to break the hashes.

A Rainbow Table is a lookup table of hashes to plaintexts, so you can quickly find out what password a user had just from the hash. A rainbow table trades the time to crack a hash for hard disk space, but it takes time to create. Here’s a quick example to get an idea of what a rainbow table looks like.

    Hash 	Password
    02c75fb22c75b23dc963c7eb91a062cc 	zxcvbnm
    b0baee9d279d34fa1dfd71aadb908c3f 	11111
    c44a471bd78cc6c2fea32b9fe028d30a 	asdfghjkl
    d0199f51d2728db6011945145a1b607a 	basketball
    dcddb75469b4b4875094e14561e573d8 	000000
    e10adc3949ba59abbe56e057f20f883e 	123456
    e19d5cd5af0378da05f63f891c7467af 	abcd1234
    e99a18c428cb38d5f260853678922e03 	abc123
    fcea920f7412b5da7be0cf42b8c93759 	1234567
    4c5923b6a6fac7b7355f53bfe2b8f8c1 	inS3CyourP4$$

- Websites like CrackStation and Hashes.com internally use massive rainbow tables to provide fast password cracking for hashes without salts. Doing a lookup in a sorted list of hashes is quicker than trying to crack the hash.
![image](https://github.com/user-attachments/assets/2cda0c21-4c30-4d0f-a8e4-4be8a2120e46)


## Protecting Against Rainbow Tables

- To protect against rainbow tables, we add a salt to the passwords. The salt is a randomly generated value stored in the database and should be unique to each user. In theory, you could use the same salt for all users, but duplicate passwords would still have the same hash and a rainbow table could still be created for passwords with that salt.

- The salt is added to either the start or the end of the password before it’s hashed, and this means that every user will have a different password hash even if they have the same password. Hash functions like Bcrypt and Scrypt handle this automatically. Salts don’t need to be kept private.
Example of Securely Storing Passwords

You can find many good guides online that promote best security practices when storing passwords. Please check if there are any standards you need to follow when storing passwords before adopting one. Consider this example following good security practices when storing user passwords:

    We select a secure hashing function, such as Argon2, Scrypt, Bcrypt, or PBKDF2.
    We add a unique salt to the password, such as Y4UV*^(=go_!
    Concatenate the password with the unique salt. For example, if the password is AL4RMc10k, the result string would be AL4RMc10kY4UV*^(=go_!
    Calculate the hash value of the combined password and salt. In this example, using the chosen algorithm, you need to calculate the hash value of AL4RMc10kY4UV*^(=go_!.
    Store the hash value and the unique salt used (Y4UV*^(=go_!).

## Using Encryption to Store Passwords

- Considering the problem of saving passwords for authentication, why don’t we encrypt passwords instead of all these cumbersome steps? The reason is that even if we select a secure hashing algorithm to encrypt the passwords before storing them, we still need to store the used key. Consequently, if someone gets the key, they can easily decrypt all the passwords.

# Recognising password hashes (Task5)

# Password Cracking (Task6)
# Hashing for Integrity checking  (Task7)
# Conclusion (Task8)
