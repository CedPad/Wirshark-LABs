# 🔍 LAB 4: Follow TCP Stream

## 📌 Scenario

You are a SOC Analyst investigating network traffic after your manager reports that some employees are using an outdated office chat application that transmits messages in **plain text**.

Your task is to capture and analyze the network traffic using Wireshark, identify the chat communication, and use the **Follow TCP Stream** feature to inspect the messages exchanged between the employee and IT support.

---

## 📝 Investigation and Findings

### Q1. Most of the traffic is web traffic on port 80 or 443. One computer also used a plain-text office chat tool on a different TCP port. Which port is it?

**Answer: TCP Port 5000**

To identify the port used by the office chat application, I navigated to **Statistics → Conversations** in Wireshark and examined the TCP conversations.

After reviewing the destination ports (Port B), I identified three commonly used ports: **80, 443, and 5000**. Since ports 80 and 443 are associated with web traffic, port **5000** was identified as the port used by the plain-text chat application.

![TCP Conversations](https://github.com/user-attachments/assets/620cb062-5c74-45fd-a697-b5aae9b9fbcd)

---

### Q2. The chat uses TCP port 5000. Which IP address started the chat? (The computer that sends the first SYN packet starts the connection.)

**Answer: 192.168.10.45**

I applied the following Wireshark display filter to isolate traffic using TCP port 5000:

`tcp.port == 5000`

I then examined the TCP handshake and identified the source IP address of the first SYN packet. The computer with IP address **192.168.10.45** initiated the connection.

![TCP Port 5000 Filter](https://github.com/user-attachments/assets/9ee740c1-0bef-4102-b6af-4b89856759e7)

---

### Q3. Follow the chat using Follow → TCP Stream. What is the stream number?

**Answer: TCP Stream 5**

I selected one of the packets associated with TCP port 5000, right-clicked it, and selected **Follow → TCP Stream**.

Wireshark opened the conversation and displayed its corresponding stream number in the display filter:

`tcp.stream eq 5`

This confirmed that the chat communication belongs to **TCP Stream 5**.

![Follow TCP Stream](https://github.com/user-attachments/assets/4b3e8d52-c8e9-4eb3-92ac-4d856087e90c)

---

### Q4. How many bytes does the entire conversation contain?

**Answer: 413 bytes**

After opening the Follow TCP Stream window, I checked the information displayed at the bottom of the window. The entire conversation contained **413 bytes** of data.

---

### Q5. What is the first name of the staff member who asks for help?

**Answer: Ali**

---

By examining the messages exchanged in the TCP stream, I identified **Ali** as the staff member requesting assistance from IT support.

### Q6. Which shared drive could the staff member not open?

**Answer: Finance Drive**

The conversation revealed that Ali was experiencing difficulties accessing the **Finance shared drive**.

---

### Q7. What temporary password did IT provide to the staff member?

**Answer: Tiger#4821**

IT support provided Ali with a temporary password.

---

### Q8. What is the guest Wi-Fi password?

**Answer: BlueHarbor77**

Ali also requested the guest Wi-Fi password during the conversation.

---

## 🎯 Key Takeaways

Through this activity, I learned how to:

- Apply Wireshark display filters to isolate specific network traffic.
- Use **Follow TCP Stream** to reconstruct and inspect conversations between communicating devices.
- Use encrypted tools (**TLS/HTTPS**)

---
## 🛠️ Wireshark Filters Used

| Display Filter | Purpose |
|---|---|
| `tcp.port == 5000` | Identifies traffic using TCP port 5000 and isolates the office chat communication. |
| `tcp.stream eq 5` | Filters packets belonging to TCP Stream 5 to examine the complete chat conversation. |
  
## 📊 Summary

- **Chat Communication:** `192.168.10.45` → `192.168.10.20`
- **Protocol / Port:** TCP `5000`
- **TCP Stream:** `5`
- **Sensitive Information Exposed:** A temporary password and guest Wi-Fi password were transmitted in plain text.


