# 🔍 LAB 8: VoIP call replay

Scenario. A phone call was made through the company VoIP system. Capture the traffic, find the call and play it back to learn what was said.

<img width="982" height="485" alt="image" src="https://github.com/user-attachments/assets/4dedd825-15e5-49a5-ba71-92b170287e1a" />

Q1. A phone call was made on the network. Which protocol set up the call (INVITE, Ringing, OK)?

I filtered base on the choices available **(SIP, RTP, DNS, FTP)**. Using the `SIP` filter it shows in the info column the **INVITE, Ringing, OK**

<img width="1350" height="759" alt="image" src="https://github.com/user-attachments/assets/0c53efa7-bb97-4a2f-9bb2-c3bbd5dd9eba" />

Q2. Which protocol carried the voice itself (the many small packets, one every 60 ms)?

I filtered using the `RTP` and shows a 60 ms difference in each number over time and a total of 804 over 1051 packets captured.

<img width="1363" height="765" alt="image" src="https://github.com/user-attachments/assets/86be5631-2753-47b9-92bd-9c0d87c9b7a0" />

Q3. Which extension number made the call? Look at the SIP INVITE.

By using the `sip.Method == "INVITE"` and viewed the packet detail the extension number used from the call is `From: "Ali Raza" <sip101@192.168.10.45>`


