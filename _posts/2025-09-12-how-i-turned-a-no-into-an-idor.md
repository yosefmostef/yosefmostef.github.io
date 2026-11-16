---
title: "How I Turned a “No” into an IDOR.."
date: 2025-09-12 03:47:51 +0000
categories: [bugbounty writeup]
tags: [bug-bounty, writeup, web-security]
toc: true
---

**بِسْمِ اللَّهِ الرَّحْمٰنِ الرَّحِيمِ، وَالصَّلَاةُ وَالسَّلَامُ عَلَى النَّبِيِّ الْمُجَاهِدِ الشَّهِيدِ… اللَّهُمَّ انْصُرْ أَهْلَ غَزَّةَ وَفِلَسْطِينَ، وَطَهِّرِ الْأَقْصَى مِنْ دَنَسِ الْيَهُودِ… أَمَّا بَعْدُ**

**One day, while sipping a cup of my favorite drink — coffee — a thought crossed my mind… Why not try hunting for vulnerabilities again after a break that lasted for about two months…? So I said to myself, why not? Let’s go!**

![](https://cdn-images-1.medium.com/max/228/1*GwuDl1qK62AVCOMNk_tjMg.jpeg)

I decided to look for a target on my favorite platform, “Bugcrowd,” and after a while I identified a specific target and decided to work on it — let’s assume its name is *anything.com*. First of all, before hunting for any vulnerabilities, you need to understand the application. So, I opened Burp Suite and spent around five minutes actively working on the site, and the rest of the hour analyzing the requests and how the site handled them. After a few hours — I can’t recall exactly how many — I found an interesting endpoint:

```text
https://anything.com/webapi/3.9/users/{id}
```

Yes, yes… so let’s try it, why not…

![](https://cdn-images-1.medium.com/max/474/1*SLXKvxhzHQC0H1BoRETTHA.jpeg)

**IDOR (Insecure Direct Object Reference) is a vulnerability where an attacker manipulates a parameter or an object identifier (such as a user ID, file ID, or record number) in a request. If the application does not properly enforce authorization checks, the server may return data or perform actions that belong to another user. For example, by changing the ****id=123 in a request to ****id=124, an attacker might gain access to another user’s profile, sensitive information, or even modify someone else’s data. This happens because the system relies only on the provided identifier without validating whether the requesting user is authorized to access the requested object.**

So, I changed it to another ID — an account I had created for testing the vulnerability and, of course, to confirm it — and the server responded with unauthorized access….

![](https://cdn-images-1.medium.com/max/736/1*u8DwzbdlRJ76c1UdziIHSw.jpeg)

Yes, yes, this is something very likely to happen, but wait… do you see what I see?*The request was originally using the **PUT method, so I thought—why not try changing it to **GET to see if the response would be different? To my surprise, it actually returned information from another user’s account.*…Did we really do it? Did we actually find an IDOR? But wait — the fields returned this time were fewer.

In the first response, the server disclosed highly sensitive information such as the user’s email, date of birth, and more. However, in this new response, although it didn’t reveal equally critical fields, it still exposed sensitive information.

After some digging, I noticed it returned fields such as:

lastModified: showing the exact date/time when the profile was last updated.timezone: which should only be accessible to the account owner.subscriptionPlan: indicating whether the account is on a free or paid plan.*Most importantly, the **timestamp field, which can have multiple implications. In many systems, it may be used in token generation, digital signatures, session validation, or other security-critical mechanisms.*

Okay, I’ll go ahead and submit the report to the site. Honestly, at first, I wasn’t sure whether this information was sensitive or not, so I thought, why not? There’s nothing to lose. I submitted it, and the thought crossed my mind that it might be a P2 worth $1,500 — the dream of getting rich quick.

![](https://cdn-images-1.medium.com/max/630/1*JO4VMvSs0k_frcAZ8TAEaA.jpeg)

**However, the platform had a different opinion. After six days, it was classified as a P3 and also marked as a duplicate.**

![](https://cdn-images-1.medium.com/max/1024/1*ksqQwr2PS0YPPj3bflWiHQ.jpeg)

And at that moment, I remembered a quote I read a year ago: *“Maybe I didn’t succeed in anything this evening, but that’s okay — I will try again tomorrow.”*

![](https://cdn-images-1.medium.com/max/626/1*pspNDe0X0cNSoA2VC2eAbw.jpeg)

### Lessons Learned:

Always test different HTTP methods — sometimes a simple change can reveal hidden data.**Not all bugs are hard to hunt; sometimes just changing one thing can make the difference.**Going through the experience matters, even if your intuition turns out to be wrong.

And finally…**Happy hacking!**
