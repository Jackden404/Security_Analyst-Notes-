This final review consolidates your understanding of command-line packet sniffing, Wireshark display filters, TCP handshake states, command-and-control connection metrics, and Zeek structured logging.

---
## 🧰 Packet Capturing & Hardware Interfaces

Before network analysis can occur, packet frames must be mirrored or tapped.

### Command-Line Capture

- The primary command-line tool commonly used for packet capture on Linux systems is: **tcpdump**.
### Hardware TAPs vs. Software Mirrors

[!IMPORTANT] **TAP Advantages:** The primary advantage of a network TAP over a switch SPAN port is: **TAPs are passive and do not drop packets under load**. TAPs split the physical cable signal directly at the hardware layer, ensuring that no packet frames are dropped during network congestion.

---
## 🔍 Wireshark Filters & Handshake Mechanics

### 1. HTTP Display Filters

To isolate specific request frames without displaying response blocks:

- The Wireshark display filter that shows HTTP requests only is: **`http.request`**.
### 2. Connection Handshakes

The baseline TCP protocol establishes sessions using a 3-way handshake.

[!NOTE] **TCP Handshake Sequence:** The correct, sequential TCP three-way handshake sequence is: **SYN, SYN-ACK, ACK**.

---
## 📡 Detecting Command-and-Control (C2) Activity

Threat actors use automated scripts to schedule C2 client check-ins.

### Beaconing Telemetry

[!WARNING] **C2 Beacon Signal:** When analyzing network flow records, regular, periodic network traffic at consistent intervals indicates: **Possible C2 beaconing**. While user-driven traffic is bursty, malware beacon timing is highly predictable.

### DNS Tunneling Detections

Attackers bypass firewalls by embedding payloads inside DNS query subdomains.

[!TIP] **Zeek Log Auditing:** The specific Zeek log file best suited for detecting DNS tunneling activity is: **`dns.log`**.

## Mission Verification

1 .What tool is commonly used for command-line packet capture on Linux?
Wireshark

2 .Which Wireshark display filter shows HTTP requests only?
http.request

3 .What is the correct TCP three-way handshake sequence?
SYN, SYN-ACK, ACK

4 .What does regular, periodic network traffic at consistent intervals indicate?
Possible C2 beaconing

5 .Which Zeek log is best for detecting DNS tunneling?
dns.log

6 .What is the primary advantage of a network TAP over a SPAN port?
TAPs are passive and do not drop packets under load
