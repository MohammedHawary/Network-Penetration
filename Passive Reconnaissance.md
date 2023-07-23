## Passive Reconnaissonce

1. DNS Recon Website
   
   ```bash
   https://dnsdumpster.com/
   ```

2. IP address ,hosting company ,geographic location ,server type and version
   
   ```bash
   https://www.shodan.io/ 
   ```

3. whois,nslookup,dig
   
   | Query type | Result             |
   | ---------- | ------------------ |
   | A          | IPv4 Addresses     |
   | AAAA       | IPv6 Addresses     |
   | CNAME      | Canonical Name     |
   | MX         | Mail Server        |
   | SOA        | Start of Authority |
   | TXT        | TXT Records        |
   
   | Command Line Example                         | Purpose                             |
   | -------------------------------------------- | ----------------------------------- |
   | whois example.com                            | Lookup WHOIS record                 |
   | nslookup -type=A example.com                 | Lookup DNS A records                |
   | nslookup -type=MX example.com $dns_server_IP | Lookup DNS MX records at DNS server |
   | nslookup -type=TXT example.com               | Lookup DNS TXT records              |
   | dig example.com A                            | Lookup DNS A records                |
   | dig @$dns_server_IP example.com MX           | Lookup DNS MX records at DNS server |
   | dig example.com TXT                          | Lookup DNS TXT records              |
