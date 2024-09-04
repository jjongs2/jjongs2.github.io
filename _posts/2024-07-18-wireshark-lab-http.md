---
title: "Wireshark Lab: HTTP v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-07-18 12:23:49 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. Is your browser running HTTP version 1.0, 1.1, or 2? What version of HTTP is the server running?
   - HTTP/1.1

2. What languages (if any) does your browser indicate that it can accept to the server?
   - **en-US**: English (United States)
   - **en**: English

3. What is the IP address of your computer? What is the IP address of the gaia.cs.umass.edu server?
   - `10.0.0.44`
   - `128.119.245.12`

   ![](/posts/20240718/request-1.png){: .border }
   _**Figure 1.** HTTP request 1_

   <br>

4. What is the status code returned from the server to your browser?
   - `200 OK`

5. When was the HTML file that you are retrieving last modified at the server?
   - `Last-Modified: Sat, 30 Jan 2021 06:59:02 GMT`

6. How many bytes of content are being returned to your browser?
   - `Content-Length: 128`

7. By inspecting the raw data in the packet content window, do you see any headers within the data that are not displayed in the packet-listing window? If so, name one.
   - `Connection: Keep-Alive`

   ![](/posts/20240718/response-1.png){: .border }
   _**Figure 2.** HTTP response 1_

   <br>

8. Inspect the contents of the first HTTP GET request from your browser to the server. Do you see an "IF-MODIFIED-SINCE" line in the HTTP GET?
   - 보이지 않음

   ![](/posts/20240718/request-2-1.png){: .border }
   _**Figure 3.** HTTP request 2-1_

   <br>

9. Inspect the contents of the server response. Did the server explicitly return the contents of the file? How can you tell?
   - 상태 코드와 응답 헤더 및 본문을 통해 **서버가 객체를 반환하였음**을 알 수 있다.

   ![](/posts/20240718/response-2-1.png){: .border }
   _**Figure 4.** HTTP response 2-1_

   <br>

10. Now inspect the contents of the second HTTP GET request from your browser to the server. Do you see an "IF-MODIFIED-SINCE:" line in the HTTP GET? If so, what information follows the "IF-MODIFIED-SINCE:" header?
    - 이전 응답의 `Last-Modified` 헤더 값

    ![](/posts/20240718/request-2-2.png){: .border }
    _**Figure 5.** HTTP request 2-2_

    <br>

11. What is the HTTP status code and phrase returned from the server in response to this second HTTP GET? Did the server explicitly return the contents of the file? Explain.
    - `304 Not Modified`
    - 객체가 `If-Modified-Since` 이후로 수정되지 않았으므로 **서버가 객체를 반환하지 않음** (캐시 이용)

    ![](/posts/20240718/response-2-2.png){: .border }
    _**Figure 6.** HTTP response 2-2_

    <br>

12. How many HTTP GET request messages did your browser send? Which packet number in the trace contains the GET message for the Bill of Rights?
    - 1개 (#26)

13. Which packet number in the trace contains the status code and phrase associated with the response to the HTTP GET request?
    - #32

14. What is the status code and phrase in the response?
    - `200 OK`

15. How many data-containing TCP segments were needed to carry the single HTTP response and the text of the Bill of Rights?
    - 4개 (#28, #29, #31, #32)

    ![](/posts/20240718/multi-packet.png){: .border }
    _**Figure 7.** Multi-packet response_

    <br>

16. How many HTTP GET request messages did your browser send? To which Internet addresses were these GET requests sent?
    - 1, 2번째: `128.119.245.12`
    - 3번째: `178.79.137.164`
    - 4번째: `104.98.115.146`

17. Can you tell whether your browser downloaded the two images serially, or whether they were downloaded from the two web sites in parallel? Explain.
    - 요청 및 응답 시간을 보면 **순차적**임을 알 수 있다.

    ![](/posts/20240718/embedded-objects.png){: .border }
    _**Figure 8.** HTML with embedded objects_

    <br>

18. What is the server's response (status code and phrase) in response to the initial HTTP GET message from your browser?
    - `401 Unauthorized`

19. When your browser's sends the HTTP GET message for the second time, what new field is included in the HTTP GET message?
    - `Authorization`

    ![](/posts/20240718/authentication.png){: .border }
    _**Figure 9.** HTTP authentication_

    <br>

## References

---

- [J. F. Kurose and K. W. Ross. _Wireshark Lab: HTTP v8.1_. (2021). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_HTTP_v8.1.doc)
