---
title: "The Illusion of Instant Block"
date: 2026-09-25 14:16:36 +0000
categories: [bugbounty writeup]
tags: [bug-bounty, writeup, web-security]
image:
  path: "https://cdn-images-1.medium.com/max/736/1*ld-BvwaYMC7tqLcEYRmKCg.png"
  alt: "The Illusion of Instant Block"
description: "بِسْمِ اللَّهِ الرَّحْمٰنِ الرَّحِيمِ، وَالصَّلَاةُ وَالسَّلَامُ عَلَى النَّبِيِّ الْمُجَاهِدِ الشَّهِيدِ…اللَّهُمَّ انصُرْ أَهْلَ غَزَّةَ وَفِلَسْطِينَ وَالسُّودَانَ، وَطَهِّرِ الْأَقْصَى مِنْ دَنَسِ الْيَهُودِ… أَمَّا بَعْدُ"
toc: true
---

بِسْمِ اللَّهِ الرَّحْمٰنِ الرَّحِيمِ، وَالصَّلَاةُ وَالسَّلَامُ عَلَى النَّبِيِّ الْمُجَاهِدِ الشَّهِيدِ…اللَّهُمَّ انصُرْ أَهْلَ غَزَّةَ وَفِلَسْطِينَ وَالسُّودَانَ، وَطَهِّرِ الْأَقْصَى مِنْ دَنَسِ الْيَهُودِ… أَمَّا بَعْدُ

Hey folks, I’m back after quite a long hiatus… and honestly, it’s good to be back. Oh man, I’d almost forgotten how to write writeups :’(

![Very sad :’(](https://cdn-images-1.medium.com/max/736/1*ld-BvwaYMC7tqLcEYRmKCg.png)
*Very sad :’(*

Anyway, enough of that. As usual, on just another random day, I was doing what I love — putting security systems to the test — and I stumbled on a vulnerability built on a familiar idea, just executed differently.

**When you get someone deactivated, are you sure that’s really the end of it?**

As usual, you test any given feature by first understanding how it works, then trying to break it by any means possible. In this case, the idea was that the site offered a feature letting you deactivate any user — even if that user was an Admin.

![User](https://cdn-images-1.medium.com/max/1024/1*bnUHDVk3bEZfZT8uRBRbXA.png)
*User*

> But is that claim actually true?

Take a closer look at the second user’s account, and you’ll find that it was indeed blocked — but the site never invalidated the old sessions. They stayed alive and kept communicating with the server as if nothing had changed.

I found that the existing session remained valid and retained the account’s pre-deactivation privileges. To prove the vulnerability — and the site made this embarrassingly easy — I changed the account’s name as a simple proof of concept, and even went ahead and blocked the other users, evend delete other admins !!…all from that same “deactivated” account :)

![Change name or anything to proof..](https://cdn-images-1.medium.com/max/1024/1*8i-mR9ALeZqkWagqgs9dMw.png)
*Change name or anything to proof..*

Digging into it further, the root cause -i think- The application validated the account status during authentication, but failed to revoke existing sessions or enforce the blocked state on subsequent requests.

The triager was about to close it as Out of Scope, but I walked him through the actual impact :)

![](https://cdn-images-1.medium.com/max/342/1*Hqz7EVnPH0al3weBGGWEYg.png)

![](https://cdn-images-1.medium.com/max/332/1*lHejTaVOJnUlxt-iAtxa6g.png)

And thankfully, it was accepted 😁

![الحمدلله](https://cdn-images-1.medium.com/max/343/1*neHxaUEGw_T5egIyDE8asQ.png)
*الحمدلله*

**Scenario 2-Trust nothing that comes from the user. And if you do… you’ve already been fooled !**

![The Account of testing](https://cdn-images-1.medium.com/max/1024/1*VMxWzdfBwRAxco2pHOiqrA.png)
*The Account of testing*

So I dug a bit deeper, going back through the old requests, and one of them caught my attention. While reproducing the original vulnerability — following the same steps as before: the user gets blocked, and from there we check whether they can still interact with the system, and whether they can still change anything while blocked — I noticed that when I changed the name, the response came back with “status”: “BLOCKED”

I thought — why not take this parameter and throw it back into the request that edits any profile field? And to my surprise, it actually worked! By manually injecting that parameter into the request body, I was able to modify the status — a textbook Mass Assignment vulnerability.

![](https://cdn-images-1.medium.com/max/1024/1*l0Q7y_5xKpFoG6lITBo27w.png)

And sure enough, looking at it from the admin’s side, you can actually see that the user lifted the block off themselves.

![](https://cdn-images-1.medium.com/max/1024/1*dgYsWaOzm2lnKLbdDSJtZw.png)

Naturally, the next question was: if “status”was injectable this way, what about “permissions”? I tried the exact same approach — grabbing the permissions field from a legitimate response and replaying it in the profile-update request — but this one was properly validated server-side and got rejected. So the mass assignment issue here was scoped specifically to the status field, not a full privilege-escalation path.

This isn’t just a passing session-management issue, or some minor edge-case tweak. It strikes at the very structure of your organization on your SaaS platform — a user being able to remove or delete assets, remove members, or block them outright, all while you’re supposed to be confident that the assumption you built your system on can’t be broken. The door was never locked in the first place for it to be “broken.” The door was already wide open. And in other cases, a copy of that very door was sitting in the hands of the person it was supposedly locked against.

I hope these simple scenarios gave you something useful. And in closing — don’t forget to keep our Uyghur brothers and sisters, the people of Gaza, and oppressed Muslims everywhere in your prayers. Wassalamu alaikum.

![](https://cdn-images-1.medium.com/max/736/1*e1vJ0ughE6xxie1W6qb9Dw.png)

U can find me on:

[www.linkedin.com/in/yosef-mostef](http://www.linkedin.com/in/yosef-mostef)

[https://t.me/yosefmostef](https://t.me/yosefmostef)
