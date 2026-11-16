---
title: "When the UI lies and new powers are discovered."
date: 2026-03-11 17:27:03 +0000
categories: [bugbounty writeup]
tags: [bug-bounty, writeup, web-security]
image: "https://cdn-images-1.medium.com/max/736/1*sX-gXXwahTu8wC9v4ir_7g.jpeg"
thumbnail: "https://cdn-images-1.medium.com/max/736/1*sX-gXXwahTu8wC9v4ir_7g.jpeg"
medium_link: "https://medium.com/@yosefmostef99/when-the-ui-lies-and-new-powers-are-discovered-bb39d97beeaf"
description: "سْمِ اللَّهِ الرَّحْمٰنِ الرَّحِيمِ، وَالصَّلَاةُ وَالسَّلَامُ عَلَى النَّبِيِّ الْمُجَاهِدِ الشَّهِيدِ…اللَّهُمَّ انصُرْ أَهْلَ غَزَّةَ وَفِلَسْطِينَ وَالسُّودَانَ، وَطَهِّرِ الْأَقْصَى مِنْ دَنَسِ الْيَهُودِ… أَمَّا بَعْدُ"
toc: true
---

### Finding PII via Broken Access Control.

**سْمِ اللَّهِ الرَّحْمٰنِ الرَّحِيمِ، وَالصَّلَاةُ وَالسَّلَامُ عَلَى النَّبِيِّ الْمُجَاهِدِ الشَّهِيدِ…اللَّهُمَّ انصُرْ أَهْلَ غَزَّةَ وَفِلَسْطِينَ وَالسُّودَانَ، وَطَهِّرِ الْأَقْصَى مِنْ دَنَسِ الْيَهُودِ… أَمَّا بَعْدُ**

Hello folks ,Yosef here .Today I will share the idea of one vulnerability I discovered recently a **PII exposure** across multiple endpoints in organization level ,no custom tools nothing expect my mind and burp.

![](https://cdn-images-1.medium.com/max/736/1*sX-gXXwahTu8wC9v4ir_7g.jpeg)

In my last [blog post](https://medium.com/@yosefmostef99/phantom-roles-surviving-removal-with-delete-powers-and-bypassing-add-restrictions-404ea6439ad0?source=user_profile_page---------0-------------a21460dfdabc----------------------), when I said that I had finally started **hunting seriously** and stopped procrastinating, I meant it. Since then, I have submitted **more than 20 reports** to both large and small companies. Some were closed as **NA by bots** or marked as **Duplicate (Dup)**.

Of course, I no longer aim to report the **largest possible number of vulnerabilities**. Instead, I focus only on the **better and stronger ones**, and I try to **chain vulnerabilities together** to reach the **highest possible impact**. That has been my strategy.

I continued hunting throughout the past period, averaging **around 6 hours per day**, trying to stay consistent. One evening, I was discussing things with my brother and my friend **Mohamed** — I recommend following his [account](https://medium.com/@canitey), he is a good person. He suggested that I try another platform. So I started searching until I finally **found what I was looking for**.

![my reaction when he said this :)](https://cdn-images-1.medium.com/max/314/1*n_oowixB-wTdqQ36n_Gs_A.jpeg)
*my reaction when he said this :)*

After a lot of searching and digging, my eyes landed on a platform that was also recommended to me by him.At first, when I started using it, I thought to myself that since it seemed heavily used, it would probably be difficult to find any vulnerabilities in it. But then I told myself: *what do you have to lose? Just try — you won’t lose anything.*

So I picked a target and started using it normally, just like any regular user. My goal was simply to understand its logic — how it works and how it behaves.

That’s how the first day ended, after a long session that honestly felt quite boring.

![me after this day :(](https://cdn-images-1.medium.com/max/720/1*G-Y8Jl9EMTY_2siyay5cbw.jpeg)
*me after this day :(*

On the second day, I started working seriously. I told myself there was no special security system to worry about, put my trust in God, and began.

The target — let’s call it **target.com** — provides four roles in its system:

**Admin** → Can do anything.**Manager** → Can do anything except editing users and handling payments.**User** → Limited access due to the large size of the application.**Viewer** → Can only view the call center tab and nothing else, and is isolated from other users in the organization.

I told myself the situation was simple: start by **breaking the “no” rules and the assumptions** that the developers had built into the system.

![](https://cdn-images-1.medium.com/max/736/1*LvpaRCKHUubCW7w1uuR0VQ.jpeg)

So what is **“no”**? [“No” refers to **BAC (Broken Access Control)** or **logic bugs**](https://www.youtube.com/watch?v=IaUl6g82C1Q) — the kinds of vulnerabilities that come from breaking the restrictions developers assume users will follow.So I started with the first step: **what are the boundaries?**Simple. Whatever is allowed within the interface is the **expected behavior**, and **anything we manage to access outside of that becomes a “no.”**

![](https://cdn-images-1.medium.com/max/1024/1*YWHpRxg6TYCbgI6OCwmoZQ.png)

Every time I tried to reach **admin endpoints** using the **viewer role**, I kept running into a **403 response**.

I paused for a moment and thought about it more. Then I told myself: *sooner or later, I’m going to get you ! .*

So I turned on **Burp** and left it running in the background while I continued browsing the application normally. Eventually, I found an interesting endpoint:

```text
/modules/pricebook/users/users
```

This endpoint belongs to another feature in the application called **Price Book**, and it returns **all active users** within the organization.

I took the endpoint and tested it, and the surprise was that **it worked**. It returned **all the organization’s user data**, including **phone numbers and email addresses**.

That means the endpoint was exposing **PII at the organization level**.

![Nice Catch :)](https://cdn-images-1.medium.com/max/1024/1*AvpzKYUZEbzxOJ9mttcRWg.png)
*Nice Catch :)*

The leaked information included:

**Email****Office phone****Mobile phone****User status** — *(0 means active, 1 means suspended)***The user’s internal ID in the platform**

Suddenly my **adrenaline spiked**. I told myself this couldn’t be real — there was no way this was actually a vulnerability. I was sure that someone must have already noticed it and reported it before me.

But thankfully, I was surprised to see that **the report was accepted**. The severity was **low**, yes, but it was still valid — and the **bounty is on the way**, thankfully.

![الحمدلله](https://cdn-images-1.medium.com/max/853/1*wDIH7QlJqbpYE7MHVNdHbg.png)
*الحمدلله*

An attacker with the lowest role (Viewer) could enumerate all users in the organization.

This allows attackers to:- Harvest corporate emails- Perform targeted phishing- Map internal teams- Identify active or suspended users

I wrote the **PoC**, documented the steps, took screenshots, and submitted the report. Then I kept digging around.

Eventually, I found **another endpoint** that was leaking all **user emails and IDs**. The amount of data exposed was smaller, but it was still a vulnerability.

The endpoint was:

```text
GET /modules/reporting-rollups/api/users/GetHubUsers HTTP/2
```

This one came from a **different feature** in the application. I reported it as well, but unfortunately, by the time they started reviewing it, it was closed as a **duplicate (Dup)**.

![nice try :)](https://cdn-images-1.medium.com/max/479/1*Z2PlLC3dPe2814HwBSP3tw.png)
*nice try :)*

![Screen From POC :)](https://cdn-images-1.medium.com/max/1024/1*VQ8oAe_YAtxRed-SzQwFUw.png)
*Screen From POC :)*

**Lessons Learned:**

Always test hidden endpoints.Don’t trust UI restrictions.Explore other modules.

Finally, this was a simple write‑up meant to highlight one idea: **look for the “no.”**

If an endpoint appears secure at first glance, that doesn’t mean the road is truly closed. **All roads lead to Rome** — just because one path is blocked doesn’t mean the others are. It only means you need to keep looking for another way.Security assumptions are often where vulnerabilities hide. Developers build systems based on what they believe users *won’t do* — and that’s exactly where we should start looking.

![](https://cdn-images-1.medium.com/max/474/1*NYOsPIj27lfR019Mq-YwLw.jpeg)

All praise is due to God, who blessed me with His generosity and provision. I hope everyone is doing well.

**Happy hacking. Happy hunting.**

لو استفدت متنساش تدعيلي بالتوفيق والهداية ..والسلام عليكم
