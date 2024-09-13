---
title: "Wireshark Lab: NAT v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-07-27 09:16:22 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. What is the IP address of the client that sends the HTTP GET request in the _nat-inside-wireshark-trace1-1.pcapng_ trace? What is the source port number of the TCP segment in this datagram containing the HTTP GET request? What is the destination IP address of this HTTP GET request? What is the destination port number of the TCP segment in this datagram containing the HTTP GET request?
   - `Source Address: 192.168.10.11` (클라이언트)
   - `Source Port: 53924`
   - `Destination Address: 138.76.29.8` (웹 서버)
   - `Destination Port: 80`

2. At what time is the corresponding HTTP 200 OK message from the webserver forwarded by the NAT router to the client on the router's LAN side?
   - 0.030672101 s

3. What are the source and destination IP addresses and TCP source and destination ports on the IP datagram carrying this HTTP 200 OK message?
   - `Source Address: 138.76.29.8` (웹 서버)
   - `Source Port: 80`
   - `Destination Address: 192.168.10.11` (클라이언트)
   - `Destination Port: 53924`

   ![](/posts/20240727/lan-side.png){: .border }
   _**Figure 1.** LAN side of the NAT router_

   <br>

4. At what time does this HTTP GET message appear in the _nat-outside-wireshark-trace1-1.pcapng_ trace file?
   - 0.027356291 s

5. What are the source and destination IP addresses and TCP source and destination port numbers on the IP datagram carrying this HTTP GET (as recorded in the _nat-outside-wireshark-trace1-1.pcapng_ trace file)?
   - `Source Address: 10.0.1.254` (NAT 라우터)
   - `Source Port: 53924`
   - `Destination Address: 138.76.29.8` (웹 서버)
   - `Destination Port: 80`

6. Which of these four fields are different than in your answer to question 1 above?
   - `Source Address`

7. Are any fields in the HTTP GET message changed?
   - 그대로임

8. Which of the following fields in the IP datagram carrying the HTTP GET are changed from the datagram received on the local area network (inside) to the corresponding datagram forwarded on the Internet side (outside) of the NAT router: Version, Header Length, Flags, Checksum?
   - `Header Checksum`

9. At what time does this message appear in the _nat-outside-wireshark-trace1-1.pcapng_ trace file?
   - 0.030625966 s

10. What are the source and destination IP addresses and TCP source and destination port numbers on the IP datagram carrying this HTTP reply ("200 OK") message (as recorded in the _nat-outside-wireshark-trace1-1.pcapng_ trace file)?
    - `Source Address: 138.76.29.8` (웹 서버)
    - `Source Port: 80`
    - `Destination Address: 10.0.1.254` (NAT 라우터)
    - `Destination Port: 53924`

    ![](/posts/20240727/internet-side.png){: .border }
    _**Figure 2.** Internet side of the NAT router_

    <br>

11. What are the source and destination IP addresses and TCP source and destination port numbers on the IP datagram carrying the HTTP reply ("200 OK") that is forwarded from the router to the destination host?
    - `Source Address: 138.76.29.8` (웹 서버)
    - `Source Port: 80`
    - `Destination Address: 192.168.10.11` (클라이언트)
    - `Destination Port: 53924`

    <br>

## References

---

- [J. F. Kurose and K. W. Ross. _Wireshark Lab: NAT v8.1_. (2021). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_NAT_v8.1.doc)
