---
title: "Wireshark Lab: ICMP v8.0"
description: "Computer Networking: A Top-Down Approach"
date: 2024-07-30 13:00:40 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. What is the IP address of your host? What is the IP address of the destination host?
   - `Source Address: 192.168.1.101`
   - `Destination Address: 143.89.14.34`

2. Why is it that an ICMP packet does not have source and destination port numbers?
   - ICMP는 IP 위에서 동작하는 **네트워크 계층 프로토콜**이다. 전송 계층 프로토콜이 아니므로, 포트 개념이 존재하지 않는다.

3. Examine one of the ping request packets sent by your host. What are the ICMP type and code numbers? What other fields does this ICMP packet have? How many bytes are the checksum, sequence number and identifier fields?
   - `Type: 8 (Echo (ping) request)`
   - `Code: 0`
   - `Checksum`, `Identifier`, `Sequence Number` (각각 2 bytes)

   ![](/posts/20240730/request.png){: .border }
   _**Figure 1.** Echo request packet_

   <br>

4. Examine the corresponding ping reply packet. What are the ICMP type and code numbers? What other fields does this ICMP packet have? How many bytes are the checksum, sequence number and identifier fields?
   - `Type: 0 (Echo (ping) reply)`
   - `Code: 0`
   - `Checksum`, `Identifier`, `Sequence Number` (각각 2 bytes)

   ![](/posts/20240730/reply.png){: .border }
   _**Figure 2.** Echo reply packet_

   <br>

5. What is the IP address of your host? What is the IP address of the target destination host?
   - `Source Address: 192.168.1.101`
   - `Destination Address: 138.96.146.2`

6. If ICMP sent UDP packets instead (as in Unix/Linux), would the IP protocol number still be 01 for the probe packets? If not, what would it be?
   - `Protocol: UDP (17)`

7. Examine the ICMP echo packet in your screenshot. Is this different from the ICMP ping query packets in the first half of this lab? If yes, how so?
   - 패킷 자체는 특별히 다른 점이 없지만, Traceroute의 패킷들은 점진적으로 TTL 값이 증가한다.

   ![](/posts/20240730/request-tracert.png){: .border }
   _**Figure 3.** Echo request packet (Traceroute)_

   <br>

8. Examine the ICMP error packet in your screenshot. It has more fields than the ICMP echo packet. What is included in those fields?
   - `Unused: 00000000`
   - Request 패킷의 IP 헤더, ICMP 메시지의 첫 8바이트

   ![](/posts/20240730/error-tracert.png){: .border }
   _**Figure 4.** Error packet (Traceroute)_

   <br>

9. Examine the last three ICMP packets received by the source host. How are these packets different from the ICMP error packets? Why are they different?
   - Request 패킷이 목적지에 도달했기 때문에, error가 아니라 reply 패킷이 반환된다.

   ![](/posts/20240730/reply-tracert.png){: .border }
   _**Figure 5.** Echo reply packet (Traceroute)_

   <br>

10. Within the tracert measurements, is there a link whose delay is significantly longer than others? On the basis of the router names, can you guess the location of the two routers on the end of this link?
    - 라우터 #9에서 #10으로 넘어갈 때 RTT가 크게 증가한다.
    - 두 라우터 사이의 링크는 New York City(미국)에서 Pastourelle(프랑스)로 연결되는 대륙 간 링크이다.

    ![](/posts/20240730/tracert.png)
    _**Figure 6.** `tracert` measurements_

    <br>

## References

---

- [J. F. Kurose and K. W. Ross. _Wireshark Lab: ICMP v8.0_. (2020). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_ICMP_v8.0.doc)
