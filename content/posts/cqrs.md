---
title: "Autopsy of a CQRS implementation"
date: 2019-12-30T16:27:16+01:00
draft: true
---

Command Query Responsibility Segregation pattern

## The problem

5% of the time saving data and 95% of the time reading data.

- Complex domains
- Separate load 
- <https://community.risingstack.com/when-to-use-cqrs/>
- <https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs>

## The solution

Two different models :
Command model (aka write model): optimized for making decisions. Changing state and saving.
Query model(s) (aka read model): Optimized for read and access to 
Command model and Query model
I prefer to use command model instead of "Read model" because we do both read and write in command side.
The same way we both read and write in the query side.

## Example

### Command model

Typical anatomy of a commandHandler:

1. repository.get(aggregateId)-> aggregate
2. aggregate.process(command) -> commandResponse
   > *A CommandResponse usually wraps output events + an ACK*
3. repository.save(aggregate)
4. return commandResponse

```java
public class MakeReservationCommandHandler implements CommandHandler<CommandResponse<ReservationId>, MakeReservationCommand> {
    private final RoomRepository roomRepository;

    MakeReservationCommandHandler(RoomRepository roomRepository) {
        this.roomRepository = roomRepository;
    }

    public CommandResponse<ReservationId> handle(MakeReservationCommand makeReservationCommand) {
        var room = roomRepository.get(new RoomId(makeReservationCommand.getRoomNumber()));
        var commandResponse = room.makeReservation(makeReservationCommand);
        this.roomRepository.save(room);
        return commandResponse;
    }

    @Override
    public Class<? extends Command> listenTo() {
        return MakeReservationCommand.class;
    }
}
```

### Query model(s)

projection
- <https://buildplease.com/pages/fpc-20/>

Pro tips:
- Rebuild the view model after each release

- <http://udidahan.com/2009/12/09/clarified-cqrs/>

### Challenges

REPlAY

## When not to use it

It's hard to answer this question.
When aske this question to Greg Young, the answer was: Whenever I hear a CRUD requirement.

Well, at the moment this answer did not help me a lot. I have another heuristic whenever I hear crud, I try to scratch the underlying domain.

But what Greg's answer...
CQRS was atypical years ago. Now, it becomes mainstream.


By Martin Fowler 2011 <https://martinfowler.com/bliki/CQRS.html>:

Like any pattern, CQRS is useful in some places, but not in others. Many systems do fit a CRUD mental model, and so should be done in that style. CQRS is a significant mental leap for all concerned, so shouldn't be tackled unless the benefit is worth the jump. While I have come across successful uses of CQRS, so far the majority of cases I've run into have not been so good, with CQRS seen as a significant force for getting a software system into serious difficulties.

In particular CQRS should only be used on specific portions of a system (a BoundedContext in DDD lingo) and not the system as a whole. In this way of thinking, each Bounded Context needs its own decisions on how it should be modeled.

So far I see benefits in two directions. Firstly is that a few complex domains may be easier to tackle by using CQRS. I must stress, however, that such suitability for CQRS is very much the minority case. Usually there's enough overlap between the command and query sides that sharing a model is easier. Using CQRS on a domain that doesn't match it will add complexity, thus reducing productivity and increasing risk.

The other main benefit is in handling high performance applications. CQRS allows you to separate the load from reads and writes allowing you to scale each independently. If your application sees a big disparity between reads and writes this is very handy. Even without that, you can apply different optimization strategies to the two sides. An example of this is using different database access techniques for read and update.

If your domain isn't suited to CQRS, but you have demanding queries that add complexity or performance problems, remember that you can still use a ReportingDatabase. CQRS uses a separate model for all queries. With a reporting database you still use your main system for most queries, but offload the more demanding ones to the reporting database.

Despite these benefits, you should be very cautious about using CQRS. Many information systems fit well with the notion of an information base that is updated in the same way that it's read, adding CQRS to such a system can add significant complexity. I've certainly seen cases where it's made a significant drag on productivity, adding an unwarranted amount of risk to the project, even in the hands of a capable team. So while CQRS is a pattern that's good to have in the toolbox, beware that it is difficult to use well and you can easily chop off important bits if you mishandle it.

Back in the days, it was an exotic fashion:

- <https://martinfowler.com/bliki/CQRS.html>

Now, it's not the case any 
more

- <https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf>
- <https://medium.com/@drozzy/long-running-processes-event-sourcing-cqrs-c87fbb2ca644>

- <http://udidahan.com/2011/04/22/when-to-avoid-cqrs/>
- <https://microservices.io/patterns/data/cqrs.html>
- <https://medium.com/tiller-systems/pourquoi-avoir-choisi-dutiliser-l-architecture-cqrs-e04c082f8b5f>

- <https://github.com/eventuate-tram/eventuate-tram-examples-customers-and-orders-redis>
- <https://github.com/eventuate-local/eventuate-local>
- <http://eventuate.io/exampleapps.html>


- <https://www.infoq.com/news/2015/10/cqrs-read-models-persistence/>
- <https://enterprisecraftsmanship.com/posts/types-of-cqrs/>
- <http://squirrel.pl/blog/2015/09/22/writing-an-event-sourced-cqrs-read-model/>


RDBMS for Event store: 
- <https://stackoverflow.com/questions/7065045/using-an-rdbms-as-event-sourcing-storage>
- <https://dzone.com/articles/achieving-consistency-in-cqrs-with-linear-event-st>
- <http://scottlobdell.me/2017/01/practical-implementation-event-sourcing-mysql/>

Event driven pitfalls:
- <https://www.infoq.com/presentations/event-driven-benefits-pitfalls/?itm_campaign=rightbar_v2&itm_source=infoq&itm_medium=presentations_link&itm_content=link_text>