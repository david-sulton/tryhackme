![image](https://github.com/user-attachments/assets/259c4659-b386-4c07-83ef-ff9834a923c0)

## Intro

    Cryptography key terms
    Importance of cryptography
    Caesar Cipher
    Standard symmetric ciphers
    Common asymmetric ciphers
    Basic mathematics commonly used in cryptography

## Importance of Cryptography
Consider the following scenarios where you would use cryptography:

    When you log in to TryHackMe, your credentials are encrypted and sent to the server so that no one can retrieve them by snooping on your connection.
    When you connect over SSH, your SSH client and the server establish an encrypted tunnel so no one can eavesdrop on your session.
    When you conduct online banking, your browser checks the remote server’s certificate to confirm that you are communicating with your bank’s server and not an attacker’s.
    When you download a file, how do you check if it was downloaded correctly? Cryptography provides a solution through hash functions to confirm that your file is identical to the original one.
- Payment Card Industry Data Security Standard (PCI DSS)
-  HIPAA (Health Insurance Portability and Accountability Act)
-  HITECH (Health Information Technology for Economic and Clinical Health)
-  GDPR (General Data Protection Regulation)
## Plaintext to Ciphertext
![image](https://github.com/user-attachments/assets/88f64f34-7cf0-4945-b978-0d393f43f3d6)
![image](https://github.com/user-attachments/assets/ad13e610-328e-409f-b104-f398cc629546)
We have just introduced several new terms, and we need to learn them to understand any text about cryptography. The terms are listed below:

    Plaintext is the original, readable message or data before it’s encrypted. It can be a document, an image, a multimedia file, or any other binary data.
    Ciphertext is the scrambled, unreadable version of the message after encryption. Ideally, we cannot get any information about the original plaintext except its approximate size.
    Cipher is an algorithm or method to convert plaintext into ciphertext and back again. A cipher is usually developed by a mathematician.
    Key is a string of bits the cipher uses to encrypt or decrypt data. In general, the used cipher is public knowledge; however, the key must remain secret unless it is the public key in asymmetric encryption. We will visit asymmetric encryption in a later task.
    Encryption is the process of converting plaintext into ciphertext using a cipher and a key. Unlike the key, the choice of the cipher is disclosed.
    Decryption is the reverse process of encryption, converting ciphertext back into plaintext using a cipher and a key. Although the cipher would be public knowledge, recovering the plaintext without knowledge of the key should be impossible (infeasible).
## Historical Ciphers
![image](https://github.com/user-attachments/assets/7dcd01b8-c159-4d13-93cc-4dd6a95e2405)

## Types of Encryption
### Symmetric Encryption
- Symmetric encryption, also known as symmetric cryptography, uses the same key to encrypt and decrypt the data, as shown in the figure below. Keeping the key secret is a must; it is also called private key cryptography. Furthermore, communicating the key to the intended parties can be challenging as it requires a secure communication channel. Maintaining the secrecy of the key can be a significant challenge, especially if there are many recipients. The problem becomes more severe in the presence of a powerful adversary; consider the threat of industrial espionage, for instance.
![image](https://github.com/user-attachments/assets/1ddfb600-7015-4bfc-89a9-5a06085bccff)
Examples of symmetric encryption are DES (Data Encryption Standard), 3DES (Triple DES) and AES (Advanced Encryption Standard).

        DES was adopted as a standard in 1977 and uses a 56-bit key. With the advancement in computing power, in 1999, a DES key was successfully broken in less than 24 hours, motivating the shift to 3DES.
        3DES is DES applied three times; consequently, the key size is 168 bits, though the effective security is 112 bits. 3DES was more of an ad-hoc solution when DES was no longer considered secure. 3DES was deprecated in 2019 and should be replaced by AES; however, it may still be found in some legacy systems.
        AES was adopted as a standard in 2001. Its key size can be 128, 192, or 256 bits.

### Asymmetric Encryption
![image](https://github.com/user-attachments/assets/4b339f04-4e5b-4b36-94b7-7ef8d9ff1ce4)
- Unlike symmetric encryption, which uses the same key for encryption and decryption, asymmetric encryption uses a pair of keys, one to encrypt and the other to decrypt, as shown in the illustration below. To protect confidentiality, asymmetric encryption or asymmetric cryptography encrypts the data using the public key; hence, it is also called public key cryptography.

## Basic Math
### XOR Operation

    XOR, short for “exclusive OR”, is a logical operation in binary arithmetic that plays a crucial role in various computing and cryptographic applications. In binary, XOR compares two bits and returns 1 if the bits are different and 0 if they are the same, as shown in the truth table below. This operation is often represented by the symbol ⊕ or ^.
![image](https://github.com/user-attachments/assets/ebf927bf-849f-427e-80af-c525f0242448)
### Modulo Operation
- Another mathematical operation we often encounter in cryptography is the modulo operator, commonly written as % or as mod. The modulo operator, X%Y, is the remainder when X is divided by Y. In our daily life calculations, we focus more on the result of division than on the remainder. The remainder plays a significant role in cryptography.

- You need to work with large numbers when solving some cryptography exercises. If your calculator fails, we suggest using a programming language such as Python. Python has a built-in int type that can handle integers of arbitrary size and would automatically switch to larger types as needed. Many other programming languages have dedicated libraries for big integers. If you prefer to do your math online, consider WolframAlpha.

Let’s consider a few examples.

    25%5 = 0 because 25 divided by 5 is 5, with a remainder of 0, i.e., 25 = 5 × 5 + 0
    23%6 = 5 because 23 divided by 6 is 3, with a remainder of 5, i.e., 23 = 3 × 6 + 5
    23%7 = 2 because 23 divided by 7 is 3 with a remainder of 2, i.e., 23 = 3 × 7 + 2

- An important thing to remember about modulo is that it’s not reversible. If we are given the equation x%5 = 4, infinite values of x would satisfy this equation.

- The modulo operation always returns a non-negative result less than the divisor. This means that for any integer a and positive integer n, the result of a%n will always be in the range 0 to n − 1.
