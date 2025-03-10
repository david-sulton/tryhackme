![image](https://github.com/user-attachments/assets/52dda82c-2341-4d90-ac49-0a5ccbd264dd)
# Public Key Cryptography Basics
- https://tryhackme.com/room/publickeycrypto

## Introduction

Consider the following scenario from everyday life. Let’s say you are meeting a business partner over coffee and discussing somewhat confidential business plans. Let’s break down the meeting from the security perspective.

    You can see and hear the other person. Consequently, it is easy to be sure of their identity. That’s authentication, i.e., you are confirming the identity of who you are talking with.
    You can also confirm that what you are “hearing” is coming from your business partner. You can tell what words and sentences are coming from your business partner and what is coming from others. That’s authenticity, i.e., you verify that the message genuinely comes from a specific sender. Moreover, you know that what they are saying is reaching you, and there is no chance of anything changing the other party’s words across the table. That’s integrity, i.e., ensuring that the data has not been altered or tampered with.
    Finally, you can pick a seat away from the other customers and keep your voice low so that only your business partner can hear you. That’s confidentiality, i.e., only the authorised parties can access the data.

Let’s quickly compare this with correspondence in the cyber realm. When someone sends you a text message, how can you be sure they are who they claim to be? How can you be sure that nothing changed the text as it travelled across various network links? When you are communicating with your business partner over an online messaging platform, you need to be sure of the following:

    Authentication: You want to be sure you communicate with the right person, not someone else pretending.
    Authenticity: You can verify that the information comes from the claimed source.
    Integrity: You must ensure that no one changes the data you exchange.
    Confidentiality: You want to prevent an unauthorised party from eavesdropping on your conversations.

Cryptography can provide solutions to satisfy the above requirements, among many others. Private key cryptography, i.e., symmetric encryption, mainly protects confidentiality. However, public key cryptography, i.e., asymmetric cryptography, plays a significant role in authentication, authenticity, and integrity. This room will show various examples of how public key cryptography achieves that.
Learning Prerequisites

This room is the second of three introductory rooms about cryptography. Before starting this room, ensure you have finished the first one on the list.

    Cryptography Basics
    Public Key Cryptography Basics (this room)
    Hashing Basics

Learning Objectives

In this room, we will cover various asymmetric cryptosystems and applications that use them, such as:

    RSA
    Diffie-Hellman
    SSH
    SSL/TLS Certificates
    PGP and GPG

First, let’s start the Virtual Machine by pressing the Start Machine button below.

The machine will start in Split-Screen view. In case the VM is not visible, use the blue Show Split View button at the top of the page.

You can also access the virtual machine using SSH at the IP address 10.10.195.31 using the following credentials:

    Username: user
    Password: Tryhackme123!

Answer the questions below

    Let’s begin!

Answer: No answer needed
Common Use of Asymmetric Encryption

Exchanging keys for symmetric encryption is a widespread use of asymmetric cryptography. Asymmetric encryption is relatively slow compared to symmetric encryption; therefore, we rely on asymmetric encryption to negotiate and agree on symmetric encryption ciphers and keys.

But the question is, how do you agree on a key with the server without transmitting the key for people snooping to see?
Analogy

Imagine you have a secret code for communicating and instructions for using the secret code. The question is how you can send these instructions to your friend without anyone else being able to read them. The answer is more straightforward than it seems; you could ask your friend for a lock. Only your friend has the key for this lock, and we’ll assume you have an indestructible box you can lock with it.

If you send the instructions in a locked box to your friend, they can unlock it once it reaches them and read the instructions. After that, you can communicate using the secret code without the risk of people snooping.

In this metaphor, the secret code represents a symmetric encryption cipher and key, the lock represents the server’s public key, and the key represents the server’s private key.

Consequently, you would only need to use asymmetric cryptography once so that it won’t affect the speed, and then you can communicate privately using symmetric encryption.
The Real World

In reality, you need more cryptography to verify that the person you’re talking to is who they say they are. This is achieved using digital signatures and certificates, which we will visit later in this room.

Answer the questions below

    In the analogy presented, what real object is analogous to the public key?

Answer: Lock
RSA

RSA (Rivest-Shamir-Adleman) is a popular asymmetric encryption algorithm based on the mathematical difficulty of factoring large numbers. It uses two large prime numbers to generate a public key (used for encryption) and a private key (used for decryption).

Example Calculation:

    Multiply two primes ppp and qqq to get nnn.
    Calculate ϕ(n)\phi(n)ϕ(n) (Euler’s Totient).
    Select eee such that it is relatively prime to ϕ(n)\phi(n)ϕ(n), then find ddd where e×d≡1 (mod ϕ(n))e \times d \equiv 1 \ (\text{mod} \ \phi(n))e×d≡1 (mod ϕ(n)).

RSA in CTFs

The math behind RSA comes up relatively often in CTFs, requiring you to calculate variables or break some encryption based on them. Many good articles online explain RSA, and they will give you almost all of the information you need to complete the challenges. One good example of an RSA CTF challenge is the Breaking RSA room.

There are some excellent tools for defeating RSA challenges in CTFs. My favourite is RsaCtfTool, which has worked well for me. I’ve also had some success with rsatool.

You need to know the main variables for RSA in CTFs: p, q, m, n, e, d, and c. As per our numerical example:

    p and q are large prime numbers
    n is the product of p and q
    The public key is n and e
    The private key is n and d
    m is used to represent the original message, i.e., plaintext
    c represents the encrypted text, i.e., ciphertext

Crypto CTF challenges often present you with a set of these values, and you need to break the encryption and decrypt a message to retrieve the flag.

Answer the questions below

    Knowing that p = 4391 and q = 6659. What is n?

Answer: 29239669

Calculate 𝑛
Formula:n=p×q
Given values:
p=4391
q=6659
Calculation:
n=4391×6659=29239669

    Knowing that p = 4391 and q = 6659. What is ϕ(n)?

Answer: 29228620

Calculate φ(n):
Formula: φ(n)=(p−1)×(q−1)
Calculation:
φ(n)=(4391−1)×(6659−1)=4390×6658=29228620

Diffie-Hellman Key Exchange

The Diffie-Hellman (DH) Key Exchange enables two parties to establish a shared secret over an insecure channel without prior shared secrets. It involves selecting a large prime ppp, a generator ggg, and exchanging partial results to ultimately compute the same secret key on both sides.

Steps:

    Choose public values ppp and ggg.
    Each party selects a private key and calculates their public key.
    Both parties exchange public keys and compute the shared secret.

Answer the questions below

    Consider p = 29, g = 5, a = 12. What is A?

Answer: 7

Calculate AAA:
Formula: A=gamod pA = g^a \mod pA=gamodp
Calculation: A=512mod 29A = 5^{12} \mod 29A=512mod29
Result: A=7A = 7A=7

    Consider p = 29, g = 5, b = 17. What is B?

Answer: 9

Calculate BBB:
Formula: B=gbmod pB = g^b \mod pB=gbmodp
Calculation: B=517mod 29B = 5^{17} \mod 29B=517mod29
Result: B=9B = 9B=9

    Knowing that p = 29, a = 12, and you have B from the second question, what is the key calculated by Bob? (key = Ba mod p)

Answer: 24

Calculate Bob's Key (using BBB and aaa):
Formula: Key=Bamod p\text{Key} = B^a \mod pKey=Bamodp
Calculation: Key=912mod 29\text{Key} = 9^{12} \mod 29Key=912mod29
Result: Key=24\text{Key} = 24Key=24

    Knowing that p = 29, b = 17, and you have A from the first question, what is the key calculated by Alice? (key = Ab mod p)

Answer: 24

Calculate Alice's Key (using AAA and bbb):
Formula: Key=Abmod p\text{Key} = A^b \mod pKey=Abmodp
Calculation: Key=717mod 29\text{Key} = 7^{17} \mod 29Key=717mod29
Result: Key=24\text{Key} = 24Key=24

SSH

SSH (Secure Shell) is a protocol used to securely log into remote systems. Authentication can involve usernames and passwords or public-private key pairs.

ssh-keygen is the program usually used to generate key pairs. It supports various algorithms, as shown on its manual page below.

root@TryHackMe# man ssh-keygen
[...]
-t dsa | ecdsa | ecdsa-sk | ed25519 | ed25519-sk | rsa
Specifies the type of key to create. The possible values are “dsa”, “ecdsa”, “ecdsa-sk”, “ed25519”, “ed25519-sk”, or “rsa”.
[...]

The following is just for your information. At this stage, we recommend that you recognise their names only.

    DSA (Digital Signature Algorithm) is a public-key cryptography algorithm specifically designed for digital signatures.
    ECDSA (Elliptic Curve Digital Signature Algorithm) is a variant of DSA that uses elliptic curve cryptography to provide smaller key sizes for equivalent security.
    ECDSA-SK (ECDSA with Security Key) is an extension of ECDSA. It incorporates hardware-based security keys for enhanced private key protection.
    Ed25519 is a public-key signature system using EdDSA (Edwards-curve Digital Signature Algorithm) with Curve25519.
    Ed25519-SK (Ed25519 with Security Key) is a variant of Ed25519. Similar to ECDSA-SK, it uses a hardware-based security key for improved private key protection.

Commands:

    ssh-keygen -t ALGORITHM: Generates a public and private key pair using a specified algorithm (e.g., RSA, ED25519).
    ssh -i PRIVATE_KEY user@host: Specifies a private key for SSH login.

When generating an SSH key to log in to a remote machine, you should generate the keys on your machine and then copy the public key over, as this means the private key never exists on the target machine using ssh-copy-id. However, this doesn’t matter as much for temporary keys generated to access CTF boxes.

The permissions must be set up correctly to use a private SSH key; otherwise, your SSH client will ignore the file with a warning. Only the owner should be able to read or write to the private key (600 or stricter). ssh -i privateKeyFileName user@host is how you specify a key for the standard Linux OpenSSH client.
Keys Trusted by the Remote Host

The ~/.ssh folder is the default place to store these keys for OpenSSH. The authorized_keys (note the US English spelling) file in this directory holds public keys that are allowed access to the server if key authentication is enabled. By default on many Linux distributions, key authentication is enabled as it is more secure than using a password to authenticate. Only key authentication should be accepted if you want to allow SSH access for the root user.

Answer the questions below

    Check the SSH Private Key in ~/Public-Crypto-Basics/Task-5. What algorithm does the key use?

Answer: RSA

ssh-keygen -l -f ~/Public-Crypto-Basics/Task-5

Digital Signatures and Certificates

Digital Signatures verify the authenticity and integrity of a message, ensuring it hasn’t been altered. They are generated using a private key and verified with a public key. Certificates are used to verify the identity of web servers (HTTPS) through a chain of trust starting with a Certificate Authority (CA).

Let’s say you have a website and want to use HTTPS. This step requires having a TLS certificate. You can get one from the various certificate authorities for an annual fee. Furthermore, you can get your own TLS certificates for domains you own using Let’s Encrypt for free. If you run a website, it’s worth setting up and switching to HTTPS, as any modern website would do.

Answer the questions below

    What does a remote web server use to prove itself to the client?

Answer: Certificate

    What would you use to get a free TLS certificate for your website?

Answer: Let’s Encrypt
PGP and GPG

PGP and GPG

PGP stands for Pretty Good Privacy. It’s software that implements encryption for encrypting files, performing digital signing, and more. GnuPG or GPG is an open-source implementation of the OpenPGP standard.

GPG is commonly used in email to protect the confidentiality of the email messages. Furthermore, it can be used to sign an email message and confirm its integrity.

Below is an example of generating GPG. You are asked about the purpose of using gpg, whether signing only or signing and encrypting. Besides selecting the cryptographic algorithm, we needed to choose an expiry date for the generated key. Finally, we provided some information about us: our name, email address, and a comment usually about the purpose of this key.

gpg --full-gen-key
gpg (GnuPG) 2.4.4; Copyright (C) 2024 g10 Code GmbH
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Please select what kind of key you want:
   (1) RSA and RSA
   (2) DSA and Elgamal
   (3) DSA (sign only)
   (4) RSA (sign only)
   (9) ECC (sign and encrypt) *default*
  (10) ECC (sign only)
  (14) Existing key from card
Your selection? 9
Please select which elliptic curve you want:
   (1) Curve 25519 *default*
   (4) NIST P-384
   (6) Brainpool P-256
Your selection? 1
Please specify how long the key should be valid.
         0 = key does not expire
      <n>  = key expires in n days
      <n>w = key expires in n weeks
      <n>m = key expires in n months
      <n>y = key expires in n years
Key is valid for? (0) 
Key does not expire at all
Is this correct? (y/N) y
GnuPG needs to construct a user ID to identify your key.
Real name: strategos
Email address: strategos@tryhackme.thm
[...]
pub   ed25519 2024-08-29 [SC]
      AB7E6AA87B6A8E0D159CA7FFE5E63DBD5F83D5ED
uid                      Strategos <strategos@tryhackme.thm>
sub   cv25519 2024-08-29 [E]

You may need to use GPG to decrypt files in CTFs. With PGP/GPG, private keys can be protected with passphrases in a similar way that we protect SSH private keys. If the key is passphrase protected, you can attempt to crack it using John the Ripper and gpg2john. The key provided in this task is not protected with a passphrase. The man page for GPG can be found online here.
Practi cal Example

Now that you have your GPG key pair, you can share the public key with your contacts. Whenever your contacts want to communicate securely, they encrypt their messages to you using your public key. To decrypt the message, you will have to use your private key. Due to the importance of the GPG keys, it is vital that you keep a backup copy in a secure location.

Let’s say you got a new computer. All you need to do is import your key, and you can start decrypting your received messages again:

    You would use gpg --import backup.key to import your key from backup.key
    To decrypt your messages, you need to issue gpg --decrypt confidential_message.gpg

Answer the questions below

    Use GPG to decrypt the message in ~/Public-Crypto-Basics/Task-7. What secret word does the message hold?

Answer: Pineapple

gpg --decrypt ~/Public-Crypto-Basics/Task-7
