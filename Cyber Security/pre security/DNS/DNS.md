# DNS(Domain Name System)
- It is a simple way for us to communicate with devices on the internet without remembering complex numbers.
- An IP address starts from 0 and goes to 255 
- every site has its own ip 
- instead of remembering every webs unique ip dns helps access by just its name like tryhackme.com
# Hierarchy of domain
1. TLD (top level domain)
2. SLD(Second Level Domain)
3. Subdomain

## Top level domain
- the most right hand part of a domain name 
- in tryhackme.com the tld is .com
### two types of tld
1. gTLD (generic top level domain)
2. ccTLD(country code top level domain)

### gTLD
- these are used to tell us the domain name'purpose
- like .com for commercial .org for organisation .edu for education .gov for government
- there are over 2000 like this 
### ccTLD
- it is for geographical purposes 
- .ca for canada .co.uk for united kingdom
## SLD(Second Level Domain)
- the tryhackme is the second level domain 
- the limit for this is 63 characters plus the tld 
- it can only use a to z and 0 to 9 and hyphens 
- no consecutive hyphens and cannot start or end with hyphens 
## Subdomain
- its on the left side of the second level domain separated by a period 
- in admin.tryhackme.com the .admin part is the subdomain 
- has the same rules for the name as the second level domain 
- you can use multiple subdomain separated by a period like jupiter.service.thm.com
- but the length must be 253 characters or less (domain name)
- you can create unlimited number of subdomains for your domain name
# DNS record types 
- DNS isn't just for websites though, and multiple types of DNS record exist
## types of dns records
1. A record 
- these records resolve to ipv4 addresses 
2. AAAA record
- these records resolve to ipv6 addresses
3. CNAME record
-  these recoeds resolve to another domain name 
- for example tryhackme online shop has the subdomain name store.tryhackme.com which returns CNAME record shops.shopify.com 
- another dns request would then be made to work out the ip address
5. Mx record
- these records resolve to the address of the servers that handle the email for the domain you are querying 
- for example for mx record of tryhackme.com would be alt1.aspmx.l.google.com 
- these come with a priority flag 
- these are for the case that if the main server goes down email needs to be sent to the backup server
6. TXT record
- these are free text fields where any text based data can be stored 
- these have multiple uses like to list servers that have the authority to send an email on behalf of the domain
- these can also be used to verify ownership of the domain name when signing up for third party services 
- for example @ TXT "MS = ms 12345678"
# process of a dns request 
1. When you request a domain name, your computer first checks its local cache to see if you've previously looked up the address recently; if not, a request to your Recursive DNSServer will be made
2. A Recursive DNS Server is usually provided by your ISP, but you can also choose your own. This server also has a local cache of recently looked up domain names. If a result is found locally, this is sent back to your computer, and your request ends here . If the request cannot be found locally, a journey begins to find the correct answer, starting with the internet's root DNS servers.
3. The root servers act as the DNS backbone of the internet; their job is to redirect you to the correct Top Level Domain Server, depending on your request. If, for example, you request [www.tryhackme.com](http://www.tryhackme.com/), the root server will recognise the Top Level Domain of .com and refer you to the correct TLD server that deals with .com addresses.
4. 1. The TLD server holds records for where to find the authoritative server to answer the DNS request. The authoritative server is often also known as the nameserver for the domain. For example, the name server for [tryhackme.com](http://tryhackme.com/) is [kip.ns.cloudflare.com](http://kip.ns.cloudflare.com/) and [uma.ns.cloudflare.com](http://uma.ns.cloudflare.com/). You'll often find multiple nameservers for a domain name to act as a backup in case one goes down.
5. An authoritative DNS server is the server that is responsible for storing the DNS records for a particular domain name and where any updates to your domain name DNS records would be made. Depending on the record type, the DNS record is then sent back to the Recursive DNSServer, where a local copy will be cached for future requests and then relayed back to the original client that made the request. DNS records all come with a TTL (Time To Live) value. This value is a number represented in seconds that the response should be saved for locally until you have to look it up again. Caching saves on having to make a DNS request every time you communicate with a server.