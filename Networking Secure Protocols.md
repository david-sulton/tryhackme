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
![image](https://github.com/user-attachments/assets/7f57d557-edaf-4225-b55e-429cf38e99e3)

## SSH
- OpenSSH offers several benefits. We will list a few key points:
- Secure authentication: Besides password-based authentication, SSH supports public key and two-factor authentication.
```Confidentiality: OpenSSH provides end-to-end encryption, protecting against eavesdropping. Furthermore, it notifies you of new server keys to protect against man-in-the-middle attacks.
    Integrity: In addition to protecting the confidentiality of the exchanged data, cryptography also protects the integrity of the traffic.
    Tunneling: SSH can create a secure “tunnel” to route other protocols through SSH. This setup leads to a VPN-like connection.
    X11 Forwarding: If you connect to a Unix-like system with a graphical user interface, SSH allows you to use the graphical application over the network.
```
- Port 22
- ```ssh username@hostname```
- ```ssh 192.168.124.148 -X``` #supports graphical user interfaces
- 
## SFTP and FTPS
- SFTP stands for SSH File Transfer Protocol and allows secure file transfer. It is part of the SSH protocol suite and shares the same port number, 22. If enabled in the OpenSSH server configuration, you can connect using a command such as ```sftp username@hostname```. Once logged in, you can issue commands such as ```get filename``` and ```put filename``` to download and upload files, respectively. Generally speaking, SFTP commands are Unix-like and can differ from FTP commands.
## VPN
- virtual private network (VPN)
![image](https://github.com/user-attachments/assets/b19b2f24-d730-4131-9a3a-6019fad7306b)

Once a VPN tunnel is established, all our Internet traffic will usually be routed over the VPN connection, i.e. via the VPN tunnel. Consequently, when we try to access an Internet service or web application, they will not see our public IP address but the VPN server’s. This is why some Internet users connect over VPN to circumvent geographical restrictions. Furthermore, the local ISP will only see encrypted traffic, which limits its ability to censor Internet access.

In other words, if a user connects to a VPN server in Japan, they will appear to the servers they access as if located in Japan. These servers will customise their experience accordingly, such as redirecting them to the Japanese version of the service. The screenshot below shows the Google Search page after connecting to a VPN server in Japan.
![image](https://github.com/user-attachments/assets/3306469b-f085-4140-9504-e25892436040)


## Closing Notes
