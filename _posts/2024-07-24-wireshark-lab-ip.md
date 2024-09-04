---
title: "Wireshark Lab: IP v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-07-24 12:37:15 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. Select the first UDP segment sent by your computer via the `traceroute` command to gaia.cs.umass.edu. (Hint: this is 44th packet in the trace file in the _ip-wireshark-trace1-1.pcapng_ file in footnote 2). Expand the Internet Protocol part of the packet in the packet details window. What is the IP address of your computer?
   - `Source Address: 192.168.86.61`

2. What is the value in the time-to-live (TTL) field in this IPv4 datagram's header?
   - `Time to Live: 1`

3. What is the value in the upper layer protocol field in this IPv4 datagram's header? [Note: the answers for Linux/MacOS differ from Windows here].
   - `Protocol: UDP (17)`

4. How many bytes are in the IP header?
   - `Header Length: 20 bytes`

5. How many bytes are in the payload of the IP datagram? Explain how you determined the number of payload bytes.
   - `Total Length: 56` - `Header Length: 20` = 36 bytes

6. Has this IP datagram been fragmented? Explain how you determined whether or not the datagram has been fragmented.
   - 단편화 X
     - `More fragments: Not set`
     - `Fragment Offset: 0`

   ![](/posts/20240724/ipv4-datagram.png){: .border }
   _**Figure 1.** IPv4 datagram_

   <br>

7. Which fields in the IP datagram _always_ change from one datagram to the next within this series of UDP segments sent by your computer destined to 128.119.245.12, via `traceroute`? Why?
   - `Identification`, `Header Checksum`
   - 데이터그램을 식별하기 위함
     - 데이터그램이 단편화되는 경우, 각 fragment는 동일한 `Identification` 값을 가진다. 이들은 목적지에 도달한 뒤 재조합되어 원래의 데이터그램으로 복원된다.

8. Which fields in this sequence of IP datagrams (containing UDP segments) stay constant? Why?
   - `Identification`, `Time to Live`, `Header Checksum`을 제외한 나머지 필드

9. Describe the pattern you see in the values in the Identification field of the IP datagrams being sent by your computer.
   - 순차적으로 1씩 증가

   ![](/posts/20240724/udp-sequence.png){: .border }
   _**Figure 2.** Sequence of UDP segments_

   <br>

10. What is the upper layer protocol specified in the IP datagrams returned from the routers? [Note: the answers for Linux/MacOS differ from Windows here].
    - `Protocol: ICMP (1)`

11. Are the values in the Identification fields (across the sequence of all of ICMP packets from all of the routers) similar in behavior to your answer to question 9 above?
    - 따로 패턴이 존재하지 않음

12. Are the values of the TTL fields similar, across all of ICMP packets from all of the routers?
    - 반환하는 라우터에 따라 `TTL` 값이 다름

    ![](/posts/20240724/ttl-exceeded.png){: .border }
    _**Figure 3.** ICMP packets returned by routers_

    <br>

13. Find the first IP datagram containing the first part of the segment sent to 128.119.245.12 sent by your computer via the `traceroute` command to gaia.cs.umass.edu, _after_ you specified that the `traceroute` packet length should be 3000. (Hint: This is packet 179 in the _ip-wireshark-trace1-1.pcapng_ trace file in footnote 2. Packets 179, 180, and 181 are three IP datagrams created by fragmenting the first single 3000-byte UDP segment sent to 128.119.145.12). Has that segment been fragmented across more than one IP datagram? (Hint: the answer is yes!)
    - 단편화 O

14. What information in the IP header indicates that this datagram been fragmented?
    - `Flags: 0x1, More fragments`

15. What information in the IP header for this packet indicates whether this is the first fragment versus a latter fragment?
    - `Fragment Offset: 0`

16. How many bytes are there in this IP datagram (header plus payload)?
    - `Total Length: 1500`

    ![](/posts/20240724/fragment-1.png){: .border }
    _**Figure 4.** First fragment of IP datagram_

    <br>

17. Now inspect the datagram containing the second fragment of the fragmented UDP segment. What information in the IP header indicates that this is not the first datagram fragment?
    - `Fragment Offset: 1480`

18. What fields change in the IP header between the first and second fragment?
- `Fragment Offset`, `Header Checksum`

    ![](/posts/20240724/fragment-2.png){: .border }
    _**Figure 5.** Second fragment of IP datagram_

    <br>

19. Now find the IP datagram containing the third fragment of the original UDP segment. What information in the IP header indicates that this is the last fragment of that segment?
    - `More fragments: Not set`

    ![](/posts/20240724/fragment-3.png){: .border }
    _**Figure 6.** Last fragment of IP datagram_

    <br>

20. What is the IPv6 address of the computer making the DNS AAAA request? This is the source address of the 20th packet in the trace. Give the IPv6 source address for this datagram in the exact same form as displayed in the Wireshark window.
    - `Source Address: 2601:193:8302:4620:215c:f5ae:8b40:a27a`

21. What is the IPv6 destination address for this datagram? Give this IPv6 address in the exact same form as displayed in the Wireshark window.
    - `Destination Address: 2001:558:feed::1`

22. What is the value of the flow label for this datagram?
    - `Flow Label: 0x63ed0`

23. How much payload data is carried in this datagram?
    - `Payload Length: 37`

24. What is the upper layer protocol to which this datagram's payload will be delivered at the destination?
    - `Next Header: UDP (17)`

    ![](/posts/20240724/aaaa-request.png){: .border }
    _**Figure 7.** DNS AAAA request_

    <br>

25. How many IPv6 addresses are returned in the response to this AAAA request?
    - `Answer RRs: 1`

26. What is the first of the IPv6 addresses returned by the DNS for youtube.com (in the _ip-wireshark-trace2-1.pcapng_ trace file, this is also the address that is numerically the smallest)? Give this IPv6 address in the exact same shorthand form as displayed in the Wireshark window.
    - `AAAA Address: 2607:f8b0:4006:815::200e`

    ![](/posts/20240724/aaaa-response.png){: .border }
    _**Figure 8.** DNS AAAA response_

    <br>

## References

---

- [J. F. Kurose and K. W. Ross. _Wireshark Lab: IP v8.1_. (2021). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_IP_v8.1.doc)
