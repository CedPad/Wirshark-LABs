# 🛡️ NetworkWalks – Wireshark Labs

This repository showcases my hands-on activities and learning experiences in network analysis and cybersecurity using **Wireshark**. It documents my progress in identifying suspicious network traffic, analyzing packets, and understanding common network attacks.

---

## 🔍 LAB 3: Identifying the Attacker's IP Address

### 📌 Scenario

You are a SOC analyst. A staff member reports unusual network traffic directed toward the company's web server.

Your task is to capture and analyze the network packets using Wireshark, identify the possible attack, and determine the attacker's IP address.

### 1. 📡 Analyzing Captured Packets

Started by examining the captured network traffic in Wireshark and looking for unusual packet patterns.

![Initial Packet Capture](https://github.com/user-attachments/assets/348dc731-50c8-4285-ba39-7d792346b794)

While browsing the captured packets, I noticed several **red-colored TCP packets**. Upon further observation, most computers generated only one or two of these packets, while one IP address generated a large number of consecutive packets.

![Red Highlighted Packets](https://github.com/user-attachments/assets/ad2e8c6a-a455-4e5e-828c-fd0bcd2a7249)

This unusual pattern prompted me to investigate the source of the traffic and determine what was happening.

### 2. 📝 Investigation and Findings

**Q1. What is the attacker's IP address?**

Using the display filter `tcp.flags.reset == 1`, I isolated the TCP reset packets and examined their source and destination addresses. Based on the packet analysis, I identified `192.168.10.66` as the suspected attacker's IP address.

**Q2. Which IP address is the target of the attacker's packets?**

By examining the source and destination IP addresses, I identified `192.168.10.20` as the target server.

**Q3. Look at the attacker's packets in the Info column. One port number is the same in every packet. What is it?**

The common port number is **52311**.

**Q4. How many different ports did the attacker try on the target?**

Based on the captured packets and the applied display filter, the attacker attempted to access **40 different ports** on the target server.

![Port Scanning Analysis](https://github.com/user-attachments/assets/f594712e-bed7-4330-8f34-37a2ddec46e6)

**Q5. Some of these ports are open on the server. An open port answers with a SYN, ACK packet. Which ports are open?**

I used the following Wireshark display filter to identify SYN-ACK responses:

`tcp.flags.syn == 1 && tcp.flags.ack == 1`

After examining the results, I identified the following open ports:

- Port 80 – HTTP
- Port 443 – HTTPS
- Port 22 – SSH

![Open Ports Identification](https://github.com/user-attachments/assets/b83b1811-4c2d-42f6-9648-1153c3e9ece5)

**Q6. For all the other ports, the server sends a different reply. Which flags does it send back for a closed port?**

The server responds with **RST, ACK (Reset and Acknowledgment)** flags.

This indicates that the connection is being rejected or reset, commonly because the requested port is closed or not accepting connections.

**Q7. What type of attack is this?**

Based on the observed traffic pattern, the activity is identified as a **TCP Port Scanning Attack**.

The suspected attacker attempted to connect to multiple ports on the target server to determine which services were open and available.

---

### 📊 Summary of Findings

Attacker: 192.168.10.66 · Target: web server 192.168.10.20
Ports tried: 40 · Open: 22, 80, 443 · Closed: 37 (reply RST, ACK)
Attack type: Port scan

### 💡 My Key Takeaways

Through this activity, I gained a better understanding of:

- Using Wireshark display filters to isolate and analyze specific packets.
- Identifying suspicious network traffic through packet patterns.
- Understanding TCP flags, particularly SYN, ACK, and RST.
- Examining source and destination IP addresses and port numbers.
- Identifying open and closed ports through TCP responses.
- Recognizing potential port scanning activities during network investigations.

### 🚀 LAB 3 Completed!

Another step forward in my cybersecurity learning journey. Looking forward to exploring more network analysis techniques and hands-on SOC investigation activities!

