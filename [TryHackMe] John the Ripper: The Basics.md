# John the Ripper: The Basics
![image](https://github.com/user-attachments/assets/f005bef3-dae5-434f-bd13-ec3dd6d0b9b3)
## TOC
    Task1 Introduction
    Task2 Basic Terms
    Task3 Setting up your system
    Task4 Cracking basic hashes
    Task5 Cracking windows Authentication hashes
    Task6 Cracking /etc/shadow hashes
    Task7 Single crack mode
    Task8 Custom Rules
    Task9 Cracking Password protected zip files
    Task10 Cracking password-protected RAR archives
    Task11 Cracking SSH keys with John
    Task12 Further reading
    
## Task1 Introduction
Learning Objectives

Upon the completion of this room, you learn about using John for:

    Cracking Windows authentication hashes
    Crack /etc/shadow hashes
    Cracking password-protected Zip files
    Cracking password-protected RAR files
    Cracking SSH keys
##  Task2 Basic Terms

### What are Hashes?

A hash is a way of taking a piece of data of any length and representing it in another fixed-length form. This process masks the original value of the data. The hash value is obtained by running the original data through a hashing algorithm. Many popular hashing algorithms exist, such as MD4, MD5, SHA1 and NTLM. Let’s try and show this with an example:

If we take “polo”, a string of four characters, and run it through an MD5 hashing algorithm, we end up with an output of b53759f3ce692de7aff1b5779d3964da, a standard 32-character MD5 hash.

Likewise, if we take “polomints”, a string of 9 characters, and run it through the same MD5 hashing algorithm, we end up with an output of 584b6e4f4586e136bc280f27f9c64f3b, another standard 32-character MD5 hash.

### What Makes Hashes Secure?

Hashing functions are designed as one-way functions. In other words, it is easy to calculate the hash value of a given input; however, it is a hard problem to find the original input given the hash value. In simple terms, a hard problem quickly becomes computationally infeasible in computer science. This computational problem has its roots in mathematics as P vs NP.

In computer science, P and NP are two classes of problems that help us understand the efficiency of algorithms:

    P (Polynomial Time): Class P covers the problems whose solution can be found in polynomial time. Consider sorting a list in increasing order. The longer the list, the longer it would take to sort; however, the increase in time is not exponential.
    NP (Non-deterministic Polynomial Time): Problems in the class NP are those for which a given solution can be checked quickly, even though finding the solution itself might be hard. In fact, we don’t know if there is a fast algorithm to find the solution in the first place.

While this is a fascinating mathematical concept that proves fundamental to computing and cryptography, it is entirely outside the scope of this room. But abstractly, the algorithm to hash the value will be “P” and can, therefore, be calculated reasonably. However, an “un-hashing” algorithm would be “NP” and intractable to solve, meaning that it cannot be computed in a reasonable time using standard computers.

### Where John Comes in

Even though the algorithm is not feasibly reversible, that doesn’t mean cracking the hashes is impossible. If you have the hashed version of a password, for example, and you know the hashing algorithm, you can use that hashing algorithm to hash a large number of words, called a dictionary. You can then compare these hashes to the one you’re trying to crack to see if they match. If they do, you know what word corresponds to that hash- you’ve cracked it!

This process is called a dictionary attack, and John the Ripper, or John as it’s commonly shortened, is a tool for conducting fast brute force attacks on various hash types.

- This room will focus on the most popular extended version of John the Ripper, Jumbo John.

 ## Task3 Setting up your system
```sudo apt install john```
 ```
user@ip-10-10-53-98:~$ john
John the Ripper 1.9.0-jumbo-1+bleeding-ffd18e6b5d 2024-09-29 04:20:54 +0200 OM
P [linux-gnu 64-bit x86_64 AVX2 AC]
Copyright (c) 1996-2024 by Solar Designer and others
Homepage: https://www.openwall.com/john/

Usage: john [OPTIONS] [PASSWORD-FILES]

Use --help to list all available options.
```

### Official installation guide
- https://github.com/openwall/john/blob/bleeding-jumbo/doc/INSTALL 

### Wordlists
- https://github.com/danielmiessler/SecLists
- On Kali /usr/share/wordlists

### RockYou

For all of the tasks in this room, we will use the infamous rockyou.txt wordlist, a very large common password wordlist obtained from a data breach on a website called rockyou.com in 2009. If you are not using any of the above distributions, you can get the rockyou.txt wordlist from the SecLists repository under the /Passwords/Leaked-Databases subsection. You may need to extract it from the .tar.gz format using tar xvzf rockyou.txt.tar.gz

## Task4 Cracking basic hashes
```
John Basic Syntax

The basic syntax of John the Ripper commands is as follows. We will cover the specific options and modifiers used as we use them.

john [options] [file path]

    john: Invokes the John the Ripper program
    [options]: Specifies the options you want to use
    [file path]: The file containing the hash you’re trying to crack; if it’s in the same directory, you won’t need to name a path, just the file.

Automatic Cracking

John has built-in features to detect what type of hash it’s being given and to select appropriate rules and formats to crack it for you; this isn’t always the best idea as it can be unreliable, but if you can’t identify what hash type you’re working with and want to try cracking it, it can be a good option! To do this, we use the following syntax:

john --wordlist=[path to wordlist] [path to file]

    --wordlist=: Specifies using wordlist mode, reading from the file that you supply in the provided path
    [path to wordlist]: The path to the wordlist you’re using, as described in the previous task

Example Usage:

john --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt
Identifying Hashes

Sometimes, John won’t play nicely with automatically recognising and loading hashes, but that’s okay! We can use other tools to identify the hash and then set John to a specific format. There are multiple ways to do this, such as using an online hash identifier like this site. I like to use a tool called hash-identifier, a Python tool that is super easy to use and will tell you what different types of hashes the one you enter is likely to be, giving you more options if the first one fails.

To use hash-identifier, you can use wget or curl to download the Python file hash-id.py from its GitLab page. Then, launch it with python3 hash-id.py and enter the hash you’re trying to identify. It will give you a list of the most probable formats. These two steps are shown in the terminal below.
Terminal

          
user@TryHackMe$ wget https://gitlab.com/kalilinux/packages/hash-identifier/-/raw/kali/master/hash-id.py
$ python3 hash-id.py
   #########################################################################
   #     __  __                     __           ______    _____           #
   #    /\ \/\ \                   /\ \         /\__  _\  /\  _ `\         #
   #    \ \ \_\ \     __      ____ \ \ \___     \/_/\ \/  \ \ \/\ \        #
   #     \ \  _  \  /'__`\   / ,__\ \ \  _ `\      \ \ \   \ \ \ \ \       #
   #      \ \ \ \ \/\ \_\ \_/\__, `\ \ \ \ \ \      \_\ \__ \ \ \_\ \      #
   #       \ \_\ \_\ \___ \_\/\____/  \ \_\ \_\     /\_____\ \ \____/      #
   #        \/_/\/_/\/__/\/_/\/___/    \/_/\/_/     \/_____/  \/___/  v1.2 #
   #                                                             By Zion3R #
   #                                                    www.Blackploit.com #
   #                                                   Root@Blackploit.com #
   #########################################################################
--------------------------------------------------
 HASH: 2e728dd31fb5949bc39cac5a9f066498

Possible Hashs:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4(($pass)).(strtolower($username)))

Format-Specific Cracking

Once you have identified the hash that you’re dealing with, you can tell John to use it while cracking the provided hash using the following syntax:

john --format=[format] --wordlist=[path to wordlist] [path to file]

    --format=: This is the flag to tell John that you’re giving it a hash of a specific format and to use the following format to crack it
    [format]: The format that the hash is in

Example Usage:

john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt hash_to_crack.txt

A Note on Formats:

When you tell John to use formats, if you’re dealing with a standard hash type, e.g. md5 as in the example above, you have to prefix it with raw- to tell John you’re just dealing with a standard hash type, though this doesn’t always apply. To check if you need to add the prefix or not, you can list all of John’s formats using john --list=formats and either check manually or grep for your hash type using something like john --list=formats | grep -iF "md5".
```
## Task5 Cracking windows Authentication hashes































