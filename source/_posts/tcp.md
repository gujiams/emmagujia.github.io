---
title: TCP 3-Way Handshake
date: 2019-03-03 20:00:00
categories: security
tags:
---
TCP 3-Way Handshake
<!-- more -->
### What are TCP Flags?
TCP flags are used within TCP packet transfers to indicate a particular connection state or provide additional information. Therefore, they can be used for troubleshooting purposes or to control how a particular connection is handled. There are a few TCP flags that are much more commonly used than others as such “SYN”, “ACK”, and “FIN”. However, in this post, we’re going to go through the full list of TCP flags and outline what each one is used for.
#### List of TCP Flags
Each TCP flag corresponds to 1 bit in size. The list below describes each flag in greater detail. Additionally, check out the corresponding RFC section attributed to certain flags for a more comprehensive explanation.
![1](tcp/1-handshake.png)
1. ACK - The ACK flag, which stands for “Acknowledgment”, is used to acknowledge the successful receipt of a packet. As we can see from the diagram above, the receiver sends an ACK as well as a SYN in the second step of the 3-way handshake process to tell the sender that it received its initial packet.
2. FIN - The FIN flag, which stands for “Finished”, means there is no more data from the sender. Therefore, it is used in the last packet sent from the sender.
3. URG - The URG flag is used to notify the receiver to process the urgent packets before processing all other packets. The receiver will be notified when all known urgent data has been received. See RFC 6093 for more details.
4. PSH - The PSH flag, which stands for “Push”, is somewhat similar to the URG flag and tells the receiver to process these packets as they are received instead of buffering them.
5. RST - The RST flag, which stands for “Reset”, gets sent from the receiver to the sender when a packet is sent to a particular host that was not expecting it.
6. ECE - This flag is responsible for indicating if the TCP peer is ECN capable. See RFC 3168 for more details.
7. CWR - The CWR flag, which stands for Congestion Window Reduced, is used by the sending host to indicate it received a packet with the ECE flag set. See RFC 3168 for more details.
### Protocol operation
TCP protocol operations may be divided into three phases. Connections must be properly established in a multi-step handshake process (connection establishment) before entering the data transfer phase. After data transmission is completed, the connection termination closes established virtual circuits and releases all allocated resources.
A TCP connection is managed by an operating system through a programming interface that represents the local end-point for communications, the Internet socket. During the lifetime of a TCP connection the local end-point undergoes a series of state changes：
LISTEN
(server) represents waiting for a connection request from any remote TCP and port.
SYN-SENT
(client) represents waiting for a matching connection request after having sent a connection request.
SYN-RECEIVED
(server) represents waiting for a confirming connection request acknowledgment after having both received and sent a connection request.
ESTABLISHED
(both server and client) represents an open connection, data received can be delivered to the user. The normal state for the data transfer phase of the connection.
FIN-WAIT-1
(both server and client) represents waiting for a connection termination request from the remote TCP, or an acknowledgment of the connection termination request previously sent.
FIN-WAIT-2
(both server and client) represents waiting for a connection termination request from the remote TCP.
CLOSE-WAIT
(both server and client) represents waiting for a connection termination request from the local user.
CLOSING
(both server and client) represents waiting for a connection termination request acknowledgment from the remote TCP.
LAST-ACK
(both server and client) represents waiting for an acknowledgment of the connection termination request previously sent to the remote TCP (which includes an acknowledgment of its connection termination request).
TIME-WAIT
(either server or client) represents waiting for enough time to pass to be sure the remote TCP received the acknowledgment of its connection termination request. [According to RFC 793 a connection can stay in TIME-WAIT for a maximum of four minutes known as two MSL (maximum segment lifetime).]
CLOSED
(both server and client) represents no connection state at all.

### Two scenarios where a three-way handshake will take place
1. Establishing a connection (an active open)
2. Terminating a connection (an active close)
#### Connection establishment
To establish a connection, TCP uses a three-way handshake. Before a client attempts to connect with a server, the server must first bind to and listen at a port to open it up for connections: this is called a passive open. Once the passive open is established, a client may initiate an active open. To establish a connection, the three-way (or 3-step) handshake occurs:
1. SYN: The active open is performed by the client sending a SYN to the server. The client sets the segment's sequence number to a random value A.
2. SYN-ACK: In response, the server replies with a SYN-ACK. The acknowledgment number is set to one more than the received sequence number i.e. A+1, and the sequence number that the server chooses for the packet is another random number, B.
3. ACK: Finally, the client sends an ACK back to the server. The sequence number is set to the received acknowledgement value i.e. A+1, and the acknowledgement number is set to one more than the received sequence number i.e. B+1.

At this point, both the client and server have received an acknowledgment of the connection. The steps 1, 2 establish the connection parameter (sequence number) for one direction and it is acknowledged. The steps 2, 3 establish the connection parameter (sequence number) for the other direction and it is acknowledged. With these, a full-duplex communication is established.
#### Connection termination
![2](tcp/2-tcp-close.png)

The connection termination phase uses a four-way handshake, with each side of the connection terminating independently. When an endpoint wishes to stop its half of the connection, it transmits a FIN packet, which the other end acknowledges with an ACK. Therefore, a typical tear-down requires a pair of FIN and ACK segments from each TCP endpoint. After the side that sent the first FIN has responded with the final ACK, it waits for a timeout before finally closing the connection, during which time the local port is unavailable for new connections; this prevents confusion due to delayed packets being delivered during subsequent connections.
A connection can be "half-open", in which case one side has terminated its end, but the other has not. The side that has terminated can no longer send any data into the connection, but the other side can. The terminating side should continue reading the data until the other side terminates as well.
It is also possible to terminate the connection by a 3-way handshake, when host A sends a FIN and host B replies with a FIN & ACK (merely combines 2 steps into one) and host A replies with an ACK.
Some host TCP stacks may implement a half-duplex close sequence, as Linux or HP-UX do. If such a host actively closes a connection but still has not read all the incoming data the stack already received from the link, this host sends a RST instead of a FIN (Section 4.2.2.13 in RFC 1122). This allows a TCP application to be sure the remote application has read all the data the former sent—waiting the FIN from the remote side, when it actively closes the connection. But the remote TCP stack cannot distinguish between a Connection Aborting RST and Data Loss RST. Both cause the remote stack to lose all the data received.
Some application protocols using the TCP open/close handshaking for the application protocol open/close handshaking may find the RST problem on active close. As an example:
```
s = connect(remote);
send(s, data);
close(s);
```
For a program flow like above, a TCP/IP stack like that described above does not guarantee that all the data arrives to the other application if unread data has arrived at this end.

### References
http://www.omnisecu.com/tcpip/tcp-three-way-handshake.php
https://en.wikipedia.org/wiki/Transmission_Control_Protocol