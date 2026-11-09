# 🔍 LAB 8: VoIP Call Replay

### Scenario
A phone call was made through the company's VoIP system. Analyze the captured network traffic to identify the call details and replay the audio to understand what was discussed.

## 📝 Investigation and Findings

**Q1. Which protocol set up the call?**

I used the `sip` filter to examine the call signaling messages. In the Info column, I found the `INVITE`, `180 Ringing`, and `200 OK` messages, which confirmed that SIP (Session Initiation Protocol) was used to establish the call.

<img width="982" alt="SIP call signaling" src="https://github.com/user-attachments/assets/0c53efa7-bb97-4a2f-9bb2-c3bbd5dd9eba" />

---

**Q2. Which protocol carried the voice data?**

I used the `rtp` filter to identify the packets carrying the voice data. I observed multiple small packets transmitted approximately every 60 ms. Out of the 1,051 captured packets, 804 were RTP packets.

<img width="982" alt="RTP packets" src="https://github.com/user-attachments/assets/86be5631-2753-47b9-92bd-9c0d87c9b7a0" />

---

**Q3. Which extension number made the call?**

I used the `sip.Method == "INVITE"` filter to locate the call initiation packet. After checking the packet details, I found that the caller used extension `101`.

---

**Q4. What is the caller's name?**

From the SIP INVITE packet details, I identified the caller's name as **Ali Raza**.

<img width="982" alt="SIP INVITE details" src="https://github.com/user-attachments/assets/bebfd0ad-e395-438c-947a-f1bc76e6a38d" />

---

**Q5. Which codec carried the voice?**

I opened one of the RTP packets and checked its payload type in the packet details. I found that the voice was carried using the G.711 PCMU codec (Payload Type 0).

---

**Q6. Who answered the call?**

I replayed the captured RTP audio and listened to the beginning of the conversation. Based on what I heard, the person who answered the call was **Sara**, using extension `202`.

<img width="982" alt="VoIP audio playback" src="https://github.com/user-attachments/assets/552df6cb-631c-4536-8b3d-fa9609113ba9" />

---

**Q7. What were the last four digits of the card number?**

After listening to the recorded conversation, I identified the last four digits of the card number as `2394`.

---

**Q8. What expiry date did the caller mention?**

From the audio recording, I heard the caller mention the card's expiry date as `09/28`.

---

**Q9. How can the company prevent unauthorized people from listening to calls?**

To prevent unauthorized people from listening to VoIP conversations, the company should implement Secure RTP (SRTP) to encrypt voice traffic and TLS to protect SIP signaling.

## 📌 Summary

| Category | Findings |
|---|---|
| Call Setup | SIP |
| Voice Protocol | RTP |
| Codec | G.711 PCMU |
| Caller | Ali Raza |
| Caller Extension | 101 |
| Receiver | Sara |
| Receiver Extension | 202 |
| Card Number (Last 4 Digits) | 2394 |
| Expiry Date | 09/28 |

## 🛠️ Wireshark Filters Used

| Display Filter | Purpose |
|---|---|
| `sip` | To identify SIP call signaling messages. |
| `rtp` | To identify packets carrying voice data. |
| `sip.Method == "INVITE"` | To locate the packet that initiated the call. |

## 💡 Key Takeaways

Through this activity, I learned how SIP establishes VoIP calls and how RTP carries voice data across a network. I also practiced examining SIP INVITE messages, identifying call participants, and replaying captured audio using Wireshark.

**Lesson Learned:** I learned that unencrypted VoIP traffic can expose private conversations and sensitive information. Implementing SRTP and TLS is important to protect voice communications from unauthorized access.
