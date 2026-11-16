---
title: "Phantom Roles: Surviving Removal with Delete Powers and Bypassing Add Restrictions."
date: 2026-01-06 16:47:52 +0000
categories: [bugbounty writeup]
tags: [bug-bounty, writeup, web-security]
toc: true
---

**بِسْمِ اللَّهِ الرَّحْمٰنِ الرَّحِيمِ، وَالصَّلَاةُ وَالسَّلَامُ عَلَى النَّبِيِّ الْمُجَاهِدِ الشَّهِيدِ…اللَّهُمَّ انصُرْ أَهْلَ غَزَّةَ وَفِلَسْطِينَ وَالسُّودَانَ، وَطَهِّرِ الْأَقْصَى مِنْ دَنَسِ الْيَهُودِ… أَمَّا بَعْدُ**

**Hello everyone, Yosef here again with two simple privilege escalation scenarios. Grab a cup of coffee and let’s get started :)**

![](https://cdn-images-1.medium.com/max/827/1*dWNP3fzpOWLzst3SXDsgOQ.jpeg)

So,** **Bismillah irRahman irRahim**, **let’s begin the story. For some time, I had been hunting at a lower-than-usual pace — sometimes only two hours per week :( — spending the rest of the time casually browsing the internet. One afternoon, I said to myself: why not push a bit harder? So, I selected a target and challenged myself over the course of an entire week to find six vulnerabilities — and I actually did. Let’s start with the first scenario, which represents the simplest privilege escalation you can possibly imagine.

> First Scenario

After some time and extensive experimentation, an idea came to mind: why not test one last hypothesis? I went ahead and tried it — and indeed, when a user is removed from a certain entity, they still retain their privileges. Hmm… I know this might not be clear yet, so let’s first understand the platform. The website is an educational platform that connects teachers and students. For this experiment, the scenario involved two roles: the Owner and the the Co-Teacher (and other roles, but I focused on these two roles). The platform allows teachers to communicate with each other through a built‑in chat feature. At that point, I thought — why not test this functionality more closely? To begin, I created two accounts and ensured that both accounts were enrolled in the same class.

![](https://cdn-images-1.medium.com/max/1024/1*AuixE2wjKHekXFuKfUllgg.png)

From the first account, I removed the second user — and the operation was completed successfully.

![](https://cdn-images-1.medium.com/max/226/1*SZZtz5IBHEGpxlPTbpeswA.png)

This is secure behavior… right? Yes, it is.The removed user should no longer be able to access anything. Let’s try accessing the class URL again — or, more specifically, append /message/{chat_id}.

![](https://cdn-images-1.medium.com/max/450/1*v4VJXqelmSxKGVhdJIXYtA.png)

I observed that the second account retained access to the chat and could actively interact with it, despite having been removed from the class. More critically, the account was able to delete the entire group without being a member

![](https://cdn-images-1.medium.com/max/1024/1*tNy2DSxjqhHBqnD34IdKhQ.png)

The root cause of this issue lies in improper server‑side authorization checks. When a user is removed from a class, the application fails to invalidate or re‑verify the user’s permissions for previously authorized resources, specifically the chat functionality. As a result, endpoints such as /message/{chat_id} rely solely on the existence of a valid session or user ID, without confirming the user’s current membership or role within the class. This allows a removed user to retain unauthorized access and perform privileged actions—such as sending messages or deleting the entire group—despite no longer being an authorized participant.

Unfortunately, this issue was classified as P4 and marked as a duplicate of another report that had already been published since 2023!!

![sad :|](https://cdn-images-1.medium.com/max/769/1*6hREdH6AfamOlFWt2PuxxA.png)
*sad :|*

> Scenario Two: A Simple, Classic Privilege Escalation Case.

Alright, let’s take a step back for a moment. Do you remember when I mentioned that I was focusing on only two roles — the first being the Co‑Teacher, and the second the Owner? Let’s start by identifying the differences between them. A Co‑Teacher can perform almost all the same actions as the Owner, with one exception: they cannot add new Co‑Teachers or remove existing ones. At that point, I decided to put these roles under the microscope and thought to myself — let’s try breaking the rules.

I initially tried to remove another user-from co -teacher-, but the application correctly rejected that action — which was expected. So, I moved on to the second vulnerability and asked an important question: can a Co‑Teacher add other Co‑Teachers? To answer this, I first needed to understand how users are added. Using the Owner account, I added a third user. The process was straightforward: the Owner enters the email address → the invited user receives the invitation and completes the process. Perfect. I then took detailed notes on the endpoint, captured the request, and saved it in Notion. Next, while logged in as a Co‑Teacher, I refreshed the page, intercepted the request, and sent it to repeater. From there, I modified several parameters — most notably the endpoint itself — and replaced the email address with that of a completely different, fourth user.

![yeah we made it ✌](https://cdn-images-1.medium.com/max/1024/1*d_W-Vvi8laPLzUH7CcL5xQ.png)
*yeah we made it ✌*

As shown in the attached image, the application accepted the request from the Co‑Teacher — despite the fact that this role is not permitted to add other users — and successfully sent the invitation-**Classic Server-Side Authorization Bypass-**.To further validate the issue, let’s check the email and confirm who actually sent the invitation.

![From the tested account](https://cdn-images-1.medium.com/max/1024/1*_SChySWX1hBL1X1uwrwW6w.png)
*From the tested account*

Well, we did it man….we did it. Unfortunately, the journey ended with multiple duplicates. Still, the challenge continues until I land a bounty, so stay tuned for more scenarios to come.

The warrior within me remains relentless — constantly pushing harder and refusing to stop until the objective is achieved.

![](https://cdn-images-1.medium.com/max/736/1*P0umkWhxldyc3_p8e5wa4w.jpeg)

Keep Hacking and see u in next writeup

ولا تنسى الدعاء لأخواتنا في فلسطين والسودان والإيغور…والسلام عليكم
