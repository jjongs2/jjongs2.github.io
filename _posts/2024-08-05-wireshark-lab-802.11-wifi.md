---
title: "Wireshark Lab: 802.11 WiFi v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-08-05 12:33:13 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. What are the SSIDs of the two access points that are issuing most of the beacon frames in this trace? [Hint: look at the _Info_ field. To display only beacon frames, neter `wlan.fc.type_subtype == 8` into the Wireshark display filter].
   - `30 Munroe St`, `linksys12`

2. What 802.11 channel is being used by both of these access points? [Hint: you'll need to dig into the radio information in an 802.11 beacon frame]
   - `Channel: 6`

   ![](/posts/20240805/radio-info.png){: .border }
   _**Figure 1.** 802.11 radio information_

   <br>

3. What is the interval of time between the transmissions of beacon frames from this access point (AP)? (Hint: this interval of time is contained in a field within the beacon frame itself).
   - `Beacon Interval: 0.102400 [Seconds]`

4. What (in hexadecimal notation) is the source MAC address on the beacon frame from this access point? Recall from Figure 7.13 in the text that the source, destination, and BSS are three addresses used in an 802.11 frame. For a detailed discussion of the 802.11 frame structure, see section 9.2.3-9.2.4.1in the IEEE 802.11 standards document, excerpted [here](https://gaia.cs.umass.edu/wireshark-labs/802.11-9.2.4.1_spec+wireshark_filters.pdf).
   - `Source address: CiscoLinksys_f7:1d:51 (00:16:b6:f7:1d:51)`

5. What (in hexadecimal notation) is the destination MAC address on the beacon frame from _30 Munroe St_?
   - `Destination address: Broadcast (ff:ff:ff:ff:ff:ff)`

6. What (in hexadecimal notation) is the MAC BSS ID on the beacon frame from _30 Munroe St_?
   - `BSS Id: CiscoLinksys_f7:1d:51 (00:16:b6:f7:1d:51)`

7. The beacon frames from the _30 Munroe St_ access point advertise that the access point can support four data rates and eight additional "extended supported rates." What are these rates? [Note: the traces were taken on a rather old AP].
   - `Tag: Supported Rates 1(B), 2(B), 5.5(B), 11(B), [Mbit/sec]`
   - `Tag: Extended Supported Rates 6(B), 9, 12(B), 18, 24(B), 36, 48, 54, [Mbit/sec]`

   ![](/posts/20240805/beacon-frame.png){: .border }
   _**Figure 2.** 802.11 Beacon frame_

   <br>

8. Find the 802.11 frame containing the SYN TCP segment for this first TCP session (that downloads alice.txt) at t=24.8110. What are three MAC address fields in the 802.11 frame? Which MAC address in this frame corresponds to the wireless host (give the hexadecimal representation of the MAC address for the host)? To the access point? To the first-hop router? What is the IP address of the wireless host sending this TCP segment? What is the destination IP address for the TCP syn segment?
   - MAC 주소
     - `Source address: Intel_d1:b6:4f (00:13:02:d1:b6:4f)` (호스트)
     - `Destination address: CiscoLinksys_f4:eb:a8 (00:16:b6:f4:eb:a8)` (라우터)
     - `BSS Id: CiscoLinksys_f7:1d:51 (00:16:b6:f7:1d:51)` (AP)
   - IP 주소
     - `Source Address: 192.168.1.109`
     - `Destination Address: 128.119.245.12`

9. Does the destination IP address of this TCP SYN correspond to the host, access point, first-hop router, or the destination web server?
   - 웹 서버

   ![](/posts/20240805/tcp-syn.png){: .border }
   _**Figure 3.** TCP SYN segment_

   <br>

10. Find the 802.11 frame containing the SYNACK segment for this TCP session received at _t = 24.8277_ What are three MAC address fields in the 802.11 frame? Which MAC address in this frame corresponds to the host? To the access point? To the first-hop router? Does the sender MAC address in the frame correspond to the IP address of the device that sent the TCP segment encapsulated within this datagram? (Hint: review Figure 6.19 in the text if you are unsure of how to answer this question, or the corresponding part of the previous question. It's particularly important that you understand this).
    - `Source address: CiscoLinksys_f4:eb:a8 (00:16:b6:f4:eb:a8)` (라우터)
    - `Destination address: 91:2a:b0:49:b6:4f (91:2a:b0:49:b6:4f)` (호스트)
    - `BSS Id: CiscoLinksys_f7:1d:51 (00:16:b6:f7:1d:51)` (AP)
    - Sender의 MAC 주소는 라우터에 해당하는 반면에, IP 주소는 웹 서버에 해당한다.

    ![](/posts/20240805/tcp-syn-ack.png){: .border }
    _**Figure 4.** TCP SYN-ACK segment_

    <br>

11. What two actions are taken (i.e., frames are sent) by the host in the trace just after _t = 49_, to **end** the association with the _30 Munroe St_ AP that was initially in place when trace collection began? (Hint: one is an IP-layer action, and one is an 802.11-layer action).
    - DHCP Release 메시지 전송
    - Deauthentication 프레임 전송

    ![](/posts/20240805/end-association.png){: .border }
    _**Figure 5.** End the association with AP_

    <br>

12. Let's look first at AUTHENTICATION frames. At _t = 63.1680_, our host tries to associate with the _30 Munroe St_ AP. Use the Wireshark display filter `wlan.fc.subtype == 11` to show AUTHENICATION frames sent from the host to and AP and vice versa. What form of authentication is the host requesting?
    - `Authentication Algorithm: Open System (0)`

13. What is the `Authentication SEQ` value (authentication sequence number) of this authentication frame from host to AP?
    - `Authentication SEQ: 0x0001`

    ![](/posts/20240805/auth-request.png){: .border }
    _**Figure 6.** Authentication request_

    <br>

14. The AP response to the authentication request is received at _t = 63.1690_. Has the AP accepted the form of authentication requested by the host?
    - `Status code: Successful (0x0000)`

15. What is the `Authentication SEQ` value of this authentication frame from AP to Host?
    - `Authentication SEQ: 0x0002`

    ![](/posts/20240805/auth-response.png){: .border }
    _**Figure 7.** Authentication response_

    <br>

16. What rates are indicated in the frame as SUPPORTED RATES. Do _not_ include in your answers below any rates that are indicates as EXTENDED SUPPORTE RATES.
    - `Tag: Supported Rates 1(B), 2(B), 5.5(B), 11(B), 6(B), 9, 12(B), 18, [Mbit/sec]`

    ![](/posts/20240805/association-request.png){: .border }
    _**Figure 8.** Association request_

    <br>

17. Does the ASSOCIATION RESPONSE indicate a Successful or Unsuccessful association response?
    - `Status code: Successful (0x0000)`

18. Does the fastest (largest) Extended Supported Rate the host has offered match the fastest (largest) Extended Supported Rate the AP is able to provide?
    - 54 Mbit/sec로 일치함

    ![](/posts/20240805/association-response.png){: .border }
    _**Figure 9.** Association response_

    <br>

## References

---

- [J. F. Kurose and K. W. Ross. _802.11 WiFi v8.1_. (2023). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_802.11_v8.1.doc)
