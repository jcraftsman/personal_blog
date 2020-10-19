---
title: "Event Modeling CQRS ES"
date: 2020-01-28T10:18:24+01:00
draft: true
---

## Notes

- Software delivery predictability through event storming and event sourcing.
- Documentation automatically generated
- Tests are human readable
- The main challenge: What the software should do!
- Group by command or group by aggregate
- Business people can subscribe to automatic documentation genereated after each commit
- simple.testing
- Frameworks gives you early speed in the cost of slowness later on
- Hardest part is not about learning ES/CQRS... The hardest part is about unlearning. Unlearning Normalisation, SQL...
- Move system thinking through time
- Cannot hide things inside big procedures
- Remove the coupling
- Multiple read models
- Save commands as well
- Never replay commands
- Replay projections all the time => 4h of building a read model may not be a problem => Replaying all the time makes you good at it (You will know all risks and how to fix problems quickly)
- In case of hotfix => Replay only one projection
- You should be good at replaying
- Move towards Immutable infrastructure
- Replay the whole environment after each release

> OOP pitfall: Look for abstractions that mimic the real world

> Event Sourcing doesn't apply to all software. However, ES apply to all information system

> Event Storming is a collaborative modeling workshop. An approach really close to implementation

- Don't use ES when it's just CRUD.
- talk to business: Look how many records we need to maintain when it comes to our online system. => All day work to operate vs archive => CorrelationId
- Think of double dispatch (produce multiple event)

## DDD

### Ubiquitous language

- Break silos and islands.
- Make sure to focus on the right thing to build.
- Have a common language to understand each other

> Ma blonde avec les cheveux bruns (Au Quebec, blonde peut dire copine)

## Event modeling

Event modeling: Map of how information change over time.

Goals:

- Explain the business story to be more able to communicate across.
- Check information completness: Be able to tell that this information in this report is related to this thing that happened before (event)
- Identify pre conditions and post-conditions

## Event sourcing

- Whatever is the business question we can answer it with ES. It may take a while, but we will be able to answer it


- Event sourcing isn't new. Before hard drive, ther was tapes. Aren't tapes event logs?
- ES is inherently functional
- Not only derives state at current time, but actually be able to tell what is the system state at point of time. What did the user sees when he mades this decision => Reproduce production state in another environment.
- Reproduce the system at any point of time => Reproducing bug very quickly
- The ability to look at the chain of causality. (Identify correlation causation between events)
- Things doesn't just happen. => How did I get here?

How to fail ES:

1. Number 1 reason: **Versionning** (=> Read <https://leanpub.com/esversioning/read>)
1. Lack of the bounding. How far is your logs can go. Closing the box (Don't replay hundereds of terrabytes of history) => Archiving?

**Rule 1: Immutability**. Once written, never change it! => It makes things simpler!

How do you scale immutable data? => You copy it!

Some useful metrics we can get when using Event Sourcing:

- I want to know how many Use cases are actually used in production today
- What percentage of Use cases ran out today in production have unit tests associated with it?

Complexity increase:

- Traditional system with 1 canonical model
VS
- Low/Loosely coupeled system with multiple domain specific models

## CQRS

- Without cqrs: A web service querying data from db transforming it to json
- With CQRS: nginx serving json files.
- Always Multiple read models
  - 1st Normal form db: 1 table => Open to excel
  - In Memory
    - Try to identify in Memory read model. => e.g. data that were accessed recently => Garebage collection : Let data live untill running out of space and then drop old data
  - Files

- Event centric organisations (Finanance gambling)
- Example of events:
  - Market Closed
  - Order opened
  - Order paid
- Interface of a read model = Events
  - Can be outsourced more easily. => Outsource team doesn't have to know about my model
  - Key integration point

Read Model = Cache
Every read model is a cache that happeneed to be useful

- Rebuild the read model every release => Validating a read model
- ZDD ready => Deploy new read model next to old one then move the traffic to the new
- A read model is trivial but boaring NOT FUN ... BUT TOO MUCH VALUE COME FROM THEM

- Always save both events and commands (with correlation causation ids)

### The pattern

#### Problem

#### Solution

#### Example

#### Pre-requisites

#### When to use

#### When not to use

### Snapshots

Challenge: software version.

Goal => Speed things up. (Avoid replaying Trillions of events each time)

Cost of a snpashot:

- Creating ones
- Versionning
- Watching snapshots (complexity added)

Avoid using snapshot. => Think twice before using them

Prefer break up streams instead of using snapshots. => Close the box

Memoization of the fold

Serialized form of domain object => Memento

If you have to use snaphot, don't serialize directly domain objects. => Serialize only it's **Memento**

Short streams

Composition of streams becomes trivial (join...)

When using snapshots, you will need a lot of memoization

archiving. After closing the box remove past events from hot storage to cold storage

When using multiple streams, consider using snapshop event at the start of new stream.

### Events versionning

Challenges

- Running different versions of software side by side
- It's the old software understanding the new one which is a problem

#### Use of conversionning

```java
data class InventoryItemDeactivated(val itemId: UUID, val whoId: UUID) : Event

data class InventoryItemDeactivatedV2(val itemId: UUID, val whoId: UUID, val reason: String) : Event

val convert: (InventoryItemDeactivated) -> InventoryItemDeactivatedV2 = { v1 ->
    InventoryItemDeactivatedV2(v1.itemId, v1.whoId, "")
}

val convertBackWard: (InventoryItemDeactivatedV2) -> InventoryItemDeactivated = { v2 ->
    InventoryItemDeactivated(v2.itemId, v2.whoId)
}
```

+ Use of Accept Header (version:vX) over an atom feed

### Event sourcing with Legacy systems

## Patterns

### Saga

> Greg recommend using saga over process manager in most cases

#### Problem

#### Solution

#### Example

#### Pre-requisites

#### When to use

#### When not to use


### Process Manager

#### Problem

#### Solution

#### Example

#### Pre-requisites

#### When to use

#### When not to use

## Greg talk at DDD meetup

### TL;DR

Make URI immutable => Make advantage of all already available caching, (NGINX, CDN and rainbows) => High performance and scalability guaranteed.

One URI = One content

### Atom feeds

### Hateos

## Challenges

- Domain experts that don't want you to know what they are doing (beacause of competition, e.g. traders...) => They want to lose their competitive advantage => In this case, just provide data to people and let them do what they want with it
- It's fun to work with a client that doesn't want to tell you what he wants but expects you to deliver it


## Event store

Pull the Docker image:

- <https://hub.docker.com/r/eventstore/eventstore/>

```bash
docker pull eventstore/eventstore
```

Run the container:

```bash
docker run --name eventstore-node -it -p 2113:2113 -p 1113:1113 eventstore/eventstore
```

Admin UI
<http://localhost:2113/web/index.html#/>


Persistence subscriptions...

### Clients

- <https://github.com/msemys/esjc>
- <https://github.com/EventStore/EventStore.JVM>

## Resources

- <https://eventmodeling.org/posts/what-is-event-modeling/>
- <https://github.com/HEIWA-IT/EventModelingParisWorkshop>
- <http://cqrs.wikidot.com/doc:specification>
- <https://qualityisspeed.blogspot.com/2014/09/beyond-solid-dependency-elimination.html>
- <http://wiki.c2.com/?PrimitiveObsession>
- <https://github.com/gregoryyoung/Simple.Testing>
- <https://github.com/networknt/light-eventuate-4j/blob/master/README.md>
- <https://certbot.eff.org/>
- <https://vlingo.io/>
- <https://github.com/vlingo>
- Akka actor / model <https://doc.akka.io/docs/akka/2.5.3/scala/stream/stream-integrations.html#integrating-with-reactive-streams>
- <https://en.wikipedia.org/wiki/Double_dispatch>
- <https://fr.slideshare.net/gschmutz/kafka-as-an-event-store-is-it-good-enough>


- <https://blog.engineering.publicissapient.fr/2017/01/16/event-sourcing-comprendre-les-bases-dun-systeme-evenementiel/>

- <https://www.youtube.com/watch?v=58_Ehl3oETY>
- <https://zimarev.com/>
- <https://www.youtube.com/watch?v=t2AI9hODJ2E>


- [GOTO 2018 • Event-based Architecture and Implementations with Kafka and Atom • Eberhard Wolff](https://www.youtube.com/watch?v=Ecg7lvvm8aU&t=1190s) :
  - Get the split right
  - Bounded Context
  - Communicate via business events
  - Inconsistencies can be dealt with
  - What is an event?
  - Don't share events for event sourcing!
  - Do not overuse event sourcing
  - Kafka: Great solution for messaging

- [DDD & Event Sourcing à la rescousse pour implémenter la RGPD ? (G. Lours, J. Avoustin)](https://www.youtube.com/watch?v=RK4mrpro2B4&t=2s)

- <https://verraes.net/2014/03/practical-event-sourcing/>