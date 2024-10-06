---
date:   2024-10-06 08:00:00 +0200
image:  /img/fear.png
tags:   [DevOps]
---

## Why are people afraid to deploy on Friday?

Before wondering how we address the problem of people being afraid to deploy on Fridays (or any other day of the week for that matter), we should first understand just WHY people are afraid. Can't fix what you don't understand. And the 'never deploy on Friday' statement is not just a blanket statement, it's also just a patch to fight a symptom, not actually addressing the root causes(s) that are underneath. And there are quite a few underlying issues, so let's start with the most important one and work from there, shall we?

## The ideal situation

You are called at 3am on a Sunday morning for a production issue. You boot up your laptop, the metrics tell you what is the problem. You build and deploy a fix/mitigation, or roll back to a previous version, which only takes a few minutes. The problem is now fixed or the severity has been lowered to the point it can now be investigated during business hours when everybody is available instead of during off-hours. You go back to bed.

## Reality

You are called at 3am on a Sunday morning for a production issue. You don't have metrics. You are not allowed to take any decisions on your own. You end up in a 12-hour long conference call with 50 people across 10 different departments and 3 different continents, just to agree on deploying a one-line configuration change. There is no post-mortem and nothing is changed structurally, so you already know this will happen again next week, and the week after, and so on. Every time you or your team are blamed for things that are beyond your control. Your teams backlog consists mostly of fixing issues rather than innovation.

If the above paragraph sounds familiar: continue reading!


## The problem(s)

There is actually not one single problem here, but several underling issues that intertwine. But there's a specific one that, when addressed, let's you control all the other ones, so let's focus on this one first. And that issues is: not having a mandate to actually decide anything.

Many organizations are adamant about 'accountability' and 'responsibility' without giving their people the mandate to actually influence the outcomes they are made responsible for. It makes no sense, yet it happens all the time. And not just in a DevOps context either, but overall. Poor leadership (if you can even call it that) that wants to maintain control and take credit for successes while dumping accountability and responsibility on their teams so they have a scapegoat in case of failures. So you end up with a Personal Development Plan (PDP) with items you don't control, or on-call without the tools and data to actually fix something.

## A different approach

I have been on-call in the past and was of course also confronted with these situations. The solutution I applied was to make sure that whenever I did not call the shots, I made sure to demand that the person that wás calling the shots (typically a product manager or owner) was on-call with me, explaining that not doing so would impact business continuity and revenue due to issues dragging on longer than necessary (effectively putting the ball in their court).

Fun fact: The good ones usually agree! And that is fine with me, since it's all about team work anyway, and as long as it's a small team that can move quickly, that's perfectly fine. The bad ones didn't want to be on call, but also did not like being assigned the blame either, so I typically got the mandate. So I always end up in a situation where I am either in control, or have the person in control at my side.

Similarly, don't put items in your PDP that you don't control. If your manager still wants you to put those items in then demand a mandate from them, or make sure there's a conditional explicitly writing out the dependency for that specific goal. That way, when the dependency in question 'forgets' it or otherwise doesn't deliver, you're no longer on the hook.

Want me to be responsible for something? No problem, happy to take it, but then I get to call the shots, and if not, then the person who ís in control is pairing up with me.

## Summary

1. Make sure the person that makes the decisions is on-call with the person doing the work.
2. They can be the same individual.
3. Put a spotlight on where the actual problem is, but give them a graceful way out, or even make them the hero.
4. In general: Never commit to tasks that are beyond your control.
5. Make dependencies explicit and conditional.


Note that I've only pointed out one of the underlying issues in this post, I'll get to the other ones in future post(s). I also fully understand that in real life, situations are not exactly binary, so some times you may have to accept a situation where you're not in control. But now you're at least aware of it and can put some mitigations in place, or think about how you're going to frame it in case a problem pops up. Fixing it is the best option, but being prepared is a good second.