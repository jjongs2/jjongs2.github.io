---
title: "Wireshark Lab: TCP v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-07-22 16:11:21 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
math: true
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. What is the IP address and TCP port number used by the client computer (source) that is transferring the alice.txt file to gaia.cs.umass.edu? To answer this question, it's probably easiest to select an HTTP message and explore the details of the TCP packet used to carry this HTTP message, using the "details of the selected packet header window" (refer to Figure 2 in the "Getting Started with Wireshark" Lab if you're uncertain about the Wireshark windows).
   - `192.168.86.68`
   - 55639번 포트

2. What is the IP address of gaia.cs.umass.edu? On what port number is it sending and receiving TCP segments for this connection?
   - `128.119.245.12`
   - 80번 포트

3. What is the _sequence number_ of the TCP SYN segment that is used to initiate the TCP connection between the client computer and gaia.cs.umass.edu? (Note: this is the "raw" sequence number carried in the TCP segment itself; it is _NOT_ the packet # in the "No." column in the Wireshark window. Remember there is no such thing as a "packet number" in TCP or UDP; as you know, there _are_ sequence numbers in TCP and that's what we're after here. Also note that this is not the relative sequence number with respect to the starting sequence number of this TCP session.). What is it in this TCP segment that identifies the segment as a SYN segment? Will the TCP receiver in this session be able to use Selective Acknowledgments (allowing TCP to function a bit more like a "selective repeat" receiver, see section 3.4.4 in the text)?
   - `Sequence Number (raw): 4236649187`
   - `Flags: 0x002 (SYN)`
   - `TCP Option - SACK permitted`

   ![](/posts/20240722/syn.png){: .border }
   _**Figure 1.** SYN segment_

   <br>

4. What is the _sequence number_ of the SYNACK segment sent by gaia.cs.umass.edu to the client computer in reply to the SYN? What is it in the segment that identifies the segment as a SYNACK segment? What is the value of the Acknowledgement field in the SYNACK segment? How did gaia.cs.umass.edu determine that value?
   - `Sequence Number (raw): 1068969752`
   - `Flags: 0x012 (SYN, ACK)`
   - `Acknowledgment number (raw): 4236649188`
     - 클라이언트의 초기 `Sequence Number (raw)` + 1

   ![](/posts/20240722/syn-ack.png){: .border }
   _**Figure 2.** SYN-ACK segment_

   <br>

5. What is the sequence number of the TCP segment containing the header of the HTTP POST command? Note that in order to find the POST message header, you'll need to dig into the packet content field at the bottom of the Wireshark window, _looking for a segment with the ASCII text "POST" within its DATA field_. How many bytes of data are contained in the payload (data) field of this TCP segment? Did all of the data in the transferred file alice.txt fit into this single segment?
   - `Sequence Number (raw): 4236649188`
   - `TCP payload (1448 bytes)`
   - 106개의 세그먼트로 분할됨

6. Consider the TCP segment containing the HTTP "POST" as the first segment in the data transfer part of the TCP connection.
   - At what time was the first segment (the one containing the HTTP POST) in the data-transfer part of the TCP connection sent?
     - $0.024047 \ \mathrm{s}$
   - At what time was the ACK for this first data-containing segment received?
     - $0.052671 \ \mathrm{s}$
   - What is the RTT for this first data-containing segment?
     - $0.052671 - 0.024047 = 0.028624 \ \mathrm{s}$
   - What is the RTT value the second data-carrying TCP segment and its ACK?
     - $0.052676 - 0.024048 = 0.028628 \ \mathrm{s}$
   - What is the `EstimatedRTT` value (see Section 3.5.3 in the text) after the ACK for the second data-carrying segment is received? Assume that in making this calculation after the received of the ACK for the second segment, that the initial value of `EstimatedRTT` is equal to the measured RTT for the first segment, and then is computed using the `EstimatedRTT` equation on page 266, and a value of $\alpha = 0.125$.
     - $0.875 \cdot 0.028624 + 0.125 \cdot 0.028628 = 0.0286245 \ \mathrm{s}$

7. What is the length (header plus payload) of each of the first four data-carrying TCP segments?
   - `Header Length: 32 bytes` + `TCP payload (1448 bytes)` = 1480

8. What is the minimum amount of available buffer space advertised to the client by gaia.cs.umass.edu among these first four data-carrying TCP segments? Does the lack of receiver buffer space ever throttle the sender for these first four data-carrying segments?
   - `Calculated window size: 131712`
   - 상대적으로 큰 window size가 일정하게 유지되고 있다. 이는 **receiver가 수신한 데이터를 저장할 버퍼 공간이 충분함**을 의미하므로, sender를 제한(flow control)하지 않는다.

9. Are there any retransmitted segments in the trace file? What did you check for (in the trace) in order to answer this question?
   - Sequence number가 순차적으로 증가하며 중복되지 않으므로, **모든 세그먼트가 한 번씩만 전송되었음**을 알 수 있다.

10. How much data does the receiver typically acknowledge in an ACK among the first ten data-carrying segments sent from the client to gaia.cs.umass.edu? Can you identify cases where the receiver is ACKing every other received segment (see Table 3.2 in the text) among these first ten data-carrying segments?
    - 1448 bytes
    - 모든 세그먼트가 개별적으로 ACK되고 있음

    ![](/posts/20240722/data-carrying.png){: .border }
    _**Figure 3.** Data-carrying segment_

    <br>

11. What is the throughput (bytes transferred per unit time) for the TCP connection? Explain how you calculated this value.
    - $\mathrm{Throughput = \frac{Bytes}{Duration} = \frac{166 \ kB}{0.1927 \ s} \cdot \frac{8 \ bit}{1 \ byte} \approx 6.9 \ Mbps}$

    ![](/posts/20240722/statistics.png){: .border }
    _**Figure 4.** TCP statistics_

    <br>

12. Use the _Time Sequence (Stevens)_ plotting tool to view the sequence number versus time plot of segments being sent from the client to the gaia.cs.umass.edu server. Consider the "fleets" of packets sent around $t = 0.025, \ t = 0.053, \ t = 0.082 \ \text{and} \ t = 0.1$. Comment on whether this looks as if TCP is in its slow start phase, congestion avoidance phase or some other phase.
    - **Slow start phase**: 시간에 따른 패킷 전송량이 지수적으로 증가하고 있다.

13. These "fleets" of segments appear to have some periodicity. What can you say about the period?
    - Round Trip Time (RTT)

    ![](/posts/20240722/time-sequence-graph.png){: .border }
    _**Figure 5.** Time-Sequence graph (Stevens)_

    <br>

## References

---

- [J. F. Kurose and K. W. Ross. _Wireshark Lab: TCP v8.1_. (2021). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_TCP_v8.1.doc)
