---
title: "Cryptology Fundamentals Exam-CAPT-"
date: 2025-08-24 11:27:29 +0000
categories: [bugbounty writeup]
tags: [bug-bounty, writeup, web-security]
image: "https://cdn-images-1.medium.com/max/1024/1*jaSLUMrjWWLldJCHNBpZOw.png"
thumbnail: "https://cdn-images-1.medium.com/max/1024/1*jaSLUMrjWWLldJCHNBpZOw.png"
medium_link: "https://medium.com/@yosefmostef99/cryptology-fundamentals-exam-capt-72c1dfbe83df"
description: "I’m really excited to share my very first article on Medium! ⚔:-D It will cover a set of questions from the Cryptology Fundamentals section of the Certified Associate Penetration Tester (CAPT) certification."
toc: true
---

> بِسْمِ الَّذِي عَلَّمَ بِالْقَلَمِ، عَلَّمَ الإِنسَانَ مَا لَمْ يَعْلَمْ، وَالصَّلَاةُ وَالسَّلَامُ عَلَى خَيْرِ مُعَلِّمِ النَّاسِ، عّلّى خّيرِ مُحِمد… أَمَّا بَعْدُ….

**I’m really excited to share my very first article on Medium! ⚔:-D It will cover a set of questions from the Cryptology Fundamentals section of the Certified Associate Penetration Tester (CAPT) certification.**

![](https://cdn-images-1.medium.com/max/1024/1*jaSLUMrjWWLldJCHNBpZOw.png)

### Key Points Summary 😁

### 1. Introduction

Cryptology = **Cryptography** (protecting data) + **Cryptanalysis** (breaking/enhancing security).History: from Ancient Egypt → Caesar cipher → Enigma → modern cryptography.Importance: ensures **CIA (Confidentiality, Integrity, Availability)** for sensitive data (e.g., government info, certificates).

### 2. Cryptography Basics

**Encryption** = algorithm + key; **Decryption** = reverse process.Keys ensure correct operation.**Public / Private Keys** in asymmetric encryption.**Algorithms**: AES, RSA, DES.**Protocols**: SSL/TLS, SSH, IPsec.

### 3. Cryptography vs. Cryptanalysis

**Cryptography**: protects data using math.**Cryptanalysis**: attempts to break protection or find weaknesses.Types: **Symmetric** (same key) vs. **Asymmetric** (public/private keys).Principles: **Kerckhoffs’s Principle** (security relies on key secrecy), **Shannon’s Theory** (ciphertext must look random).Attacks: Brute Force, Dictionary, Side-Channel, Statistical.

### 4. Symmetric Encryption

Uses one key for both encryption and decryption.**Block Ciphers**: DES, 3DES, AES.**Stream Ciphers**: RC4, Salsa20, ChaCha20.**Modes of Operation**: ECB, CBC, CFB, OFB, CTR.**Pros**: fast, efficient, simple.**Cons**: key distribution/management challenges, single key compromise = total loss.**Applications**: VPNs, SSL/TLS, data storage.

### 5. Asymmetric Encryption

Two keys: **public** (shared) + **private** (kept secret).**Algorithms**: RSA, ECC, ElGamal, DSA.**Pros**: solves key distribution, stronger authentication, supports digital signatures.**Cons**: slower, requires more resources.**PKI**: system for managing public/private keys and digital certificates.**Applications**: SSL/TLS, secure email, VPN, authentication, certificates.

### 6. Hash Functions

Convert input of any size → fixed-length value.**Properties**: one-way, collision resistance, avalanche effect.**Algorithms**: MD5, SHA-1, SHA-2, SHA-3, RIPEMD-160.**Attacks**: rainbow tables, collision attacks.**Uses**: password storage, signatures, data integrity verification.

### 7. Digital Signatures

Use asymmetric cryptography for: **authentication, integrity, non-repudiation**.**Process**: hash document → encrypt hash with private key → receiver verifies with public key.**Algorithms**: RSA, DSA, ECDSA.**Applications**: email security, contracts, financial transactions, e-government.

### 8. Cryptographic Protocols

Rules and algorithms for secure communication, identity verification, data integrity.**Examples**: SSL/TLS, IPsec, SSH, PGP, Kerberos, OAuth.**Applications**: web security, network security, email, authentication, data protection.

### 9. Cryptanalysis Techniques

Science of breaking encryption and testing security.**Techniques**: Brute Force, Dictionary, Side-Channel, Known Plaintext, Frequency Analysis, Differential, Linear, Man-in-the-Middle.**Applications**: security evaluation, digital forensics, military/intelligence, cyberattacks.

### Finally, the exam — along with its solution

*First Question*

![I managed to get this one easily because the lecturer had already explained the definition at the start](https://cdn-images-1.medium.com/max/628/1*zzKfL4uh-k-1QUVfTTpe9A.png)
*I managed to get this one easily because the lecturer had already explained the definition at the start*

![in this question, we were given multiple choices: the first option described the process of converting plain text into ciphertext, the second was the actual correct answer, the third referred to a process that encrypts data but produces a fixed — though not always consistent — output, and the last one simply meant plain text, not the operation itself. That’s why the right choice was the second one: decryption](https://cdn-images-1.medium.com/max/638/1*dIN8D-6mmT5eVXoDxQR4rw.png)
*in this question, we were given multiple choices: the first option described the process of converting plain text into ciphertext, the second was the actual correct answer, the third referred to a process that encrypts data but produces a fixed — though not always consistent — output, and the last one simply meant plain text, not the operation itself. That’s why the right choice was the second one: decryption*

![This question was about asymmetric encryption. The idea was to identify something that everyone can access in this algorithm. The first option was invalid because it referred to the private key, which is only held by the owner and never shared publicly. The second option described decryption, which clearly wasn’t relevant since the question asked about what is owned, not the process of converting ciphertext back to plaintext. The third option was plain text — which honestly made no sense in this context. So naturally, the last option was the correct one](https://cdn-images-1.medium.com/max/607/1*6Pya2H__b2tPQCAj56_6BA.png)
*This question was about asymmetric encryption. The idea was to identify something that everyone can access in this algorithm. The first option was invalid because it referred to the private key, which is only held by the owner and never shared publicly. The second option described decryption, which clearly wasn’t relevant since the question asked about what is owned, not the process of converting ciphertext back to plaintext. The third option was plain text — which honestly made no sense in this context. So naturally, the last option was the correct one*

![This was a tricky yet well-crafted question. At first glance, all the options might look equally valid, but the correct answer is the first one. That’s because non-repudiation is considered a secondary principle, not a primary goal of cryptography](https://cdn-images-1.medium.com/max/608/1*jg62vGBwXJCSXHiopZwDYQ.png)
*This was a tricky yet well-crafted question. At first glance, all the options might look equally valid, but the correct answer is the first one. That’s because non-repudiation is considered a secondary principle, not a primary goal of cryptography*

![Easy](https://cdn-images-1.medium.com/max/592/1*6ZzpUlRfQwUqJBnnpkhcrw.png)
*Easy*

![Asymmetric](https://cdn-images-1.medium.com/max/626/1*y-by0gRUEVxVzvVOdwDHfA.png)
*Asymmetric*

![Base64 is Encoding!!!](https://cdn-images-1.medium.com/max/555/1*s-x2u0oM5Re794Btl_Y7gQ.png)
*Base64 is Encoding!!!*

![This question is brilliant because it really tests your research skills. Even if you don’t look it up, you could still figure it out through experimentation — which is exactly what I did (admittedly, out of a bit of laziness) :)](https://cdn-images-1.medium.com/max/601/1*cRnbUaj68kGOm8KbsJVrjg.png)
*This question is brilliant because it really tests your research skills. Even if you don’t look it up, you could still figure it out through experimentation — which is exactly what I did (admittedly, out of a bit of laziness) :)*

![This is a straightforward question that mainly tests your knowledge of protocols. The one that isn’t encrypted is Telnet. According to Wikipedia, ‘Telnet transmits all information, including usernames and passwords, in plaintext, so it is not recommended for security-sensitive applications such as remote management of routers. Its use has largely declined in favor of SSH, although some extensions were proposed to add encryption.’-https://en.wikipedia.org/wiki/Telnet- So, the correct answer here is Telnet.](https://cdn-images-1.medium.com/max/503/1*QkrtQwQbey9gbf8LAKASBA.png)
*This is a straightforward question that mainly tests your knowledge of protocols. The one that isn’t encrypted is Telnet. According to Wikipedia, ‘Telnet transmits all information, including usernames and passwords, in plaintext, so it is not recommended for security-sensitive applications such as remote management of routers. Its use has largely declined in favor of SSH, although some extensions were proposed to add encryption.’-https://en.wikipedia.org/wiki/Telnet- So, the correct answer here is Telnet.*

![](https://cdn-images-1.medium.com/max/613/1*VJtQmf4MgNheiNg0LSgJew.png)

![I was only able to solve this question by using two steps: first, I identified the type of hash using https://hashes.com/en/tools/hash_identifier,Once I knew the algorithm, I entered it into https://md5hashing.net/hash/](https://cdn-images-1.medium.com/max/625/1*C1K_OCVano6BWEkL2duSPg.png)
*I was only able to solve this question by using two steps: first, I identified the type of hash using https://hashes.com/en/tools/hash_identifier,Once I knew the algorithm, I entered it into https://md5hashing.net/hash/*

![](https://cdn-images-1.medium.com/max/637/1*tN0pTejp36-J9HHdc-kGmw.png)

And with that, we’ve reached the end of the article. Wishing you a great day — take care and goodbye. 🥰
