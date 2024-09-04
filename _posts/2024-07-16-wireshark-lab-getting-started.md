---
title: "Wireshark Lab: Getting Started v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-07-16 12:25:15 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
math: true
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. Which of the following protocols are shown as appearing (i.e., are listed in the Wireshark "protocol" column) in your trace file: TCP, QUIC, HTTP, DNS, UDP, TLSv1.2?
   - TCP
   - HTTP
   - TLSv1.2

   ![](/posts/20240716/protocol.png){: .border }
   _**Figure 1.** "Protocol" column_

   <br>

2. How long did it take from when the HTTP GET message was sent until the HTTP OK reply was received? (By default, the value of the Time column in the packet-listing window is the amount of time, in seconds, since Wireshark tracing began. (If you want to display the Time field in time-of-day format, select the Wireshark _View_ pull down menu, then select _Time Display Format_, then select _Time of Day_.)
   - $8.501613 - 8.472728 = 0.028885 \ \mathrm{s}$

   ![](/posts/20240716/time.png){: .border }
   _**Figure 2.** "Time" column_

   <br>

3. What is the Internet address of the gaia.cs.umass.edu (also known as www-net.cs.umass.edu)? What is the Internet address of your computer or (if you are using the trace file) the computer that sent the HTTP GET message?
   - `128.119.245.12`
   - `10.0.0.44`

   ![](/posts/20240716/ip-addresses.png){: .border }
   _**Figure 3.** IP addresses_

   <br>

4. Expand the information on the HTTP message in the Wireshark "Details of selected packet" window (see Figure 3 above) so you can see the fields in the HTTP GET request message. What type of Web browser issued the HTTP request? The answer is shown at the right end of the information following the "User-Agent:" field in the expanded HTTP message display. [This field value in the HTTP message is how a web server learns what type of browser you are using.]
   - Firefox

   ![](/posts/20240716/user-agent.png){: .border }
   _**Figure 4.** `User-Agent` header_

   <br>

5. Expand the information on the Transmission Control Protocol for this packet in the Wireshark "Details of selected packet" window (see Figure 3 in the lab writeup) so you can see the fields in the TCP segment carrying the HTTP message. What is the destination port number (the number following "Dest Port:" for the TCP segment containing the HTTP request) to which this HTTP request is being sent?
   - `Destination Port: 80`

   ![](/posts/20240716/dst-port.png){: .border }
   _**Figure 5.** Destination port_

   <br>

6. Print the two HTTP messages (GET and OK) referred to in question 2 above. To do so, select _Print_ from the Wireshark _File_ command menu, and select the _"Selected packets only"_ and _"As displayed"_ radial buttons, and then click OK.

   ![](/posts/20240716/http-messages.png){: .border}
   _**Figure 6.** HTTP messages_

   <br>

## References

---

- [J. F. Kurose and K. W. Ross. _Wireshark Lab: Getting Started v8.1_. (2023). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_Intro_v8.1.docx)
