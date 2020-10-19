---
title: "Command Handler"
date: 2020-01-29T13:52:56+01:00
draft: true
---


## Procedural Application Service

```java
public class DeactivateInventoryItemApplicationService {
    private InventoryItemRepository repo;
    private CanLookupLicence lookup;

    public DeactivateInventoryItemApplicationService(InventoryItemRepository repo,
    private CanLookupLicence lookup){
        this.repo=repo;
        this.lookup=lookup;
    }
    public void deactivate(UUId id, String reason){
        var item = repo.getById(id);
        if (!item.isDeactivatable) throw new FooException();
        if (!item.isOtherPrecondition()throw new FooException();
        item.activated= false;
        item.deactivationDate=DateTime.now();
        repo.save(item);
    }
}
```

## OO Application Service

```java
public class DeactivateInventoryItemApplicationService {
    private InventoryItemRepository repo;
    private CanLookupLicence lookup;

    public DeactivateInventoryItemApplicationService(InventoryItemRepository repo,
    private CanLookupLicence lookup){
        this.repo=repo;
        this.lookup=lookup;
    }
    public void deactivate(UUId id, String reason){
        var item = repo.getById(id);
        item.deactivate(reason);
        repo.save(item);
    }
}
```

## CommandHandler

Refactor: introduce parameter object

```java

public class DeactivateInventoryItemCommandHandler {
    private InventoryItemRepository repo;
    private CanLookupLicence lookup;

    public DeactivateInventoryItemCommandHandler(InventoryItemRepository repo,
                                                 CanLookupLicence lookup) {
        this.repo = repo;
        this.lookup = lookup;
    }

    public void deactivate(Deactivate deactivate) {
        var item = repo.getById(deactivate.getId());
        item.deactivate(deactivate.getReason());
        repo.save(item);
    }
}

public class Deactivate implements Command {
    private final UUID id;
    private final String reason;

    public Deactivate(UUID id, String reason) {
        this.id = id;
        this.reason = reason;
    }

    public UUID getId() {
        return id;
    }

    public String getReason() {
        return reason;
    }
}

```
rename method
```java
public class DeactivateInventoryItemCommandHandler {
    private InventoryItemRepository repo;
    private CanLookupLicence lookup;

    public DeactivateInventoryItemCommandHandler(InventoryItemRepository repo,
                                                 CanLookupLicence lookup) {
        this.repo = repo;
        this.lookup = lookup;
    }

    public void handle(Deactivate deactivate) {
        var item = repo.getById(deactivate.getId());
        item.deactivate(deactivate.getReason());
        repo.save(item);
    }
}
```

Extract interface Handle

```java
public class DeactivateInventoryItemCommandHandler implements Handles<Deactivate> {
    private InventoryItemRepository repo;
    private CanLookupLicence lookup;

    public DeactivateInventoryItemCommandHandler(InventoryItemRepository repo,
                                                 CanLookupLicence lookup) {
        this.repo = repo;
        this.lookup = lookup;
    }

    @Override
    public void handle(Deactivate deactivate) {
        var item = repo.getById(deactivate.getId());
        item.deactivate(deactivate.getReason());
        repo.save(item);
    }

}

public interface Handles<T extends Command> {
    void handle(T command);
}
```

## Chaining handlers

```java

public class LoggingHandler<T extends Command> implements Handles<T> {


    private LoggingHandler<T> next;

    public LoggingHandler(LoggingHandler<T> next) {
        this.next = next;
    }

    @Override
    public void handle(T command) {
        Logger.getLogger(this.getClass().getName()).info("Processing Command: " + command.toString());
        this.next.handle(command);
    }
}

```

### Functional style

```java
```

```java

    @Test
    fun `item should become deactivated`() {
        val repository = InventoryItemSQLRepository()
        val item = repository.getById(UUID.randomUUID())
        item?.deactivate("Reason of deactivation")
        assertThat(item!!.isActivated).isFalse()
    }
```

- Don't write framework for this
- Don't use big frameworks and tools and then use less than 5% of it


- Where to put login? => Not here
