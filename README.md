# Wireshark Protocol Analysis: TCP Handshake & UDP/DNS Inspection

##  Project Overview
This lab demonstrates the fundamental differences between connection-oriented (TCP) and connectionless (UDP) transport layer protocols using Wireshark packet captures. The objective is to analyze the mechanics of the TCP three-way handshake and inspect the structure of a standard UDP Domain Name System (DNS) query.

##  Tools Used
* **Wireshark**: Protocol analyzer used to capture, filter, and inspect network packets.
* **Network PCAP Files**: Custom network traffic captures containing TCP and UDP streams.

---

##  Task 1: TCP Three-Way Handshake Analysis
Using the `TCP-handshake-capture.pcap` file, the lifecycle of a connection-oriented session was analyzed. 

###  Observations & Analysis
The TCP three-way handshake establishes a reliable connection between a client (`192.168.0.4`) and a server (`192.168.0.1`).

| Step / Packet # | Source IP | Destination IP | Flags Set | Purpose |
| :--- | :--- | :--- | :--- | :--- |
| **Packet 8** | `192.168.0.4` | `192.168.0.1` | `SYN` (Synchronize) | Client requests to initiate a connection. |
| **Packet 9** | `192.168.0.1` | `192.168.0.4` | `SYN, ACK` | Server acknowledges request and synchronizes back. |
| **Packet 10** | `192.168.0.4` | `192.168.0.1` | `ACK` (Acknowledge) | Client confirms connection establishment. |

###  Verification
<img width="942" height="188" alt="Screenshot 2026-07-13 194941" src="https://github.com/user-attachments/assets/08af487a-9e6a-4378-8490-d60fae649d7f" />
<img width="1122" height="45" alt="Screenshot 2026-07-13 195437" src="https://github.com/user-attachments/assets/9529be08-4b22-42d4-a80b-ec36abe7eb9a" />


---

##  Task 2: UDP & DNS Protocol Inspection
Using the `dns-moviefone.pcap` file, a connectionless application-layer lookup was evaluated.

###  Observations & Analysis
Unlike TCP, User Datagram Protocol (UDP) does not use a handshake sequence. 
* **Packet Analyzed**: Packet 1 (DNS Standard Query for `www.moviefone.com`)
* **Source Port**: `2463`
* **Port Type**: **Ephemeral (Dynamic) Port**. Though officially registered to LSI RAID Management, the host operating system dynamically allocated this high port out of its temporary pool as a transient return address for the outbound web request.
* **Destination Port**: `53`
* **Application Protocol**: **DNS (Domain Name System)**

###  Verification

<img width="947" height="235" alt="Screenshot 2026-07-13 195214" src="https://github.com/user-attachments/assets/26b8557e-dea3-47d5-9d18-6f5e70d31eb2" />


---

##  Key Takeaways
1. **Reliability vs. Speed**: TCP ensures reliable data delivery via sequence tracking and explicit acknowledgments, making it ideal for web traffic (HTTP). UDP trading reliability for speed allows protocols like DNS to execute fast, low-overhead queries.
2. **Ephemeral Port Allocation**: Operating systems frequently leverage registered service ports (like `2463`) as source ports if they fall into the dynamic allocation range and are not actively locked by a local listening service.
