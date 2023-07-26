### User Datagram Protocol

- Layer 4 protocol
- Ability to address processes in a host using ports
- simple protocol to send and receive the data
- prior communication is not required since its a stateless protocol
- no knowledge is stored on the host
- 8 byte header datagram

- use cases:
    - video streaming
    - VPN
    - DNS
    - WebRTC - web real time communication
    - many games 

- Multiplexing and demultiplexing
    - used in IP target hosts only
    - Hosts run many apps each with different requirements
    - ports now identify the app or process
    - sender multiplexes all its apps into UDP (in one IP, many things can be sent)
    - receiver demultiplexes UDP datagrams to each app (UDP datagrams are unique)

- Source and Destination port
    - lets say App1 on 10.0.0.1 sends data to AppX on 10.0.0.2
    - for instance, destination port is 53
    - AppX responds back to App1
    - We need source port so we know how to send back data
    - Source port 5555

- UDP Datagram
    - UDP headers 8 bytes only (IPv4)
    - Datagram slides into IP packet as 'data'
    - Ports are 16 bit from 0 to 65535 (one can fix only 65536 ports, anything more will fail)

- Pros
    - Simple protocol
    - Header size is small as datagrams are small
    - uses less bandwidth
    - stateless
    - consumes less memory (no state stored in the server/client)
    - Low latency - no handshake, order, retransmission or guaranteed delivery, no prior knowledge communication

- Cons
    - No acknowledgement
    - No guarantee delivery
    - Connection-less due to which anyone can send data without prior knowledge - used for attacks
    - No flow control
    - No congestion control (how much routers can handle)
    - No ordered packets
    - security can be easily spoofed

- Demo
    - tcpdump -n -v -i en0 src 8.8.8.8 or dst 8.8.8.8
    - DNS lookup - nslookup adhyanchawla.com 8.8.8.8 (will give me name as well as IP address)