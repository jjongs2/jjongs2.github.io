---
title: "Wireshark Lab: Ethernet and ARP v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-08-01 13:16:53 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. What is the 48-bit Ethernet address of your computer?
   - `Source: BelkinIntern_75:b1:52 (c4:41:1e:75:b1:52)` (내 컴퓨터)

2. What is the 48-bit destination address in the Ethernet frame? Is this the Ethernet address of gaia.cs.umass.edu? (Hint: the answer is _no_). What device has this as its Ethernet address? [Note: this is an important question, and one that students sometimes get wrong. Re-read pages 513-514 in the text and make sure you understand the answer here.]
   - `Destination: 3ComEurope_7e:d9:01 (00:1e:c1:7e:d9:01)` (첫 번째 라우터)

3. What is the hexadecimal value for the two-byte Frame type field in the Ethernet frame carrying the HTTP GET request? What upper layer protocol does this correspond to?
   - `Type: IPv4 (0x0800)`

4. How many bytes from the very start of the Ethernet frame does the ASCII "G" in "GET" appear in the Ethernet frame? Do not count any preamble bits in your count, i.e., assume that the Ethernet frame begins with the Ethernet frame's destination address.
   - 66 bytes

   ![](/posts/20240801/http-request.png){: .border }
   _**Figure 1.** Ethernet frame containing HTTP request_

   <br>

5. What is the value of the Ethernet source address? Is this the address of your computer, or of gaia.cs.umass.edu (Hint: the answer is _no_). What device has this as its Ethernet address?
   - `Source: 3ComEurope_7e:d9:01 (00:1e:c1:7e:d9:01)` (첫 번째 라우터)

6. What is the destination address in the Ethernet frame? Is this the Ethernet address of your computer?
   - `Destination: BelkinIntern_75:b1:52 (c4:41:1e:75:b1:52)` (내 컴퓨터)

7. Give the hexadecimal value for the two-byte Frame type field. What upper layer protocol does this correspond to?
   - `Type: IPv4 (0x0800)`

8. How many bytes from the very start of the Ethernet frame does the ASCII "O" in "OK" (i.e., the HTTP response code) appear in the Ethernet frame? Do not count any preamble bits in your count, i.e., assume that the Ethernet frame begins with the Ethernet frame's destination address.
   - 79 bytes

9. How many Ethernet frames (each containing an IP datagram, each containing a TCP segment) carry data that is part of the complete HTTP "OK 200 ..." reply message?
   - 4개 (#131, #132, #133, #134)

   ![](/posts/20240801/http-response.png){: .border }
   _**Figure 2.** Ethernet frame containing the first byte of HTTP response_

   <br>

10. How many entries are stored in your ARP cache?
    - 3개

11. What is contained in each displayed entry of the ARP cache?
    - IP 주소와 MAC 주소 간의 매핑 정보

    ```bash
    $ arp -a
    gw-vlan-2471.cs.umass.edu (128.119.247.1) at 0:1e:c1:7e:d9:1 on en9 ifscope [ethernet]
    sammac.cs.umass.edu (128.119.247.19) at (incomplete) on en9 ifscope [ethernet]
    robomac.cs.umass.edu (128.119.247.79) at 78:7b:8a:ac:ad:e1 on en9 ifscope [ethernet]
    ```

    <br>

12. What is the hexadecimal value of the source address in the Ethernet frame containing the ARP request message sent out by your computer?
    - `Source: BelkinIntern_75:b1:52 (c4:41:1e:75:b1:52)` (내 컴퓨터)

13. What is the hexadecimal value of the destination addresses in the Ethernet frame containing the ARP request message sent out by your computer? And what device (if any) corresponds to that address (e.g,, client, server, router, switch or otherwise...)?
    - `Destination: Broadcast (ff:ff:ff:ff:ff:ff)` (해당 서브넷의 모든 호스트)

14. What is the hexadecimal value for the two-byte Ethernet Frame _type_ field. What upper layer protocol does this correspond to?
    - `Type: ARP (0x0806)`

15. How many bytes from the very beginning of the Ethernet frame does the ARP _opcode_ field begin?
    - 20 bytes

16. What is the value of the _opcode_ field within the ARP request message sent by your computer?
    - `Opcode: request (1)`

17. Does the ARP request message contain the IP address of the sender? If the answer is yes, what is that value?
    - `Sender IP address: 128.119.247.66`

18. What is the IP address of the device whose corresponding Ethernet address is being requested in the ARP request message sent by your computer?
    - `Target IP address: 128.119.247.1`

    ![](/posts/20240801/arp-request.png){: .border }
    _**Figure 3.** ARP request_

    <br>

19. What is the value of the _opcode_ field within the ARP reply message received by your computer?
    - `Opcode: reply (2)`

20. _Finally(!),_ let's look at the **answer** to the ARP request message! What is the Ethernet address corresponding to the IP address that was specified in the ARP request message sent by your computer (see question 18)?
    - `Sender MAC address: 3ComEurope_7e:d9:01 (00:1e:c1:7e:d9:01)` (첫 번째 라우터)

21. We've looked the ARP request message sent by your computer running Wireshark, and the ARP reply message sent in response. But there are other devices in this network that are also sending ARP request messages that you can find in the trace. Why are there no ARP replies in your trace that are sent in response to these other ARP request messages?
    - Request 메시지는 브로드캐스트되지만, reply 메시지는 **request한 호스트에게만 전송**되기 때문이다.

    ![](/posts/20240801/arp-reply.png){: .border }
    _**Figure 4.** ARP reply_

    <br>

## Extra Credit

---

1. The arp command:
   ```bash
   arp -s InetAddr EtherAddr
   ```
   allows you to manually add an entry to the ARP cache that resolves the IP address _InetAddr_ to the physical address _EtherAddr_. What would happen if, when you manually added an entry, you entered the correct IP address, but the wrong Ethernet address for that remote interface? A security attack known as "[ARP poisoning](https://www.varonis.com/blog/arp-poisoning/)" spoofs ARP messages and causes incorrect entries to be made into an ARP table!
   - 잘못된 MAC 주소로 인해, 해당 IP 주소로의 **패킷이 올바른 목적지에 도달하지 못한다.**
   - 엉뚱한 MAC 주소로 트래픽이 리다이렉션되어 **정보가 유출되거나 조작될 수 있다.**

   <br>

2. What is the default amount of time that an entry remains in your ARP cache before being removed? You can determine this empirically (by monitoring the cache contents) or by looking this up in your operating system documentation. Indicate how/where you determined this value.
   - 60초 (Linux)

   ```bash
   $ sysctl net.ipv4.neigh.default.gc_stale_time
   net.ipv4.neigh.default.gc_stale_time = 60
   ```

   <br>

## References

---

- [J. F. Kurose and K. W. Ross. _Wireshark Lab: Ethernet and ARP v8.1_. (2021). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_Ethernet_ARP_v8.1.doc)
