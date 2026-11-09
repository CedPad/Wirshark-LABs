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

By using the `sip.Method == "INVITE"` and viewed the packet detail the extension number used from the call is **From: "Ali Raza" < sip`101`@192.168.10.45>**

<img width="1352" height="749" alt="image" src="https://github.com/user-attachments/assets/bebfd0ad-e395-438c-947a-f1bc76e6a38d" />

Q4. What is the name of the caller?

The name of the caller is from `Ali Raza`.

Q5. Which codec carried the voice? Click one RTP packet and look at the payload type.

From the packet detail of one RTP packet I checked the payload type is ITU-T G.711 `PCMU` (0).

Q.5 Who answered the call? Play the call and listen to the first words.

<img width="1353" height="762" alt="image" src="https://github.com/user-attachments/assets/552df6cb-631c-4536-8b3d-fa9609113ba9" />

From what I hear during the call was `Sara` answered the call.

Q6. The caller read out a card number to pay an invoice. What are the last 4 digits?

The last 4 didgits are `2394`.

Q7. What expiry date did the caller say?

The expiry date is `09/28`.

Q8. How can the company stop people from listening to calls like this?

Encrypt the call (SRTP).

Summary
Call setup: SIP (INVITE, 180 Ringing, 200 OK, ACK, BYE). Voice: RTP, codec G.711 PCMU
Caller: Ali Raza (extension 101). Answered by: Sara (extension 202)
Spoken in clear: card number ending 2394, expiry 09/28
Fix: encrypt calls with SRTP (and use TLS for SIP)
