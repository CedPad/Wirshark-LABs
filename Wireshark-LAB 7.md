# 🔍 LAB 7: Brute Force Catch

## 📌 Scenario

The owner of an FTP server reports multiple failed login attempts within a short period. As a SOC analyst, your task is to investigate the captured network traffic using Wireshark, identify the source of the login attempts, and determine whether the attacker successfully accessed the FTP server.

---
<img width="975" height="474" alt="image" src="https://github.com/user-attachments/assets/e3296adc-74fb-41b5-8e26-39311f0b495e" />

---

## 📝 Investigation and Findings

**Q1. The server owner sees many failed sign-ins. Which protocol are they on? Check Statistics → Protocol Hierarchy.**

**Answer: FTP (File Transfer Protocol)**

I opened **Statistics → Protocol Hierarchy** in Wireshark and examined the protocols under TCP. I found that FTP accounted for **117 packets** in the capture.

FTP uses `USER` and `PASS` commands to authenticate users when connecting to an FTP server.

---

**Q2. Which IP address is the FTP server that was attacked?**

**Answer: `192.168.10.20`**

By examining the source and destination IP addresses of the FTP communication, I identified `192.168.10.20` as the targeted FTP server.

<img width="1364" height="764" alt="FTP Server Identification" src="https://github.com/user-attachments/assets/1cb2e433-be18-4ded-865e-3b33278e3744" />

---

**Q3. Which IP address sent all the login attempts?**

**Answer: `192.168.10.66`**

I applied the `ftp.request` display filter to isolate FTP requests. By checking the source IP addresses, I identified `192.168.10.66` as the computer responsible for sending the login attempts.

<img width="1361" height="756" alt="FTP Requests" src="https://github.com/user-attachments/assets/407e51c5-e64a-4496-a524-4b0db757cc65" />

---

**Q4. Which username did the attacker try again and again?**

**Answer: `admin`**

I used the following filter to display FTP username commands:

```wireshark
ftp.request.command == "USER"
```

The captured packets showed repeated login attempts using the username `admin`.

<img width="1362" height="755" alt="FTP Username Attempts" src="https://github.com/user-attachments/assets/0646d9cd-fcc9-4abf-ac61-213215c94cbf" />

---

**Q5. Which FTP reply code means "Login incorrect"?**

**Answer: `530` – Login incorrect**

By examining the FTP response packets, I found response code `530`, which indicates that the login attempt was unsuccessful.

<img width="1362" height="765" alt="FTP Login Incorrect Response" src="https://github.com/user-attachments/assets/54881309-eb6e-4a7c-bbc3-34b3fee3bf21" />

---

**Q6. How many failed logins did the attacker make?**

**Answer: 24 failed login attempts**

I applied the following display filter to isolate unsuccessful FTP login responses:

```wireshark
ftp.response.code == 530
```

The filtered results showed **24 failed login attempts**.

<img width="1357" height="762" alt="Failed FTP Logins" src="https://github.com/user-attachments/assets/d1394ff9-774a-4aeb-bf66-2e4c6dd08fc3" />

---

**Q7. One login finally worked. Which FTP reply code shows a successful login?**

**Answer: `230` – Login successful**

I examined the FTP response in packet number 284. The server returned response code `230`, confirming that the login was successful.

<img width="1360" height="757" alt="Successful FTP Login" src="https://github.com/user-attachments/assets/fd391973-3620-4de9-bb61-98ab97c952ea" />

---

**Q8. What password did the attacker use in the successful login?**

**Answer: `Sunshine99`**

I used **Follow → TCP Stream** on packet number 284 to inspect the FTP communication. The successful login credentials were visible because FTP transmitted them in plain text.

<img width="1351" height="767" alt="FTP Credentials in TCP Stream" src="https://github.com/user-attachments/assets/0ce8c543-b811-409f-a99d-c193ac23df18" />

---

**Q9. Many password guesses for one user, followed by one successful login. What type of attack is this?**

**Answer: Brute Force Attack**

The repeated attempts to guess the password for the same username, followed by a successful login, indicate a **Brute Force Attack**.

---

## 🛠️ Wireshark Filters Used

| Display Filter | Purpose |
|---|---|
| `ftp` | Displays FTP traffic. |
| `ftp.request` | Identifies FTP requests sent by the client. |
| `ftp.request.command == "USER"` | Displays FTP username commands. |
| `ftp.response.code == 530` | Identifies failed FTP login responses. |
| `ftp.response.code == 230` | Identifies successful FTP login responses. |
| `ip.addr == 192.168.10.66` | Filters traffic involving the suspected attacker's IP address. |
| `tcp.port == 21` | Displays TCP traffic using the standard FTP control port. |

## 📊 Investigation Summary

| Finding | Result |
|---|---|
| Attack Type | Brute Force Attack |
| Protocol | FTP |
| FTP Server | `192.168.10.20` |
| Attacker IP | `192.168.10.66` |
| Target Username | `admin` |
| Failed Login Attempts | 24 |
| Failed Login Code | `530` |
| Successful Login Code | `230` |
| Successful Password | `Sunshine99` |
| FTP Port | `21` |

**Lessons I learned:**
- Enforce strong and unique passwords.
- Replace plain-text FTP with secure alternatives such as **SFTP or FTPS**.
