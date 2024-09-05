---
date:   2024-09-05 22:00:00 +0200
---

## Blog Roadmap

<img align="left" src="/img/roadmap.png" width="64"/>A blog is worthless if it doesn't have a decent amount of quality content. My previous blog died after a handful of ideas. While that certainly may happen again in the future, I've got a good pipeline of incoming articles at the ready, on a variety of topics. So let's take a closer look, shall we?!

### Deploy on Friday
Deploy on Friday is the OG series of articles this blog started with all the way back in 2018 and I intend to run a series of articles on how to make deploying to production a whole lot less scary; on any day of the week, not just Friday. I'll go over the following sub-topics:

* Observability
* Graceful degradation
* Resilience
* Error budgets
* Testing: contract, forward and backward compatibility, rollback testing
* Fast feedback loops
* Team best practices

### Java/Spring
As part of brushing up my own skills on the latest and greatest that Java has to offer I found quite a few interesting things that keep things simple and make your life a lot easier in general. Keeping apps simple and small, and using language features to your advantage where possible. Going to start out with the items below and pretty sure there will be more to follow.

* Spring JDBC + Java Records: yes, that works, and it's super simple too.
* Using Generics and Type Erasure to design client libraries: keeping it simple for your consumers
* Building fast applications using [Valkey](https://valkey.io/).

### Zoom APIs and SDKs
Zoom has a lot of cool features for developers that want to do more with Zoom products and features. [Meeting SDK](https://developers.zoom.us/docs/meeting-sdk/), [Video SDK](https://developers.zoom.us/docs/video-sdk/), [APIs](https://developers.zoom.us/docs/api/) and much more. This goes way beyond just meetings and I'll show you what cool stuff you can do!

* Using Zoom APIs and generating JWT tokens ([Sample app for Video SDK](https://github.com/NLxAROSA/zoom-vdsk-auth-endpoint))
* Using Java + Spring Boot to schedule meetings and sessions (sample app incoming very soon)
* Using JavaScript to run a Zoom Meeting or Zoom Video SDK session in your own web app ([Sample app](https://github.com/zoom/videosdk-ui-toolkit-javascript-sample))
* Using Zoom for video applications that are not meetings.

### Generative AI and LLMs
Oh boy, I posted the A-word. ;) But fear not, I will show you exactly which parts are useful and which parts are BS. Let's start by taking back control of your precious data and run LLMs locally and build apps that can work with them. No definitive list of topics yet, but I have done a few talks on this already and have some nice example apps. May involve Python. ;)

### Final notes
I have no interest in writing blog posts with dozens of pages and apps with hundreds of lines of code. I assume you don't have the time to go over those either. So the posts and examples will be very specific (small), useful and to the point. Less is more! And last, but not least: articles will appear in random order, but will be tagged/categorized. Happy reading!