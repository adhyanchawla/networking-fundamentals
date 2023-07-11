OSI Model

Why do we need a communication model?

- Since we follow a particular standard, such information like what happens when we hit a request to receive a response
- If we have to build the communication part by ourselves, then the application must have the knowledge of underlying network medium
- We can decouple innovation in each layer which does not hamper the working of the model


example on the basis of sender

7 layers of OSI Model protocols/responsibilities:
- Application layer: HTTP, FTP, gRPC 
    - eg POST request with JSON data to HTTPs server --- HTTPs ensures encrytion using TLS
- Presentation layer: Encoding, Serialized data for 
    - eg, converts JSON form to flat byte strings, UTF-8, proxies
- Session layer: Responsible for connection establishment, TLS (state stored in a server) 
    - eg Connection could be TCP/TLS
- Transport layer: TCP (state), UDP (stateless) (segments) --- we care about ports 
    - eg sends SYN request target port 443
- Network layer: IP protocol (packet destined to an IP) 
    - eg SYN is placed an IP packet and adds source/dest IPs (we need to do DNS to get IP)
- Data link layer: MAC address ethernet, Frames (sending a frame over an ethernet)
    - eg Each packet goes into single frame and adds source/dest MAC addresses
- Physical layer: Deals with the way data is transferred to other medium for eg via fibre cables, electric signal or radio waves (WIFI)
    - eg Each frame converts into a string of bits which converted into radio signal (WIFI), electric signal (ethernet), or light (fiber)


example on the basis of receiver

7 layers of OSI Model protocols/responsibilities:
- Physical layer
    - eg Radio, electric, or light is received and converted into digital bits (data can be sent via WIFI can be received in a different medium lets say fiber)
- Data link layer
    - eg Bits from layer 1 converted back into frames
- Network layer
    - eg Frames from layer 2 converted into IP packet
- Transport layer
    - eg IP packets are converted back into TCP segments
    - deals with congestion control/flow control in case of TCP
    - If segment is SYN we dont need to go down further to establish a connection as connection request is still being processed
- Session layer
    - connection establishment
- Presentation layer
    - Deserialize flat byte strings back to JSON for the app to consume (can be a different language here)
- Application layer
    - Application understands the JSON post request and request receive event is triggered (maybe a query into database)

- Points to remember here are
    - A client is not directly connected to a server. There are switches and routers that lie in the path or load balancer or proxy
    - Switch
        - A switch (layer 2 device) connects different subnets together, but not only subnets, can connect hosts in the same subnet
        - Switch understands where to send the data based on Mac Address, most switches dont look at layer 3
    - Router 
        - will act like a switch if you are sending data across the same subnet
        - needs IP addresses to route (layer 3 device)
    - Firewall
        - blocks certain apps from sending data or blocks certain unwanted packets
        - checks for IP address at layer 3 and checks for ports at layer 4
        - can be transparent i.e. public (visible to everyone) and not encrypted
        - ISPs
    - VPNs
        - layer 3 protocol which takes the IP packets and puts them into another IP packets
    - Load balancer (slower than router or firewall)
        - lets say layer 7 load balancer eg nginx
            /images - go to this server
        - CDN
            - layer 7
            - decrypt everything you send

- Shortcomings of OSI model
    - Too many layers
        - Alternative to OSI is TCP/IP model
            - just 4 layers
            - Application (5, 6, 7)
            - Transport (4)
            - Internet (3)
            - Data link (2)
            - Physical (not officially covered)

- How to hosts talk to each other
    - layer 2 concepts
    - each host network card has a unique Media Access Control (MAC)
    - lets say there is a network with hosts A, B, C, D (mesh network)
        - A sends message to B
        - But that message is broadcasted to all the hosts but B accepts it
        - Message has a mac address with which every host is compared
        - Dangerous as its not secure as these MAC addresses dont have the information about where to send it and where not to send it
        - eg Public wifi
        - Thats why we need routability which brings in the need for IP addresses
            - IP address: (4 bytes) two parts
            - Part 1: identifies the network
            - Part 2: identifies the host
            eg 192.168.2.3/24 that slash 24 is the network part

            - How to check if two IPs are in the same network 
                - Apply the subnet mask 
        - MAC addresses are more effective when we are inside a local network

        - In case the host has many apps running with different reqts
            - Then ports come into picture (running on same server)
                - HTTP request on port 80
                - DNS request on port 53
                - SSH request on port 22

- Localhost: 