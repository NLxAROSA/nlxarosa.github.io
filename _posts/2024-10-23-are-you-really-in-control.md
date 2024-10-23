---
date:   2024-10-23 08:00:00 +0200
image:  /img/dependency.png
tags:   [DevOps]
---

## So you don't deploy on Friday. But does the same apply to all your dependencies?

No deploys, no problems, right? This may be true if your applications run in isolation in an environment fully under your control, but in my experience this is rarely the case in modern enterprise software development. Are you really in control as much as you think you are? Let's check it out!

## Are you really in full control?

Are you deploying on a 3rd party cloud service, such as AWS, Azure or Google Cloud? Are you using 3rd party services in your application(s)? We are often depending on many 3rd party services, including (but certainly not limited to) hosting, DNS, databases, message queues, container runtimes, observability, logging, paging, Generative AI and many, many more. Many of them may be part of your own company/organisation, which may give you some control (but typically not full), but there may be just as many, or more, that you consume from a 3rd party, for which you will have no control at all.

When it comes to 3rd party dependencies, you don't know when they'll deploy, regardless of the day and time. Even if none of them deploy on a Friday, things can still break or go down for reasons not known to you at this point in time. Things break.

The providers of the services you consume know this: check their SLAs if you don't believe me, but you too will learn that exactly zero of them will guarantee a 100% availability. So the question is not íf downtime will occur, but when. Even when you yourself are not deploying anything.

## Deploy on Friday or not deploy on Friday, that's not really the question. ;)

Because you do not control any of the 3rd party services you depend on, think about what would happen if one of them would go down. On a Friday, or any other day of the week. Will it cause massive downtime for your application(s)? Can you switch/failover/mitigate quickly? How much impact is there on revenue if it does happen? Will you even know right away, or not until the contact centers are overloaded by a barrage of angry customers? 

Not all dependencies are created equally or will have business/revenue impact when they become unavailable. Note that none of this depends on which day of the week it happens to be. ;)

The reality is that many of the things that help deploying on Friday with confidence, you should also be doing when deploying on any other day of the week, and even when you're not deploying at all. You will always need observability, resilience, failover, fast builds/deployments/rollbacks, etc. etc. regardless of the day of the week and regardless of whether you are deploying something yourself or not. Once those are in place, being able to deploy on a Friday is just a bonus.

## 'Deploy on Friday!' is just the hook

This blog is called 'Deploy on Friday!', but it's really just a hook. It's not (just) about the Friday at all, but it's about any day of the week. As a matter of fact, it's not always about you doing any deploying at all!

Design your systems to be observable and resilient (esp. to outside changes) and they will be able to weather a storm whenever it may occur. Which is typically on the moment that's the least convenient to you. Implementing better practices is hard and difficult work, but you can do this at a time of your choosing. Outages pick the time for you, especially those in dependencies beyond your control!

Of course a post such as this one is completely useless without some actual practical examples. I'll get to that, soon (-ish)!