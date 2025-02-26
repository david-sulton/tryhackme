![image](https://github.com/user-attachments/assets/f637100c-9f94-4140-937d-f8bff3d0e4ea)
# Networking Secure Protocols
## Intro 
## TLS
- SSL (Secure Sockets Layer) 2.0 in 1995
- Internet Engineering Task Force (IETF) developed TLS (Transport Layer Security) in 1999
- TLS 1.3 in 2018
- The first step for every server (or client) that needs to identify itself is to get a signed TLS certificate. Generally, the server administrator creates a Certificate Signing Request (CSR) and submits it to a Certificate Authority (CA); the CA verifies the CSR and issues a digital certificate. Once the (signed) certificate is received, it can be used to identify the server (or the client) to others, who can confirm the validity of the signature. For a host to confirm the validity of a signed certificate, the certificates of the signing authorities need to be installed on the host. In the non-digital world, this is similar to recognising the stamps of various authorities. The screenshot below shows the trusted authorities installed in a web browser.
![image](https://github.com/user-attachments/assets/97383d6b-bb77-4f26-b66c-939a3ec65b9b)
- Generally speaking, getting a certificate signed requires paying an annual fee. However, 
<a href="https://letsencrypt.org/">Let's Encrypt</a> allows you to get your certificate signed for free.
## HTTPS
- HTTPS stands for Hypertext Transfer Protocol Secure. It is basically HTTP over TLS. Consequently, requesting a page over HTTPS will require the following three steps (after resolving the domain name):

      Establish a TCP three-way handshake with the target server
      Establish a TLS session
      Communicate using the HTTP protocol; for example, issue HTTP requests, such as
      GET / HTTP/1.1

## SMTPS, POP3S, IMAPS
- 
## SSH
## SFTP and FTPS
## VPN
## Closing Notes
