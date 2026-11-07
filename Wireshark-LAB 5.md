# 🔍 LAB 5: Follow TCP Stream – Part 2

## 📝 Scenario

A staff member browsed the company intranet, accessing several web pages and downloading a file. An administrator suspects that one of the pages contains hidden information that should not be publicly visible.

Your task is to analyze the captured network traffic using **Wireshark's Follow TCP Stream** feature, identify the page containing the hidden note, and uncover the information exchanged between the client and server.

---
<img width="970" height="475" alt="Wireshark Packet Capture" src="https://github.com/user-attachments/assets/7cb50c71-e902-4b89-9443-f876fce655e9" />

---

## 🔎 Investigation

**Q1. A staff member opened five pages on the company intranet. An administrator suspects that one of these pages contains a hidden note. Which page is it?**

To identify the page, use the following display filter to isolate HTTP GET requests:

```wireshark
http.request.method == "GET"
```

Then, select each HTTP request and use **Follow → TCP Stream** to inspect the contents of the HTML pages.



<img width="1354" height="762" alt="HTTP GET Requests" src="https://github.com/user-attachments/assets/56d22878-9532-4854-ba67-4067cae2cc1a" />

<img width="1346" height="754" alt="Follow TCP Stream Analysis" src="https://github.com/user-attachments/assets/6f77a540-9d1c-4a3c-abc4-cb6ca94d8f63" />

**Answer:** `/staff.html`

**Q2. The hidden note is not shown to someone who opens the page in a browser. Where is it hidden?**

**Answer:** Inside an HTML comment, making it invisible during normal browser viewing.

**Q3. Which TCP stream number carries the `/staff.html` page?**

Use the following filter to isolate the identified communication:

```wireshark
tcp.stream eq 7
```

**Answer:** TCP stream `7`.

**Q4. The hidden note contains an access code word. What is it?**

**Answer:** `Falcon-9921`

**Q5. The note also identifies a backup server. What is its IP address?**

**Answer:** `192.168.10.77`

**Q6. In the Follow TCP Stream window, the hidden note appears in blue text. Which direction did this text travel?**

**Answer:** Server to client.

**Q7. What HTTP status code did the server return for the `/staff.html` page?**

**Answer:** `200 OK` – The server successfully processed the request.

**Q8. The staff member also downloaded a plain text file from the `/download/` directory. What is the file name?**

**Answer:** `report.txt`

**Q9. After following the TCP stream of the downloaded text file, who prepared the report?**

**Answer:** Nadia.

<img width="1347" height="762" alt="Downloaded Report in TCP Stream" src="https://github.com/user-attachments/assets/99987056-bde6-4c3d-ab7b-7ba6289fef9a" />

---

## 📋 Investigation Summary

| Finding | Result |
|---|---|
| Identified webpage | `/staff.html` |
| TCP stream | `7` |
| HTTP status code | `200 OK` |
| Hidden information location | HTML comment |
| Backup server IP | `192.168.10.77` |
| Access code word | `Falcon-9921` |
| Data direction | Server to client |
| Downloaded file | `report.txt` |
| Report prepared by | Nadia |

## 🛠️ Wireshark Filters Used

| Display Filter | Purpose |
|---|---|
| `http.request.method == "GET"` | Identifies HTTP GET requests. |
| `tcp.stream eq 7` | Isolates the TCP stream containing `/staff.html`. |
