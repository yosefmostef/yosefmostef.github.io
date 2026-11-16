---
title: "Kicked Out, Still Posting: An Authorization Bypass in Class API"
date: 2025-10-29 10:04:28 +0000
categories: [bugbounty writeup]
tags: [bug-bounty, writeup, web-security]
toc: true
---

### Kicked Out, Still Posting: An Authorization Bypass in Class API.

**بِسْمِ اللَّهِ الرَّحْمٰنِ الرَّحِيمِ، وَالصَّلَاةُ وَالسَّلَامُ عَلَى النَّبِيِّ الْمُجَاهِدِ الشَّهِيدِ…اللَّهُمَّ انصُرْ أَهْلَ غَزَّةَ وَفِلَسْطِينَ وَالسُّودَانَ، وَطَهِّرِ الْأَقْصَى مِنْ دَنَسِ الْيَهُودِ… أَمَّا بَعْدُ**

![](https://cdn-images-1.medium.com/max/262/1*AG874IyNfTlceXUbcYE4SQ.jpeg)

One evening, after returning from an exhausting day at university, I decided to see what I should do. So I thought, why not continue my journey in securing websites? I took the site and began the inspection.

After a not so short while — about two hours — I discovered a strange and scenario…

Let’s start by **understanding the website** before anything else — because if you don’t grasp how it fundamentally works, you can’t break it.

The site is an **educational platform** specialized in virtual classrooms, providing tools for students to facilitate memorization and revision. Let’s assume its name is **target.com**.

It offers features to **create classrooms**, assign **teachers** and **students**, and manage learning resources.

So, I decided to **break the rules they assumed were unbreakable**.

### First: What are the “rules” in the first place?

![Admin rules](https://cdn-images-1.medium.com/max/965/1*Gzbb0Ca_NPjB6FVWsa5AUQ.png)
*Admin rules*

The rules, as shown in the image, are very simple: the owner or admin can allow a student to add or delete a **SET**— within the classroom, of class, not globally…So, let’s assume a simple scenario where I am the attacker.

First, what is the **unacceptable behavior** in this group? It is something like modifying a set you do not own-within the classroom, of class, not globally- and you cannot delete any user because that is outside your permissions. You also cannot add or delete a set if the admin has disabled the add/delete feature for sets. And you cannot invite someone if the admin has disabled invitations.

Great — so we have several scenarios and operations to break. But today, I will share **one operation** that I successfully broke — maybe I broke the rest or maybe not… that’s what we’ll find out in the coming days.

![🤔😁🤷‍♂️](https://cdn-images-1.medium.com/max/700/1*edBklDjfI8lpabLQVMkAzQ.jpeg)
*🤔😁🤷‍♂️*

Let me think out loud with you a bit…

What if a malicious person wanted to disrupt the group or classroom**? Let’s say the class is for **history studies**, and I go ahead and add and share content about **science **or **math** on the classroom.

That’s illogical, undesirable, and would annoy the class owners.

Now… what if I could do this without anyone being able to stop me — at all? Even the **classroom owner** can’t do anything about it.

And what if I could pull all this off while **not even being in the group anymore** — I was in it, but got kicked out because of my behavior…

Yet, even after being expelled, I still manage to add public sets to the group…Brilliant, right?Is the picture starting to clear up now? 😈

![](https://cdn-images-1.medium.com/max/480/1*UNdRrobwgMtKmtJHpMPm9A.jpeg)

Yes, the **removed member** was able to add **sets** to the group even if the admin **disabled the set-adding feature** for the group, and the admin **could not stop them at all**.

**Vulnerability Reproduction Steps:**

The attacker accesses the **endpoint** responsible for associating public sets with classrooms.Create a **private classroom** of your own and add **any public set** to it.**Intercept the request** using a proxy (e.g., Burp Suite), capture it, and send it to the **Repeater** tab.

```text
POST /webapi/3.2/class-sets/save
```

Request Body:

```text
{"data":[{"classId":{YOUR_CLASS_ID},"setId":{PUBLIC_SET_ID}}]}Request Body:
```

Now, **modify the classId** to the **ID of the target group** — the one you were **previously in and got kicked out of**.

**Send the request**…**And… yes — it works!** We successfully exploited the vulnerability.

![](https://cdn-images-1.medium.com/max/567/1*RWmzq33x-O4JeGsYtxIG2Q.jpeg)

the vulnerability:

**Bypass of "Add Set" Disable Feature** Yes – the vulnerability **fully bypasses** the admin's setting to disable adding sets. Even when the admin explicitly turns off the "Allow students to add sets" option, the attacker can still add public sets via the API. The added sets appear in the classroom **as if nothing happened**, with no indication of unauthorized access….and **the admin can never prevent this action!**

**Classroom ID (classId) Accessibility** The classId is **trivially obtainable** because it is **exposed directly in the classroom URL**.The key issue is whether the backend validates a user’s permission to modify the specified class (classId) on the server side. If authorization is enforced only in the client (UI) and not rechecked by the API, an attacker can tamper with the request body (e.g., change classId) to perform unauthorized actions.

*In the end, are you waiting for a happy ending…?*

![](https://cdn-images-1.medium.com/max/556/1*zZCnnOkHDLYpHIbIabE0YQ.jpeg)

You’re a **bug bounty hunter**: you hunt for a vulnerability, write a report, submit it, and move on with your life as if nothing happened.

If it gets **accepted** — that’s your **rizq** (provision), and you’ve earned it. If it comes back as **NA or Informative**, even though the bug has real **security impact** — it’s *still* your rizq.

Just **keep striving**, keep searching, and **trust in your Lord** that you *will* receive your provision…

**But you won’t get your rizq if you don’t chase it!**

![الحمدلله](https://cdn-images-1.medium.com/max/1024/1*usneuDEuMBLb_upVaV4pRw.png)
*الحمدلله*

**Final advice…**

If you find a vulnerability, **try applying the same logic** to other parts of the site — wherever the context allows.

With high probability, you’ll discover **more instances of the same vulnerability type** in different places.

Just **keep digging** — you might just find them!

Happy hacking! 🧑‍💻
