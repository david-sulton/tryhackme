# Subdomain Enumeration
<img width="512" height="512" alt="609f307f6fa5f1f7f39d50154d0d1db3(1)" src="https://github.com/user-attachments/assets/7948c7c2-0998-4f87-aa46-0fe906539b45" />

## OSINT - SSL/TLS Certificates
- When an SSL/TLS (Secure Sockets Layer/Transport Layer Security) certificate is created for a domain by a CA (Certificate Authority), CA's take part in what's called "Certificate Transparency (CT) logs". These are publicly accessible logs of every SSL/TLS certificate created for a domain name. The purpose of Certificate Transparency logs is to stop malicious and accidentally made certificates from being used. We can use this service to our advantage to discover subdomains belonging to a domain, sites like https://crt.sh offer a searchable database of certificates that shows current and historical results.

## OSINT - Search Engines
- Search engines contain trillions of links to more than a billion websites, which can be an excellent resource for finding new subdomains. Using advanced search methods on websites like Google, such as the site: filter, can narrow the search results. For example, site:*.domain.com -site:www.domain.com would only contain results leading to the domain name domain.com but exclude any links to www.domain.com; therefore, it shows us only subdomain names belonging to domain.com.

## DNS bruteforce
- Bruteforce DNS (Domain Name System) enumeration is the method of trying tens, hundreds, thousands or even millions of different possible subdomains from a pre-defined list of commonly used subdomains. Because this method requires many requests, we automate it with tools to make the process quicker. In this instance, we are using a tool called dnsrecon to perform this. Click the "View Site" button to open the static site, press the "Run DNSrecon Request" button to start the simulation, and then answer the question below.
- dnsrecon -t brt -d acmeitsupport.thm

## OSINT - Sublist3r
- To speed up the process of OSINT subdomain discovery, we can automate the above methods with the help of tools like Sublist3r, click the "View Site" button to open up the static site and run the sublist3r simulation to discover a new subdomain that will help answer the question below.
- ./sublist3r.py -d acmeitsupport.thm

## Virtual Hosts
- ```ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://10.10.163.14 -fs {size}```
- Some subdomains aren't always hosted in publically accessible DNS results, such as development versions of a web application or administration portals. Instead, the DNS record could be kept on a private DNS server or recorded on the developer's machines in their /etc/hosts file (or c:\windows\system32\drivers\etc\hosts file for Windows users), which maps domain names to IP addresses. 
- Because web servers can host multiple websites from one server when a website is requested from a client, the server knows which website the client wants from the Host header. We can utilize this host header by making changes to it and monitoring the response to see if we've discovered a new website.
- Like with DNS Bruteforce, we can automate this process by using a wordlist of commonly used subdomains.
