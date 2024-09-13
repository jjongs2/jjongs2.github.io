---
title: "Wireshark Lab: TLS v8.1"
description: "Computer Networking: A Top-Down Approach"
date: 2024-08-09 13:00:24 +0900
categories: [Computer Science, Computer Networking]
tags: [wireshark]
image:
  path: https://live.staticflickr.com/2688/4021711687_b4f073cfa3_z.jpg
---

## Questions

---

1. What is the packet number in your trace that contains the initial TCP SYN message? (By "packet number", we meant the number in the "No." column at the left of the Wireshark display, not the sequence number in the TCP segment itself).
   - #17

2. Is the TCP connection set up before or after the first TLS message is sent from client to server?
   - TCP 연결은 첫 번째 TLS 메시지가 전송되기 전에 설정됨

3. What is the packet number in your trace that contains the TLS _Client Hello_ message?
   - #28

4. What version of TLS is your client running, as declared in the _Client Hello_ message?
   - `Version: TLS 1.2 (0x0303)`

5. How many cipher suites are supported by your client, as declared in the _Client Hello_ message? A cipher suite is a set of related cryptographic algorithms that determine how session keys will be derived, and how data will be encrypted and be digitally signed via a HMAC algorithm.
   - `Cipher Suites (17 suites)`

6. Your client generates and sends a string of "random bytes" to the server in the _Client Hello_ message. What are the first two hexadecimal digits in the random bytes field of the _Client Hello_ message? Enter the two hexadecimal digits (without spaces between the hex digits and without any leading '0x', using lowercase letters where needed). _Hint:_ be careful to fully dig into the `Random` field to find the `Random Bytes` subfield (do not consider the `GMT UNIX Time` subfield of `Random`).
   - `Random Bytes: 4b…`

7. What is the purpose(s) of the "random bytes" field in the _Client Hello_ message? Note: you'll have do some searching and reading to get the answer to this question; see section 8.6 and in RFC 5246 (section 8.1 in [RFC 5246](https://www.rfc-editor.org/rfc/rfc5246.html) in particular).
   - TLS 세션의 고유성 보장
   - `master_secret` 생성에 사용됨

   ![](/posts/20240809/client-hello.png){: .border }
   _**Figure 1.** Client Hello message_

   <br>

8. What is the packet number in your trace that contains the TLS _Server Hello_ message?
   - #32

9. Which cipher suite has been chosen by the server from among those offered in the earlier _Client Hello_ message?
   - `Cipher Suite: TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256 (0xc02f)`

10. Does the _Server Hello_ message contain random bytes, similar to how the _Client Hello_ message contained random bytes? And if so, what is/are their purpose(s)?
    - `Random Bytes: 5c…`
      - TLS 세션의 고유성 보장
      - `master_secret` 생성에 사용됨

    ![](/posts/20240809/server-hello.png){: .border }
    _**Figure 2.** Server Hello message_

    <br>

11. What is the packet number in your trace for the TLS message part that contains the public key certificate for the www.cics.umass.edu server (actually the www.cs.umass.edu server)?
    - #37

12. A server may return more than one certificate. If more than one certificate is returned, are all of these certificates for www.cs.umass.edu? If not all are for www.cs.umass.edu, then who are these other certificates for? You can determine who the certificate is for by checking the `id-at-commonName` field in the returned certificate.
    - `RDNSequence item: 1 item (id-at-commonName=www.cs.umass.edu)`
    - `RDNSequence item: 1 item (id-at-commonName=InCommon RSA Server CA)`
    - `RDNSequence item: 1 item (id-at-commonName=USERTrust RSA Certification Authority)`

13. What is the name of the certification authority that issued the certificate for `id-at-commonName=www.cs.umass.edu`?
    - `RDNSequence item: 1 item (id-at-commonName=InCommon RSA Server CA)`

14. What digital signature algorithm is used by the CA to sign this certificate? Hint: this information can be found in `signature` subfield of the `SignedCertificate` field of the certificate for www.cs.umass.edu.
    - `Algorithm Id: 1.2.840.113549.1.1.11 (sha256WithRSAEncryption)`

15. Let's take a look at what a real public key looks like! What are the first four hexadecimal digits of the modulus of the public key being used by www.cics.umass.edu? Enter the four hexadecimal digits (without spaces between the hex digits and without any leading '0x', using lowercase letters where needed, and including any leading 0s after '0x'). Hint: this information can be found in `subjectPublicKeyInfo` subfield of the `SignedCertificate` field of the certificate for www.cs.umass.edu.
    - `modulus: 0x00b3…`

16. Look in your trace to find messages between the client and a CA to get the CA's public key information, so that the client can verify that the CA-signed certificate sent by the server is indeed valid and has not been forged or altered. Do you see such message in your trace? If so, what is the number in the trace of the first packet sent from your client to the CA? If not, explain why the client did not contact the CA.
    - 클라이언트가 이미 CA의 공개 키 정보를 가지고 있기 때문에, CA와 통신할 필요가 없다.

17. What is the packet number in your trace for the TLS message part that contains the _Server Hello Done_ TLS record?
    - #37

    ![](/posts/20240809/certificates.png){: .border }
    _**Figure 3.** Certificates_

    <br>

18. What is the packet number in your trace for the TLS message that contains the public key information, _Change Cipher Spec_, and _Encrypted Handshake_ message, being sent from client to server?
    - #39

19. Does the client provide its own CA-signed public key certificate back to the server? If so, what is the packet number in your trace containing your client's certificate?
    - 클라이언트는 서버에게 공개 키 인증서를 제공하지 않는다. (서버 요청 시 제공)

20. What symmetric key cryptography algorithm is being used by the client and server to encrypt application data (in this case, HTTP messages)?
    - AES-128-GCM

21. In which of the TLS messages is this symmetric key cryptography algorithm finally decided and declared?
    - Server Hello 메시지

22. What is the packet number in your trace for the first encrypted message carrying application data from client to server?
    - #41

    ![](/posts/20240809/application-data.png){: .border }
    _**Figure 4.** Encrypted application data_

    <br>

23. What do you think the content of this encrypted application-data is, given that this trace was generated by fetching the homepage of www.cics.umass.edu?
    - HTTP GET 요청

24. What packet number contains the client-to-server TLS message that shuts down the TLS connection? Because TLS messages are encrypted in our Wireshark traces, we can't actually look _inside_ a TLS message and so we'll have to make an educated guess here.
    - #358

    ![](/posts/20240809/alert.png){: .border }
    _**Figure 5.** Encrypted alert_

    <br>

## References

---

- [J. F. Kurose and K. W. Ross. _TLS v8.1_. (2021). [Word].](https://www-net.cs.umass.edu/wireshark-labs/Wireshark_TLS_v8.1.doc)
