### IP Address

- Layer 3 property
- Can be set automatically or statically
- Network and host portion
- 4 bytes in IPv4

Network vs Host
-  a.b.c.d/x (a.b.c.d are integers) - x is the network bits and remains the host
- eg 192.168.254.0/24 means first 24 bits are meant for network and the rest are meant for host
- that means we can have 2^24 networks and each network has 2^8 hosts
- Also referred to as a subnet

Subnet Mask
- 192.169.254.0/24 is a subnet
- The subnet has a subnet mask 255.255.255.0
- Subnet mask is used to determine whether the IP belongs to the same subnet

Default gateway
- Lets consider 2 cases:
    - when host A wants to talk to host B if both are in the same subnet (using MAC address)
    - otherwise A sends it someone who might know, gateway (to talk to outside the subnet) -- MAC address is the router and due to this a lot of attack happens (poisoning)
- the gateway has an IP address and each host should know its gateway

- eg lets say our database in not in the subnet and when we query it takes time to receive a response from the database that is because the gateway is dealing with a lot of requests at the same time and your request seems to be left behind. thats why its not recommended to keep the DB in the different subnet. Switch can be a good option to solve this problem.

### ICMP (Internet Control Message Protocol)
- One of the most critical protocols in Layer 3 eg Ping, Traceroute uses it
- Why we use it? Designed for informational messages
    - eg Host unreachable, port unreachable, fragmentation needed
    - Packet expired (infinite loop in routers)
- Uses IP directly
- Dont fragment the messages
- Doesnt require listeners or ports to be opened up
- Some firewalls block ICMP for security reasons thats why PING doesnt work in some cases
- Disabling ICMP can cause damage with connection establishment (TCP blackhole)


- Traceroute
    - used to identify the entire path that the packet takes
    - clever use of TTL and you will get to know router IP address of each hop
    - doesnt always work as path changes and ICMP might get blocked
*** TTL doesnt get affected when you send something in the same network


### IP Packet

- IP Packet
    - Headers
    - Data Sections

- IP Packet header is 20 bytes (can extend upto 60 bytes) - this is not our data, data required for business
- Data section can go upto 65536 bytes
- Packets need to be fragmented if they dont fit the frame size

- IP protocol is stateless

ARP (Address resolution protocol) -- layer 2
- Why do we need it?
- To map IP address with MAC address (sometimes we dont know the MAC)
- MAC addrrss is required to send frames between machines
- ARP table is a cached table that maps IP address to MAC
- Attacks can be peformed on ARP (ARP poisioning)

- Used when ypu want to do load balancing via virtual IP address, than ARP becomes a critical component

DEMO on icmp, arp, ip
command: tcpdump -n -i en0 arp
-n - hostnames
-i - info
-v - verbose mode
people save tcpdump for analysis