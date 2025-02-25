![image](https://github.com/user-attachments/assets/92e66d3f-bdac-4b5b-90f3-086fc0a9af42)

# Networking Core Protocols

## DNS
- DNS operates at the Application Layer, i.e., Layer 7 of the ISO OSI model. DNS traffic uses UDP port 53 by default and TCP port 53 as a default fallback. There are many types of DNS records; however, in this task, we will focus on the following four:

      A record: The A (Address) record maps a hostname to one or more IPv4 addresses. For example, you can set example.com to resolve to 172.17.2.172.
      AAAA Record: The AAAA record is similar to the A Record, but it is for IPv6. Remember that it is AAAA (quad-A), as AA and AAA would refer to a battery size; furthermore, AAA refers to Authentication, Authorization, and Accounting; neither falls under DNS.
      CNAME Record: The CNAME (Canonical Name) record maps a domain name to another domain name. For example, www.example.com can be mapped to example.com or even to example.org.
      MX Record: The MX (Mail Exchange) record specifies the mail server responsible for handling emails for a domain.
- nslookup www.amazon.com
## WHOIS
- whois amazon.com
- You can register any available domain name for one or more years. You need to pay the annual fee, and you are required to provide accurate contact information as the registrant. This information is part of the data available via WHOIS records and is available publicly. (Although written in uppercase, WHOIS is not an acronym; it is pronounced who is.) However, don’t worry if you want to register a domain without revealing your contact information publicly; you can use one of the privacy services that hide all your information from the WHOIS records.
- You can look up the WHOIS records of any registered domain name using one of the online services or via the command-line tool whois, available on Linux systems, among others. As expected, a WHOIS record provides information about the entity that registered a domain name, including name, phone number, email, and address. In the screenshot shown below, you can see when the record was first created and when it was last updated. Moreover, you can find the registrant’s name, address, phone, and email.
## HTTPS
- When you fire up your browser, you mainly use HTTP and HTTPS protocols. HTTP stands for Hypertext Transfer Protocol; the S in HTTPS stands for Secure. This protocol relies on TCP and defines how your web browser communicates with the web servers.

- Some of the commands or methods that your web browser commonly issues to the web server are:

      GET retrieves data from a server, such as an HTML file or an image.
      POST allows us to submit new data to the server, such as submitting a form or uploading a file.
      PUT is used to create a new resource on the server and to update and overwrite existing information.
      DELETE, as the name suggests, is used to delete a specified file or resource on the server.

- HTTP and HTTPS commonly use TCP ports 80 and 443, respectively, and less commonly other ports such as 8080 and 8443.
```
root@ip-10-10-222-208:~# telnet 10.10.131.93 80
Trying 10.10.131.93...
Connected to 10.10.131.93.
Escape character is '^]'.
GET /flag.html HTTP/1.1
Host: anything

HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Tue, 25 Feb 2025 16:39:43 GMT
Content-Type: text/html
Content-Length: 478
Last-Modified: Thu, 27 Jun 2024 07:28:15 GMT
Connection: keep-alive
ETag: "667d148f-1de"
Accept-Ranges: bytes

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Hidden Message</title>
    <style>
        body {
            background-color: white;
            color: white;
            font-family: Arial, sans-serif;
        }
        .hidden-text {
            font-size: 1px;
        }
    </style>
</head>
<body>
    <div class="hidden-text">THM{TELNET-HTTP}</div>
</body>
</html>
```
        
## FTP
- Unlike HTTP, which is designed to retrieve web pages, File Transfer Protocol (FTP) is designed to transfer files. As a result, FTP is very efficient for file transfer, and when all conditions are equal, it can achieve higher speeds than HTTP.

Example commands defined by the FTP protocol are:

    USER is used to input the username
    PASS is used to enter the password
    RETR (retrieve) is used to download a file from the FTP server to the client.
    STOR (store) is used to upload a file from the client to the FTP server.

```
ftp 10.10.10.10
anonymous
<enter>
 ls
get flag.txt
```

## SMTP
- As with browsing the web and downloading files, sending email needs its own protocol. Simple Mail Transfer Protocol (SMTP) defines how a mail client talks with a mail server and how a mail server talks with another.

- The analogy for the SMTP protocol is when you go to the local post office to send a package. You greet the employee, tell them where you want to send your package, and provide the sender’s information before handing them the package. Depending on the country you are in, you might be asked to show your identity card. This process is not very different from an SMTP session.

Let’s present some of the commands used by your mail client when it transfers an email to an SMTP server:

    HELO or EHLO initiates an SMTP session
    MAIL FROM specifies the sender’s email address
    RCPT TO specifies the recipient’s email address
    DATA indicates that the client will begin sending the content of the email message
    . is sent on a line by itself to indicate the end of the email message

## POP3
- You’ve received an email and want to download it to your local mail client. The Post Office Protocol version 3 (POP3) is designed to allow the client to communicate with a mail server and retrieve email messages.

- Without going into in-depth technical details, an email client sends its messages by relying on SMTP and retrieves them using POP3. SMTP is similar to handing your envelope or package to the post office, and POP3 is similar to checking your local mailbox for new letters or packages.

- POP3 is like a personal mailbox. SMTP is like the Post Office designated box.

Some common POP3 commands are:

    USER <username> identifies the user
    PASS <password> provides the user’s password
    STAT requests the number of messages and total size
    LIST lists all messages and their sizes
    RETR <message_number> retrieves the specified message
    DELE <message_number> marks a message for deletion
    QUIT ends the POP3 session applying changes, such as deletions

```
telnet 10.10.10.10 110
AUTH
USER linda
PASS Pa$$123
LIST
RETR 1
RETR 2
RETR 3
RETR 4
```
## IMAP
- POP3 is enough when working from one device, e.g., your favourite email client on your desktop computer. However, what if you want to check your email from your office desktop computer and from your laptop or smartphone? In this scenario, you need a protocol that allows synchronization of messages instead of deleting a message after retrieving it. One solution to maintaining a synchronized mailbox across multiple devices is Internet Message Access Protocol (IMAP).

- IMAP allows synchronizing read, moved, and deleted messages. IMAP is quite convenient when you check your email via multiple clients. Unlike POP3, which tends to minimize server storage as email is downloaded and deleted from the remote server, IMAP tends to use more storage as email is kept on the server and synchronized across the email clients.

The IMAP protocol commands are more complicated than the POP3 protocol commands. We list a few examples below:

    LOGIN <username> <password> authenticates the user
    SELECT <mailbox> selects the mailbox folder to work with
    FETCH <mail_number> <data_item_name> Example fetch 3 body[] to fetch message number 3, header and body.
    MOVE <sequence_set> <mailbox> moves the specified messages to another mailbox
    COPY <sequence_set> <data_item_name> copies the specified messages to another mailbox
    LOGOUT logs out

# conclusion
![image](https://github.com/user-attachments/assets/3135d68b-e87c-4800-ab1f-136b8de3ac90)
