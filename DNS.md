### Why DNS
- People cant remember IPs
- A domain is a plain text that points to 1 IP or a collection of IPs
- Addition of layer of abstraction is good
- IP can change while domain remains
- We can serve the closest IP to a client requesting the same domain
- Load balancing

## What is DNS
- A new addressing system means that we need mapping, meet DNS
- Built on top of UDP
- Port 53 (default), but can use any port
- If you have a name and you need an IP, use DNS
- If you have an IP and you need a MAC, we use ARP

## How DNS Works
- So when a request for an IP to DNS: the request goes to the DNS resolver; first it checks the cache if there is any entry available
- if not, it asks for the root server, to give me the top level domain server which stores .com or something which is a part of the suffix of the URL, 
- in response the root servers return the particular TLD server where it can ask for a particular authoritative server where the IP can be made available for the particular request
- in response TLD server gives the path to Authoritative Name server from where it fetches the IP which is a part of response returned by the authoritative server

## Why so many layers in DNS?
- because we want the system to be decentralized and well distributed -- we want to partition the namespace and we dont want to slow down the queries

- DNS is not encrypted by default
- Many attacks happen against DNS - (DNS hijacking/DNS poisoning)
- DoT/DOH attempts to address this problem