---
title: "Wireshark Lab: DNS v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-07-19 08:53:09 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. Run `nslookup` to obtain the IP address of the web server for the Indian Institute of Technology in Bombay, India: www.iitb.ac.in. What is the IP address of www.iitb.ac.in?
   - `103.21.124.10`

2. What is the IP address of the DNS server that provided the answer to your `nslookup` command in question 1 above?
   - `75.75.75.75`

3. Did the answer to your `nslookup` command in question 1 above come from an authoritative or non-authoritative server?
   - Non-authoritative server

4. Use the `nslookup` command to determine the name of the authoritative name server for the iit.ac.in domain. What is that name? (If there are more than one authoritative servers, what is the name of the first authoritative server returned by `nslookup`)? If you had to find the IP address of that authoritative name server, how would you do so?
   - `dns1.iitb.ac.in`
   - `nslookup dns1.iitb.ac.in`

   ```bash
   $ nslookup www.iitb.ac.in
   Server:         75.75.75.75
   Address:        75.75.75.75#53

   Non-authoritative answer:
   Name:   www.iitb.ac.in
   Address: 103.21.124.10

   $ nslookup -type=NS iitb.ac.in
   Server:         75.75.75.75
   Address:        75.75.75.75#53

   Non-authoritative answer:
   iitb.ac.in      nameserver = dns1.iitb.ac.in.
   iitb.ac.in      nameserver = dns2.iitb.ac.in.
   iitb.ac.in      nameserver = dns3.iitb.ac.in.
   ```

   <br>

5. Locate the first DNS query message resolving the name gaia.cs.umass.edu. What is the packet number in the trace for the DNS query message? Is this query message sent over UDP or TCP?
   - #15
   - UDP

6. Now locate the corresponding DNS response to the initial DNS query. What is the packet number in the trace for the DNS response message? Is this response message received via UDP or TCP?
   - #17
   - UDP

7. What is the destination port for the DNS query message? What is the source port of the DNS response message?
   - 53번 포트

8. To what IP address is the DNS query message sent?
   - `75.75.75.75`

9. Examine the DNS query message. How many "questions" does this DNS message contain? How many "answers" does it contain?
   - `Questions: 1`
   - `Answer RRs: 0`

10. Examine the DNS response message to the initial query message. How many "questions" does this DNS message contain? How many "answers" does it contain?
    - `Questions: 1`
    - `Answer RRs: 1`

    ![](/posts/20240719/query-browser.png){: .border }
    _**Figure 1.** DNS query & response with a web browser_

    <br>

11. The web page for the base file http://gaia.cs.umass.edu/kurose_ross/ references the image object http://gaia.cs.umass.edu/kurose_ross/header_graphic_book_8E_2.jpg, which, like the base webpage, is on gaia.cs.umass.edu. What is the packet number in the trace for the initial HTTP GET request for the base file http://gaia.cs.umass.edu/kurose_ross/? What is the packet number in the trace of the DNS query made to resolve gaia.cs.umass.edu so that this initial HTTP request can be sent to the gaia.cs.umass.edu IP address? What is the packet number in the trace of the received DNS response? What is the packet number in the trace for the HTTP GET request for the image object http://gaia.cs.umass.edu/kurose_ross/header_graphic_book_8E2.jpg? What is the packet number in the DNS query made to resolve gaia.cs.umass.edu so that this second HTTP request can be sent to the gaia.cs.umass.edu IP address? Discuss how DNS caching affects the answer to this last question.
    - #22
    - #15
    - #17
    - #205
    - 2번째 HTTP 요청을 보내기 위한 DNS 쿼리는 존재하지 않는다.
        - 1번째 쿼리에 대한 응답(`gaia.cs.umass.edu`의 IP 주소)을 저장하여, 해당 레코드가 유효한 동안에는 추가적인 DNS 쿼리 없이 바로 HTTP 요청을 보낼 수 있다.

    ![](/posts/20240719/caching.png){: .border }
    _**Figure 2.** DNS caching_

    <br>

12. What is the destination port for the DNS query message? What is the source port of the DNS response message?
    - 53번 포트

13. To what IP address is the DNS query message sent? Is this the IP address of your default local DNS server?
    - `75.75.75.75` (기본 DNS 서버)

14. Examine the DNS query message. What "Type" of DNS query is it? Does the query message contain any "answers"?
    - Type A
    - `Answer RRs: 0`

15. Examine the DNS response message to the query message. How many "questions" does this DNS response message contain? How many "answers"?
    - `Questions: 1`
    - `Answer RRs: 1`

    ![](/posts/20240719/query-nslookup.png){: .border }
    _**Figure 3.** DNS query & response with `nslookup`_

    <br>

16. To what IP address is the DNS query message sent? Is this the IP address of your default local DNS server?
    - `75.75.75.75` (기본 DNS 서버)

17. Examine the DNS query message. How many questions does the query have? Does the query message contain any "answers"?
    - `Questions: 1`
    - `Answer RRs: 0`

18. Examine the DNS response message (in particular the DNS response message that has type "NS"). How many answers does the response have? What information is contained in the answers? How many additional resource records are returned? What additional information is included in these additional resource records (if additional information is returned)?
    - `Questions: 1`
    - `Answer RRs: 3`
      - 도메인 `umass.edu`에 대한 authoritative 네임 서버의 호스트 이름
    - `Additional RRs: 3`
      - 각 네임 서버의 IP 주소

    ![](/posts/20240719/query-nslookup-ns.png){: .border }
    _**Figure 3.** DNS query & response with `nslookup -type=NS`_

    <br>

## References

---

- [J. F. Kurose and K. W. Ross. _Wireshark Lab: DNS v8.1_. (2021). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_DNS_v8.1.doc)
