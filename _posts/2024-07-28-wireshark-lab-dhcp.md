---
title: "Wireshark Lab: DHCP v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-07-28 13:26:06 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. Is this DHCP Discover message sent out using UDP or TCP as the underlying transport protocol?
   - UDP

2. What is the source IP address used in the IP datagram containing the Discover message? Is there anything special about this address? Explain.
   - `Source Address: 0.0.0.0`
   - 클라이언트가 아직 **IP 주소를 할당받지 않았음**을 의미한다.

3. What is the destination IP address used in the datagram containing the Discover message. Is there anything special about this address? Explain.
   - `Destination Address: 255.255.255.255`
   - **브로드캐스트 주소**: 해당 서브넷의 모든 호스트(DHCP 서버)에게 전송한다.

4. What is the value in the transaction ID field of this DHCP Discover message?
   - `Transaction ID: 0x56f415ed`

5. Now inspect the options field in the DHCP Discover message. What are five pieces of information (beyond an IP address) that the client is suggesting or requesting to receive from the DHCP server as part of this DHCP transaction?
   - `Parameter Request List`
   - `Maximum DHCP Message Size`
   - `Client identifier`
   - `IP Address Lease Time`
   - `Host Name`

   ![](/posts/20240728/discover.png){: .border }
   _**Figure 1.** DHCP Discover message_

   <br>

6. How do you know that this Offer message is being sent in response to the DHCP Discover message you studied in questions 1-5 above?
   - `Transaction ID`가 동일함

7. What is the _source_ IP address used in the IP datagram containing the Offer message? Is there anything special about this address? Explain.
   - `Source Address: 192.168.86.1` (DHCP 서버의 IP 주소)

8. What is the _destination_ IP address used in the datagram containing the Offer message? Is there anything special about this address? Explain. [Hint: Look at your trace carefully. The answer to this question may differ from what you see in Figure 4.24 in the textbook. If you really want to dig into this, consult the [DHCP RFC](https://www.rfc-editor.org/rfc/rfc2131.html), page 24.]
   - `Destination Address: 192.168.86.65` (클라이언트에게 제안되는 IP 주소)

9. Now inspect the options field in the DHCP Offer message. What are five pieces of information that the DHCP server is providing to the DHCP client in the DHCP Offer message?
   - `Subnet Mask`
   - `Broadcast Address`
   - `Router`
   - `Domain Name`
   - `Domain Name Server`

   ![](/posts/20240728/offer.png){: .border }
   _**Figure 2.** DHCP Offer message_

   <br>

10. What is the UDP source port number in the IP datagram containing the first DHCP Request message in your trace? What is the UDP destination port number being used?
    - `Source Port: 68`
    - `Destination Port: 67`

11. What is the source IP address in the IP datagram containing this Request message? Is there anything special about this address? Explain.
    - `Source Address: 0.0.0.0`
    - 클라이언트가 아직 **IP 주소를 할당받지 않았음**을 의미한다.

12. What is the destination IP address used in the datagram containing this Request message. Is there anything special about this address? Explain.
    - `Destination Address: 255.255.255.255`
    - **브로드캐스트 주소**: 해당 서브넷의 모든 호스트(DHCP 서버)에게 전송한다.
    
13. What is the value in the transaction ID field of this DHCP Request message? Does it match the transaction IDs of the earlier Discover and Offer messages?
    - `Transaction ID: 0x56f415ed`
    - Discover 및 Offer 메시지의 `Transaction ID`와 같음

14. Now inspect the options field in the DHCP Discover message and take a close look at the "Parameter Request List". The [DHCP RFC](https://www.rfc-editor.org/rfc/rfc2131.html) notes that

    > The client can inform the server which configuration parameters the client is interested in by including the 'parameter request list' option. The data portion of this option explicitly lists the options requested by tag number.

    What differences do you see between the entries in the 'parameter request list' option in this Request message and the same list option in the earlier Discover message?
    - Request와 Discover 메시지의 `Parameter Request List`는 동일함

	![](/posts/20240728/request.png){: .border }
    _**Figure 3.** DHCP Request message_

    <br>

15. What is the source IP address in the IP datagram containing this ACK message? Is there anything special about this address? Explain.
    - `Source Address: 192.168.86.1` (DHCP 서버의 IP 주소)

16. What is the destination IP address used in the datagram containing this ACK message. Is there anything special about this address? Explain.
    - `Destination Address: 192.168.86.65` (클라이언트에게 할당된 IP 주소)

17. What is the name of the field in the DHCP ACK message (as indicated in the Wireshark window) that contains the assigned client IP address?
    - `Your (client) IP address`

18. For how long a time (the so-called "lease time") has the DHPC server assigned this IP address to the client?
    - `IP Address Lease Time: 1 day (86400)`

19. What is the IP address (returned by the DHCP server to the DHCP client in this DHCP ACK message) of the first-hop router on the default path from the client to the rest of the Internet?
    - `Router: 192.168.86.1`

	![](/posts/20240728/ack.png){: .border }
    _**Figure 4.** DHCP ACK message_

## References

---

- [J. F. Kurose and K. W. Ross. _Wireshark Lab: DHCP v8.1_. (2021). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_DHCP_v8.1.doc)
