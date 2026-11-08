# 🔍 LAB 6: HTTP Plain-Text Credentials

## 📌 Scenario

An old staff portal still uses **HTTP**, which does not encrypt the data transmitted between a user's browser and the web server.

As a SOC analyst, your task is to capture and analyze the network traffic using Wireshark, investigate a staff member's login activity, and determine what sensitive information can be exposed when credentials are transmitted over an unencrypted connection.

---
<img width="975" height="470" alt="Initial Packet Capture" src="https://github.com/user-attachments/assets/d4797dbc-ba22-4bb6-8ce8-c35119408438" />

---

## 📝 Investigation and Findings

**Q1. A staff member signed in to the old staff portal. Which protocol carried the sign-in?**

To identify the login activity, I used the following display filter to inspect HTTP request methods:

```wireshark
http.request.method
```

By examining the captured packets, I identified the login communication as using **HTTP**.

<img width="1361" height="760" alt="HTTP Request Analysis" src="https://github.com/user-attachments/assets/5e9aa6f7-b1e2-40dc-8e3d-65d576689ed8" />

**Answer:** HTTP (Hypertext Transfer Protocol).

**Q2. A web form sends the typed values to the server. Which HTTP method did the sign-in use?**

**Answer:** `POST`

The POST method was used to submit the login credentials from the staff member's browser to the web server.

**Q3. Which page path did the form send the sign-in to?**

**Answer:** `/login`

The packet analysis showed that the login form was submitted to the `/login` endpoint in packet number 72.

**Q4. Which PC sent the sign-in?**

**Answer:** Staff PC – `192.168.10.45`

By examining the source IP address of the HTTP POST request, I identified `192.168.10.45` as the computer that submitted the login credentials.

**Q5. How many times did the user send the sign-in form?**

**Answer:** 2 POST requests.

Based on the captured traffic, the staff member submitted the login form twice. The first attempt failed, while the second attempt was successful.

**Q6. Click one POST packet. In the packet details, open HTML Form URL Encoded. What is the username?**

**Answer:** `farhan.k`

By expanding **HTML Form URL Encoded** in the packet details, I was able to inspect the submitted form fields and identify the username.

<img width="1349" height="761" alt="HTML Form URL Encoded" src="https://github.com/user-attachments/assets/70f67c43-5c61-4def-a6f4-3f1e19dbcb4e" />

**Q7. The second sign-in worked (the page said "Welcome back"). What password did the user type that time?**

**Answer:** `Mango#2026`

The second POST request revealed the password submitted by the staff member. Since the portal used unencrypted HTTP, the credentials could be read directly from the captured network traffic.

<img width="1354" height="165" alt="Successful Login Response" src="https://github.com/user-attachments/assets/a20e82a3-76e5-48e2-8eab-033595e2a1d2" />

**Q8. Which line in the packet details shows the typed username and password in an easy list?**

**Answer:** `HTML Form URL Encoded`

This section displays the submitted form fields, including the username and password, in a readable format.

**Q9. How can the company stop people from reading passwords like this?**

**Answer:** The company should implement **HTTPS** instead of HTTP by using TLS encryption. This protects passwords and other sensitive information from being easily intercepted and read while being transmitted across the network.

---

## 🛠️ Wireshark Filters Used

| Display Filter | Purpose |
|---|---|
| `http` | Displays HTTP traffic captured in the network. |
| `http.request.method` | Identifies HTTP requests and their methods, such as GET and POST. |
| `http.request.method == "POST"` | Isolates POST requests, including submitted login forms. |
| `ip.addr == 192.168.10.45` | Displays packets involving the staff PC. |

---

## 📊 Investigation Summary

| Finding | Result |
|---|---|
| Protocol | HTTP (Unencrypted) |
| HTTP Method | POST |
| Login Endpoint | `/login` |
| Staff PC | `192.168.10.45` |
| Login Attempts | 2 |
| Username | `farhan.k` |
| Exposed Password | `Mango#2026` |
| Form Data Location | HTML Form URL Encoded |
| Security Issue | Credentials transmitted in plain text |
| Recommended Solution | Implement HTTPS with TLS encryption |
