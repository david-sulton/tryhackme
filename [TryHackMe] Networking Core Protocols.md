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
## FTP
## SMTP
## POP3
## IMAP
