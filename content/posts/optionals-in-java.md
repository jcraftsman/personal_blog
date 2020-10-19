---
title: "Optionals in Java"
date: 2020-02-19T09:08:13+01:00
draft: true
---

## TL;DR

- ✅ Local variable
- ✅ Method return type
- ❌ Field
- ❌ Method parameter
- ❌ With a container: `Optional<List<MyType>>` => `List<MyType>`
- ❌ Assign Null to an Optional Variable

<https://stackoverflow.com/questions/26327957/should-java-8-getters-return-optional-type?answertab=votes#tab-top>
> Of course, people will do what they want. But we did have a clear intention when adding this feature, and it was not to be a general purpose Maybe type, as much as many people would have liked us to do so. Our intention was to provide a limited mechanism for library method return types where there needed to be a clear way to represent "no result", and using null for such was overwhelmingly likely to cause errors.
>
> For example, you probably should never use it for something that returns an array of results, or a list of results; instead return an empty array or list. You should almost never use it as a field of something or a method parameter.
>
> I think routinely using it as a return value for getters would definitely be over-use.
>
> There's nothing wrong with Optional that it should be avoided, it's just not what many people wish it were, and accordingly we were fairly concerned about the risk of zealous over-use.
>
> (Public service announcement: NEVER call Optional.get unless you can prove it will never be null; instead use one of the safe methods like orElse or ifPresent. In retrospect, we should have called get something like getOrElseThrowNoSuchElementException or something that made it far clearer that this was a highly dangerous method that undermined the whole purpose of Optional in the first place. Lesson learned. (UPDATE: Java 10 has Optional.orElseThrow(), which is semantically equivalent to get(), but whose name is more appropriate.))

La classe Optional est à utiliser avec les retours de méthode, d’après Brian Goetz, créateur de cette classe (source) :
you probably should never use it for something that returns an array of results, or a list of results; instead return an empty array or list. You should almost never use it as a field of something or a method parameter.

1. N'utilisez pas optional sur votre retour de méthode si c'est un container (un tableau, une List ou Map), retournez un container vide.
2. N'utilisez pas optional pour vos attributs => Optional n'est pas sérialisable + modélisation perfectible (Une voiture avec des roues optionnelles est une carrosserie)
3. N'utilisez pas optional pour vos paramètres de méthode (<https://rules.sonarsource.com/java/RSPEC-3553>) => Contre productif (3 possiblités de valeurs : null, non-null-without-value and non-null-with-value)
4. N'affecter pas null à un optional

Exemples de mauvaise utilisation :

Combo violation 1 et 3

```java
 public List<SafetyReportGraphElement> execute(Optional<List<String>> definitionTypes,
                                                  ZonedDateTime startDate, ZonedDateTime endDate, String tenant)
```

Practice:

- <https://www.codingame.com/playgrounds/20782/java-guild-meeting-52018/optionals---outro>
- <https://dzone.com/articles/optional-ispresent-is-bad-for-you>
