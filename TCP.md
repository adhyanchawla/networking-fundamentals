### TCP

- Transmission Control Protocol that controls the transmission unlike UDP (firehose)
- Layer 4 Protocol
- Ability to address processes in a host using ports
- Connection exists (knowledge shared between client and server i.e. they store a state ie stateful)
- requires handshake
- 20 bytes headers segment (can go to 60)

- TCP use cases:
    - Reliable communication (chat)
    - Remote shell
    - Databases connection
    - Web communications (HTTP protocol built on TCP)
    - Any bidirectional communication (web sockets)

- TCP connection
    - Connection is a layer 5 (session)
    - Connection is an agreement between client and server
    - Must create a connection to send data
    - Connection is identified by 4 properties
        - SourceIP - SourcePort
        - DestinationIP - DestinationPort
    - why?? main reason is the security
    - the details of the connection is hashed and looked up in the operating system (could be stored in memory or persistent disk)
    - cant send data outside the connection
    - sometimes called socket or file descriptor (Layer 5 thing, storing a session - state 5 stateful)
    - requires a 3 way TCP handshake
    - segments are sequenced and ordered
    - segments are acknowledged
    - lost segments are retransmitted -- reliable 

- Multiplexing and Demultiplexing
    - IP target hosts only
    - Hosts may run many apps each with different requirements
    - Ports now identify 'apps' or 'process'
    - Sender multiplexes all its apps into TCP connections
    - Receiver demultiplexes TCP segments to each app based on connection pairs

- Connection Establishment
    - App1 on 10.0.0.1 wants to send data to AppX on 10.0.0.2
    - App1 sends SYN to AppX to synchronous sequence numbers
    - AppX sends SYN/ACK to synchronous its sequence number
    - App1 ACK AppX SYN
    - Three way handshake
    - The 4 details mentioned
        - SourceIP - SourcePort
        - DestinationIP - DestinationPort
    - are hashed and looked up from the table and stored in file desciptor

- Sending Data
    - App1 sends data to AppX (sending an ls)
    - App1 encapsulates data in a segment and send it
    - AppX acknowledges the segment
    - Question is:
        - Can App1 send new segment before ack of old segment arrives??
            - Yes it can but there is a threshold limit upto which it cannot send new segments

- Acknowledgement
    - App1 sends segments 1, 2, 3 to AppX
    - AppX acknowledges all of them with a single segment ACK3 (multiple segments acknowledged)

- Lost Data
    - App1 sends segments 1, 2, 3 to AppX
    - Seg 3 is lost, AppX acknowledge 2
    - App1 resends Seg 3
    - finally ACK3 is done

- Closing connection (4 way handshake)
    - App1 wants to close the connection
    - App1 sends FIN, AppX ACK
    - AppX sends FIN, App1 ACK


### Anatomy of TCP Segment

- Maximum Segment Size thats to be transmitted
    - Segment size depends on the MTU of the network
    - Usually 512 bytes but can go upto 1460
    - Default MTU in the internet is 1500
    - Jumbo frames MTU goes upto 9000 or more
    - MSS (Maximum Segment Size) can be larger in jumbo frames cases

### Flow Control
- How much a receiver can handle??
    - A wants to send 10 segments to B
    - A sends 1 seg
    - B sends 1 ACK
    (... and so on)
    - very slow!! to much latency

- Solution to the problem
    - A can send multiple segments and B can ack in 1 ACK
    - The question arises how much A can send??
    - This is called flow control

    - When TCP segments arrive they are put in receiver's buffer
    - If we kept sending the data, the receiver will be overwhelmed
    - Segments can be dropped in that case
    - Solution? Let the sender know how much you can handle

    - Window Size (RWND) -- Receiver Window
        - 16 bit to 64 KB
        - Updated with each Acknowldgement
        - Tells the sender how much to send before waiting for ACK
        - Receiver can decide to decrease the window size (out of memory) -- very critical stuff
    
    - Sliding Window (Sender property)
        - In above approach, we just cant keep waiting for the receiver to acknowledge the segments
        - whatever gets acknowledge moves
        - therefore we slide the window once we receive an acknowledgment
        - Sender maintains sliding window for the receiver
    
    - Window Scaling
        - 64 KB seems to be too small
        - We cannot increase the bits in the segment
        - So while exchange during handshake we mention the scaling factor b/w (0 - 14)
        - Due to which window size can go upto 1GB -> 2^16-1 * 2^14

### Congestion Control
- How much the network can handle?
    - the receiver might handle the load but the middle boxes might not
    - the routers in the middle have limit
    - we do not want to congest the network with data
    - we need to avoid congestion
    - a new window i.e. congestion window (CWND) is required and sender is the one that is responsible for it
    - this is to avoid a lot of retransmissions due to congestion happening at routers (which is to prevent the decrease in performance)

- Algorithms which help to maintain congestion control:
    - TCP Slow start
    - Congestion avoidance

- TCP slow start
    - slow start is faster than congestion avoidance
    - CWND + 1 MSS after each ACK (exponentially increase the CWND after each ACK)
    - CWND starts with 1 MSS or more
    - Sends 1 segment and waits for 1 ACK
    - With each ACK received CWND is incremented by 1 MSS
    - until we reach the slow start threshold we switch to congestion avoidance algo

- Congestion avoidance (linear)
    - Once slow start reaches its threshold this algo kicks in
    - CWND + 1 MSS after every complete RTT (means lets you send 7 packets, you will be acknowledged for all the 7 packets, that means 1 RTT -- round trip)
    - CWND must not always exceed RWND
    - during 1 RTT, CWND is incremented by rougly 1 full sized segment per RTT
    - Send CWND worth of segments and wait for ACK
    - only when all segments are ACKed add upto 1 MSS to CWND
    - Precisely CWND = CWND + MSS * MSS / CWND

- Congestion Notification
    - We dont want routers to drop packets
    - Can routers let us know if they are about to hit the congestion
    - Meet ECN (Explicit congestion notification)
    - routers and middle boxes can tag IP packets with ECN
    - the receiver will copy this bit back to the sender
    - ECN is IP header bit
    - so routers dont drop packets just let the sender know when you about 

- Congestion detection
    - the moment we get timeouts, dup ACKs or packet drops
    - The slow start threshold is reduced to half to whatever unacknowledged data is sent (CWND/2 if all CWND worth data is unacknowledged)
    - The CWND is reset to 1 and we start over
    - Min slow start threshold is 2 * MSS

### NAT
    - How the WAN sees your internal devices??
- NAT is a table of network addresses in a table, lives in the router and maps the private IP address to a public image
- IPv4 is limited to only 4 billion (thats why NAT came up as a solution to mitigate this issue)
- Private vs public IP addresses
- Eg 192.168.x.x or 10.0.0.x is private and is not routable throughout the internet
- internal hosts can be assigned private addresses
- router needs to translate requests
- there is a limit to which private IP addresses are mapped to public IPs via NAT (65000)

    - NAT Applications
        - Private to Public translations
            - So we dont run out of IPv4
        - Port forwarding
            - Add a NAT entry to the router to forward the packets to port 80 to a machine in your LAN
            - No need to have root access to listen on port 80 on your device
            - Expose local web server locally
        - Layer 4 load balancing
            - HAProxy NAT Mode - The LB is our gateway
            - Clients send a request to a bogus IP address
            - Routers intercepts the packet and replaces the service IP with the destination server
            - Layer 4 reverse proxying
            - Cons its limited as you need to configure client fleets to be gateways to your LBs

### TCP Pros and Cons
- Pros
    - Guaranteed delivery
    - No one can send data without prior knowledge
    - Flow and congestion control
    - ordered packets so no corruption or app level work
    - secure and cant be easily spoofed

- Cons
    - Large header overhead as compared to UDP
    - More bandwidth
    - Stateful consumes memory at client and server
    - considered high latency for certain workloads (slow start/congestion/ acks)
    - Does too much at low level (hence QUIC)
        - Single connection to send multiple streams of data (HTTP requests)
        - Stream 1 has nothing to with Stream 2
        - Both stream 1 and stream 2 packets must arrive
    - TCP meltdown
        - Not a good candidate for VPN

