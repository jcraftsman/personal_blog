---
title: "Ddd Sagas"
date: 2020-02-23T11:33:27+01:00
draft: true
---

Long-Running Processes, aka Sagas
The synthetic Pipes and Filters example can be extended to demonstrate another Event-Driven, distributed, parallel processing pattern, namely, Long-Running Processes. A Long-Running Process is sometimes called a Saga, but depending on your background that name may collide with a preexisting pattern. An early description of Sagas is presented in [Garcia-Molina & Salem]. In an attempt to avoid confusion and ambiguity, I have chosen to use the name Long-Running Process, and sometimes I use the name Process for brevity.

Cowboy Logic

LB: “Dallas and Dynasty, now those are what I call sagas!”

AJ: “For all you German readers, y’all know Dynasty as Der Denver Clan.”

Extending the previous example, we could create parallel ...

- <https://www.simpleorientedarchitecture.com/storing-the-state-of-a-long-running-process/>
- <https://fr.slideshare.net/BerndRuecker/long-running-processes-in-ddd>
- <https://github.com/berndruecker/flowing-retail>
- <https://softwareengineering.stackexchange.com/questions/332942/how-to-implement-a-process-manager-in-event-sourcing>
- <https://abdullin.com/post/ddd-evolving-business-processes-a-la-lokad/>

- <https://driggl.com/blog/a/process-manager-vs-saga-confusion>
- <https://www.enterpriseintegrationpatterns.com/patterns/messaging/ProcessManager.html>
- <https://stackoverflow.com/questions/15528015/what-is-the-difference-between-a-saga-a-process-manager-and-a-document-based-ap>
- <http://vasters.com/archive/Sagas.html>
- <http://web.archive.org/web/20161205130022/http://kellabyte.com:80/2012/05/30/clarifying-the-saga-pattern>
- <https://microservices.io/patterns/data/saga.html>
- <https://blog.bernd-ruecker.com/what-are-long-running-processes-b3ee769f0a27>
- <https://blog.bernd-ruecker.com/zeebe-loves-kafka-d82516030f99>
