---
title: "Wireshark Lab: UDP v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-07-23 13:11:56 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
math: true
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. Select the first UDP segment in your trace. What is the packet number of this segment in the trace file? What type of application-layer payload or protocol message is being carried in this UDP segment? Look at the details of this packet in Wireshark. How many fields there are in the UDP header? (You shouldn't look in the textbook! Answer these questions directly from what you observe in the packet trace.) What are the names of these fields?
   - #5
   - SSDP
   - 4개 (`Source port`, `Destination port`, `Length`, `Checksum`)

2. By consulting the displayed information in Wireshark's packet content field for this packet (or by consulting the textbook), what is the length (in bytes) of each of the UDP header fields?
   - 2 bytes

3. The value in the Length field is the length of what? (You can consult the text for this answer). Verify your claim with your captured UDP packet.
   - UDP 세그먼트의 길이 (헤더 포함)

4. What is the maximum number of bytes that can be included in a UDP payload? (Hint: the answer to this question can be determined by your answer to 2. above)
   - ${\text{Max value of unsigned 2 bytes} - \text{Length of UDP header} = 2^{16} - 8 = 65527}$

5. What is the largest possible source port number? (Hint: see the hint in 4.)
   - $2^{16} - 1 = 65535$

6. What is the protocol number for UDP? Give your answer in decimal notation. To answer this question, you'll need to look into the Protocol field of the IP datagram containing this UDP segment (see Figure 4.17 in the text, and the discussion of IP header fields).
   - `Protocol: UDP (17)`

   ![](/posts/20240723/udp-packet.png){: .border }
   _**Figure 1.** UDP packet_

   <br>

7. Examine the pair of UDP packets in which your host sends the first UDP packet and the second UDP packet is a reply to this first UDP packet. (Hint: for a second packet to be sent in response to a first packet, the sender of the first packet should be the destination of the second packet). What is the packet number of the first of these two UDP segments in the trace file? What is the value in the source port field in this UDP segment? What is the value in the destination port field in this UDP segment? What is the packet number of the second of these two UDP segments in the trace file? What is the value in the source port field in this second UDP segment? What is the value in the destination port field in this second UDP segment? Describe the relationship between the port numbers in the two packets.
   - #15
     - `Source Port: 58350`
     - `Destination Port: 53`
   - #17
     - `Source Port: 53`
     - `Destination Port: 58350`
   - 두 패킷의 `Source Port`와 `Destination Port`가 서로 뒤바뀌어 있다.

   ![](/posts/20240723/udp-pair.png){: .border }
   _**Figure 2.** Pair of UDP packets_

   <br>

## References

---

- [J. F. Kurose and K. W. Ross. _Wireshark Lab: UDP v8.1_. (2021). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_UDP_v8.1.doc)
